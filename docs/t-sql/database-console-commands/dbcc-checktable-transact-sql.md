---
description: DBCC CHECKTABLE (Transact-SQL)
title: DBCC CHECKIDENT (Transact-SQL) | Microsoft Docs
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKTABLE_TSQL
- DBCC_CHECKTABLE_TSQL
- DBCC CHECKTABLE
- CHECKTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], DBCC CHECKTABLE
- page integrity checks [SQL Server]
- consistency [SQL Server], tables
- consistency [SQL Server], indexed views
- DBCC CHECKTABLE statement
- integrity [SQL Server]
- low overhead checks
- table integrity checks [SQL Server]
ms.assetid: 0d6cb620-eb58-4745-8587-4133a1b16994
author: pmasl
ms.author: umajay
ms.openlocfilehash: 78a84099b202ca55588e1365b27d80004d2cdaa5
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172613"
---
# <a name="dbcc-checktable-transact-sql"></a>DBCC CHECKTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Controlla l'integrità di tutte le pagine e le strutture che compongono la tabella o la vista indicizzata.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
    
## <a name="syntax"></a>Sintassi    
    
```syntaxsql
DBCC CHECKTABLE     
(    
    table_name | view_name    
    [ , { NOINDEX | index_id }    
     |, { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD }     
    ]     
)    
    [ WITH     
        { [ ALL_ERRORMSGS ]    
          [ , EXTENDED_LOGICAL_CHECKS ]     
          [ , NO_INFOMSGS ]    
          [ , TABLOCK ]     
          [ , ESTIMATEONLY ]     
          [ , { PHYSICAL_ONLY | DATA_PURITY } ]     
          [ , MAXDOP = number_of_processors ]    
        }    
    ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *table_name* | *view_name*  
 Tabella o vista indicizzata per cui si desidera eseguire i controlli di integrità. I nomi di tabelle e viste devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
    
NOINDEX  
 Specifica che non è necessario eseguire controlli estesi di indici non cluster per le tabelle utente. In questo modo, è possibile ridurre i tempi di esecuzione complessivi. NOINDEX non influisce sulle tabelle di sistema perché i controlli di integrità vengono sempre eseguiti su tutti gli indici delle tabelle di sistema.  
    
 *index_id*  
 Numero di identificazione (ID) dell'indice per cui eseguire i controlli di integrità. Se si specifica *index_id*, DBCC CHECKTABLE esegue i controlli di integrità solo su tale indice, insieme all'indice heap o cluster.  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 Specifica che DBCC CHECKTABLE corregge gli errori rilevati. Per utilizzare un'opzione di correzione, è necessario che il database sia in modalità utente singolo.  
    
REPAIR_ALLOW_DATA_LOSS  
 Tenta di riparare tutti gli errori rilevati. Le operazioni di correzione possono comportare la perdita di dati.  
    
REPAIR_FAST  
 La sintassi è stata mantenuta solo a scopo di compatibilità con le versioni precedenti. Non vengono eseguite correzioni.  
    
REPAIR_REBUILD  
 Esegue operazioni di ripristino senza possibilità di perdita dei dati. Sono incluse operazioni di ripristino rapide, ad esempio ripristino di righe mancanti in indici non cluster, nonché operazioni che richiedono una maggiore quantità di tempo, come la ricompilazione di un indice.  
 Questo argomento non consente di correggere errori relativi ai dati FILESTREAM.  
    
 > [!NOTE]  
 > Utilizzare le opzioni REPAIR solo come ultima risorsa. Per correggere gli errori, è consigliabile eseguire un ripristino da un backup. Le operazioni di correzione non tengono conto degli eventuali vincoli esistenti per le tabelle o tra le tabelle. Se la tabella specificata è interessata da uno o più vincoli, è consigliabile eseguire `DBCC CHECKCONSTRAINTS` dopo l'operazione di correzione.
 > Se è necessario usare REPAIR, eseguire `DBCC CHECKTABLE` senza opzioni di correzione per individuare il livello di correzione da applicare. Se si prevede di usare il livello REPAIR_ALLOW_DATA_LOSS, è consigliabile eseguire il backup del database prima di eseguire `DBCC CHECKTABLE` con questa opzione.  
    
ALL_ERRORMSGS  
 Visualizza un numero illimitato di errori. Tutti i messaggi di errore vengono visualizzati per impostazione predefinita. La specifica o l'omissione di questa opzione non ha alcun effetto.  
    
EXTENDED_LOGICAL_CHECKS  
 Se il livello di compatibilità è 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) o maggiore, esegue controlli di consistenza logica in una vista indicizzata, indici XML e indici spaziali, dove presenti.  
 Per altre informazioni, vedere *Esecuzione di controlli di consistenza logica negli indici* nella sezione [Osservazioni](#remarks) più avanti in questo argomento.  
    
NO_INFOMSGS  
 Disattiva tutti i messaggi informativi.  
    
TABLOCK  
 Impone l'acquisizione di un blocco condiviso a livello di tabella per l'esecuzione di DBCC CHECKTABLE, anziché utilizzare uno snapshot interno del database. TABLOCK consente l'esecuzione più rapida di DBCC CHECKTABLE in una tabella con carico di lavoro elevato, ma comporta una diminuzione del livello di concorrenza della tabella durante l'esecuzione di DBCC CHECKTABLE.  
    
ESTIMATEONLY  
 Visualizza lo spazio di tempdb stimato necessario per eseguire l'istruzione DBCC CHECKTABLE con tutte le altre opzioni specificate.  
    
PHYSICAL_ONLY  
 Limita il controllo di integrità alla struttura fisica della pagina, alle intestazioni dei record e alla struttura fisica degli alberi B. Progettato per consentire un controllo a basso overhead della consistenza fisica della tabella, questo controllo consente inoltre di rilevare le pagine incomplete e i comuni problemi a livello di hardware che possono compromettere i dati. Un'esecuzione completa di DBCC CHECKTABLE può richiedere tempi notevolmente più lunghi rispetto alle versioni precedenti, per i seguenti motivi:  
 -   I controlli logici sono più completi.  
 -   Alcune delle strutture sottostanti da controllare sono più complesse.  
 -   Sono stati introdotti molti nuovi controlli per includere le nuove funzionalità.  
   
Per questo motivo, l'utilizzo dell'opzione PHYSICAL_ONLY può consentire di ottenere tempi molto più brevi per l'esecuzione di DBCC CHECKTABLE su tabelle di grandi dimensioni ed è quindi l'opzione consigliata per l'esecuzione frequente nei sistemi di produzione. È tuttora consigliabile prevedere periodicamente un'esecuzione completa di DBCC CHECKTABLE. La frequenza di esecuzione dipende da fattori specifici per i singoli ambienti aziendali e di produzione. L'opzione PHYSICAL_ONLY implica sempre l'utilizzo dell'opzione NO_INFOMSGS e non è consentita con le opzioni di correzione.  
    
 > [!NOTE]  
 > Se si specifica PHYSICAL_ONLY, DBCC CHECKTABLE ignora tutti i controlli dei dati FILESTREAM.  
    
DATA_PURITY  
 Consente a DBCC CHECKTABLE di controllare la tabella per individuare valori di colonna non validi o non compresi nell'intervallo dei valori consentiti. Ad esempio, DBCC CHECKTABLE rileva le colonne con valori di data e ora maggiori o minori dell'intervallo accettabile per il tipo di dati **datetime** oppure le colonne di tipi di dati numerici approssimati o **decimal** con valori di precisione o di scala non validi.  
 I controlli di integrità dei valori di colonna sono abilitati per impostazione predefinita e non richiedono l'opzione DATA_PURITY. Per i database aggiornati da versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile utilizzare DBCC CHECKTABLE WITH DATA_PURITY per individuare e correggere gli errori in una specifica tabella. I controlli dei valori di colonna nella tabella, tuttavia, non vengono abilitati per impostazione predefinita fino a quando DBCC CHECKDB WITH DATA_PURITY non è stato eseguito senza errori nel database. A questo punto, DBCC CHECKDB e DBCC CHECKTABLE controllano l'integrità dei valori di colonna per impostazione predefinita.  
 Gli errori di convalida rilevati da questa opzione non possono essere corretti utilizzando le opzioni di correzione DBCC. Per informazioni sulla correzione manuale di questi errori, vedere l'articolo 923247 della Knowledge Base: [Risoluzione dell'errore DBCC 2570 in SQL Server 2005 e versioni successive](https://support.microsoft.com/kb/923247).  
 Se si specifica PHYSICAL_ONLY, i controlli di integrità di colonna non vengono eseguiti.  
    
MAXDOP  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 e versioni successive).  
 
 Esegue l'override dell'opzione di configurazione **Massimo grado di parallelismo** di **sp_configure** per l'istruzione. MAXDOP può superare il valore configurato con sp_configure. Se MAXDOP supera il valore configurato con Resource Governor, il motore di database usa il valore MAXDOP di Resource Governor descritto in ALTER WORKLOAD GROUP (Transact-SQL). Quando si utilizza l'hint per la query MAXDOP sono valide tutte le regole semantiche utilizzate con l'opzione di configurazione max degree of parallelism. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
    
 > [!NOTE]  
 > Se MAXDOP è impostato su zero, il server sceglie il grado massimo di parallelismo.  
    
## <a name="remarks"></a>Commenti    
    
> [!NOTE]    
> Per eseguire DBCC CHECKTABLE su ogni tabella del database, usare [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).    
    
DBCC CHECKTABLE esegue i controlli seguenti per la tabella specificata:
-   Collegamenti corretti per le pagine di dati di indice, all'interno di righe, LOB e di overflow della riga.    
-   Ordinamento corretto degli indici.    
-   Consistenza dei puntatori.    
-   Presenza di dati accettabili in ogni pagina, incluse le colonne calcolate.    
-   Presenza di offset di pagina accettabili.    
-   A ogni riga nella tabella di base corrisponde una riga in ogni indice non cluster e viceversa.    
-   Ogni riga in una tabella o un indice partizionato è inclusa nella partizione corretta.    
-   Coerenza a livello di collegamenti tra il file system e la tabella quando vengono archiviati i dati **varbinary(max)** nel file system usando FILESTREAM.    
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>Esecuzione di controlli di consistenza logica negli indici    
I controlli di consistenza logica negli indici variano in base al livello di compatibilità del database, come indicato di seguito:
-   Se il livello di compatibilità è 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) o maggiore:    
    -   A meno che non venga specificato NOINDEX, tramite DBCC CHECKTABLE vengono eseguiti controlli di consistenza sia fisica che logica in una singola tabella e in tutti i relativi indici non cluster. Per impostazione predefinita, tuttavia, negli indici XML, negli indici spaziali e nelle viste indicizzate vengono eseguiti solo controlli di consistenza fisica.    
    -   Se viene specificato WITH EXTENDED_LOGICAL_CHECKS, vengono eseguiti controlli logici in una vista indicizzata, indici XML e indici spaziali, dove presenti. Per impostazione predefinita, i controlli di consistenza fisica vengono eseguiti prima di quelli di consistenza logica. Se viene specificato anche NOINDEX, vengono eseguiti solo i controlli logici.    
         Tramite questi controlli di consistenza logica vengono eseguiti controlli incrociati della tabella degli indici interna dell'oggetto Index con la tabella utente a cui viene fatto riferimento. Per trovare le righe esterne, viene creata una query interna per eseguire un'intersezione completa della tabella interna e della tabella utente. L'esecuzione di questa query può influire notevolmente sulle prestazioni e non è possibile tenere traccia del relativo stato di avanzamento. È pertanto consigliabile specificare WITH EXTENDED_LOGICAL_CHECKS solo se si sospetta la presenza di problemi dell'indice non correlati a danni fisici o se i checksum a livello di pagina sono stati disabilitati e si sospetta la presenza di danni hardware a livello di colonna.    
    -   Se l'indice è un indice filtrato, tramite DBCC CHECKDB vengono eseguiti controlli di consistenza per verificare che le voci di indice soddisfino il predicato del filtro.   
      
- A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], non verranno eseguiti per impostazione predefinita controlli aggiuntivi su colonne calcolate persistenti, colonne UDT e indici filtrati per evitare valutazioni di espressioni costose. Questa modifica riduce notevolmente la durata di CHECKDB su database contenenti tali oggetti. Tuttavia, le verifiche di coerenza fisica di questi oggetti vengono sempre completate. Le valutazioni delle espressioni vengono eseguite, oltre alle verifiche logiche già presenti, ovvero viste indicizzate, indici XML e indici spaziali, solo quando è specificata l'opzione EXTENDED_LOGICAL_CHECKS, in quanto parte di tale opzione.
-  Se il livello di compatibilità è 90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) o minore, a meno che non venga specificato NOINDEX, tramite DBCC CHECKTABLE vengono eseguite verifiche di coerenza sia fisica che logica in una singola tabella o vista indicizzata e in tutti i relativi indici non cluster e XML. Gli indici spaziali non sono supportati.
    
 **Per informazioni sul livello di compatibilità di un database**    
[Visualizzare o modificare il livello di compatibilità di un database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>Snapshot di database interno    
DBCC CHECKTABLE utilizza uno snapshot interno del database per garantire la consistenza transazionale necessaria per l'esecuzione di questi controlli. Per altre informazioni, vedere [Visualizzare le dimensioni del file sparse di uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) e la sezione "Utilizzo dello snapshot interno del database DBCC" in [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Se non è possibile creare uno snapshot o se viene specificato TABLOCK, DBCC CHECKTABLE acquisisce un blocco condiviso a livello di tabella per ottenere la consistenza necessaria.
    
> [!NOTE]    
> Se si esegue DBCC CHECKTABLE sul database tempdb, è richiesta l'acquisizione di un blocco di tabella condiviso. Questo funzionamento dipende dal fatto che per motivi di prestazioni gli snapshot di database non sono disponibili in tempdb. Ciò significa che non è possibile ottenere la consistenza delle transazioni necessaria.    
    
## <a name="checking-and-repairing-filestream-data"></a>Controllo e ripristino dei dati FILESTREAM    
Quando FILESTREAM è abilitato per un database e una tabella, è possibile, facoltativamente, archiviare oggetti binari di grandi dimensioni (BLOB) **varbinary(max)** nel file system. Quando si utilizza DBCC CHECKTABLE in una tabella tramite cui vengono archiviati oggetti BLOB nel file system, tramite DBCC viene verificata la consistenza a livello di collegamenti tra il file system e il database.
Se, ad esempio, una tabella contiene una colonna **varbinary(max)** che usa l'attributo FILESTREAM, tramite DBCC CHECKTABLE viene verificato che sia presente un mapping uno-a-uno tra le directory e i file del file system e le righe, le colonne e i valori di colonna della tabella. DBCC CHECKTABLE consente di correggere i danneggiamenti se si specifica l'opzione REPAIR_ALLOW_DATA_LOSS. Per correggere i danneggiamenti relativi a FILESTREAM, DBCC elimina qualsiasi riga di tabella in cui non sono presenti dati del file system e qualsiasi directory e file sui quali non è stato eseguito il mapping a una riga, una colonna o un valore di colonna della tabella.
    
## <a name="checking-objects-in-parallel"></a>Controllo parallelo degli oggetti    
Per impostazione predefinita, DBCC CHECKTABLE esegue un controllo parallelo degli oggetti. Il grado di parallelismo viene determinato in modo automatico da Query Processor. Il grado di parallelismo massimo viene configurato esattamente come per le query parallele. Per limitare il numero massimo di processori disponibili per la verifica DBCC, usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
È possibile disabilitare il controllo parallelo tramite il flag di traccia 2528. Per altre informazioni, vedere [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
    
> [!NOTE]    
> Durante un'operazione DBCC CHECKTABLE i byte archiviati in una colonna con tipo definito dall'utente ordinato per byte devono essere uguali alla serializzazione calcolata del valore con tipo definito dall'utente. Se questa condizione non si verifica, la routine DBCC CHECKTABLE restituirà un errore di consistenza.    
    
## <a name="understanding-dbcc-error-messages"></a>Informazioni sui messaggi di errore DBCC    
Dopo il completamento del comando DBCC CHECKTABLE, nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene scritto un messaggio. Se il comando DBCC viene eseguito correttamente, il messaggio indica il completamento corretto e la durata dell'esecuzione del comando. Se il comando DBCC viene arrestato prima del completamento del controllo a causa di un errore, il messaggio indica che il comando è stato terminato e specifica un valore di stato e la durata dell'esecuzione del comando. Nella tabella seguente sono elencati e descritti i valori di stato che possono essere inclusi nel messaggio.
    
|State|Descrizione|    
|-----------|-----------------|    
|0|È stato generato l'errore numero 8930. Indica che il comando DBCC è stato terminato a causa di un danneggiamento dei metadati.|    
|1|È stato generato l'errore numero 8967. Si è verificato un errore DBCC interno.|    
|2|Si è verificato un errore durante un ripristino di database in modalità di emergenza.|    
|3|Indica che il comando DBCC è stato terminato a causa di un danneggiamento dei metadati.|    
|4|È stata rilevata una violazione di accesso o asserzione.|    
|5|il comando DBCC è stato terminato da un errore sconosciuto.|    
    
## <a name="error-reporting"></a>Segnalazione errori    
Quando DBCC CHECKTABLE rileva un errore di danneggiamento, viene creato un piccolo file di dump denominato `SQLDUMP*nnnn*.txt` nella directory LOG di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando la raccolta di dati *Utilizzo caratteristiche* e le funzionalità *Segnalazione errori* sono abilitate per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il file viene inoltrato automaticamente a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. I dati raccolti consentono di migliorare la funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Il file di dump contiene i risultati dell'esecuzione del comando DBCC CHECKTABLE e l'output di dati diagnostici supplementari. Il file dispone di elenchi di controllo di accesso discrezionale (DACL) limitati. L'accesso è limitato all'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e ai membri del ruolo sysadmin. Per impostazione predefinita il ruolo sysadmin contiene tutti i membri del gruppo BUILTIN\Administrators di Windows e del gruppo dell'amministratore locale. Se il processo di raccolta dei dati non ha esito positivo, l'esecuzione del comando DBCC viene completata comunque.
    
## <a name="resolving-errors"></a>Risoluzione degli errori    
 Se vengono segnalati errori dopo l'esecuzione di DBCC CHECKTABLE, è consigliabile eseguire il ripristino del database dal backup, anziché eseguire REPAIR con una delle opzioni REPAIR. Se non è disponibile un backup, l'esecuzione di REPAIR può consentire la correzione degli errori segnalati. L'opzione REPAIR da utilizzare viene indicata alla fine dell'elenco degli errori segnalati. Si noti, tuttavia, che la correzione di errori con l'opzione REPAIR_ALLOW_DATA_LOSS potrebbe richiedere l'eliminazione di alcune pagine, con conseguente perdita di dati.    
È possibile eseguire l'operazione di correzione tramite una transazione utente che consente il rollback delle modifiche apportate. Se si esegue il rollback delle correzioni, il database include ancora errori e deve essere ripristinato da un backup. Dopo il completamento di tutte le correzioni, eseguire il backup del database.
    
## <a name="result-sets"></a>Set di risultati    
DBCC CHECKTABLE restituisce il set dei risultati seguente. Viene restituito lo stesso set di risultati se si specifica solo il nome della tabella o qualsiasi opzione.
    
```
DBCC results for 'HumanResources.Employee'.    
There are 288 rows in 13 pages for object 'Employee'.    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
Se si specifica l'opzione ESTIMATEONLY, DBCC CHECKTABLE restituisce il set di risultati seguente:
    
```
Estimated TEMPDB space needed for CHECKTABLES (KB)     
--------------------------------------------------     
21    
(1 row(s) affected)    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
## <a name="permissions"></a>Autorizzazioni    
L'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server sysadmin o dei ruoli predefiniti del database db_owner o db_ddladmin.    
    
## <a name="examples"></a>Esempi    
    
### <a name="a-checking-a-specific-table"></a>R. Controllo di una tabella specifica    
Nell'esempio seguente viene controllata l'integrità delle pagine di dati della tabella `HumanResources.Employee` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee');    
GO    
```    
    
### <a name="b-performing-a-low-overhead-check-of-the-table"></a>B. Esecuzione di un controllo della tabella a basso overhead    
 Nell'esempio seguente viene eseguito un controllo a basso overhead della tabella `Employee` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].    
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee') WITH PHYSICAL_ONLY;    
GO    
```    
    
### <a name="c-checking-a-specific-index"></a>C. Controllo di un indice specifico    
 Nell'esempio seguente viene eseguito il controllo di un indice specifico, recuperato tramite l'accesso a `sys.indexes`.    
    
```sql    
DECLARE @indid int;    
SET @indid = (SELECT index_id     
              FROM sys.indexes    
              WHERE object_id = OBJECT_ID('Production.Product')    
                    AND name = 'AK_Product_Name');    
DBCC CHECKTABLE ('Production.Product',@indid);    
```    
    
## <a name="see-also"></a>Vedere anche    
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)     
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)    
    
  
