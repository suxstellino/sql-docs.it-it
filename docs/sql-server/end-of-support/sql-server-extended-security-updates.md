---
title: Che cosa sono gli aggiornamenti della sicurezza estesa?
description: Informazioni su come usare il Registro di sistema di SQL Server per ottenere gli aggiornamenti della sicurezza estesa per i prodotti SQL Server che hanno raggiunto la fine del supporto e a fine vita, ad esempio SQL Server 2008 e SQL Server 2008 R2.
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0f97fcf001db6fa561d54b3a9df6642f2aa148ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346171"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>Che cosa sono gli aggiornamenti della sicurezza estesa per SQL Server?
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo offre informazioni sull'uso del servizio del Registro di sistema di SQL Server per ricevere gli aggiornamenti della sicurezza estesa per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. Per altre informazioni su altre opzioni, vedere [Opzioni di fine del supporto](sql-server-end-of-life-overview.md). 

## <a name="overview"></a>Panoramica
Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha raggiunto la fine del ciclo di vita del supporto, è possibile scegliere di effettuare una sottoscrizione per gli aggiornamenti della sicurezza estesa per i server e rimanere protetti per un massimo di tre anni fino a quando non si è pronti a eseguire l'aggiornamento a una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a eseguire la migrazione a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Questa sottoscrizione è disponibile in due modi:
-  Può essere acquistata per i server di ambienti locali o ospitati.
-  In forma gratuita e abilitata per impostazione predefinita quando si esegue la migrazione di server locali a macchine virtuali di Azure. È quindi possibile usare il servizio del **Registro di sistema di SQL Server** nel portale di Azure per registrare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ha raggiunto la fine del supporto e scaricare gli aggiornamenti quando vengono resi disponibili. 

Microsoft consiglia di applicare le patch degli aggiornamenti della sicurezza estesa non appena sono disponibili per mantenere protetta l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni dettagliate sugli aggiornamenti della sicurezza estesa, vedere la pagina [Aggiornamenti di sicurezza estesa: domande frequenti](https://www.microsoft.com/cloud-platform/extended-security-updates).

> [!IMPORTANT]
> [Il supporto esteso per SQL Server 2008 e SQL Server 2008 R2 è terminato il 10 luglio 2019](https://www.microsoft.com/cloud-platform/windows-sql-server-2008). Per queste versioni, provare a usare gli aggiornamenti della sicurezza estesa descritti in questo articolo o altre opzioni di migrazione. Per altre informazioni, vedere [Opzioni di fine del supporto](sql-server-end-of-life-overview.md).

## <a name="what-are-extended-security-updates"></a>Che cosa sono gli aggiornamenti della sicurezza estesa
Gli aggiornamenti della sicurezza estesa per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] includono il provisioning degli aggiornamenti della sicurezza per i clienti che hanno acquistato una sottoscrizione di aggiornamento per il supporto esteso.

Gli aggiornamenti della sicurezza estesa vengono resi disponibili **se necessario** quando viene individuata una vulnerabilità di sicurezza classificata come **critica** da [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com). Per questa ragione, gli aggiornamenti della sicurezza estesa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non hanno una cadenza di rilascio regolare.

Gli aggiornamenti della sicurezza estesa non includono:
- Nuove funzionalità
- Miglioramenti funzionali
- Correzioni richieste dal cliente

### <a name="support"></a>Supporto
Gli aggiornamenti della sicurezza estesa non includono il supporto tecnico, ma è possibile usare un contratto di supporto attivo, ad esempio [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) o il supporto tecnico Premier/Unified per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] per ottenere supporto tecnico sui carichi di lavoro coperti dagli aggiornamenti della sicurezza estesa se si sceglie di lavorare in locale. In alternativa, se si esegue l'hosting in Azure, è possibile usare un piano di supporto di Azure per ottenere supporto tecnico. 

  > [!NOTE]
  > Microsoft non offre supporto tecnico per le istanze di [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] (sia locali che in ambienti host) che non sono coperte da una sottoscrizione degli aggiornamenti della sicurezza estesa. 

## <a name="esu-availability-and-deployment"></a>Disponibilità e distribuzione degli aggiornamenti della sicurezza estesa
Gli aggiornamenti della sicurezza estesa sono disponibili per i clienti che eseguono il carico di lavoro in ambienti Azure, locali o ospitati.

### <a name="azure-virtual-machines"></a>Macchine virtuali di Azure
Se si esegue la migrazione dei carichi di lavoro in macchine virtuali di Azure (IaaS), sarà possibile accedere agli aggiornamenti della sicurezza estesa per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] per un massimo di tre anni dopo la fine del supporto **senza costi aggiuntivi** oltre al costo di esecuzione della macchina virtuale. I clienti non necessitano di Software Assurance per ricevere gli aggiornamenti della sicurezza estesa in Azure. 

Le macchine virtuali di Azure che eseguono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in **Windows Server 2008 R2 e versioni successive** riceveranno automaticamente gli aggiornamenti della sicurezza estesa tramite i canali di aggiornamento [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistenti, quando la macchina virtuale è configurata per l'uso dell'[applicazione automatica delle patch](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching).

Le macchine virtuali di Azure che eseguono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in **Windows Server 2008** o le macchine virtuali che **_non_ sono state configurate per l'[applicazione automatica delle patch](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)** dovranno scaricare e distribuire manualmente le patch degli aggiornamenti della sicurezza estesa come descritto nella sezione [Ambienti locali o ospitati](#on-premises-or-hosted-environments).

### <a name="on-premises-or-hosted-environments"></a>Ambienti locali o ospitati
Se si ha Software Assurance, è possibile acquistare un abbonamento per gli aggiornamenti della sicurezza estesa per un massimo di tre anni dopo la data di fine del supporto con un Contratto Enterprise (EA), un Contratto Enterprise Subscription (EAS), un'iscrizione Server & Cloud Enrollment (SCE) o un'iscrizione Enrollment for Education Solutions (EES). È possibile acquistare gli aggiornamenti della sicurezza estesa solo per i server che è necessario coprire. Gli aggiornamenti della sicurezza estesa possono essere acquistati direttamente da Microsoft o da un partner licenze Microsoft. 

I clienti coperti con contratti per gli aggiornamenti della sicurezza estesa devono seguire questa procedura seguente per scaricare e distribuire una patch di aggiornamento della sicurezza estesa:
-  [Registrare le istanze idonee](#register-instances-for-esus) in **[Registro SQL Server](#create-sql-server-registry)** . 
-  Dopo aver eseguito la registrazione, ogni volta che vengono rilasciate patch di aggiornamento della sicurezza estesa, nel portale di Azure sarà disponibile un collegamento di download per scaricare il pacchetto. 
-  Il pacchetto scaricato può essere distribuito manualmente negli ambienti locali o ospitati o tramite qualsiasi soluzione di orchestrazione degli aggiornamenti usata nell'organizzazione, ad esempio Microsoft Endpoint Configuration Manager (in precedenza System Center Configuration Manager). 

> [!NOTE]
> Questo è anche il processo che i clienti dovranno eseguire per Azure Stack e Macchine virtuali di Microsoft Azure che non sono configurati per la ricezione degli aggiornamenti automatici.

Per altre informazioni, vedere [Aggiornamenti di sicurezza estesa: domande frequenti](https://www.microsoft.com/cloud-platform/extended-security-updates). 

## <a name="create-sql-server-registry"></a>Creare il registro di SQL Server
Per registrare le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilitate per gli aggiornamenti della sicurezza estesa, è necessario innanzitutto creare il registro di SQL Server nel portale di Azure. 

> [!IMPORTANT]
> Non è necessario registrare le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per gli aggiornamenti della sicurezza estesa quando si esegue una macchina virtuale di Azure configurata per gli [aggiornamenti automatici](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching). 

Per creare il registro di SQL Server, seguire questa procedura:

1. Accedere al [portale di Azure](https://portal.azure.com). 
1. Selezionare **Crea una risorsa**. 
1. Digitare `SQL Server registry` nella casella di ricerca.  
1. Scegliere l'opzione **Registro SQL Server** pubblicata da [!INCLUDE[msCoName](../../includes/msconame-md.md)] e quindi selezionare **Crea**. 

   ![Screenshot del portale di Azure che mostra come creare un registro di SQL Server.](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. In **Dettagli del progetto** scegliere la propria sottoscrizione dall'elenco a discesa. Quindi scegliere un **Gruppo di risorse** esistente o selezionare **Crea nuovo** per creare un nuovo gruppo di risorse per il nuovo servizio di registro SQL Server. 
1. In **Dettagli servizio** specificare un nome e un'area per la nuova risorsa **Registro SQL Server**: 

   ![Screenshot del registro di SQL Server che mostra la scheda Informazioni di base.](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. Selezionare **Rivedi e crea** per rivedere i dettagli del **Registro SQL Server**. Dopo aver superato la convalida, selezionare **Crea**. 

## <a name="register-instances-for-esus"></a>Registrare le istanze per gli aggiornamenti della sicurezza estesa

Dopo aver distribuito la risorsa **Registro SQL Server**, è possibile scegliere di registrare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [singola](#single-sql-server-instance) oppure è possibile registrare più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanze in [blocco](#multiple-sql-server-instances-in-bulk). Per poter scaricare un pacchetto degli aggiornamenti della sicurezza estesa, è necessario che almeno un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia registrata nell'ambito del registro SQL Server. 

### <a name="single-sql-server-instance"></a>Istanza di SQL Server singola

Per registrare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] singola, seguire questa procedura:

1. Accedere al [portale di Azure](https://portal.azure.com). 
1. Passare alla risorsa **Registro SQL Server**. 
1. Selezionare **+ Registra** dal riquadro **Panoramica**: 

   ![Scegliere Registra per registrare un'istanza singola di SQL Server](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. Specificare le informazioni richieste come indicato nella tabella seguente, quindi selezionare **Registra**: 

   |**Valore**| **Descrizione**|
   | :-------| :------------- |
   | **Istanza** | Immettere l'output del comando `SELECT @@SERVERNAME`, ad esempio `MyServer\Instance01`. | 
   | **Versione SQL** | Selezionare 2008 o 2008 R2 dall'elenco a discesa. | 
   | **Edizione** | Selezionare l'edizione applicabile dall'elenco a discesa: Datacenter, Developer (distribuzione gratuita se sono stati acquistati gli aggiornamenti della sicurezza estesa), Enterprise, Standard, Web, Workgroup. | 
   | **Core** | Immettere il numero di core per questa istanza | 
   | **Tipo di host** | Selezionare il tipo di host applicabile dall'elenco a discesa: Macchina virtuale (locale), Server fisico (locale), Macchina virtuale di Azure, Amazon EC2, Google Compute Engine, Altro. |
   | **ID sottoscrizione**<sup>1</sup> | Immettere l'ID sottoscrizione in cui viene creata la macchina virtuale.  |
   | **Gruppo di risorse**<sup>1</sup> | Immettere il gruppo di risorse in cui viene creata la macchina virtuale.  | 
   | **Nome macchina virtuale di Azure**<sup>1</sup>  | Immettere il nome della risorsa macchina virtuale.  | 
   | **Sistema operativo della macchina virtuale di Azure**<sup>1</sup> | Selezionare la versione del sistema operativo Windows Server applicabile dall'elenco a discesa. | 

   <sup>1</sup> Necessario solo per le macchine virtuali di Azure. 

L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appena registrata è ora visibile nella sezione **Registra istanze di SQL Server** del riquadro **Panoramica**: 

![Istanze di SQL Server registrate](media/sql-server-extended-security-updates/registered-sql-instance.png)

Dopo aver registrato un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], diventa disponibile la sezione **Aggiornamenti della sicurezza**. Tutti gli aggiornamenti della sicurezza estesa disponibili verranno pubblicati qui. 

### <a name="multiple-sql-server-instances-in-bulk"></a>Più istanze di SQL Server in blocco

È possibile registrare più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in blocco caricando un file CSV. Dopo che il [file CSV è stato formattato correttamente](#formatting-requirements-for-csv-file), è possibile seguire questa procedura per registrare in blocco le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la risorsa Registro SQL Server: 

1. Accedere al [portale di Azure](https://portal.azure.com). 
1. Passare alla risorsa **Registro SQL Server**. 
1. Selezionare **Registra in blocco** dal riquadro **Panoramica**:  

   ![Scegliere la registrazione in blocco per registrare più istanze di SQL Server](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. Selezionare l'icona del file per passare al percorso del file CSV. Selezionare il file CSV. Quindi selezionare **Registra** per caricare il file e registrare più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

   ![Caricare il file CSV per registrare più istanze di SQL Server](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>Requisiti per la formattazione di un file CSV
- I valori sono delimitati da virgole
- I valori non sono inclusi tra virgolette singole o doppie
- I nomi di colonna non fanno distinzione tra maiuscole e minuscole, ma devono essere **denominati** come indicato di seguito: 
  - name
  - version
  - edition
  - cores
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup> Necessario solo per le macchine virtuali di Azure. 

#### <a name="csv-example-1---on-premises"></a>Esempio di CSV 1 - locale

Per le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locali, il file CSV dovrebbe essere simile al seguente: 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

Per un esempio di file CSV, fare riferimento a [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv).

#### <a name="csv-example-2---azure-vm"></a>Esempio di CSV 2 - macchina virtuale di Azure

Per le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su macchine virtuali di Azure, il file CSV dovrebbe essere simile al seguente: 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

Per un esempio di file CSV con destinazione una macchina virtuale di Azure, fare riferimento a [MyAzureVMs.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv). 


> [!TIP]
> Per gli script di esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] e PowerShell che possono generare le informazioni di registrazione necessarie per l'istanza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un file CSV, vedere gli [esempi di script di registrazione degli aggiornamenti della sicurezza estesa](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md). 

## <a name="download-esus"></a>Scaricare gli aggiornamenti della sicurezza estesa

Dopo aver registrato le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il servizio Registro SQL Server, è possibile scaricare i pacchetti degli aggiornamenti della sicurezza estesa usando il collegamento disponibile nel portale di Azure, se e quando vengono resi disponibili. 

Per scaricare gli aggiornamenti della sicurezza estesa, seguire questa procedura: 

1. Accedere al [portale di Azure](https://portal.azure.com). 
1. Passare alla risorsa **Registro SQL Server**. 
1. Selezionare **Aggiornamenti della sicurezza** nel riquadro di spostamento. 

   ![Controllare la presenza di aggiornamenti disponibili nel riquadro Aggiornamenti della sicurezza](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. Scaricare gli aggiornamenti della sicurezza da qui, se e quando vengono resi disponibili. 

## <a name="supported-regions-and-data-residency"></a>Aree supportate e residenza dei dati

Il servizio **Registro SQL Server** (anteprima) è disponibile in un subset delle aree di Azure. Nella tabella seguente vengono illustrate le aree supportate e il tipo di residenza dei dati in ognuna di esse.

| **Area** | **Residenza dei dati** |
|:--|:--|
|Australia orientale|Area geografica|
|Australia sud-orientale|Area geografica|
|Canada centrale|Area geografica|
|Francia centrale|Area geografica|
|Giappone orientale|Area geografica|
|Giappone occidentale|Area geografica|
|Corea centrale|Area geografica|
|Corea meridionale|Area geografica|
|Stati Uniti centro-settentrionali|Area geografica|
|Europa settentrionale|Area geografica|
|Stati Uniti centro-meridionali|Area geografica|
|Asia sud-orientale|Area singola|
|India meridionale|Area geografica|
|Sudafrica settentrionale|Area geografica|
|Regno Unito meridionale|Area geografica|
|Regno Unito occidentale|Area geografica|
|Stati Uniti occidentali|Area geografica|
|Stati Uniti orientali|Area geografica|
|Stati Uniti centrali|Area geografica|
|Asia orientale|Area geografica|
|Europa occidentale|Area geografica|
|Stati Uniti centro-occidentali|Area geografica|
|West US 2|Area geografica|
|Stati Uniti orientali 2|Area geografica|

Nelle aree con residenza geografica, il servizio registro di sistema SQL gestisce i backup dei dati in un account di archiviazione con ridondanza geografica (GRS).  Nelle aree con residenza geografica unica, il servizio registro di sistema SQL gestisce i backup dei dati in un account di archiviazione con ridondanza della zona (ZRS). Per altre informazioni, visitare il [Centro protezione](https://azuredatacentermap.azurewebsites.net/).

## <a name="configure-regional-redundancy"></a>Configurare la ridondanza a livello di area 

I clienti che richiedono la ridondanza a livello di area per il **Registro di sistema di SQL Server** possono creare dati di registrazione in due aree distinte. I clienti possono quindi scaricare aggiornamenti della sicurezza da entrambe le aree in base alla disponibilità del servizio **Registro di sistema di SQL Server**. 

Per la ridondanza a livello di area, è necessario creare il servizio **Registro di sistema di SQL Server** in due aree diverse e l'inventario SQL Server deve essere suddiviso tra i due servizi. In questo modo la metà dei server SQL Server vengono registrati con il servizio Registro di sistema in un'area e quindi l'altra metà dei server vengono registrati con il servizio Registro di sistema nell'altra area. 

Per configurare la ridondanza a livello di area seguire questa procedura:

1. Dividere l'inventario SQL Server 2008 o 2008 R2 in due file, ad esempio upload1.csv e upload2.csv. 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="Esempio di caricamento dei file":::

1. Creare il primo servizio **Registro di sistema di SQL Server** in un'area e quindi eseguire la registrazione in blocco di uno dei file con estensione csv in questo servizio. Ad esempio creare il primo servizio **Registro di sistema di SQL Server** nell'area **West US** (Stati Uniti occidentali) ed eseguire la registrazione in blocco dei server SQL Server usando il file upload1.csv. 
1. Creare il secondo servizio **Registro di sistema di SQL Server** nella seconda area e quindi eseguire la registrazione in blocco di uno dei file con estensione csv in questo servizio. Ad esempio creare il secondo servizio **Registro di sistema di SQL Server** nell'area **East US** (Stati Uniti orientali) ed eseguire la registrazione in blocco dei server SQL Server usando il file upload2.csv. 


Dopo che i dati sono stati registrati con le due risorse **Registro di sistema di SQL Server** diverse è possibile scaricare gli aggiornamenti della sicurezza da entrambe le aree, a seconda della disponibilità del servizio. 


## <a name="faq"></a>Domande frequenti

Le domande frequenti generali sugli aggiornamenti della sicurezza estesa sono disponibili in [Aggiornamenti di sicurezza estesa: domande frequenti](https://www.microsoft.com/cloud-platform/extended-security-updates). Le domande frequenti specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono elencate di seguito. 

**Quando è stato terminato il supporto per SQL Server 2008 e 2008 R2?**

La data di fine del supporto per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] è stata il 9 luglio 2019. 

**Cosa significa fine del supporto?**

I criteri del ciclo di vita dei prodotti Microsoft offrono un supporto di 10 anni (5 anni per il supporto Mainstream e 5 per quello Extended) per i prodotti Business e Developer, ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Windows Server. Come indicato nei criteri, al termine del periodo di supporto Extended non verranno forniti altri aggiornamenti della sicurezza o patch. Pertanto potrebbero verificarsi problemi di sicurezza o conformità e le applicazioni e le attività aziendali dei clienti potrebbero essere esposte a gravi rischi per la sicurezza.

**Quali edizioni di SQL Server sono idonee per gli aggiornamenti della sicurezza estesa?**

Le edizioni Enterprise, Datacenter, Standard, Web e Workgroup di [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] sono idonee per gli aggiornamenti della sicurezza estesa, sia per le versioni x86 sia per quelle x64. 

**Quando sarà disponibile l'offerta per gli aggiornamenti della sicurezza estesa?**

Gli aggiornamenti della sicurezza estesa sono ora disponibili per l'acquisto e possono essere ordinati a [!INCLUDE[msCoName](../../includes/msconame-md.md)] o a un partner licenze [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Il recapito degli aggiornamenti della sicurezza estesa avrà inizio dopo le date di fine del supporto, se e quando disponibili. I clienti interessati alla migrazione ad Azure possono eseguirla immediatamente. 

**Cosa includono gli aggiornamenti della sicurezza estesa?** 

Gli aggiornamenti della sicurezza estesa includono il provisioning degli aggiornamenti della sicurezza e dei bollettini classificati come **critici** da [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/) per un massimo di tre anni dopo il 9 luglio 2019. Gli aggiornamenti della sicurezza estesa verranno distribuiti se e quando disponibili. Non includono il supporto tecnico, ma è possibile usare altri piani di supporto [!INCLUDE[msCoName](../../includes/msconame-md.md)] per ottenere assistenza per le domande relative alle versioni [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] per i carichi di lavoro coperti dagli aggiornamenti della sicurezza estesa. Gli aggiornamenti della sicurezza estesa non includono nuove funzionalità, miglioramenti funzionali o correzioni richieste dai clienti. Tuttavia, [!INCLUDE[msCoName](../../includes/msconame-md.md)] potrebbe includere alcune correzioni non di sicurezza, se dovesse reputarlo necessario.

**Perché gli aggiornamenti della sicurezza estesa per SQL Server 2008 e 2008 R2 offrono solo aggiornamenti "critici"?**

Per gli eventi di fine supporto, in passato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offriva solo aggiornamenti della sicurezza di livello critico che rispettavano i criteri di conformità dei clienti aziendali. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non invia un aggiornamento della sicurezza mensile generale. [!INCLUDE[msCoName](../../includes/msconame-md.md)] offre solo aggiornamenti della sicurezza per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su richiesta (GDR) per i bollettini MSRC in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è indicato come prodotto interessato.
Se dovessero verificarsi situazioni per cui non vengono forniti nuovi aggiornamenti importanti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ritenuti critici dal cliente ma non da MSRC, Microsoft contatterà il cliente caso per caso per suggerire una soluzione appropriata.

**Quali programmi di licenza sono idonei per gli aggiornamenti della sicurezza estesa?**

I clienti Software Assurance possono acquistare gli aggiornamenti della sicurezza estesa in locale con un Contratto Enterprise (EA), un Contratto Enterprise Subscription (EAS), un'iscrizione Server & Cloud Enrollment (SCE) o un programma Enrollment for Education Solutions (EES). Software Assurance non deve essere necessariamente incluso nella stessa iscrizione.

**I clienti SQL Server devono eseguire il Service Pack più recente per ottenere gli aggiornamenti della sicurezza estesa?**

Sì, i clienti devono eseguire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Windows Server 2008 e 2008 R2 con il Service Pack più recente per applicare gli aggiornamenti della sicurezza estesa. [!INCLUDE[msCoName](../../includes/msconame-md.md)] produrrà solo aggiornamenti che possono essere applicati al Service Pack più recente.

**Quali sono le opzioni per i clienti SQL Server senza Software Assurance?** 

I clienti che non hanno aderito al programma Software Assurance possono in alternativa eseguire la migrazione ad Azure per accedere agli aggiornamenti della sicurezza estesa. Per i carichi di lavoro variabili, è consigliabile che i clienti eseguano la migrazione ad Azure con il pagamento in base al consumo che consente di aumentare e ridurre le risorse in qualsiasi momento. Per i carichi di lavoro prevedibili, è consigliabile che i clienti eseguano la migrazione ad Azure con sottoscrizione server e istanze riservate.
  
**L'offerta si applica a SQL Server 2005?**

No. Per le versioni precedenti, si consiglia di eseguire l'aggiornamento alle versioni più recenti ma i clienti possono eseguire l'aggiornamento alle versioni [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] per usufruire dell'offerta.

**È possibile distribuire una nuova istanza di SQL Server 2008 o 2008 R2 in Azure e continuare a ricevere gli aggiornamenti della sicurezza estesa?**

Sì, i clienti possono avviare un'istanza di [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] nuova in una macchina virtuale di Azure e avere accesso agli aggiornamenti della sicurezza estesa.

**È possibile ricevere supporto tecnico locale per SQL Server 2008 o 2008 R2 dopo la data di fine del supporto senza acquistare gli aggiornamenti della sicurezza estesa?**

No. Se un cliente ha [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] e sceglie di rimanere in locale durante una migrazione senza aggiornamenti della sicurezza estesa, il cliente non può registrare un ticket di supporto, anche nel caso disponga di un piano di supporto. Tuttavia, se esegue la migrazione ad Azure, il cliente può ottenere supporto usando il proprio piano di supporto tecnico di Azure.

**Se un cliente di SQL Server 2008 e 2008 R2 vuole usare la propria licenza (BYOL), è necessario che abbia la copertura Software Assurance?**

Sì, i clienti devono avere Software Assurance per poter usare il programma BYOL per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nelle macchine virtuali di Azure nell'ambito del programma Mobilità delle licenze. Ai clienti senza Software Assurance si consiglia la migrazione dei loro ambienti [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] a Istanza gestita di SQL di Azure. I clienti possono anche eseguire la migrazione a macchine virtuali di Azure con pagamento in base al consumo. I clienti Software Assurance che acquistano una licenza SQL per core possono anche eseguire la migrazione ad Azure usando il Vantaggio Azure Hybrid.

Istanza gestita di SQL di Azure è un servizio di Azure che offre una compatibilità praticamente totale con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. Istanza gestita offre capacità predefinite di disponibilità elevata/ripristino di emergenza, oltre a funzionalità di prestazioni intelligenti e di scalabilità immediata. Istanza gestita offre anche un'esperienza senza versione che non richiede l'applicazione manuale di aggiornamenti e patch di sicurezza. Per altre informazioni sul programma BYOL, vedere la pagina relativa ai prezzi di Azure.

**Quali sono le opzioni disponibili per i clienti per eseguire SQL Server in Azure?**

I clienti possono spostare gli ambienti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] legacy in Istanza gestita di SQL di Azure, un servizio della piattaforma dati (PaaS) completamente gestito che offre un'opzione "senza versione" per eliminare le preoccupazioni legate alle data di fine del supporto oppure in Macchine virtuali di Microsoft Azure per avere accesso agli aggiornamenti della sicurezza. I database di cui è stata eseguita la migrazione manterranno la loro compatibilità con il sistema legacy. Per altre informazioni, vedere [Certificazione della compatibilità](../../database-engine/install-windows/compatibility-certification.md).

Gli aggiornamenti della sicurezza estesa saranno disponibili per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] in Macchine virtuali di Microsoft Azure dopo la data di fine del supporto del 9 luglio 2019 per i successivi tre anni. Per i clienti che vogliono eseguire l'aggiornamento da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] saranno supportate tutte le versioni successive di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per le versioni comprese tra [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], i clienti devono avere il Service Pack supportato più recente. A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], viene consigliato ai clienti di scaricare l'aggiornamento cumulativo più recente. Si noti che i Service Pack non saranno disponibili a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]. Possono essere scaricati solo gli aggiornamenti cumulativi e le General Distribution Release (GDR).

Istanza gestita di SQL di Azure è un'opzione di distribuzione nell'ambito dell'istanza in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] che offre la massima compatibilità con il motore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il supporto di rete virtuale (VNET) nativo in modo che sia possibile eseguire la migrazione dei database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a Istanza gestita senza apportare modifiche alle app. Unisce la ricca superficie di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ai vantaggi operativi e finanziari di un servizio intelligente completamente gestito. Usare il nuovo Servizio Migrazione del database di Azure per spostare [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] in Istanza gestita di SQL di Azure con poche o nessuna modifica al codice dell'applicazione.

**I clienti possono usare il Vantaggio Azure Hybrid per SQL Server 2008 e 2008 R2?**

Sì, i clienti con una licenza Software Assurance attiva o una sottoscrizione server equivalente possono usare il Vantaggio Azure Hybrid usando gli investimenti in licenze locali esistenti per ottenere prezzi scontati per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguito in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e in Macchine virtuali di Microsoft Azure.

**I clienti possono ottenere gli aggiornamenti della sicurezza estesa gratuiti nelle aree di Azure per enti pubblici?**

Sì, gli aggiornamenti della sicurezza estesa saranno disponibili in Macchine virtuali di Azure e nelle aree di Azure per enti pubblici.

**I clienti possono ottenere gli aggiornamenti della sicurezza estesa gratuiti in Azure Stack?**

Sì, i clienti possono eseguire la migrazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Windows Server 2008 e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] ad Azure Stack e ricevere gli aggiornamenti della sicurezza estesa senza alcun costo aggiuntivo dopo le date di fine del supporto.

**Per i clienti con un cluster 2008 e 2008 R2 SQL che usa l'archiviazione condivisa, quali sono le istruzioni per la migrazione ad Azure?**

Azure non supporta attualmente il clustering di archiviazione condivisa. Per suggerimenti su come configurare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a disponibilità elevata in Azure, vedere la [guida sulla disponibilità elevata di SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr).

**I clienti possono usare gli aggiornamenti della sicurezza estesa per SQL Server con un servizio di hosting di terze parti?**

I clienti non possono usare gli aggiornamenti della sicurezza estesa se spostano l'ambiente [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] in un'implementazione PaaS su altri provider di servizi cloud. Se i clienti vogliono passare alle macchine virtuali (IaaS), possono usare Mobilità delle licenze per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite Software Assurance per eseguire il trasferimento e acquistare gli aggiornamenti della sicurezza estesa da [!INCLUDE[msCoName](../../includes/msconame-md.md)] per applicare manualmente le patch alle istanze di [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] eseguite su una macchina virtuale (IaaS) in un server di un servizio di hosting SPLA autorizzato. Tuttavia, gli aggiornamenti gratuiti di Azure restano l'offerta più allettante.

**Quali sono le procedure consigliate per migliorare le prestazioni di SQL Server nelle macchine virtuali di Azure?** 

Per suggerimenti su come ottimizzare le prestazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nelle macchine virtuali di Azure, vedere la [guida per l'ottimizzazione di SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance). 

## <a name="see-also"></a>Vedere anche

- [Pagina del ciclo di vita di SQL Server 2008/2008 R2](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [Pagina della fine del supporto di SQL Server 2008/2008 R2](./sql-server-end-of-life-overview.md?WT.mc_id=akamseos)
- [Aggiornamenti di sicurezza estesa: domande frequenti](https://aka.ms/sqleosfaq)
- [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [Gestire gli aggiornamenti di Windows con Automazione di Azure](/azure/automation/update-management/overview)
- [Applicazione automatica delle patch per le VM di SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Guida sulla migrazione dei dati Microsoft](https://datamigration.microsoft.com/)
- [Migrazione ad Azure: opzioni lift-and-shift per spostare SQL Server 2008/2008 R2 in una macchina virtuale di Azure](https://azure.microsoft.com/services/azure-migrate/)
- [Cloud Adoption Framework per la migrazione SQL](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [Script per gli aggiornamenti della sicurezza estesa in GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)