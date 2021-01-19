---
title: Always Encrypted | Microsoft Docs
description: Panoramica di Always Encrypted, che supporta la crittografia lato client trasparente e il confidential computing in SQL Server e nel database SQL di Azure
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], Always Encrypted
- Always Encrypted
- TCE Always Encrypted
- Always Encrypted, about
- SQL13.SWB.COLUMNMASTERKEY.CLEANUP.F1
ms.assetid: 54757c91-615b-468f-814b-87e5376a960f
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 534d7238316fe2037ea0ce43e2b4aeeb11e6eea2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171333"
---
# <a name="always-encrypted"></a>Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  ![Always Encrypted](../../../relational-databases/security/encryption/media/always-encrypted.png "Always Encrypted")  
  
 Always Encrypted è una funzionalità progettata per proteggere dati sensibili, ad esempio numeri di carta di credito, codici fiscali, passaporti, ecc. archiviati in database [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Always Encrypted consente ai client di crittografare dati sensibili all'interno di applicazioni client senza mai rivelare le chiavi di crittografia a [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). Di conseguenza, Always Encrypted crea una separazione tra chi possiede i dati e può visualizzarli e chi gestisce i dati, ma non può accedervi. Garantendo l'impossibilità di accesso ai dati crittografati da parte di amministratori di database locali, operatori di database cloud o altri utenti con privilegi elevati ma non autorizzati, Always Encrypted consente ai clienti di archiviare in modo sicuro i dati sensibili su cui non esercitano un controllo diretto. In questo modo le organizzazioni possono archiviare i dati in Azure e delegare l'amministrazione di database locali a terze parti o ridurre i requisiti di nulla osta di sicurezza per il proprio personale DBA.

 Always Encrypted fornisce funzionalità di confidential computing consentendo al [!INCLUDE[ssDE](../../../includes/ssde-md.md)] di elaborare alcune query sui dati crittografati, mantenendo al tempo stesso la riservatezza dei dati e fornendo i vantaggi a livello di sicurezza illustrati in precedenza. In [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], [!INCLUDE[sssSQLv14](../../../includes/sssqlv14-md.md)] e in [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], Always Encrypted supporta il confronto di uguaglianza tramite la crittografia deterministica. Vedere [Selezione della crittografia deterministica o casuale](#selecting--deterministic-or-randomized-encryption). 

  > [!NOTE] 
  > In [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], gli enclavi sicuri estendono sensibilmente le funzionalità di confidential computing di Always Encrypted con criteri di ricerca, altri operatori di confronto e crittografia sul posto. Vedere [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md).

 Crittografia sempre attiva esegue la crittografia trasparente alle applicazioni. A questo scopo, un driver abilitato per Crittografia sempre attiva installato nel computer client esegue automaticamente la crittografia e la decrittografia dei dati sensibili nell'applicazione client. Il driver esegue la crittografia dei dati in colonne sensibili prima di passarli a [!INCLUDE[ssDE](../../../includes/ssde-md.md)]e riscrive automaticamente le query in modo da mantenere la semantica per l'applicazione. Analogamente, il driver decrittografa in modo trasparente i dati, archiviati nelle colonne di database crittografate, contenuti nei risultati delle query.  
  
 Always Encrypted è disponibile in tutte le edizioni di [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], a partire da [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], e in tutti i livelli di servizio di [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. Nelle versioni precedenti a [!INCLUDE[ssSQL15_md](../../../includes/sssql16-md.md)] SP1, Always Encrypted è limitato all'edizione Enterprise. Per una presentazione di Channel 9 che include Crittografia sempre attiva, vedere il video relativo al [mantenimento della protezione dei dati sensibili con Crittografia sempre attiva](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted).  

  
## <a name="typical-scenarios"></a>Scenari tipici  
  
### <a name="client-and-data-on-premises"></a>Client e dati in locale  
 Un cliente ha un'applicazione client e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , entrambi in esecuzione in locale presso la propria sede aziendale. Il cliente vuole affidare a un fornitore esterno la gestione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per proteggere i dati sensibili archiviati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il cliente usa Crittografia sempre attiva per garantire la separazione dei compiti tra amministratori di database e amministratori di applicazioni. Il cliente archivia i valori di testo non crittografato delle chiavi di Always Encrypted in un archivio di chiavi attendibili al quale l'applicazione client può accedere. Gli amministratori di[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non hanno accesso alle chiavi e, quindi, non possono decrittografare i dati sensibili archiviati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
### <a name="client-on-premises-with-data-in-azure"></a>Client locale con dati in Azure  
 Un cliente ha un'applicazione client locale presso la propria sede aziendale. L'applicazione usa dati sensibili archiviati in un database ospitato in Azure ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in esecuzione in una macchina virtuale in Microsoft Azure). Il cliente usa Always Encrypted e archivia le relative chiavi in un archivio attendibile ospitato localmente, per assicurarsi che gli amministratori cloud di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] non abbiano accesso ai dati sensibili.  
  
### <a name="client-and-data-in-azure"></a>Client e dati in Azure  
 Un cliente ha un'applicazione client, ospitata in Microsoft Azure, ad esempio in un ruolo di lavoro o in un ruolo Web, che usa dati sensibili anch'essi archiviati in Microsoft Azure, in un database SQL o SQL Server in esecuzione in una macchina virtuale di Microsoft Azure. Sebbene Always Encrypted non assicuri isolamento completo dei dati dagli amministratori di cloud, perché sia i dati sia le chiavi sono esposti agli amministratori di cloud della piattaforma che ospita il livello client, il cliente tuttavia può beneficiare della riduzione della superficie di attacco della sicurezza (i dati nel database sono sempre crittografati).  
 
## <a name="how-it-works"></a>Funzionamento

È possibile configurare Always Encrypted per le singole colonne di un database che contengono dati sensibili. Quando si imposta la crittografia per una colonna, si specificano le informazioni sulle chiavi crittografiche e sugli algoritmi di crittografia usati per proteggere i dati nella colonna. Always Encrypted usa due tipi di chiavi: chiavi di crittografia della colonna e chiavi master della colonna. Una chiave di crittografia della colonna viene usata per crittografare i dati in una colonna crittografata. Una chiave master della colonna è una chiave di protezione a livello chiave che crittografa una o più chiavi di crittografia della colonna. 

Il motore di database memorizza la configurazione di crittografia per ogni colonna nei metadati di database. Si noti tuttavia che il motore di database non archivia e non usa le chiavi di nessun tipo in testo non crittografato. Archivia solo i valori crittografati delle chiavi di crittografia della colonna e le informazioni relative al percorso di chiavi master della colonna, che vengono archiviati in attendibili archivi chiavi esterni, ad esempio l'insieme di credenziali delle chiave di Azure, archivio di certificati di Windows su un computer client o su un modulo di protezione hardware.

Per accedere ai dati archiviati in una colonna crittografata in testo non crittografato, un'applicazione deve usare un driver client abilitato a Always Encrypted. Quando un'applicazione esegue una query con parametri, il driver collabora in modo trasparente con il motore di database per determinare quali parametri interessano le colonne crittografate e per questo motivo devono essere crittografati. Per ogni parametro che deve essere crittografato, il driver ottiene le informazioni sull'algoritmo di crittografia e sul valore crittografato della chiave di crittografia della colonna, le destinazioni del parametro e il percorso della chiave master della colonna corrispondente.

Successivamente, il driver contatta l'archivio chiavi, che contiene la chiave master della colonna, per decrittografare il valore della chiave di crittografia della colonna crittografata e usa quindi la chiave di crittografia in testo non crittografato per crittografare il parametro. La risultante chiave di crittografia della colonna in testo non crittografato è memorizzata nella cache per ridurre il numero di round trip all'archivio chiavi nei successivi usi della stessa chiave di crittografia della colonna. Il driver sostituisce i valori di testo non crittografato dei parametri che interessano colonne crittografate con i relativi valori crittografati e invia la query al server per l'elaborazione.

Il server elabora il set di risultati e per ogni colonna crittografata incluso nel set dei risultati, il driver allega i metadati di crittografia per la colonna, includendo le informazioni sull'algoritmo di crittografia e sulla chiavi corrispondenti. Il driver tenta innanzitutto di trovare la chiave di crittografia della colonna in testo non crittografato nella cache locale ed esegue solo un round alla chiave master della colonna, se non può trovarla nella cache. Successivamente, il driver decrittografa i risultati e restituisce valori in testo non crittografato per l'applicazione.

 Il driver del client interagisce con un archivio chiavi, che contiene la chiave master di colonna, tramite il provider di archivio chiavi della colonna, il quale è un componente software sul lato client che incapsula un archivio chiavi contenente la chiave master della colonna. I provider dei tipi più comuni di archivi chiavi sono disponibili nelle librerie di driver lato client di Microsoft o come download autonomi. È anche possibile implementare un provider personalizzato. Le funzionalità Always Encrypted, compresi i provider di archivio chiavi master della colonna predefiniti, variano in base a una libreria di driver e alla relativa versione. 

Per informazioni dettagliate su come sviluppare applicazioni che usano Always Encrypted con driver client particolari, vedere [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md).

## <a name="remarks"></a>Osservazioni

La crittografia e la decrittografia avvengono tramite il driver client. Questo significa che alcune azioni che si verificano solo sul lato server non funzionano quando si usa Always Encrypted. Ad esempio, la copia di dati da una colonna a un'altra tramite UPDATE, BULK INSERT(T-SQL), SELECT INTO, INSERT..SELECT. 

Di seguito è riportato un esempio di operazione UPDATE che tenta di spostare dati da una colonna crittografata in una colonna non crittografata senza restituire un set di risultati al client: 

```sql
update dbo.Patients set testssn = SSN
```

Se SSN è una colonna crittografata con Always Encrypted, l'istruzione di aggiornamento precedente avrà esito negativo con un errore simile al seguente:

```
Msg 206, Level 16, State 2, Line 89
Operand type clash: char(11) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_1', column_encryption_key_database_name = 'ssn') collation_name = 'Latin1_General_BIN2' is incompatible with char
```

Per aggiornare correttamente la colonna, procedere come segue:

1. Eseguire SELECT sui dati della colonna SSN e archiviarli come set di risultati nell'applicazione. Ciò consentirà all'applicazione (*driver* client) di decrittografare la colonna.
2. Eseguire INSERT per i dati dal set di risultati in SQL Server. 

 >[!IMPORTANT]
 > In questo scenario, i dati verranno decrittografati quando vengono inviati di nuovo al server perché la colonna di destinazione è di tipo varchar regolare e non accetta dati crittografati. 
  
## <a name="selecting-deterministic-or-randomized-encryption"></a><a name="selecting--deterministic-or-randomized-encryption"></a> Selezione della crittografia deterministica o casuale  
 Il motore di database non agisce mai sui dati in testo non crittografato archiviati in colonne crittografate, ma supporta alcune query sui dati crittografati, a seconda del tipo di crittografia per la colonna. Crittografia sempre attiva supporta due tipi di crittografia: crittografia casuale e crittografia deterministica.  
  
- La crittografia deterministica genera sempre lo stesso valore crittografato per ogni valore specifico di testo non crittografato. L'uso della crittografia deterministica consente le ricerche di punti, join di uguaglianza, raggruppamento e indicizzazione di colonne crittografate. Tuttavia, può anche consentire a utenti non autorizzati di indovinare le informazioni sui valori crittografati esaminando gli schemi presenti nella colonna crittografata, soprattutto in presenza di un set di possibili valori crittografati di dimensioni ridotte, ad esempio True/False o Nord/Sud/Est/Ovest. La crittografia deterministica deve usare regole di confronto a livello di colonna con un ordinamento binario2 per colonne di tipo carattere.

- La crittografia casuale usa un metodo di crittografia dei dati meno prevedibile. La crittografia casuale è più sicura, ma impedisce le ricerche, il raggruppamento, l'indicizzazione e il join su colonne crittografate.

La crittografia deterministica è consigliata per le colonne che verranno usate come parametri di ricerca o di raggruppamento. Ad esempio un numero identificativo rilasciato da un ente pubblico. Usare la crittografia casuale per dati quali i commenti di analisi riservate, che non sono raggruppati con altri record e non vengono usati per il join di tabelle.
Per informazioni dettagliate sugli algoritmi di crittografia di Always Encrypted, vedere [Crittografia Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md).

## <a name="configuring-always-encrypted"></a>Configurazione di Always Encrypted

 L'installazione iniziale di Always Encrypted in un database comporta la generazione di chiavi di Always Encrypted, la creazione di metadati della chiave, la configurazione delle proprietà di crittografia di colonne di database selezionate e/o la crittografia di dati che potrebbero già esistere in colonne da crittografare. Si noti che alcune di queste attività non sono supportate in Transact-SQL e richiedono l'uso degli strumenti sul lato client. Poiché le chiavi di Always Encrypted e i dati sensibili e protetti non sono mai rivelati al server in testo non crittografato, il motore di database non può essere coinvolto nel provisioning della chiave e non può eseguire operazioni di crittografia o decrittografia dei dati. Per eseguire tali attività è possibile usare SQL Server Management Studio o PowerShell. 

|Attività|SSMS|PowerShell|T-SQL|
|:---|:---|:---|:---
|Provisioning di chiavi master della colonna, chiavi di crittografia di colonne e chiavi di crittografia della colonna crittografata con le chiavi master della colonna corrispondenti.|Sì|Sì|No|
|Creazione di metadati della chiave nel database.|Sì|Sì|Sì|
|Creazione di nuove tabelle con colonne crittografate|Sì|Sì|Sì|
|Crittografia dei dati esistenti nelle colonne selezionate del database|Sì|Sì|No|

> [!NOTE]
> [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md), introdotto in [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], supporta la crittografia dei dati esistenti con Transact-SQL. Elimina inoltre la necessità di spostare i dati all'esterno del database per le operazioni di crittografia.

> [!NOTE]
> Accertarsi di eseguire il provisioning delle chiavi o gli strumenti di crittografia dei dati in un ambiente protetto o in un computer diverso da quello che ospita il database. Altrimenti potrebbero verificarsi delle perdite di chiavi o dati sensibili verso l'ambiente del server, e questo ridurrebbe i benefici dell'uso di Always Encrypted.  

Per informazioni dettagliate sulla configurazione di Always Encrypted, vedere:
- [Configurare Always Encrypted tramite SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

## <a name="getting-started-with-always-encrypted"></a>Introduzione a Crittografia sempre attiva

Per iniziare a usare rapidamente questa funzionalità, eseguire la [procedura guidata Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) . La procedura guidata eseguirà il provisioning di chiavi necessarie e configurerà la crittografia per le colonne selezionate. Se le colonne, per le quali si sta impostando la crittografia, contengono già alcuni dati, la procedura guidata crittograferà i dati. L'esempio seguente descrive il processo per la crittografia di una colonna.

> [!NOTE]  
>  Per altre informazioni sull'uso della procedura guidata, vedere il video in [Getting Started with Always Encrypted with SSMS (Introduzione a Always Encrypted con SSMS)](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-Always-Encrypted-with-SSMS).

1.  Connettersi a un database esistente che contiene tabelle con colonne che si vuole crittografare usando **Esplora oggetti** di Management Studio oppure creare un nuovo database, creare una o più tabelle con colonne da crittografare e connettersi ad esso.
2.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Crittografa colonne** per aprire la **Procedura guidata Always Encrypted**.
3.  Leggere la pagina **Introduzione** , quindi fare clic su **Avanti**.
4.  Nella pagina **Selezione colonne** espandere le tabelle e selezionare le colonne da crittografare.
5.  Per ogni colonna selezionata da crittografare, impostare **Tipo di crittografia** su *Deterministico* o *Casuale*.
6.  Per ogni colonna selezionata per la crittografia, selezionare una **Chiave di crittografia**. Se in precedenza non è stata creata una chiave di crittografia per il database, selezionare l'opzione predefinita di una nuova chiave generata automaticamente e quindi fare clic su **Avanti**.
7.  Nella pagina **Configurazione chiave master** , selezionare un percorso per archiviare la nuova chiave, selezionare un'origine della chiave master e quindi fare clic su **Avanti**.
8.  Nella pagina **Convalida** scegliere se eseguire lo script immediatamente o creare uno script di PowerShell, quindi fare clic su **Avanti**.
9.  Nella pagina **Riepilogo** esaminare le opzioni selezionate e quindi fare clic su **Fine**. Al termine, chiudere la procedura guidata.

  
## <a name="feature-details"></a>Informazioni sulle funzionalità  
  
-   Le query possono eseguire il confronto delle uguaglianze sulle colonne crittografate usando la crittografia deterministica, ma non altre operazioni, come ad esempio maggiore/minore di, criteri di ricerca usando l'operatore LIKE o operazioni aritmetiche.  
  
-   Le query sulle colonne crittografate con la crittografia casuale non possono eseguire operazioni su alcuna di tali colonne. L'indicizzazione di colonne crittografate usando la crittografia casuale non è supportata.  
 > [!NOTE] 
 > [Always Encrypted con enclavi sicuri](always-encrypted-enclaves.md), introdotto in [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], risolve la limitazione precedente abilitando l'uso di criteri di ricerca, operatori di confronto e indicizzazione nelle colonne che usano la crittografia casuale.

-   Una chiave di crittografia della colonna può avere fino a due valori crittografati differenti, ciascuno crittografato con una chiave master della colonna diversa. Questa operazione facilita la rotazione delle chiavi master della colonna.  
  
-   La crittografia deterministica richiede una colonna per disporre di uno del [*binary2* le regole di confronto](../../../relational-databases/collations/collation-and-unicode-support.md).  

-   Dopo aver modificato la definizione di un oggetto codificato, eseguire [sp_refresh_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) per aggiornare i metadati di Always Encrypted per l'oggetto.
  
Always Encrypted non è supportato per le colonne con le caratteristiche seguenti. Ad esempio, se alla colonna si applica una delle condizioni seguenti, non è possibile usare la clausola `ENCRYPTED WITH` in `CREATE TABLE/ALTER TABLE` per una colonna:  
  
-   Colonne che usano uno dei tipi di dati seguenti: `xml`, `timestamp`/`rowversion`, `image`, `ntext`, `text`, `sql_variant`, `hierarchyid`, `geography`, `geometry`, alias, tipi definiti dall'utente.  
- Colonne `FILESTREAM`  
- Colonne con la proprietà `IDENTITY`.  
- Colonne con la proprietà `ROWGUIDCOL`.  
- Colonne stringa (`varchar`, `char` e così via) con regole di confronto non BIN2.  
- Colonne che corrispondono a chiavi per indici cluster e non cluster quando si usa la crittografia casuale. La crittografia deterministica è supportata.
- Colonne incluse negli indici full-text (Always Encrypted non supporta la [ricerca full-text](../../../relational-databases/search/full-text-search.md)).  
- Colonne calcolate.
- Colonne a cui fanno riferimento colonne calcolate (quando l'espressione esegue operazioni non supportate per Always Encrypted).  
- Set di colonne di tipo sparse.  
- Colonne a cui fanno riferimento le statistiche quando si usa la crittografia casuale. La crittografia deterministica è supportata.  
- Colonne che usano i tipi di alias.  
- Colonne di partizionamento.  
- Colonne con vincoli predefiniti.  
- Colonne a cui fanno riferimento vincoli univoci quando si usa la crittografia casuale (la crittografia deterministica è supportata).  
- Colonne di chiavi primarie quando si usa la crittografia casuale (la crittografia deterministica è supportata).  
- Colonne di riferimento in vincoli di chiave esterna quando si usa la crittografia casuale o la crittografia deterministica, se le colonne a cui si fa riferimento e di riferimento usano chiavi o algoritmi diversi.  
- Colonne a cui fanno riferimento vincoli CHECK.  
- Colonne acquisite/verificate con Change Data Capture.  
- Colonne chiave primaria in tabelle con rilevamento delle modifiche.  
- Colonne che vengono mascherate (con Dynamic Data Masking).  
- Colonne in tabelle di estensione database. (le tabelle con colonne crittografate con Crittografia sempre attiva possono essere abilitate per l'estensione).  
- Colonne in tabelle esterne (PolyBase) (nota: l'uso di tabelle esterne e di tabelle con colonne crittografate nella stessa query è supportato).  
- I parametri con valori di tabella per le colonne crittografate non sono supportati.  

Le clausole seguenti non possono essere usate per le colonne crittografate:

- `FOR XML`
- `FOR JSON PATH`

Le funzionalità seguenti non sono supportate nelle colonne crittografate:

- Replica transazionale o di tipo merge
- Query distribuite (server collegati, `OPENROWSET`(T-SQL), `OPENDATASOURCE`(T-SQL))

Requisiti degli strumenti

- Per eseguire query che decrittografano i risultati recuperati da colonne crittografate oppure eseguono operazioni di inserimento, aggiornamento o filtro su colonne crittografate, è consigliabile usare SQL Server Management Studio versione 18 o successiva. 
- Richiede `sqlcmd` versione 13.1 o successiva, disponibile nell'[Area download](https://go.microsoft.com/fwlink/?LinkID=825643).

  
## <a name="database-permissions"></a>Autorizzazioni per il database  
 Esistono quattro autorizzazioni per Always Encrypted:  
  
*  `ALTER ANY COLUMN MASTER KEY` (Obbligatorio per creare ed eliminare una chiave master della colonna).  
  
*  `ALTER ANY COLUMN ENCRYPTION KEY` (Obbligatorio per creare ed eliminare una chiave di crittografia della colonna). 
  
*  `VIEW ANY COLUMN MASTER KEY DEFINITION` (Obbligatorio per accedere e leggere i metadati delle chiavi master della colonna per la gestione delle chiavi o per l'esecuzione di query su colonne crittografate).  
  
*  `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` (Obbligatorio per accedere e leggere i metadati della chiave di crittografia della colonna per la gestione delle chiavi o per l'esecuzione di query su colonne crittografate).  
  
 Nella tabella seguente sono riepilogate le autorizzazioni necessarie per le azioni comuni.  
  
|Scenario|<code>ALTER ANY COLUMN MASTER KEY</code>|<code>ALTER ANY COLUMN ENCRYPTION KEY</code>|<code>VIEW ANY COLUMN MASTER KEY DEFINITION</code>|<code>VIEW ANY COLUMN ENCRYPTION KEY DEFINITION</code>|  
|--------------|-----------------------------------|---------------------------------------|---------------------------------------------|-------------------------------------------------|  
|Gestione delle chiavi (creazione/modifica/revisione metadati delle chiavi nel database)|X|X|X|X|  
|Esecuzione di query in colonne crittografate|||X|X|  
  
 **Note importanti:**  
  
-   Le autorizzazioni si applicano alle azioni tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] (finestre di dialogo e procedura guidata) o PowerShell.  
  
-   Le due autorizzazioni *VIEW* sono necessarie quando si selezionano colonne crittografate, anche se l'utente non ha l'autorizzazione per decrittografare le colonne.  
  
-   In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], entrambi le autorizzazioni *VIEW* sono assegnate per impostazione predefinita al ruolo predefinito del database `public` . Un amministratore di database può scegliere di revocare (o negare) le autorizzazioni *VIEW* per il ruolo `public` e assegnarle a specifici ruoli o utenti per implementare un controllo con maggiori restrizioni.  
  
-   In [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], le autorizzazioni *VIEW* non sono assegnate per impostazione predefinita al ruolo predefinito del database `public`. In questo modo specifici strumenti esistenti (che usano le versioni precedenti di DacFx) funzionano correttamente. Di conseguenza, per usare le colonne crittografate (anche senza decrittografarle) un amministratore del database deve concedere esplicitamente le due autorizzazioni *VIEW* .  

  
## <a name="example"></a>Esempio  
 Il codice [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente crea metadati di una chiave master della colonna, di una chiave di crittografia della colonna e una tabella con colonne crittografate. Per informazioni sulla creazione di chiavi, a cui fanno riferimento i metadati, vedere:
- [Configurare Always Encrypted tramite SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
---------------------------------------------  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
---------------------------------------------  
CREATE TABLE Customers (  
    CustName nvarchar(60)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = RANDOMIZED,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    SSN varchar(11)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = DETERMINISTIC ,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    Age int NULL  
);  
GO  
  
```  
## <a name="see-also"></a>Vedere anche  
- [Configurare Always Encrypted tramite SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)   
- [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md) 
- [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
- [Crittografia Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)   
- [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)   
- [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
- [CREATE TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql.md)   
- [column_definition &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
- [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)  
- [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
- [sys.column_master_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
- [sys.columns &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
- [sp_refresh_parameter_encryption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)   
  
  
