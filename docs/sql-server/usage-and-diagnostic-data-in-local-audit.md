---
title: Utilizzo del controllo locale e raccolta di dati di diagnostica
description: Informazioni sul controllo locale usato da SQL Server per raccogliere e inviare dati di utilizzo e di diagnostica a Microsoft.
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f927e003673cb4397250fe532d57452ddb4e6445
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474562"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>Controllo locale per la raccolta di dati di diagnostica e utilizzo di SQL Server (Analisi utilizzo software)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>Introduzione

Microsoft SQL Server include funzionalità che supportano Internet e sono in grado di raccogliere e inviare a Microsoft dati relativi al computer o al dispositivo. Queste informazioni sono denominate *informazioni standard del computer*. Il componente per il controllo locale della [raccolta di dati di diagnostica e utilizzo di SQL Server](usage-and-diagnostic-data-configuration-for-sql-server.md) scrive i dati raccolti dal servizio in una specifica cartella, che rappresenta i dati (log) da inviare a Microsoft. Lo scopo del controllo locale è quello di consentire ai clienti di visualizzare tutti i dati che Microsoft raccoglie con questa funzionalità, per motivi di conformità alle normative o rispetto della privacy.  

Per SQL Server 2016 CU2 e CU3 il controllo locale è configurabile a livello di istanza per il motore di database di SQL Server e SQL Server Analysis Services (SSAS). Per SQL Server 2016 CU4, SQL Server 2016 SP1 e versioni successive il controllo locale è abilitato anche per SQL Server Integration Services (SSIS). Per altri componenti di SQL Server installati durante la fase di installazione del programma e per strumenti di SQL Server scaricati o installati successivamente, la funzionalità di controllo locale per la raccolta dei dati di diagnostica e utilizzo non è disponibile.

## <a name="remarks"></a>Osservazioni

 - La rimozione o la disabilitazione del servizio Analisi utilizzo software di SQL non è supportata. 
 - La rimozione di risorse di Analisi utilizzo software di SQL dal gruppo di cluster non è supportata. 

Per rifiutare esplicitamente la raccolta di dati, vedere [Attivazione o disattivazione del controllo locale](#turning-local-audit-on-or-off)

## <a name="prerequisites"></a>Prerequisiti 

Per abilitare il controllo locale in ogni istanza di SQL Server sono previsti i prerequisiti seguenti: 

1. L'istanza deve essere stata aggiornata a SQL Server 2016 RTM CU2 o versione successiva. Per Integration Services, l'istanza deve essere stata aggiornata a SQL 2016 RTM CU4, SQL 2016 SP1 o versioni successive.

1. L'utente deve essere un amministratore di sistema o un ruolo con diritti di accesso per aggiungere e modificare la chiave del Registro di sistema, creare cartelle, gestire la sicurezza delle cartelle e arrestare o avviare un servizio di Windows.  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>Passaggi di configurazione preliminari all'attivazione del controllo locale

Prima di attivare il controllo locale, un amministratore di sistema deve:

1. Conoscere il nome dell'istanza di SQL Server e l'account di accesso al servizio Analisi utilizzo software di SQL Server. 

1. Configurare una nuova cartella per i file del controllo locale.

1. Concedere autorizzazioni all'account di accesso al servizio Analisi utilizzo software di SQL Server.

1. Creare un'impostazione della chiave del Registro di sistema per configurare la directory di destinazione della funzionalità di controllo locale. 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>Ottenere l'account di accesso al servizio Analisi utilizzo software di SQL Server 

Per ottenere l'account di accesso al servizio Analisi utilizzo software di SQL Server, seguire questa procedura
 
1. Aprire la console **Servizi**. A tale scopo, premere **Tasto WINDOWS+R** sulla tastiera per aprire la finestra di dialogo **Esegui**. Digitare *services.msc* nel campo di testo e scegliere **OK** per avviare la console **Servizi**.  

2. Passare al servizio appropriato. Ad esempio, per il motore di database trovare **Servizio Analisi utilizzo software di SQL Server**  **(*nome-istanza*)** . Per Analysis Services trovare **Analisi utilizzo software di SQL Server Analysis Services**  **(*nome-istanza*)** . Per Integration Services trovare **Analisi utilizzo software di SQL Server Integration Services**.

3. Fare clic con il pulsante destro del mouse sul servizio e scegliere **Proprietà**. 

4. Fare clic sulla scheda **Accesso**. L'account di accesso è elencato in **Il seguente account**. 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>Configurare una nuova cartella per i file del controllo locale.    

Creare una nuova cartella (directory del controllo locale) in cui scrivere i log della funzionalità di controllo locale. Ad esempio, il percorso completo della directory del controllo locale per un'istanza predefinita del motore di database sarà: *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >Configurare il percorso di directory per il controllo locale all'esterno del percorso di installazione di SQL Server, per evitare che la funzionalità di controllo e l'applicazione di patch possano causare problemi a SQL Server.

|Decisione di progettazione|Recommendation|  
|-----------------|----------|  
|Pianificare la disponibilità di spazio |Per un carico di lavoro moderato con circa 10 database, prevedere circa 2 MB di spazio su disco per ogni database e ogni istanza.|  
|Definire directory separate | Creare una directory per ogni istanza. Ad esempio, usare *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* per un'istanza di SQL Server denominata `MSSQLSERVER`. Ciò semplifica la gestione dei file.
|Definire cartelle separate |Usare una cartella specifica per ogni servizio. Ad esempio, per un determinato nome di istanza, specificare una singola cartella per il motore di database. Se un'istanza di Analysis Services usa lo stesso nome di istanza, creare una cartella separata per Analysis Services. Se per entrambe le istanze del motore di database e di Analysis Services è configurata la stessa cartella, tutti i dati del controllo locale provenienti da tali istanze verranno scritti nello stesso file di log.| 
|Concedere autorizzazioni all'account di accesso al servizio Analisi utilizzo software di SQL Server|Abilitare i diritti **Visualizzazione contenuto cartella**, **Lettura** e **Scrittura** per l'account di accesso del servizio Analisi utilizzo software di SQL Server|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>Concedere autorizzazioni all'account di accesso al servizio Analisi utilizzo software di SQL Server
  
1. In **Esplora file** individuare la posizione della nuova cartella.

1. Fare clic con il pulsante destro del mouse sulla nuova cartella e scegliere **Proprietà**. 

1. Nella scheda **Sicurezza** selezionare l'autorizzazione di gestione **Modifica**.

1. Selezionare **Aggiungi** e digitare le credenziali del servizio Analisi utilizzo software di SQL Server. Ad esempio `NT Service\SQLTELEMETRY`.

1. Selezionare **Controlla nomi** per convalidare il nome specificato e quindi selezionare **OK**.

1. Nella finestra di dialogo **Autorizzazione** scegliere l'account di accesso al servizio Analisi utilizzo software di SQL Server e selezionare **Visualizzazione contenuto cartella**, **Lettura** e **Scrittura**.

1. Selezionare **OK** per applicare immediatamente le modifiche alle autorizzazioni. 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>Creare un'impostazione della chiave del Registro di sistema per configurare la directory di destinazione della funzionalità di controllo locale

1. Avviare regedit.

1. Passare al percorso di CPE appropriato:

   | Versione | **_Motore di database_* _ - Chiave del Registro di sistema |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL_ *13**.* Nome-istanza*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **14**.*Nome-istanza*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **15**.*Nome-istanza*\\CPE |
   | &nbsp; | &nbsp; |

   | Versione | ***Analysis Services** _ - Chiave del Registro di sistema |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS_ *13**.* Nome-istanza*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **14**.*Nome-istanza*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **15**.*Nome-istanza*\\CPE |  
   | &nbsp; | &nbsp; |

   | Versione | ***Integration Services** _ - Chiave del Registro di sistema |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\_ *130** |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**140** |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**150** |
   | &nbsp; | &nbsp; |

1. Fare clic con il pulsante destro del mouse sul percorso di CPE e scegliere **Nuovo**. Selezionare **Valore stringa**.

1. Denominare la nuova chiave del Registro di sistema `UserRequestedLocalAuditDirectory`. 
 
## <a name="turning-local-audit-on-or-off"></a>Attivazione o disattivazione del controllo locale

Dopo aver completato i passaggi di configurazione preliminari, è possibile attivare il controllo locale. A tale scopo, usare un account di amministratore di sistema, o un ruolo simile con accesso alla modifica delle chiavi del Registro di sistema, per attivare o disattivare il controllo locale seguendo questa procedura. 

1. Avviare **regedit**.  

1. Passare al [percorso](#create-a-registry-key-setting-to-configure-local-audit-target-directory) di CPE appropriato. 

1. Fare clic con il pulsante destro del mouse su **UserRequestedLocalAuditDirectory** e selezionare *Modifica*. 

1. Per attivare il controllo locale, digitare il relativo percorso, ad esempio *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* .
 
    Per disattivare il controllo locale, svuotare il valore in **UserRequestedLocalAuditDirectory**.

1. Chiudere **regedit**. 

Analisi utilizzo software di SQL Server dovrebbe riconoscere immediatamente l'impostazione del controllo locale se il servizio è già in esecuzione. Per avviare il servizio Analisi utilizzo software di SQL Server, un amministratore di sistema o un utente con diritti di accesso per avviare o arrestare Servizi di Windows può seguire questa procedura: 

1. Aprire la console **Servizi**. A tale scopo, premere **Tasto WINDOWS+R** sulla tastiera per aprire la finestra di dialogo **Esegui**. Digitare *services.msc* nel campo di testo e scegliere **OK** per avviare la console **Servizi**.  

1. Passare al servizio appropriato. 

    - Per il motore di database usare **Servizio Analisi utilizzo software di SQL Server (*Nome-istanza*)**.     
    - Per Analysis Services usare **Analisi utilizzo software di SQL Server Analysis Services (*Nome-istanza*)**.
    - Per Integration Services, 
        - Per SQL 2016 usare *Servizio Analisi utilizzo software di SQL Server Integration Services 13.0*.
        - Per SQL 2017 usare *Servizio Analisi utilizzo software di SQL Server Integration Services 14.0*.
    - Per SQL 2019 usare *Servizio Analisi utilizzo software di SQL Server Integration Services 15.0*.

1. Fare clic con il pulsante destro del mouse sul servizio e scegliere Riavvia. 

1. Verificare che lo stato del servizio sia **In esecuzione**. 

La funzionalità di controllo locale genererà un file di log al giorno. Ai file di log viene assegnato un nome nel formato `<YYYY-MM-DD>.json`. Ad esempio, *2016-07-12.json*. Se nella directory designata esiste già un file per il giorno corrente, i dati verranno aggiunti a tale file. In caso contrario, ne verrà creato uno nuovo. 

  >[!NOTE]
  > Dopo l'abilitazione del controllo locale, possono trascorrere fino a 5 minuti prima che il file di log venga scritto per la prima volta. 

## <a name="maintenance"></a>Manutenzione 

1. Per limitare l'utilizzo di spazio su disco per i file scritti dalla funzionalità di controllo locale, configurare criteri o un processo regolare per rimuovere dalla directory del controllo locale i file meno recenti e non necessari.  

2. Proteggere il percorso della directory del controllo locale in modo che sia accessibile solo alle persone appropriate. Si noti che i file di log contengono informazioni come descritto in [Come configurare SQL Server 2016 per inviare commenti e suggerimenti a Microsoft](https://support.microsoft.com/kb/3153756). È quindi opportuno impostare diritti di accesso a questi file in modo da impedirne la lettura alla maggior parte dei membri dell'organizzazione.  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>Dizionario dei dati della struttura di dati di output del controllo locale 

- I file di log del controllo locale sono in formato JSON, contenente un set di oggetti (righe) che rappresentano i punti dati inviati a Microsoft nella data e all'ora definite da **emitTime**.
- Ogni riga segue uno schema specifico identificato da **schemaVersion**.
- Ogni riga corrisponde a un output di una sessione del servizio SQLCEIP identificata come **sessionID**.
- Le righe vengono inviate in base a una sequenza identificata da **sequence**.
- Ogni riga di punto dati contiene l'output di un elemento **queryIdentifier** che può essere una query T-SQL, una sessione XE o un messaggio associato a un tipo di traccia, identificata come **traceName**.
- Gli elementi **queryIdentifier** vengono riuniti in gruppi a cui viene assegnata una versione con **querySetVersion**.
- L'elemento **data** contiene l'output dell'esecuzione query corrispondente, la cui durata è definita da **queryTimeInTicks**.
- Gli elementi **queryIdentifier** per le query T-SQL contengono la definizione di query T-SQL archiviata nella query.

| Gerarchia logica delle informazioni del controllo locale | Colonne correlate |
| ------ | -------|
| Intestazione | emitTime, schemaVersion 
| Computer | operatingSystem 
| Istanza | instanceUniqueID, correlationID, clientVersion 
| sessione | sessionID, traceName 
| Query | sequence, querySetVersion, queryIdentifier, query, queryTimeInTicks 
| Data |  data 

### <a name="namevalue-pairs-definition-and-examples"></a>Definizione di coppie nome/valore ed esempi 

Le colonne elencate di seguito rappresentano l'ordinamento dell'output dei file del controllo locale. Per rendere anonimi i valori per alcune delle colonne seguenti viene usato un hash unidirezionale con SHA 256.  

| Nome | Descrizione | Valori di esempio
|-------|--------| ----------|
|instanceUniqueID| Identificatore dell'istanza reso anonimo | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| Versione dello schema di SQLCEIP |  3 
|emitTime |Data e ora di invio dei punti dati in UTC (Universal Time Coordinated) | 2016-09-08T17:20:22.1124269Z 
|sessionID | Identificatore di sessione del servizio SQLCEIP | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | Segnaposto per un identificatore aggiuntivo | 0 
|sequence | Numero di sequenza dei punti dati inviati nel corso della sessione | 15 
|clientVersion | Versione dell'istanza di SQL Server | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | Versione del sistema operativo in cui è installata l'istanza di SQL Server | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | Versione di un gruppo di definizioni di query | 1.0.0.0 
|traceName | Categorie di tracce: (SQLServerXeQueries, SQLServerPeriodicQueries, SQLServerOneSettingsException) | SQLServerPeriodicQueries 
|queryIdentifier | Identificatore della query | SQLServerProperties.002 
|data   | Output delle informazioni raccolte su queryIdentifier come output della query T-SQL, della sessione XE o dell'applicazione |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tStandard Edition (64-bit) on Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|query| Se applicabile, definizione di query T-SQL correlata all'elemento queryIdentifier che genera i dati.        Questo componente non viene caricato dal servizio Analisi utilizzo software di SQL Server. È incluso nel controllo locale solo come riferimento per i clienti.| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027PolyBaseRole\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | Tempo impiegato per l'esecuzione della query con la categoria di traccia seguente: (SQLServerXeQueries, SQLServerPeriodicQueries) |  0 
 
### <a name="trace-categories"></a>Categorie di traccia 
Attualmente i dati vengono raccolti in base alle categorie di traccia seguenti: 

- **SQLServerXeQueries**: contiene i punti dati raccolti tramite la sessione di eventi estesi.
- **SQLServerPeriodicQueries**: contiene i punti dati raccolti tramite query periodiche eseguite in un'istanza di SQL Server.
- **SQLServerPerDBPeriodicQueries**: contiene i punti dati raccolti tramite query periodiche eseguite su un massimo di 30 database in un'istanza di SQL Server.
- **SQLServerOneSettingsException**: contiene i messaggi di eccezione correlati all'aggiornamento dello schema e/o al set di query.
- **DigitalProductID**: contiene i punti dati per l'aggregazione degli ID di prodotto digitale (resi anonimi) con hash (SHA-256) delle istanze di SQL Server. 

### <a name="local-audit-file-examples"></a>Esempi di file del controllo locale



Di seguito è riportato un estratto di un output di file JSON della funzionalità di controllo locale.

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>Domande frequenti

**Come fanno gli amministratori di database a leggere i file di log del controllo locale?**
Questi file di log vengono scritti in formato JSON. Ogni riga corrisponde a un oggetto JSON che rappresenta un elemento di dei dati di diagnostica e utilizzo inviati a Microsoft. I nomi dei campi devono essere facilmente comprensibili.

**Cosa accade se l'amministratore di database disabilita la raccolta dei dati di diagnostica e utilizzo?**
Non verrà scritto alcun file del controllo locale.

**Cosa accade se la connettività Internet non è disponibile o il computer si trova all'interno del firewall?**
I dati di diagnostica e utilizzo di SQL Server non verranno inviati a Microsoft. Se configurato correttamente, il computer proverà comunque a scrivere i log del controllo locale.

**Come fanno gli amministratori di database a disabilitare il controllo locale?**
Per eseguire questa operazione, rimuovere la voce della chiave del Registro di sistema UserRequestedLocalAuditDirectory.

**Chi può leggere i file di log del controllo locale?**
Tutti gli utenti dell'organizzazione con accesso alla directory del controllo locale.

**Come fanno gli amministratori di database a gestire i file di log scritti nella directory designata?**
Gli amministratori di database devono gestire autonomamente la pulizia dei file nella directory per evitare un utilizzo eccessivo dello spazio su disco.

**Quale client o strumento è possibile usare per leggere l'output JSON?**
L'output può essere letto con il Blocco note, in Visual Studio o nel lettore JSON desiderato.
In alternativa, è possibile leggere il file JSON e analizzare i dati in un'istanza di SQL Server, come illustrato di seguito. Per altre informazioni su come leggere il file JSON in SQL Server, visitare [Importing JSON files into SQL Server using OPENROWSET (BULK) and OPENJSON (Transact-SQL)](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server)(Importazione di file JSON in SQL Server mediante OPENROWSET (BULK) e OPENJSON (Transact-SQL)).

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>Vedere anche
[Controllo locale per la raccolta di dati di diagnostica e utilizzo di SSMS](../ssms/sql-server-management-studio-telemetry-ssms.md)