---
title: Pianificare l'attestazione del servizio Sorveglianza host
description: Pianificare l'attestazione del servizio Sorveglianza host per SQL Server Always Encrypted con enclave sicure.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c4c80a51370de62410367b1225fd85e3ffe7f261
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534800"
---
# <a name="plan-for-host-guardian-service-attestation"></a>Pianificare l'attestazione del servizio Sorveglianza host

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Quando si usa [Always Encrypted con enclave sicure](always-encrypted-enclaves.md), accertarsi che l'applicazione client comunichi con un'enclave attendibile all'interno del processo di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Per un'enclave di sicurezza basata sulla virtualizzazione, questo requisito significa verificare che il codice all'interno dell'enclave sia valido e che il computer che ospita [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sia attendibile. L'attestazione remota introduce a questo scopo una terza parte che può convalidare l'identità (e, facoltativamente, la configurazione) del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Prima che [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] possa usare un enclave per eseguire una query, deve fornire al servizio di attestazione informazioni sull'ambiente operativo per ottenere un certificato di integrità. Questo certificato di integrità viene quindi inviato al client, che può verificarne in modo indipendente l'autenticità con il servizio di attestazione. Quando il client considera attendibile il certificato di integrità, sa di comunicare con un enclave di sicurezza basata sulla virtualizzazione attendibile ed eseguirà la query che userà tale enclave.

Il ruolo Servizio Sorveglianza host (HGS) in Windows Server 2019 fornisce funzionalità di attestazione remota per Always Encrypted con enclave di sicurezza basata sulla virtualizzazione.
Questo articolo illustra le decisioni relative alla pre-distribuzione e i requisiti per usare Always Encrypted con enclave di sicurezza basata sulla virtualizzazione e l'attestazione HGS.

## <a name="architecture-overview"></a>Panoramica dell'architettura

Il servizio Sorveglianza host (HGS) è un servizio Web in cluster eseguito in Windows Server 2019.
In una distribuzione tipica saranno presenti da 1 a 3 server HGS, almeno un computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e un computer che esegue un'applicazione client o strumenti come SQL Server Management Studio.
Poiché HGS deve determinare quali dei computer che eseguono [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] siano attendibili, richiede l'isolamento sia fisico che logico dall'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] protetta.
Se gli stessi amministratori hanno accesso a HGS e a un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], potrebbero configurare il servizio di attestazione per consentire a un computer dannoso di eseguire [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e compromettere in questo modo l'enclave di sicurezza basata sulla virtualizzazione.

### <a name="hgs-domain"></a>Dominio HGS

Il programma di installazione di HGS creerà automaticamente un nuovo dominio di Active Directory per i server HGS, le risorse cluster di failover e gli account amministratore.

Non è necessario che il computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sia in un dominio, ma, in caso contrario, deve trattarsi di un dominio diverso da quello usato dal server HGS.

### <a name="high-availability"></a>Disponibilità elevata

La funzionalità HGS installa e configura automaticamente un cluster di failover.
In un ambiente di produzione è consigliabile usare tre server HGS per la disponibilità elevata. Per informazioni dettagliate su come viene determinato il quorum del cluster e sulle configurazioni alternative, inclusi i cluster a due nodi con una risorsa di controllo esterna, vedere la [documentazione del cluster di failover](/windows-server/failover-clustering/manage-cluster-quorum).

L'archiviazione condivisa non è necessaria tra i nodi HGS. Una copia del database di attestazione viene archiviata in ogni server HGS e viene replicata automaticamente in rete dal servizio cluster.

### <a name="network-connectivity"></a>Connettività di rete

Sia il client SQL che [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] devono riuscire a comunicare con HGS su HTTP.
Configurare HGS con un certificato TLS per crittografare tutte le comunicazioni tra il client SQL e HGS, nonché tra SQL Server e HGS.
Questa configurazione offre protezione dagli attacchi man-in-the-middle e garantisce la comunicazione con il server HGS corretto.

I server HGS richiedono la connettività tra ogni nodo del cluster per garantire che il database del servizio di attestazione rimanga sincronizzato. Per i cluster di failover è consigliabile connettere i nodi HGS in una rete per la comunicazione del cluster e usare una rete separata per consentire agli altri client di comunicare con HGS.

### <a name="attestation-modes"></a>Modalità di attestazione

Quando un computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] prova a eseguire l'attestazione con HGS, chiederà prima di tutto a HGS come eseguire l'attestazione.
HGS supporta due modalità di attestazione per l'uso con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

| Modalità di attestazione | Spiegazione |
| ---------------- | ------- |
| TPM | L'attestazione TPM (Trusted Platform Module) offre la massima garanzia sull'identità e sull'integrità del computer che esegue l'attestazione con HGS. Richiede che nei computer che eseguono [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sia installato TPM versione 2.0. Ogni chip TPM contiene un'identità univoca e non modificabile (chiave di verifica dell'autenticità) che può essere usata per identificare un determinato computer. I moduli TPM misurano anche il processo di avvio del computer, archiviando hash di misurazioni basate sulla sicurezza nei registri PCR che possono essere letti, ma non modificati dal sistema operativo. Queste misurazioni vengono usate durante l'attestazione per fornire la prova crittografica che la configurazione di sicurezza di un computer corrisponda effettivamente a quella dichiarata. |
| Chiave host | L'attestazione chiave host è una forma più semplice di attestazione che verifica solo l'identità di un computer usando una coppia di chiavi asimmetriche. La chiave privata viene archiviata nel computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e la chiave pubblica viene fornita a HGS. La configurazione di sicurezza del computer non viene misurata e non è necessario un chip TPM 2.0 nel computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. È importante proteggere la chiave privata installata nel computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] perché chiunque ottenga questa chiave può rappresentare un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] legittimo e l'enclave di sicurezza basata sulla virtualizzazione in esecuzione all'interno di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |

In generale, tenere presenti le indicazioni seguenti:

- Per i **server di produzione fisici**, è consigliabile usare l'attestazione TPM per le garanzie aggiuntive fornite.
- Per i **server di produzione virtuali**, è consigliabile usare l'attestazione con chiave host perché la maggior parte delle macchine virtuali non ha TPM virtuali o l'avvio protetto. Se si usa una macchina virtuale con sicurezza avanzata, come una [macchina virtuale schermata locale](/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node), è possibile scegliere di usare la modalità TPM. In tutte le distribuzioni virtualizzate, il processo di attestazione analizza solo l'ambiente della macchina virtuale e non la piattaforma di virtualizzazione sottostante.
- Per gli **scenari di sviluppo/test**, è consigliabile usare l'attestazione con chiave host perché è più facile da configurare.

### <a name="trust-model"></a>Modello di attendibilità

Nel modello di attendibilità dell'enclave di sicurezza basata sulla virtualizzazione, le query e i dati crittografati vengono valutati in un'enclave basata su software per proteggerli dal sistema operativo host.
L'accesso a questo enclave è protetto dall'hypervisor nello stesso modo in cui due macchine virtuali in esecuzione nello stesso computer non possono accedere l'una alla memoria dell'altra.
Per consentire a un client di considerare attendibile la comunicazione con un'istanza legittima della sicurezza basata sulla virtualizzazione, è necessario usare l'attestazione basata su TPM che stabilisce una radice hardware di trust nel computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Le misurazioni TPM acquisite durante il processo di avvio includono la chiave di identità univoca dell'istanza della sicurezza basata sulla virtualizzazione, assicurando che il certificato di integrità sia valido solo in quel determinato computer.
Inoltre, quando un modulo TPM è disponibile in un computer che esegue la sicurezza basata sulla virtualizzazione, la parte privata della chiave di identità della sicurezza basata sulla virtualizzazione è protetta dal modulo TPM, che impedisce a chiunque di provare a rappresentare tale istanza della sicurezza basata sulla virtualizzazione.

L'avvio protetto è necessario con l'attestazione TPM per garantire che UEFI abbia caricato un bootloader legittimo firmato da Microsoft e che nessun rootkit abbia intercettato il processo di avvio dell'hypervisor.
Per impostazione predefinita, è anche necessario un dispositivo IOMMU per garantire che nessun dispositivo hardware con accesso diretto alla memoria possa ispezionare o modificare la memoria dell'enclave.

Tutte queste protezioni presuppongono che il computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sia un computer fisico.
Se si virtualizza il computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], non è più possibile garantire che la memoria della macchina virtuale sia protetta da ispezione da parte dell'hypervisor o dell'amministratore dell'hypervisor. Un amministratore dell'hypervisor potrebbe, ad esempio, eseguire il dump della memoria della macchina virtuale e ottenere l'accesso alla versione in testo non crittografato della query e dei dati nell'enclave.
Analogamente, anche se la macchina virtuale ha un modulo TPM virtuale, può solo misurare lo stato e l'integrità del sistema operativo e dell'ambiente di avvio della macchina virtuale.
Non può misurare lo stato dell'hypervisor che controlla la macchina virtuale.

Tuttavia, anche quando [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] è virtualizzato, l'enclave continua a essere protetta da attacchi provenienti dal sistema operativo della macchina virtuale.
Se si considera attendibile l'hypervisor o il provider di servizi cloud e la preoccupazione principale deriva dell'amministratore del database e dagli attacchi dell'amministratore del sistema operativo ai dati sensibili, un'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] virtualizzato può essere la soluzione ideale.

Analogamente, l'attestazione con chiave host resta utile nelle situazioni in cui non è installato un modulo TPM 2.0 nel computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o in scenari di sviluppo/test in cui la sicurezza non è fondamentale.
È comunque possibile usare molte delle funzionalità di sicurezza descritte in precedenza, tra cui l'avvio protetto e un modulo TPM 1.2, per proteggere meglio la sicurezza basata sulla virtualizzazione e il sistema operativo nel suo complesso.
Poiché tuttavia HGS non può verificare che nel computer queste impostazioni siano effettivamente abilitate con l'attestazione chiave host, il client non ha la certezza che l'host stia realmente usando tutte le protezioni disponibili.

## <a name="prerequisites"></a>Prerequisiti

### <a name="hgs-server-prerequisites"></a>Prerequisiti del server HGS

I computer che eseguono il ruolo Servizio Sorveglianza host devono soddisfare i requisiti seguenti:

| Componente | Requisito |
| --------- | ----------- |
| Sistema operativo | Windows Server 2019 Standard o Datacenter Edition |
| CPU | 2 core (min), 4 core (scelta consigliata) |
| RAM | 8 GB (min) |
| Schede di interfaccia di rete | Si consigliano 2 schede di interfaccia di rete con indirizzi IP statici (1 per il traffico del cluster, 1 per il servizio HGS) |

HGS è un ruolo associato alla CPU a causa del numero di azioni che richiedono la crittografia e la decrittografia.
L'uso di processori moderni con funzionalità di accelerazione della crittografia migliorerà le prestazioni di HGS.
I requisiti di archiviazione per i dati di attestazione sono minimi, da 10 KB a 1 MB per ogni singolo computer che esegue l'attestazione.

Non aggiungere i computer HGS a un dominio prima di iniziare la procedura.

### <a name="ssnoversion-md-computer-prerequisites"></a>Prerequisiti del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

I computer che eseguono [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] devono soddisfare sia i [requisiti per l'installazione di SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) che i [requisiti hardware di Hyper-V](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements#hardware-requirements).

Questi requisiti includono:

- [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] o versioni successive
- Windows 10 Enterprise versione 1809 o successiva oppure Windows Server 2019 edizione Datacenter. Le altre edizioni di Windows 10 e Windows Server non supportano l'attestazione con HGS.
- Supporto della CPU per le tecnologie di virtualizzazione:
  - Intel VT-x con Extended Page Tables.
  - AMD-V con Rapid Virtualization Indexing.
  - Se si esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in una macchina virtuale, l'hypervisor e la CPU fisica devono offrire funzionalità di virtualizzazione annidata. Per informazioni sulle garanzie quando si eseguono enclave di sicurezza basata sulla virtualizzazione in una macchina virtuale, vedere la sezione [Modello di attendibilità](#trust-model).
    - In Hyper-V 2016 o versione successiva [abilitare le estensioni di virtualizzazione annidata nel processore della macchina virtuale](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - In Azure selezionare una dimensione di macchina virtuale che supporta la virtualizzazione annidata, Tutte le macchine virtuali della serie v3 supportano la virtualizzazione annidata, ad esempio Dv3 ed Ev3. Vedere [Creare una VM di Azure in grado di supportare l'annidamento](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - In VMware vSphere 6.7 o versioni successive, abilitare il supporto della sicurezza basata sulla virtualizzazione per la macchina virtuale come descritto nella [documentazione di VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html).
    - Anche altri hypervisor e cloud pubblici possono supportare le funzionalità di virtualizzazione annidata che abilitano Always Encrypted con enclave di sicurezza basata sulla virtualizzazione. Vedere la documentazione della soluzione di virtualizzazione per informazioni sulla compatibilità e istruzioni per la configurazione.
- Se si prevede di usare l'attestazione TPM, è necessario un chip TPM 2.0 rev 1.16 pronto per l'uso nel server. Per il momento, l'attestazione HGS non funziona con i chip TPM 2.0 rev 1.38. Il modulo TPM deve anche avere un certificato valido della chiave di verifica dell'autenticità.

## <a name="roles-and-responsibilities-when-configuring-attestation-with-hgs"></a>Ruoli e responsabilità per la configurazione dell'attestazione con HGS

La configurazione dell'attestazione con HGS comporta la configurazione di componenti di tipi diversi: HGS, computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], istanze di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e applicazioni che attivano l'attestazione dell'enclave. La configurazione dei componenti di ogni tipo viene eseguita dagli utenti presupponendo uno dei ruoli distinti seguenti:

- Amministratore di HGS: distribuisce HGS, registra i computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in HGS e condivide l'URL di attestazione di HGS con amministratori di computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e amministratori delle applicazioni client.
- Amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]: installa i componenti client di attestazione, abilita VBS nei computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], fornisce all'amministratore di HGS le informazioni necessarie per registrare i computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in HGS, configura l'URL di attestazione nei computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e verifica che l'attestazione dei computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in HGS funzioni correttamente.
- DBA: configura le enclave sicure nelle istanze di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
- Amministratore delle applicazioni: configura l'applicazione con l'URL di attestazione ottenuto dall'amministratore di HGS.

Negli ambienti di produzione (che gestiscono dati sensibili reali), è importante che l'organizzazione rispetti la separazione dei ruoli durante la configurazione dell'attestazione, assicurandosi che ogni ruolo distinto venga assunto da persone diverse. In particolare, se l'obiettivo della distribuzione di Always Encrypted nell'organizzazione è quello di ridurre la superficie di attacco garantendo che gli amministratori di computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e i DBA non possano accedere ai dati sensibili, gli amministratori di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e i DBA non devono controllare i server HGS.

## <a name="devtest-environment-considerations"></a>Considerazioni sull'ambiente di sviluppo/test

Se si usa Always Encrypted con gli enclave do sicurezza basata sulla virtualizzazione in un ambiente di sviluppo o di test e non è necessaria la disponibilità elevata o la protezione avanzata del computer che esegue [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], è possibile applicare alcune o tutte le concessioni seguenti per una distribuzione semplificata:

- Distribuire un solo nodo di HGS. Anche se HGS installa un cluster di failover, non è necessario aggiungere altri nodi se la disponibilità elevata non è un problema.
- Usare la modalità chiave host invece della modalità TPM per semplificare la configurazione.
- Virtualizzare HGS e/o [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per salvare le risorse fisiche.
- Eseguire SSMS o altri strumenti per la configurazione di Always Encrypted con enclave sicuri nello stesso computer di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Poiché questo approccio lascia le chiavi master della colonna nello stesso computer di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], evitare di adottarlo in un ambiente di produzione.

## <a name="next-steps"></a>Passaggi successivi

- [Distribuire il servizio Sorveglianza host per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)