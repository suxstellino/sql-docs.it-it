---
description: DBCC CHECKFILEGROUP (Transact-SQL)
title: DBCC CHECKFILEGROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKFILEGROUP_TSQL
- DBCC_CHECKFILEGROUP_TSQL
- DBCC CHECKFILEGROUP
- CHECKFILEGROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKFILEGROUP statement
- database objects [SQL Server], checking
- allocation checks
- integrity [SQL Server], database objects
- filegroups [SQL Server], consistency checks
- table integrity checks [SQL Server]
- checking database objects
ms.assetid: 8c70bf34-7570-4eb6-877a-e35064a1380a
author: pmasl
ms.author: umajay
ms.openlocfilehash: f95e730dac286be230c27a8e0fc14aa52d07cfee
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813025"
---
# <a name="dbcc-checkfilegroup-transact-sql"></a>DBCC CHECKFILEGROUP (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Controlla l'integrità strutturale e di allocazione di tutte le tabelle e le viste indicizzate nel filegroup specificato del database corrente.
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC CHECKFILEGROUP   
[  
    [ ( { filegroup_name | filegroup_id | 0 }   
        [ , NOINDEX ]   
  ) ]  
    [ WITH   
        {   
            [ ALL_ERRORMSGS | NO_INFOMSGS ]   
            [ , TABLOCK ]   
            [ , ESTIMATEONLY ]  
            [ , PHYSICAL_ONLY ]    
            [ , MAXDOP  = number_of_processors ]  
        }   
    ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *filegroup_name*  
 Nome del filegroup nel database corrente di cui si desidera controllare l'integrità strutturale e di allocazione delle tabelle. Se omesso oppure se viene specificato 0, per impostazione predefinita viene utilizzato il filegroup primario. I nomi di filegroup devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
 *filegroup_name* non può essere un filegroup FILESTREAM.  
  
 *filegroup_id*  
 Numero di identificazione (ID) del filegroup nel database corrente di cui si desidera controllare l'integrità strutturale e di allocazione delle tabelle.  
  
 NOINDEX  
 Specifica che non è necessario eseguire controlli estesi di indici non cluster per le tabelle utente. In questo modo, è possibile ridurre i tempi di esecuzione complessivi. NOINDEX non influisce sulle tabelle di sistema perché DBCC CHECKFILEGROUP controlla sempre tutti gli indici delle tabelle di sistema.  
  
 ALL_ERRORMSGS  
 Visualizza un numero illimitato di errori per oggetto. Tutti i messaggi di errore vengono visualizzati per impostazione predefinita. La specifica o l'omissione di questa opzione non ha alcun effetto.  
  
 NO_INFOMSGS  
 Disattiva tutti i messaggi informativi.  
  
 TABLOCK  
 Consente all'istruzione DBCC CHECKFILEGROUP di ottenere blocchi anziché utilizzare uno snapshot interno del database.  
  
 ESTIMATE ONLY  
 Visualizza lo spazio di tempdb stimato necessario per eseguire l'istruzione DBCC CHECKFILEGROUP con tutte le opzioni specificate.  
  
 PHYSICAL_ONLY  
 Limita il controllo di integrità alla struttura fisica della pagina, alle intestazioni dei record e alla struttura fisica degli alberi B. Progettato per consentire un controllo con overhead limitato della consistenza fisica del filegroup, questo controllo consente inoltre di rilevare le pagine incomplete e i problemi comuni a livello di hardware che possono compromettere i dati. Un'esecuzione completa di DBCC CHECKFILEGROUP può richiedere tempi notevolmente più lunghi rispetto alle versioni precedenti, per i seguenti motivi:  
 -   I controlli logici sono più completi.  
 -   Alcune delle strutture sottostanti da controllare sono più complesse.  
 -   Sono stati introdotti molti nuovi controlli per includere le nuove funzionalità.  
 Per questo motivo, l'utilizzo dell'opzione PHYSICAL_ONLY può consentire di ottenere tempi molto più brevi per l'esecuzione di DBCC CHECKFILEGROUP su filegroup di grandi dimensioni ed è quindi l'opzione consigliata per l'esecuzione frequente nei sistemi di produzione. È comunque consigliabile prevedere periodicamente un'esecuzione completa di DBCC CHECKFILEGROUP. La frequenza di esecuzione dipende da fattori specifici per i singoli ambienti aziendali e di produzione. L'opzione PHYSICAL_ONLY implica sempre l'utilizzo dell'opzione NO_INFOMSGS e non è consentita con le opzioni di correzione.  
  
> [!NOTE]  
>  Se si specifica PHYSICAL_ONLY, DBCC CHECKFILEGROUP ignora tutti i controlli dei dati FILESTREAM.  
  
 MAXDOP  
 **Si applica a**: da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 SP2 fino alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level).  
  
 Esegue l'override dell'opzione di configurazione **Massimo grado di parallelismo** di **sp_configure** per l'istruzione. MAXDOP può superare il valore configurato con sp_configure. Se MAXDOP supera il valore configurato con Resource Governor, il motore di database usa il valore MAXDOP di Resource Governor descritto in ALTER WORKLOAD GROUP (Transact-SQL). Quando si utilizza l'hint per la query MAXDOP sono valide tutte le regole semantiche utilizzate con l'opzione di configurazione max degree of parallelism. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!CAUTION]  
>  Se MAXDOP è impostato su zero, il server sceglie il grado massimo di parallelismo.  
  
## <a name="remarks"></a>Osservazioni  
DBCC CHECKFILEGROUP e DBCC CHECKDB sono comandi DBCC simili. La differenza principale consiste nel fatto che il comando DBCC CHECKFILEGROUP è limitato al singolo filegroup specificato e alle tabelle obbligatorie.
Tramite DBCC CHECKFILEGROUP vengono eseguiti i comandi seguenti:
-   [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) per il filegroup.  
-   [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) per ogni tabella e vista indicizzata nel filegroup.  
  
Non è necessario eseguire DBCC CHECKALLOC o DBCC CHECKTABLE separatamente da DBCC CHECKFILEGROUP.
  
## <a name="internal-database-snapshot"></a>Snapshot di database interno  
DBCC CHECKFILEGROUP utilizza uno snapshot interno del database per garantire la consistenza delle transazioni necessaria per eseguire questi controlli. Per altre informazioni, vedere [Visualizzare le dimensioni del file sparse di uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) e la sezione "Utilizzo dello snapshot interno del database DBCC" in [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Se non è possibile creare uno snapshot oppure viene specificata l'opzione TABLOCK, DBCC CHECKFILEGROUP acquisisce blocchi per ottenere la consistenza necessaria. In questo caso, è necessario un blocco esclusivo a livello di database per eseguire i controlli di allocazione, nonché blocchi condivisi a livello di tabella per eseguire i controlli delle tabelle. TABLOCK consente l'esecuzione più rapida di DBCC CHECKFILEGROUP in un database con carico di lavoro elevato, ma comporta una diminuzione del livello di concorrenza del database durante l'esecuzione dell'istruzione.
  
> [!NOTE]  
>  L'esecuzione di DBCC CHECKFILEGROUP nel database tempdb non comporta l'esecuzione di controlli di allocazione ed è pertanto necessario acquisire blocchi condivisi a livello di tabella per eseguire i controlli delle tabelle. Questo funzionamento dipende dal fatto che per motivi di prestazioni gli snapshot di database non sono disponibili in tempdb. Ciò significa che non è possibile ottenere la consistenza delle transazioni necessaria.  
  
## <a name="checking-objects-in-parallel"></a>Controllo parallelo degli oggetti  
Per impostazione predefinita, DBCC CHECKFILEGROUP esegue il controllo parallelo degli oggetti. Il grado di parallelismo viene determinato in modo automatico da Query Processor. Il livello massimo di parallelismo viene configurato allo stesso modo delle query parallele. Per limitare il numero massimo di processori disponibili per la verifica DBCC, usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
È possibile disabilitare il controllo parallelo tramite il flag di traccia 2528. Per altre informazioni, vedere [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
  
## <a name="nonclustered-indexes-on-separate-filegroups"></a>Indici non cluster in filegroup separati  
Se un indice non cluster nel filegroup specificato è associato a una tabella in un altro filegroup, l'indice non viene controllato poiché la tabella di base non è disponibile per la convalida.
Se una tabella nel filegroup specificato include un indice non cluster in un altro filegroup, tale indice non viene controllato per i motivi seguenti:
-   La struttura della tabella di base non dipende dalla struttura di un indice non cluster. Non è necessario eseguire un'analisi degli indici non cluster per convalidare la tabella di base.  
-   Il comando DBCC CHECKFILEGROUP convalida gli oggetti solo nel filegroup specificato.  
Un indice cluster e una tabella non possono trovarsi in filegroup diversi. Le considerazioni precedenti sono pertanto valide solo per gli indici non cluster.
  
## <a name="partitioned-tables-on-separate-filegroups"></a>Tabelle partizionate in filegroup separati  
Quando una tabella partizionata è distribuita su più filegroup, DBCC CHECKFILEGROUP controlla i set di righe delle partizioni presenti nel filegroup specificato e ignora quelli negli altri filegroup. Il messaggio informativo 2594 indica le partizioni non controllate. Gli indici non cluster non residenti nel filegroup specificato non vengono controllati.
  
## <a name="understanding-dbcc-error-messages"></a>Informazioni sui messaggi di errore DBCC  
Al termine del comando DBCC CHECKFILEGROUP, viene scritto un messaggio nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se il comando DBCC viene eseguito correttamente, il messaggio indica il completamento corretto e la durata dell'esecuzione del comando. Se il comando DBCC viene arrestato prima del completamento del controllo a causa di un errore, il messaggio indica che il comando è stato terminato e specifica un valore di stato e la durata dell'esecuzione del comando. Nella tabella seguente sono elencati e descritti i valori di stato che possono essere inclusi nel messaggio.
  
|State|Descrizione|  
|-----------|-----------------|  
|0|È stato generato l'errore numero 8930. Indica che il comando DBCC è stato terminato a causa di un danneggiamento dei metadati.|  
|1|È stato generato l'errore numero 8967. Si è verificato un errore DBCC interno.|  
|2|Si è verificato un errore durante un ripristino di database in modalità di emergenza.|  
|3|Indica che il comando DBCC è stato terminato a causa di un danneggiamento dei metadati.|  
|4|È stata rilevata una violazione di accesso o asserzione.|  
|5|il comando DBCC è stato terminato da un errore sconosciuto.|  
  
## <a name="error-reporting"></a>Segnalazione errori  
Quando un comando DBCC CHECKFILEGROUP rileva un errore di danneggiamento, viene creato un piccolo file di dump, denominato SQLDUMP *nnnn*.txt, nella directory LOG di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se le funzionalità di segnalazione degli errori e di raccolta di dati relativi all'utilizzo delle funzionalità sono abilitate per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il file verrà inoltrato automaticamente a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. I dati raccolti consentono di migliorare la funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Il file di dump contiene i risultati dell'esecuzione del comando DBCC CHECKFILEGROUP e l'output di dati diagnostici supplementari. Il file dispone di elenchi di controllo di accesso discrezionale (DACL) limitati. L'accesso è limitato all'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e ai membri del ruolo **sysadmin**. Per impostazione predefinita il ruolo **sysadmin** contiene tutti i membri del gruppo BUILTIN\Administrators di Windows e del gruppo dell'amministratore locale. Se il processo di raccolta dei dati non ha esito positivo, l'esecuzione del comando DBCC viene completata comunque.
  
## <a name="resolving-errors"></a>Risoluzione degli errori  
Se vengono segnalati errori da DBCC CHECKFILEGROUP, è consigliabile ripristinare il database dal backup. Non è possibile specificare le opzioni di correzione nel comando DBCC CHECKFILEGROUP.
Se non è disponibile un backup, per correggere gli errori restituiti è necessario eseguire DBCC CHECKDB e specificare un'opzione di correzione. L'opzione di correzione da utilizzare è specificata in fondo all'elenco degli errori restituiti. La correzione degli errori tramite l'opzione REPAIR_ALLOW_DATA_LOSS potrebbe richiedere l'eliminazione di alcune pagine e pertanto dei dati corrispondenti.
  
## <a name="result-sets"></a>Set di risultati  
DBCC CHECKFILEGROUP restituisce il set di risultati seguente (i valori possono variare):
-   Eccetto quando si specifica ESTIMATEONLY o NO_INFOMSGS.  
-   Per il database corrente, se non si specifica alcun database, indipendentemente dalla specifica delle opzioni (eccetto NOINDEX).  
  
```
DBCC results for 'master'.  
DBCC results for 'sys.sysrowsetcolumns'.  
There are 630 rows in 7 pages for object 'sys.sysrowsetcolumns'.  
DBCC results for 'sys.sysrowsets'.  
There are 97 rows in 1 pages for object 'sys.sysrowsets'.  
DBCC results for 'sysallocunits'.  
There are 195 rows in 3 pages for object 'sysallocunits'.  
  
There are 2340 rows in 16 pages for object 'spt_values'.  
DBCC results for 'MSreplication_options'.  
There are 2 rows in 1 pages for object 'MSreplication_options'.  
CHECKFILEGROUP found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
Se si specifica NO_INFOMSGS, DBCC CHECKFILEGROUP restituisce:
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
 Se si specifica ESTIMATEONLY, DBCC CHECKFILEGROUP restituisce (i valori possono variare):  

```
Estimated TEMPDB space needed for CHECKALLOC (KB)
-------------------------------------------------   
15  
  
(1 row(s) affected)  
  
Estimated TEMPDB space needed for CHECKTABLES (KB)   
--------------------------------------------------   
207  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **db_owner** .
  
## <a name="examples"></a>Esempi  
### <a name="a-checking-the-primary-filegroup-in-the-a-database"></a>R. Controllo del filegroup PRIMARY nel database  
Nell'esempio seguente viene controllato il filegroup primario del database corrente.
  
```sql
DBCC CHECKFILEGROUP;  
GO  
```  
  
### <a name="b-checking-the-adventureworks-primary-filegroup-without-nonclustered-indexes"></a>B. Controllo del filegroup PRIMARY nel database AdventureWorks senza indici non cluster  
Nell'esempio seguente viene controllato il filegroup primario del database `AdventureWorks2012`senza gli indici non cluster, specificando il numero di identificazione del filegroup primario e l'opzione `NOINDEX`.
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKFILEGROUP (1, NOINDEX);  
GO  
```  
  
### <a name="c-checking-the-primary-filegroup-with-options"></a>C. Controllo del filegroup PRIMARY con opzioni  
Nell'esempio seguente viene controllato il filegroup primario del database `master` specificando l'opzione `ESTIMATEONLY`.
  
```sql
USE master;  
GO  
DBCC CHECKFILEGROUP (1)  
WITH ESTIMATEONLY;  
```  
  
## <a name="see-also"></a>Vedere anche  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)  
[sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)  
[sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)  
[sys.sysfilegroups &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysfilegroups-transact-sql.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKALLOC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)
  
