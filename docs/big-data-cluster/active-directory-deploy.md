---
title: Distribuire in modalità Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come aggiornare un cluster Big Data di SQL Server in un dominio di Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 02/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9417444a1c9d28181529ace79b6dcff6162b7f2d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837033"
---
# <a name="deploy-sql-server-big-data-cluster-in-active-directory-mode"></a>Distribuire un cluster Big Data di SQL Server in modalità Active Directory

Questo articolo descrive come distribuire un cluster Big Data di SQL Server in modalità Active Directory. La procedura descritta in questo articolo richiede l'accesso a un dominio di Active Directory. Prima di procedere, è necessario completare i requisiti descritti in [Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-prerequisites.md).

## <a name="prepare-deployment"></a>Preparare la distribuzione

Per la distribuzione di un cluster Big Data con l'integrazione di Active Directory, è necessario fornire alcune informazioni aggiuntive per la creazione degli oggetti correlati al cluster Big Data in Active Directory.

Usando il profilo `kubeadm-prod` (o `openshift-prod` a partire dalla versione CU5), verranno automaticamente immessi i segnaposto per le informazioni relative alla sicurezza e all'endpoint necessarie per l'integrazione di Active Directory.

Inoltre, è necessario fornire le credenziali che verranno usate da [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] per creare gli oggetti necessari in Active Directory. Queste credenziali vengono fornite come variabili di ambiente.

### <a name="traffic-and-ports"></a>Traffico e porte

Verificare che tutti i firewall o le applicazioni di terze parti consentano le porte necessarie per la comunicazione Active Directory. 

![Diagramma di traffico tra cluster di Big Data e Active Directory. Il controller, il servizio di supporto per la sicurezza e altri servizi cluster parlano tramite LDAP/Kerberos ai controller di dominio. Il servizio proxy DNS BDC parla tramite DNS ai server DNS.](media/big-data-cluster-overview/big-data-cluster-active-directory-dns-traffic-ports.png)

Le richieste vengono eseguite su questi protocolli da e verso i servizi cluster Kubernetes al dominio Active Directory, quindi dovrebbero essere consentiti in ingresso e in uscita in qualsiasi firewall o applicazione di terze parti in ascolto sulle porte necessarie sia per TCP che per UDP. Numeri di porta standard utilizzati da Active Directory:

| Servizio | Porta |
|:---|:---|
| DNS | 53 |
| LDAP <BR> LDAPS | 389<BR> 636 |
| Kerberos | 88 |
| Porta del catalogo globale <BR>tramite LDAP<BR>tramite LDAPs |<BR> 3268 <BR> 3269 |

## <a name="set-security-environment-variables"></a>Impostare le variabili di ambiente di sicurezza

Le variabili di ambiente seguenti forniscono le credenziali per l'account del servizio di dominio del cluster Big Data, che verrà usato per configurare l'integrazione di Active Directory. Questo account viene usato anche dal cluster Big Data per mantenere gli oggetti di Active Directory correlati al cluster Big Data stesso in futuro.

```bash
export DOMAIN_SERVICE_ACCOUNT_USERNAME=<AD principal account name>
export DOMAIN_SERVICE_ACCOUNT_PASSWORD=<AD principal password>
```

## <a name="provide-security-and-endpoint-parameters"></a>Fornire parametri di sicurezza e dell'endpoint

Oltre alle variabili di ambiente per le credenziali, è necessario fornire anche le informazioni sulla sicurezza e sull'endpoint per il funzionamento dell'integrazione di Active Directory. I parametri necessari sono inclusi automaticamente nel [profilo di distribuzione](deployment-guidance.md#configfile) `kubeadm-prod`/`openshift-prod`.

Per l'integrazione di Active Directory sono necessari i parametri seguenti. Aggiungere questi parametri ai file `control.json` e `bdc.json` usando i comandi `config replace` mostrati più avanti in questo articolo. Tutti gli esempi seguenti usano il dominio di esempio `contoso.local`.

- `security.activeDirectory.ouDistinguishedName`: nome distinto di un'unità organizzativa (OU) in cui verranno aggiunti tutti gli account Active Directory creati tramite la distribuzione del cluster. Se il dominio è denominato `contoso.local`, il nome distinto dell'unità organizzativa è: `OU=BDC,DC=contoso,DC=local`.

- `security.activeDirectory.dnsIpAddresses`: contiene l'elenco di indirizzi IP dei server DNS del dominio. 

- `security.activeDirectory.domainControllerFullyQualifiedDns`: elenco di nomi di dominio completi del controller di dominio. Il nome di dominio completo contiene il nome computer/host del controller di dominio. Se sono presenti più controller di dominio, è possibile fornirne un elenco qui. Esempio: `HOSTNAME.CONTOSO.LOCAL`.

  > [!IMPORTANT]
  > Quando più controller di dominio gestiscono un dominio, usare il controller di dominio primario (PDC) come prima voce nell'elenco `domainControllerFullyQualifiedDns` della configurazione di sicurezza. Per ottenere il nome del PDC, digitare `netdom query fsmo` al prompt dei comandi e quindi premere **INVIO**.

- `security.activeDirectory.realm` **Parametro facoltativo**: nella maggior parte dei casi, l'area di autenticazione è uguale al nome di dominio. Per i casi in cui non sono uguali, usare questo parametro per definire il nome dell'area di autenticazione (ad esempio, `CONTOSO.LOCAL` ). Il valore specificato per questo parametro deve essere completo.

- `security.activeDirectory.netbiosDomainName`**Parametro facoltativo**: si tratta del nome NetBIOS del dominio ad. Nella maggior parte dei casi, si tratta della prima etichetta del nome di dominio AD. Per i casi in cui si differenzia, usare questo parametro per definire il nome di dominio NETBIOS. Questo valore non deve contenere punti. Questo nome viene in genere usato per qualificare gli account utente nel dominio. Ad esempio, CONTOSO\user, dove CONTOSO è il nome di dominio NETBIOS.

  > [!NOTE]
  > Supporto per una configurazione in cui il nome di dominio Active Directory è diverso dal nome **NetBIOS** del dominio Active Directory mediante la *sicurezza. activeDirectory. netbiosDomainName* è stato abilitato a partire da SQL Server 2019 CU9.

- `security.activeDirectory.domainDnsName`: Nome del dominio DNS che verrà usato per il cluster (ad esempio, `contoso.local` ).

- `security.activeDirectory.clusterAdmins`: questo parametro accetta un solo gruppo di AD. L'ambito del gruppo di AD deve essere universale o globale. I membri di questo gruppo avranno il ruolo del cluster `bdcAdmin` che concederà autorizzazioni di amministratore nel cluster. Questo significa che avranno [autorizzazioni `sysadmin` in SQL Server](../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles), [autorizzazioni `superuser` in HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The_Super-User) e autorizzazioni di amministratore quando connessi all'endpoint del controller.

  >[!IMPORTANT]
  >Creare questo gruppo in AD prima dell'avvio della distribuzione. Se l'ambito di questo gruppo di AD è il dominio, la distribuzione locale ha esito negativo.

- `security.activeDirectory.clusterUsers`: elenco dei gruppi di Active Directory che sono utenti normali (senza autorizzazioni di amministratore) nel cluster Big Data. L'elenco può includere gruppi di AD che hanno come ambito gruppi globali o universali. Non possono essere gruppi locali di dominio.

I gruppi di AD in questo elenco sono associati al ruolo del cluster Big Data `bdcUser` e necessitano dell'accesso a SQL Server (vedere [Autorizzazioni di SQL Server](../relational-databases/security/permissions-hierarchy-database-engine.md)) o HDFS (vedere [Guida alle autorizzazioni per HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#:~:text=Permission%20Checks%20%20%20%20Operation%20%20,%20%20N%2FA%20%2029%20more%20rows%20)). Quando sono connessi all'endpoint del controller, questi utenti possono elencare solo gli endpoint disponibili nel cluster usando il comando `azdata bdc endpoint list`.

Per informazioni dettagliate su come aggiornare i gruppi di Active Directory per queste impostazioni, vedere [gestire l'accesso ai cluster di Big data in modalità Active Directory](manage-user-access.md).

  >[!TIP]
  >Per abilitare l'esperienza di esplorazione HDFS quando si è connessi all'istanza master di SQL Server in Azure Data Studio, è necessario concedere a un utente con ruolo bdcUser le autorizzazioni VIEW SERVER STATE perché Azure Data Studio usa il DMV `sys.dm_cluster_endpoints` per ottenere l'endpoint del gateway Knox richiesto per la connessione a HDFS.

  >[!IMPORTANT]
  >Creare questi gruppi in AD prima che venga avviata la distribuzione. Se l'ambito per uno di questi gruppi di AD è locale al dominio, la distribuzione non riesce.

  >[!IMPORTANT]
  >Se gli utenti del dominio hanno un numero elevato di appartenenze ai gruppi, è consigliabile modificare i valori per l'impostazione del gateway `httpserver.requestHeaderBuffer` (il valore predefinito è `8192`) e l'impostazione HDFS `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` (il valore predefinito è `10`), usando il file di configurazione della distribuzione *bdc.json* personalizzato. Si tratta di una procedura consigliata per evitare i timeout di connessione al gateway e/o le risposte HTTP con un codice di stato 431 (*Campi intestazione richiesta troppo grandi*). Di seguito è riportata una sezione del file di configurazione che mostra come definire i valori di queste impostazioni e indica i valori consigliati per un numero maggiore di appartenenze ai gruppi:

```json
{
    ...
    "spec": {
        "resources": {
            ...
            "gateway": {
                "spec": {
                    "replicas": 1,
                    "endpoints": [{...}],
                    "settings": {
                        "gateway-site.gateway.httpserver.requestHeaderBuffer": "65536"
                    }
                }
            },
            ...
        },
        "services": {
            ...
            "hdfs": {
                "resources": [...],
                "settings": {
                  "core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels": "4"
                }
            },
            ...
        }
    }
}
```

  >[!IMPORTANT]
  >Creare i gruppi specificati per le impostazioni di seguito in Active Directory prima che venga avviata la distribuzione. Se l'ambito per uno di questi gruppi di AD è locale al dominio, la distribuzione non riesce.

- `security.activeDirectory.appOwners` **Parametro facoltativo**: elenco dei gruppi di AD che hanno le autorizzazioni necessarie per creare, eliminare ed eseguire qualsiasi applicazione. L'elenco può includere gruppi di AD che hanno come ambito gruppi globali o universali. Non possono essere gruppi locali di dominio.

- `security.activeDirectory.appReaders` **Parametro facoltativo**: elenco dei gruppi di AD che hanno le autorizzazioni necessarie per eseguire qualsiasi applicazione. L'elenco può includere gruppi di AD che hanno come ambito gruppi globali o universali. Non possono essere gruppi locali di dominio.

La tabella seguente mostra il modello di autorizzazione per la gestione delle applicazioni:

|   Ruoli autorizzati   |   Comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]   |
|----------------------|--------------------|
|   appOwner           | azdata app create  |
|   appOwner           | azdata app update  |
|   appOwner, appReader| azdata app list    |
|   appOwner, appReader| azdata app describe|
|   appOwner           | azdata app delete  |
|   appOwner, appReader| azdata app run     |

- `security.activeDirectory.subdomain`: **Parametro facoltativo**: questo parametro è stato introdotto in SQL Server 2019 CU5 per supportare la distribuzione di più cluster Big Data nello stesso dominio. Usando questa impostazione, è possibile specificare nomi DNS diversi per ogni cluster Big Data distribuito. Se il valore di questo parametro non è specificato nella sezione relativa ad Active Directory del file `control.json`, per impostazione predefinita verrà usato il nome del cluster Big Data (uguale al nome dello spazio dei nomi Kubernetes) per calcolare il valore dell'impostazione del sottodominio. 

  >[!NOTE]
  >Il valore passato tramite l'impostazione del sottodominio non è un nuovo dominio di Active Directory, ma solo un dominio DNS usato dal cluster BDC internamente.

  >[!IMPORTANT]
  >È necessario installare o aggiornare la versione più recente di **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** a partire da SQL Server 2019 CU5 per sfruttare queste nuove funzionalità e distribuire più cluster Big Data nello stesso dominio.

  Per altri dettagli sulla distribuzione di più cluster Big Data nello stesso dominio di Active Directory, vedere [Concetto: distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deployment-background.md).

- `security.activeDirectory.accountPrefix`: **Parametro facoltativo**: questo parametro è stato introdotto in SQL Server 2019 CU5 per supportare la distribuzione di più cluster Big Data nello stesso dominio. Questa impostazione garantisce l'univocità dei nomi degli account per vari servizi dei cluster Big Data, che devono essere diversi tra due cluster. La personalizzazione del nome del prefisso dell'account è facoltativa. Per impostazione predefinita, come prefisso dell'account viene usato il nome del sottodominio. Se il nome del sottodominio supera i 12 caratteri, come prefisso dell'account vengono usati i primi 12 caratteri del nome del sottodominio.  

  >[!NOTE]
  >Active Directory impone il limite di 20 caratteri per i nomi degli account. Il cluster BDC deve usare 8 caratteri per distinguere pod e StatefulSet. Rimangono così al massimo 12 caratteri per il prefisso dell'account.

[Controllare l'ambito del gruppo AD](/powershell/module/addsadministration/get-adgroup) per determinare se è DomainLocal.

Se il file di configurazione della distribuzione non è stato ancora inizializzato, è possibile eseguire questo comando per ottenere una copia della configurazione. Gli esempi seguenti usano il profilo `kubeadm-prod`, ma la stessa procedura è applicabile a `openshift-prod`.

```bash
azdata bdc config init --source kubeadm-prod  --target custom-prod-kubeadm
```

Per impostare i parametri precedenti nel file `control.json`, usare i comandi [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] seguenti. I comandi sostituiscono la configurazione e forniscono i valori personalizzati prima della distribuzione.

> [!IMPORTANT]
> Nella versione SQL Server 2019 CU2 la struttura della sezione relativa alla configurazione della sicurezza nel profilo di distribuzione è cambiata leggermente e tutte le impostazioni di Active Directory correlate si trovano nella nuova sezione `activeDirectory` nell'albero json in `security` nel file `control.json`.

>[!NOTE]
> Oltre a fornire valori diversi per il sottodominio come descritto in questa sezione, è necessario usare anche numeri di porta diversi per gli endpoint BDC quando si distribuiscono più BDC nello stesso cluster Kubernetes. Questi numeri di porta sono configurabili in fase di distribuzione tramite i profili di [configurazione della distribuzione](deployment-custom-configuration.md).

L'esempio seguente si basa sull'uso di SQL Server 2019 CU2. Illustra come sostituire i valori di parametri correlati ad Active Directory nella configurazione della distribuzione. I dettagli del dominio riportati di seguito sono valori di esempio.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Facoltativamente, solo a partire da SQL Server 2019 CU5, è possibile sostituire i valori predefiniti per le impostazioni `subdomain` e `accountPrefix`.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.subdomain=[\"bdctest\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.accountPrefix=[\"bdctest\"]"
```

Analogamente, nelle versioni precedenti a SQL Server 2019 CU2, è possibile eseguire:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Oltre alle informazioni precedenti, è necessario fornire anche i nomi DNS per i diversi endpoint del cluster. In fase di distribuzione verranno automaticamente create le voci DNS che usano i nomi DNS specificati nel server DNS. Questi nomi verranno usati durante la connessione ai diversi endpoint del cluster. Ad esempio, se il nome DNS per l'istanza master SQL è `mastersql` e considerando che il sottodominio usi il valore predefinito del nome del cluster in `control.json`, sarà possibile usare `mastersql.contoso.local,31433` o `mastersql.mssql-cluster.contoso.local,31433` (a seconda dei valori specificati nei file di configurazione della distribuzione per i nomi DNS degli endpoint) per connettersi all'istanza master dagli strumenti. 

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[1].dnsName=<monitoring services DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=<SQL Master Primary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=<SQL Master Secondary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=<Gateway (Knox) DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=<app proxy DNS name>.<Domain name. e.g. contoso.local>"
```

> [!IMPORTANT]
> È possibile usare nomi DNS degli endpoint personalizzati purché siano completi e non siano in conflitto tra due cluster Big Data distribuiti nello stesso dominio. Facoltativamente, è possibile usare il valore del parametro `subdomain` per assicurarsi che i nomi DNS siano diversi tra i cluster. Ad esempio:

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.<subdomain e.g. mssql-cluster>.contoso.local"
```

Qui è possibile trovare uno script di esempio per la [distribuzione di un cluster Big Data di SQL Server in un cluster Kubernetes a nodo singolo (kubeadm) con l'integrazione di AD](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm-ad).

> [!Note]
> Il nuovo parametro `subdomain` introdotto con l'aggiornamento potrebbe non essere utilizzabile in alcuni scenari, ad esempio quando è necessario distribuire una versione precedente alla CU5 ed è già stato eseguito l'aggiornamento di **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** . Questo scenario è altamente improbabile, ma se è necessario ripristinare il comportamento precedente alla versione CU5, è possibile impostare il parametro `useSubdomain` su `false` nella sezione relativa ad Active Directory di `control.json`.  Il comando da usare è il seguente:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false"
```

A questo punto, sono stati impostati tutti i parametri necessari per una distribuzione del cluster Big Data con l'integrazione di Active Directory.

È ora possibile distribuire il cluster BDC integrato con Active Directory usando il comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] e il profilo di distribuzione kubeadm-prod. Per la documentazione completa su come eseguire [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] la distribuzione, vedere la pagina relativa alla [modalità di distribuzione SQL Server cluster di Big data in Kubernetes](deployment-guidance.md).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Verificare la voce DNS inversa per il controller di dominio

Assicurarsi che sia presente una voce DNS inversa (record PTR) per il controller di dominio stesso, registrata nel server DNS. A questo scopo, è possibile eseguire `nslookup` del nome di dominio nel controller di dominio per verificare che possa essere risolto nell'indirizzo IP del controller di dominio.

## <a name="known-issues-and-limitations"></a>Problemi noti e limitazioni

**Limitazioni da tenere presenti in SQL Server 2019 CU5**

- Attualmente, il dashboard Ricerca log e il dashboard Metriche non supportano l'autenticazione di Active Directory. Per l'autenticazione a questi dashboard è possibile usare il nome utente e la password di base impostati in fase di distribuzione. Tutti gli altri endpoint del cluster supportano l'autenticazione di Active Directory.

- La modalità Active Directory sicura funziona attualmente solo negli ambienti di distribuzione `kubeadm` e `openshift` e non nel servizio Azure Kubernetes o in Azure Red Hat OpenShift. I profili di distribuzione `kubeadm-prod` e `openshift-prod` includono le sezioni di sicurezza per impostazione predefinita.

- Prima di SQL Server 2019 CU5, era consentito un solo BDC per dominio (Active Directory). L'abilitazione di più BDC per dominio è una funzionalità disponibile a partire dalla versione CU5.

- Nessuno dei gruppi AD specificati nelle configurazioni di sicurezza può avere l'ambito DomainLocal. È possibile controllare l'ambito di un gruppo di Active Directory seguendo [queste istruzioni](/powershell/module/addsadministration/get-adgroup).

- L'account AD che può essere usato per accedere al BDC è consentito dallo stesso dominio configurato per il BDC. L'abilitazione di account di accesso da altri domini attendibili non è supportata.

## <a name="next-steps"></a>Passaggi successivi

[Connettersi a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: modalità Active Directory](active-directory-connect.md)

[Risolvere i problemi di integrazione di Active Directory di cluster Big Data di SQL Server](troubleshoot-active-directory.md)

[Concetto: distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deployment-background.md)
