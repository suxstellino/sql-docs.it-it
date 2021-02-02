---
description: ALTER TABLE index_option (Transact-SQL)
title: " | Microsoft Docs"
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- index_option
ms.assetid: 8a14f12d-2fbf-4036-b8b2-8db3354e0eb7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1df9583fb18c87693074769b099151281c6136f8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237055"
---
# <a name="alter-table-index_option-transact-sql"></a>ALTER TABLE index_option (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica un set di opzioni che possono essere applicate a un indice incluso in una definizione di vincolo creata tramite l'istruzione [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
{   
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF } 
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } 
  | SORT_IN_TEMPDB = { ON | OFF }   
  | ONLINE = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = { NONE |ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }   
      [ , ...n ] ) ]  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<range> ::=   
<partition_number_expression> TO <partition_number_expression>  
  
<single_partition_rebuild__option> ::=  
{  
    SORT_IN_TEMPDB = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                           ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 PAD_INDEX **=** { ON | **OFF** }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica il riempimento dell'indice. Il valore predefinito è OFF.  
  
 ON  
 La percentuale di spazio disponibile specificata in FILLFACTOR viene applicata alle pagine di livello intermedio dell'indice.  
  
 OFF o *fillfactor* non è specificato  
 Le pagine di livello intermedio vengono riempite quasi completamente, ma viene lasciato spazio sufficiente per almeno una riga avente le dimensioni massime consentite dall'indice, in base al set di chiavi nelle pagine intermedie.  
  
 FILLFACTOR **=** _fillfactor_  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica una percentuale indicante il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la modifica dell'indice. Il valore specificato deve essere un valore integer compreso tra 1 e 100. Il valore predefinito è 0.  
  
> [!NOTE]  
>  I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.  
  
 IGNORE_DUP_KEY **=** { ON | **OFF** }  
 Specifica il tipo di risposta quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. L'opzione non ha alcun effetto se si esegue [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md). Il valore predefinito è OFF.  
  
 ON  
 Viene visualizzato un messaggio di avviso quando in un indice univoco vengono inseriti valori chiave duplicati. Vengono considerate errate solo le righe che violano il vincolo di unicità.  
  
 OFF  
 Viene visualizzato un messaggio di errore quando in un indice univoco vengono inseriti valori chiave duplicati. Viene eseguito il rollback dell'intera operazione INSERT.  
  
 L'opzione IGNORE_DUP_KEY non può essere impostata su ON per gli indici creati in una vista, negli indici non univoci, XML, spaziali e filtrati.  
  
 Per visualizzare IGNORE_DUP_KEY, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH IGNORE_DUP_KEY equivale a WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | **OFF** }  
 Specifica se vengono ricalcolate le statistiche. Il valore predefinito è OFF.  
  
 ON  
 Le statistiche non aggiornate non vengono ricalcolate automaticamente.  
  
 OFF  
 Abilita l'aggiornamento automatico delle statistiche.  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.  
  
 ON  
 I blocchi di riga sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga.  
  
 OFF  
 I blocchi di riga non vengono utilizzati.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.  
  
 ON  
 I blocchi a livello di pagina sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina.  
  
 OFF  
 I blocchi a livello di pagina non vengono utilizzati.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive.

Specifica se eseguire o meno l'ottimizzazione per la contesa di inserimento dell'ultima pagina. Il valore predefinito è OFF. Per altre informazioni, vedere le sezione [Chiavi sequenziali](./create-index-transact-sql.md#sequential-keys) della pagina CREATE INDEX.
 
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica se archiviare i risultati dell'ordinamento in **tempdb**. Il valore predefinito è OFF.  
  
 ON  
 I risultati intermedi dell'ordinamento usati per la compilazione dell'indice vengono archiviati in **tempdb**. In questo modo si può ridurre il tempo necessario per creare un indice se **tempdb** si trova in un set di dischi diverso rispetto al database utente. La quantità di spazio su disco utilizzata durante la compilazione dell'indice sarà tuttavia maggiore.  
  
 OFF  
 I risultati intermedi dell'ordinamento vengono archiviati nello stesso database dell'indice.  
  
 ONLINE **=** { ON | **OFF** }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica se le tabelle sottostanti e gli indici associati sono disponibili per le query e la modifica dei dati durante l'operazione sugli indici. Il valore predefinito è OFF. L'opzione REBUILD può essere eseguita come operazione ONLINE.  
  
> [!NOTE]  
>  Non è possibile creare online indici non cluster univoci, tra cui gli indici creati a causa di un vincolo UNIQUE o PRIMARY KEY.  
  
 ON  
 I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. Durante la fase principale dell'operazione viene mantenuto solo un blocco preventivo condiviso (IS, Intent Shared) sulla tabella di origine. in modo da consentire l'esecuzione di query o l'aggiornamento della tabella sottostante e degli indici. All'inizio dell'operazione viene mantenuto un blocco condiviso (S) sull'oggetto di origine per un periodo molto breve. Al termine dell'operazione, per un breve periodo viene acquisito un blocco condiviso (S) sull'origine, in caso di creazione di un indice non cluster. In caso di creazione o di eliminazione di un indice cluster online o di ricompilazione di un indice cluster o non cluster, viene acquisito un blocco di modifica dello schema (SCH-M). Sebbene i blocchi sull'indice online siano blocchi di metadati brevi, soprattutto il blocco Sch-M deve attendere il completamento di tutte le transazioni bloccanti su questa tabella. Durante il tempo di attesa il blocco Sch-M impedisce tutte le altre transazioni in attesa dietro il blocco stesso per l'accesso alla stessa tabella. L'opzione ONLINE non può essere impostata su ON quando viene creato un indice per una tabella temporanea locale.  
  
> [!NOTE]  
>  Con la ricompilazione dell'indice online è possibile impostare le opzioni *low_priority_lock_wait* descritte più avanti in questa sezione. L'opzione *low_priority_lock_wait* gestisce la priorità dei blocchi Sch-M e S durante la ricompilazione dell'indice online.  
  
 OFF  
 I blocchi di tabella vengono applicati per la durata dell'operazione sugli indici. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione. Un'operazione sugli indici offline che crea, ricompila o elimina un indice cluster oppure ricompila o elimina un indice non cluster acquisisce un blocco di modifica dello schema (SCH-M) sulla tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione. Un'operazione sugli indici offline che crea un indice non cluster acquisisce un blocco condiviso (S) sulla tabella. Tale blocco impedisce l'aggiornamento della tabella sottostante ma consente operazioni di lettura, ad esempio l'esecuzione di istruzioni SELECT.  
  
 Per altre informazioni, vedere [Funzionamento delle operazioni sugli indici online](../../relational-databases/indexes/how-online-index-operations-work.md).  
  
> [!NOTE]
>  Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Sostituisce l'opzione di configurazione **max degree of parallelism** per la durata dell'operazione sull'indice. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
 *max_degree_of_parallelism* può essere:  
  
 - 1: disattiva la generazione di piani paralleli.  
 - \>1: limita il numero massimo di processori usati in un'operazione parallela sugli indici in base al numero specificato.  
 - 0 (valore predefinito): usa il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 DATA_COMPRESSION  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica l'opzione di compressione dei dati per la tabella, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:  
  
 NONE  
 La tabella o le partizioni specificate non vengono compresse. Si applica solo alle tabelle rowstore, non si applica alle tabelle columnstore.  
  
 ROW  
 La tabella o le partizioni specificate vengono compresse utilizzando la compressione di riga. Si applica solo alle tabelle rowstore, non si applica alle tabelle columnstore.  
  
 PAGE  
 La tabella o le partizioni specificate vengono compresse utilizzando la compressione di pagina. Si applica solo alle tabelle rowstore, non si applica alle tabelle columnstore.  
  
 COLUMNSTORE  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Si applica solo alle tabelle columnstore. Con COLUMNSTORE si specifica di decomprimere una partizione compressa con l'opzione COLUMNSTORE_ARCHIVE. Quando i dati vengono ripristinati, l'indice COLUMNSTORE continua a essere compresso con la compressione columnstore usata per tutte le tabelle columnstore.  
  
 COLUMNSTORE_ARCHIVE  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Si applica solo alle tabelle columnstore, ovvero tabelle archiviate con un indice columnstore cluster. COLUMNSTORE_ARCHIVE comprime anche la partizione specificata riducendone le dimensioni. Può essere utilizzata per l'archiviazione o in altre situazioni in cui sono richieste dimensioni di archiviazione inferiori ed è possibile concedere più tempo per l'archiviazione e il recupero.  
  
 Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...*n* ] **)** **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica le partizioni alle quali si applica l'impostazione DATA_COMPRESSION. Se la tabella non è partizionata, l'argomento ON PARTITIONS genera un errore. Se la clausola ON PARTITIONS non viene specificata, l'opzione DATA_COMPRESSION si applica a tutte le partizioni di una tabella partizionata.  
  
\<partition_number_expression> può essere specificato nei modi seguenti:  
  
-   Fornire il numero di una partizione, ad esempio: ON PARTITIONS (2).  
-   Specificare i numeri di partizione per più partizioni singole separati da virgole, ad esempio ON PARTITIONS (1, 5).  
-   Specificare sia intervalli, sia singole partizioni, ad esempio ON PARTITIONS (2, 4, 6 TO 8).  
  
\<range> può essere specificato sotto forma di numeri di partizione separati dalla parola TO, ad esempio: ON PARTITIONS (6 TO 8).  
  
 Per impostare tipi diversi di compressione dei dati per partizioni diverse, specificare più volte l'opzione DATA_COMPRESSION, ad esempio:  
  
```sql  
--For rowstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
)  
  
--For columnstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = COLUMNSTORE ON PARTITIONS (1, 3, 5),   
DATA_COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (2, 4, 6 TO 8)  
)  
```  
  
**\<single_partition_rebuild__option>**  
 Nella maggior parte dei casi, la ricompilazione di un indice determina la ricompilazione di tutte le partizioni di un indice partizionato. Le opzioni seguenti, in caso di applicazione a una partizione singola, non ricompilano tutte le partizioni.  
  
-   SORT_IN_TEMPDB  
-   MAXDOP  
-   DATA_COMPRESSION  
  
**low_priority_lock_wait**  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Un'operazione **SWITCH** o di ricompilazione dell'indice online viene completata appena non esistono operazioni di blocco per questa tabella. *WAIT_AT_LOW_PRIORITY* indica che, se l'operazione **SWITCH** o di ricompilazione dell'indice online non può essere completata immediatamente, resta in attesa. L'operazione conserva i blocchi con priorità bassa, consentendo ad altre operazioni con blocchi in conflitto con l'istruzione DDL di continuare. L'omissione dell'opzione **WAIT AT LOW PRIORITY** equivale a `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.  
  
MAX_DURATION = *time* [**MINUTES** ]  
 Tempo (valore intero specificato in minuti) di attesa del blocco dell'operazione **SWITCH** o di ricompilazione dell'indice online che deve essere acquisito durante l'esecuzione del comando DDL. L'operazione SWITCH o di ricompilazione dell'indice online tenta il completamento immediato. Se l'operazione viene bloccata per il tempo specificato in **MAX_DURATION**, una delle azioni **ABORT_AFTER_WAIT** viene eseguita. Il tempo **MAX_DURATION** è sempre espresso in minuti e la parola **MINUTES** può essere omessa.  
  
ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
 NONE  
 Continua l'operazione **SWITCH** o di ricompilazione dell'indice online senza modificare la priorità dei blocchi (usando la priorità normale).  
  
SELF  
 Esce dall'operazione **SWITCH** o DDL di ricompilazione dell'indice online attualmente in esecuzione senza eseguire alcuna azione.  
  
BLOCKERS  
 Termina tutte le transazioni utente che attualmente bloccano l'operazione **SWITCH** o DDL di ricompilazione dell'indice online in modo da poter continuare l'operazione.  
 Per BLOCKERS è richiesta l'autorizzazione **ALTER ANY CONNECTION**.  
  
## <a name="remarks"></a>Osservazioni  
 Per una descrizione completa delle opzioni per gli indici, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)   
 [computed_column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-computed-column-definition-transact-sql.md)   
 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  
  
 
