---
description: Hint (Transact-SQL) - Tabella
title: Hint di tabella (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TABLE_HINT_TSQL
- Table Hint
dev_langs:
- TSQL
helpviewer_keywords:
- SERIALIZABLE table hint
- UPDLOCK table hint
- HOLDLOCK table hint
- TABLOCKX table hint
- READUNCOMMITTED table hint
- hints [SQL Server], tables
- READCOMMITTEDLOCK table hint
- FORCESCAN table hint
- ROWLOCK table hint
- XLOCK table hint
- NOLOCK table hint
- READPAST table hint
- NOWAIT table hint
- FORCESEEK table hint
- table hints [SQL Server]
- TABLOCK table hint
- REPEATABLEREAD table hint
- READ COMMITTED table hint
- NOEXPAND table hint
- PAGLOCK table hint
ms.assetid: 8bf1316f-c0ef-49d0-90a7-3946bc8e7a89
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2043bf4c60a1154b719d81b583d055b60b85c6ec
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172333"
---
# <a name="hints-transact-sql---table"></a>Hint (Transact-SQL) - Tabella
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gli hint di tabella consentono di modificare il comportamento predefinito di Query Optimizer per la durata dell'istruzione DML (Data Manipulation Language) specificando un metodo di blocco, uno o più indici, un'operazione di elaborazione di query, ad esempio una scansione di tabella o una ricerca nell'indice, oppure altre opzioni. Gli hint di tabella sono specificati nella clausola FROM dell'istruzione DML e influiscono solo sulla tabella o sulla vista a cui viene fatto riferimento nella clausola.  
  
> [!CAUTION]  
>  Poiché Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seleziona in genere il piano di esecuzione migliore per una query, gli hint devono essere usati solo se strettamente necessari ed esclusivamente da sviluppatori e amministratori di database esperti.  
  
 **Si applica a:**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [SELECT](../../t-sql/queries/select-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
WITH  ( <table_hint> [ [, ]...n ] )  
  
<table_hint> ::=   
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ] 
  | FORCESCAN  
  | FORCESEEK  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
  
<table_hint_limited> ::=  
{  
    KEEPIDENTITY   
  | KEEPDEFAULTS   
  | HOLDLOCK   
  | IGNORE_CONSTRAINTS   
  | IGNORE_TRIGGERS   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
WITH **(** \<table_hint> **)** [[ **,** ]...*n*]  
Con alcune eccezioni, gli hint di tabella sono supportati nella clausola FROM solo se vengono specificati con la parola chiave WITH. Tali hint devono inoltre essere racchiusi tra parentesi.  
  
> [!IMPORTANT]  
> L'omissione della parola chiave WITH è una funzionalità deprecata: [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
Gli hint di tabella seguenti sono consentiti con e senza la parola chiave `WITH`: `NOLOCK`, `READUNCOMMITTED`, `UPDLOCK`, `REPEATABLEREAD`, `SERIALIZABLE`, `READCOMMITTED`, `TABLOCK`, `TABLOCKX`, `PAGLOCK`, `ROWLOCK`, `NOWAIT`, `READPAST`, `XLOCK`, `SNAPSHOT` e `NOEXPAND`. Se vengono specificati senza la parola chiave WITH, questi hint devono essere specificati da soli. Ad esempio:  
  
```sql  
FROM t (TABLOCK)  
```  
  
Se l'hint viene specificato con un'altra opzione, è necessario usare la parola chiave WITH:  
  
```sql  
FROM t WITH (TABLOCK, INDEX(myindex))  
```  
  
Si consiglia di separare gli hint di tabella tramite virgole.  
  
> [!IMPORTANT]  
> L'utilizzo di spazi al posto delle virgole per separare gli hint è una funzionalità deprecata: [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
NOEXPAND  
Specifica che qualsiasi vista indicizzata non verrà espansa per accedere alle tabelle sottostanti quando Query Optimizer elabora la query. In Query Optimizer la vista viene gestita come una tabella con indici cluster. L'hint NOEXPAND è applicabile solo alle viste indicizzate. Per altre informazioni, vedere [Utilizzo di NOEXPAND](#using-noexpand).  
  
INDEX  **(** _<index\_value>_ [ **,** ... _n_ ] ) | INDEX =  ( _<index\_value>_ **)**  
La sintassi INDEX() consente di specificare i nomi o gli ID di uno o più indici che devono essere usati in Query Optimizer quando viene elaborata l'istruzione. La sintassi alternativa `INDEX =` specifica un singolo valore di indice. È possibile specificare solo un hint per l'indice per ogni tabella.  
  
Se esiste un indice cluster, `INDEX(0)` attiva un'analisi degli indici cluster, mentre `INDEX(1)` esegue un'analisi o una ricerca degli indici cluster. Se non esiste alcun indice cluster, `INDEX(0)` attiva una scansione di tabella, mentre `INDEX(1)` viene interpretato come errore.  
  
 Se in un unico elenco di hint vengono usati più indici, gli indici duplicati vengono ignorati, mentre gli altri indici vengono usati per recuperare le righe della tabella. L'ordine degli indici nell'hint è significativo. Un hint con più indici impone inoltre il collegamento degli indici tramite operatore AND e Query Optimizer applica il numero massimo di condizioni possibili a ogni indice a cui viene effettuato l'accesso. Se la raccolta di indici con hint non include tutte le colonne a cui la query fa riferimento, viene eseguita un'operazione di recupero delle colonne rimanenti dopo che tutte le colonne indicizzate sono state recuperate da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
> [!NOTE]  
> Se in una tabella dei fatti in un join a stella viene usato un hint per l'indice che fa riferimento a più indici, l'hint per l'indice viene ignorato e verrà restituito un messaggio di avviso. Non è inoltre consentito il collegamento degli indici tramite OR per una tabella per la quale è stato specificato un hint per l'indice.  
  
 Un hint di tabella può includere al massimo 250 indici non cluster.  
  
KEEPIDENTITY  
Applicabile solo in un'istruzione INSERT se viene usata l'opzione BULK con [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
 Specifica che il valore o i valori Identity presenti nel file di dati importato devono essere usati per la colonna Identity. Se KEEPIDENTITY viene omesso, i valori Identity per questa colonna vengono verificati ma non importati e Query Optimizer assegna automaticamente valori univoci in base ai valori di inizializzazione e di incremento specificati in fase di creazione della tabella.  
  
> [!IMPORTANT]  
> Se il file di dati non contiene valori per la colonna Identity nella tabella o nella vista, tale colonna deve essere ignorata, a meno che non sia l'ultima colonna della tabella. Per altre informazioni, vedere [Usare un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md). Se una colonna Identity viene ignorata correttamente, Query Optimizer assegna automaticamente valori univoci per la colonna Identity nelle righe importate della tabella.  
  
Per un esempio di questo hint in un'istruzione `INSERT ... SELECT * FROM OPENROWSET(BULK...)`, vedere [Mantenere i valori Identity durante l'importazione bulk dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).  
  
Per altre informazioni sul controllo del valore Identity per una tabella, vedere [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).  
  
KEEPDEFAULTS  
Applicabile solo in un'istruzione INSERT se viene usata l'opzione BULK con [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Specifica l'inserimento di un valore predefinito per la colonna della tabella, se disponibile, al posto del valore NULL quando nel record di dati non è presente un valore per la colonna.  
  
Per un esempio d'uso di questo hint in un'istruzione INSERT... SELECT * FROM OPENROWSET(BULK...), vedere [Mantenere i valori Null o usare i valori predefiniti durante un'importazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... _n_ ] **))** ]  
Specifica che Query Optimizer deve usare solo un'operazione di ricerca nell'indice come percorso di accesso ai dati della tabella o della vista. 

> [!NOTE]
> A partire da [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1, è anche possibile specificare i parametri di indice. In questo caso, in Query Optimizer vengono considerate solo le operazioni di ricerca nell'indice specificato, utilizzando almeno le colonne dell'indice specificate.  
  
 *index_value*  
 Nome o valore ID dell'indice. Non è possibile specificare un ID di indice pari a 0 (heap). Per restituire l'ID o il nome dell'indice, eseguire una query sulla vista del catalogo **sys.indexes**.  
  
 *index_column_name*  
 Nome della colonna dell'indice da includere nell'operazione di ricerca. L'utilizzo di FORCESEEK con i parametri di indice è analogo all'utilizzo di FORCESEEK con un hint INDEX. È tuttavia possibile ottenere un maggior controllo sul percorso di accesso usato da Query Optimizer specificando sia l'indice su cui eseguire la ricerca, sia le colonne dell'indice da prendere in considerazione durante l'operazione di ricerca. Se necessario, possono essere considerate ulteriori colonne. A esempio, se si specifica un indice non cluster, è possibile che in Query Optimizer vengano usate le colonne chiave dell'indice cluster oltre alle colonne specificate.  
  
È possibile specificare l'hint FORCESEEK nei modi riportati di seguito.  
  
|Sintassi|Esempio|Descrizione|  
|------------|-------------|-----------------|  
|Senza un indice o un hint INDEX|`FROM dbo.MyTable WITH (FORCESEEK)`|In Query Optimizer vengono considerate solo le operazioni di ricerca nell'indice per accedere alla tabella o alla vista in un indice rilevante.|  
|In combinazione con un hint INDEX|`FROM dbo.MyTable WITH (FORCESEEK, INDEX (MyIndex))`|In Query Optimizer vengono considerate solo le operazioni di ricerca nell'indice per accedere alla tabella o alla vista nell'indice specificato.|  
|Con parametri mediante la specifica di un indice e colonne di indice|`FROM dbo.MyTable WITH (FORCESEEK (MyIndex (col1, col2, col3)))`|In Query Optimizer vengono considerate solo le operazioni di ricerca nell'indice per accedere alla tabella o alla vista nell'indice specificato, utilizzando almeno le colonne dell'indice specificate.|  
  
Quando si usa l'hint FORCESEEK (con o senza parametri di indice), è opportuno considerare le indicazioni generali riportate di seguito:  
-   L'hint può essere specificato come hint di tabella o hint per la query. Per altre informazioni sull'hint per la query, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
-   Per applicare FORCESEEK a una vista indicizzata, è necessario specificare anche l'hint NOEXPAND.  
-   L'hint può essere applicato al massimo una volta per ogni tabella o vista.  
-   Non è possibile specificare l'hint per un'origine dei dati remota. Se FORCESEEK viene specificato con un hint per l'indice, viene restituito l'errore 7377. L'errore 8180 viene invece restituito se FORCESEEK viene usato senza un hint per l'indice.  
-   Se con FORCESEEK non viene trovato alcun piano, viene restituito l'errore 8622.  
  
Quando si specifica FORCESEEK con parametri di indice, è opportuno considerare le indicazioni generali e le restrizioni riportate di seguito:  
-   Non è possibile specificare l'hint per una tabella che rappresenta la destinazione di un'istruzione INSERT, UPDATE o DELETE.  
-   Non è possibile specificare questo hint in combinazione con un hint INDEX o un altro hint FORCESEEK.  
-   È necessario specificare almeno una colonna, la quale deve corrispondere alla colonna chiave iniziale.  
-   Sebbene sia possibile specificare ulteriori colonne di indice, non è possibile ignorare le colonne chiave. Ad esempio, se l'indice specificato contiene le colonne chiave `a`, `b` e `c`, la sintassi valida include `FORCESEEK (MyIndex (a))` e `FORCESEEK (MyIndex (a, b)`, mentre la sintassi non valida include `FORCESEEK (MyIndex (c))` e `FORCESEEK (MyIndex (a, c)`.  
-   L'ordine dei nomi di colonna specificato nell'hint deve corrispondere a quello delle colonne nell'indice a cui viene fatto riferimento.  
-   Non è possibile specificare le colonne non incluse nella definizione della chiave dell'indice. In un indice non cluster è ad esempio possibile specificare solo le colonne chiave dell'indice definite. Le colonne chiave cluster incluse automaticamente nell'indice non possono essere specificate, sebbene possano essere usate da Query Optimizer.  
-   Un indice columnstore con ottimizzazione per la memoria xVelocity non può essere specificato come parametro di indice. Viene restituito l'errore 366.  
-   La modifica della definizione dell'indice, ad esempio l'aggiunta o la rimozione di colonne, può richiedere la modifica delle query che fanno riferimento a tale indice.  
-   L'hint impedisce a Query Optimizer di prendere in considerazione gli eventuali indici spaziali o XML nella tabella.  
-   Non è possibile specificare l'hint in combinazione con l'hint FORCESCAN.  
-   Per gli indici partizionati, la colonna di partizionamento aggiunta in modo implicito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può essere specificata nell'hint FORCESEEK.  
  
> [!CAUTION]  
> Se si specifica FORCESEEK con parametri, il numero di piani che possono essere considerati da Query Optimizer viene limitato più di quanto avvenga se si specifica FORCESEEK senza parametri. Ciò potrebbe causare un errore `Plan cannot be generated` in più casi. In una versione futura le modifiche interne a Query Optimizer potrebbero consentire di prendere in considerazione più piani.  
  
FORCESCAN **Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1 e versioni successive.
Specifica che Query Optimizer deve usare solo un'operazione di analisi dell'indice come percorso di accesso alla tabella o alla vista a cui viene fatto riferimento. L'hint FORCESCAN può essere utile per le query in cui, a causa della valutazione errata (in difetto) del numero di righe interessate da parte di Query Optimizer, viene eseguita un'operazione di ricerca anziché di analisi. In questo caso, la quantità di memoria concessa per l'operazione è troppo ridotta, con conseguente calo delle prestazioni di query.  
  
È possibile specificare FORCESCAN con o senza un hint INDEX. Quando viene specificato in combinazione con un hint per l'indice (`INDEX = index_name, FORCESCAN`), in Query Optimizer vengono presi in considerazione solo i percorsi di accesso all'analisi nell'indice specificato per accedere alla tabella a cui viene fatto riferimento. FORCESCAN può essere specificato con l'hint per l'indice INDEX(0) per forzare un'operazione di analisi delle tabelle nella tabella di base.  
  
Per le tabelle e gli indici partizionati, FORCESCAN viene applicato dopo l'eliminazione delle partizioni tramite la valutazione del predicato della query. L'analisi viene pertanto applicata solo alle partizioni rimanenti, anziché all'intera tabella.  
  
Per l'hint FORCESCAN sono applicate le restrizioni indicate di seguito:  
-   Non è possibile specificare l'hint per una tabella che rappresenta la destinazione di un'istruzione INSERT, UPDATE o DELETE.  
-   Non è possibile usare l'hint con più hint per l'indice.  
-   L'hint impedisce a Query Optimizer di prendere in considerazione gli eventuali indici spaziali o XML nella tabella.  
-   Non è possibile specificare l'hint per un'origine dei dati remota.  
-   Non è possibile specificare l'hint in combinazione con l'hint FORCESEEK.  
  
HOLDLOCK  
Equivale a SERIALIZABLE. Per altre informazioni, vedere la sezione SERIALIZABLE di seguito in questo argomento. HOLDLOCK si applica solo alla tabella o alla vista per la quale è stato specificato e solo per la durata della transazione definita dall'istruzione in cui è usato. Non è possibile usare HOLDLOCK in un'istruzione SELECT che include l'opzione FOR BROWSE.  
  
IGNORE_CONSTRAINTS  
Applicabile solo in un'istruzione INSERT se viene usata l'opzione BULK con [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Specifica che qualsiasi vincolo sulla tabella verrà ignorato dall'operazione di importazione bulk. Per impostazione predefinita, INSERT controlla i [vincoli UNIQUE e CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) e i [vincoli di chiavi primarie ed esterne](../../relational-databases/tables/primary-and-foreign-key-constraints.md). Se si specifica IGNORE_CONSTRAINTS per un'operazione di importazione bulk, l'istruzione INSERT deve ignorare tali vincoli in una tabella di destinazione. Si noti che non è possibile disabilitare i vincoli UNIQUE, PRIMARY KEY o NOT NULL.  
  
Se i dati di input contengono righe che violano i vincoli, potrebbe essere necessario disabilitare i vincoli CHECK e FOREIGN KEY. Disabilitando i vincoli CHECK e FOREIGN KEY, è possibile importare i dati e quindi usare istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per pulire i dati.  
  
Quando, tuttavia, i vincoli CHECK e FOREIGN KEY vengono ignorati, ogni vincolo ignorato nella tabella viene contrassegnato come **is_not_trusted** nella vista del catalogo [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) o [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md) dopo l'operazione. A un certo punto sarà necessario controllare i vincoli nell'intera tabella. Se la tabella non era vuota prima dell'operazione di importazione bulk, l'impegno per la riconvalida dei vincoli può essere superiore all'impegno per l'applicazione dei vincoli CHECK e FOREIGN KEY ai dati incrementali.  
  
IGNORE_TRIGGERS  
Applicabile solo in un'istruzione INSERT se viene usata l'opzione BULK con [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Specifica che qualsiasi trigger definito sulla tabella verrà ignorato dall'operazione di importazione bulk. Per impostazione predefinita, l'istruzione INSERT applica i trigger.  
  
Usare l'argomento IGNORE_TRIGGERS solo se l'applicazione non dipende dai trigger e l'ottimizzazione delle prestazioni è un fattore importante.  
  
NOLOCK  
Equivale a READUNCOMMITTED. Per altre informazioni, vedere la sezione READUNCOMMITTED di seguito in questo argomento.  
  
> [!NOTE]  
> Per le istruzioni UPDATE e DELETE: [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
NOWAIT  
Imposta [!INCLUDE[ssDE](../../includes/ssde-md.md)] in modo che venga restituito un messaggio non appena viene rilevato un blocco nella tabella. NOWAIT equivale a specificare `SET LOCK_TIMEOUT 0` per una tabella specifica. L'hint NOWAIT non funziona quando viene incluso anche l'hint TABLOCK. Per terminare una query senza attendere il momento in cui usare l'hint TABLOCK, anteporre invece `SETLOCK_TIMEOUT 0;` alla query.  
  
PAGLOCK  
Acquisisce i blocchi di pagina dove in genere vengono acquisiti i singoli blocchi su righe o chiavi oppure dove in genere viene acquisito un singolo blocco di tabella. Per impostazione predefinita, viene usata la modalità di blocco appropriata per l'operazione specifica. Se viene specificato nelle transazioni operative al livello di isolamento SNAPSHOT, i blocchi di pagina vengono acquisiti solo se PAGLOCK è combinato con altri hint di tabella che richiedono i blocchi, ad esempio UPDLOCK e HOLDLOCK.  
  
READCOMMITTED  
Specifica la conformità delle operazioni di lettura con le regole relative al livello di isolamento READ COMMITTED mediante l'utilizzo dei blocchi o del controllo delle versioni delle righe. Se l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su OFF, [!INCLUDE[ssDE](../../includes/ssde-md.md)] acquisisce blocchi condivisi durante la lettura dei dati e rilascia tali blocchi al completamento dell'operazione di lettura. Se l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON, [!INCLUDE[ssDE](../../includes/ssde-md.md)] non acquisisce i blocchi e utilizza il controllo delle versioni delle righe. Per altre informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
> [!NOTE]  
> Per le istruzioni UPDATE e DELETE: [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
READCOMMITTEDLOCK  
Specifica la conformità delle operazioni di lettura con le regole relative al livello di isolamento READ COMMITTED mediante l'utilizzo dei blocchi. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] acquisisce i blocchi condivisi durante la lettura dei dati e li rilascia al completamento dell'operazione di lettura, indipendentemente dall'impostazione dell'opzione di database READ_COMMITTED_SNAPSHOT. Per altre informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md). Non è possibile specificare questo hint in una tabella di destinazione di un'istruzione INSERT. Viene restituito l'errore 4140.  
  
READPAST  
Specifica che le righe bloccate da altre transazioni non devono essere lette dal [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Se si specifica READPAST, vengono ignorati i blocchi a livello di riga ma non a livello di pagina. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] ignora pertanto le righe anziché bloccare la transazione corrente finché i blocchi non vengono rilasciati. Si supponga, ad esempio, che la tabella `T1` contenga una sola colonna integer con i valori 1, 2, 3, 4, 5. Se la transazione A modifica il valore da 3 a 8 senza eseguire il commit, l'istruzione SELECT * FROM T1 (READPAST) restituisce i valori 1, 2, 4, 5. READPAST è usato principalmente per ridurre la contesa tra blocchi durante l'implementazione di una coda processi che usa una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un agente di lettura coda che utilizza l'argomento READPAST ignora le voci della coda bloccate da altre transazioni e passa alla successiva voce disponibile senza attendere il rilascio dei blocchi da parte delle altre transazioni.  
  
È possibile specificare l'argomento READPAST per qualsiasi tabella a cui viene fatto riferimento in un'istruzione UPDATE o DELETE e per qualsiasi tabella a cui viene fatto riferimento in una clausola FROM. Se specificato in un'istruzione UPDATE, l'argomento READPAST viene applicato solo durante la lettura dei dati per l'identificazione dei record da aggiornare, indipendentemente dalla posizione in cui è stato specificato all'interno dell'istruzione. Non è possibile specificare l'argomento READPAST per le tabelle nella clausola INTO di un'istruzione INSERT. È possibile che le operazioni di aggiornamento o eliminazione che utilizzano l'argomento READPAST applichino blocchi durante la lettura delle chiavi esterne o delle viste indicizzate oppure durante la modifica degli indici secondari.  
  
L'argomento READPAST può essere specificato solo nelle transazioni operative ai livelli di isolamento READ COMMITTED o REPEATABLE READ. Se viene specificato nelle transazioni operative al livello di isolamento SNAPSHOT, è necessario combinare READPAST con altri hint di tabella che richiedono i blocchi, ad esempio UPDLOCK e HOLDLOCK.  
  
L'hint di tabella READPAST non può essere specificato quando l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON e una delle condizioni seguenti è vera:  
-   Il livello di isolamento delle transazioni della sessione è READ COMMITTED.  
-   L'hint di tabella READCOMMITTED viene specificato anche nella query.  
  
Per specificare l'hint READPAST in questi casi, rimuovere l'hint di tabella READCOMMITTED, se presente, e includere l'hint di tabella READCOMMITTEDLOCK nella query.  
  
READUNCOMMITTED  
Indica che le letture dirty sono consentite. Non viene acquisito alcun blocco condiviso per impedire alle altre transazioni di modificare i dati letti dalla transazione corrente. I blocchi esclusivi impostati dalle altre transazioni non impediscono alla transazione corrente di leggere i dati bloccati. Le letture dirty possono provocare un livello più alto di concorrenza a discapito della lettura delle modifiche dei dati per le quali le altre transazioni eseguiranno il rollback. Di conseguenza, è possibile che vengano generati errori per la transazione, che vengano forniti agli utenti dati di cui non è mai stato eseguito il commit oppure che gli utenti visualizzino i record due volte o che non li visualizzino affatto.  
  
Gli hint READUNCOMMITTED e NOLOCK sono applicabili soltanto a blocchi a livello di dati. Tutte le query, incluse quelle con hint READUNCOMMITTED e NOLOCK, acquisiscono blocchi di stabilità dello schema (Sch-S) durante la compilazione e l'esecuzione. Per questo motivo, le query vengono bloccate quando una transazione simultanea mantiene attivo un blocco di modifica dello schema sulla tabella. Ad esempio, un'operazione DDL (Data Definition Language) acquisisce un blocco Sch-M prima di modificare le informazioni dello schema della tabella. Tutte le query simultanee che tentano di acquisire un blocco Sch-S, incluse quelle in esecuzione con hint READUNCOMMITTED o NOLOCK, vengono bloccate. Una query con blocco Sch-S blocca invece le transazioni simultanee che tentano di acquisire un blocco Sch-M.  
  
Non è possibile specificare READUNCOMMITTED e NOLOCK per le tabelle modificate da operazioni di inserimento, aggiornamento o eliminazione. Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignora gli hint READUNCOMMITTED e NOLOCK della clausola FROM applicata alla tabella di destinazione di un'istruzione UPDATE o DELETE.  
  
> [!NOTE]  
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà rimosso il supporto per l'utilizzo degli hint READUNCOMMITTED e NOLOCK nella clausola FROM per la tabella di destinazione di un'istruzione UPDATE o DELETE. Evitare di usare questi hint in questo contesto nei nuovi progetti di sviluppo e pianificare la modifica delle applicazioni in cui sono attualmente usati.  
  
È possibile ridurre al minimo la contesa tra blocchi proteggendo nello stesso tempo le transazioni da letture dirty di modifiche dei dati di cui non è stato eseguito il commit utilizzando una delle impostazioni seguenti:  
-   Livello di isolamento READ COMMITTED con l'opzione di database READ_COMMITTED_SNAPSHOT impostata su ON.  
-   Livello di isolamento SNAPSHOT.  
  
Per altre informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
> [!NOTE]  
> Se viene visualizzato il [messaggio di errore 601](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999) quando è specificata l'opzione READUNCOMMITTED, risolverlo come un errore di deadlock ([messaggio di errore 1205](../../relational-databases/errors-events/mssqlserver-1205-database-engine-error.md)) e quindi provare a rieseguire l'istruzione.  
  
REPEATABLEREAD  
Specifica che viene eseguita un'analisi con la stessa semantica di blocco di una transazione eseguita con un livello di isolamento REPEATABLE READ. Per altre informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
ROWLOCK  
Specifica l'acquisizione dei blocchi di riga quando in genere vengono acquisiti i blocchi di pagina o i blocchi a livello di tabella. Se viene specificato nelle transazioni operative al livello di isolamento SNAPSHOT, i blocchi di riga vengono acquisiti solo se ROWLOCK è combinato con altri hint di tabella che richiedono i blocchi, ad esempio UPDLOCK e HOLDLOCK. Non è possibile usare ROWLOCK con una tabella con un indice columnstore cluster. L'esempio seguente restituisce l'[errore 651](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999) all'applicazione.  

```sql 
UPDATE [dbo].[FactResellerSalesXL_CCI] WITH (ROWLOCK)
SET UnitPrice = 50
WHERE ProductKey = 150;
```  
  
SERIALIZABLE  
Equivale a HOLDLOCK. Rende i blocchi condivisi più restrittivi mantenendoli attivi fino al completamento di una transazione anziché rilasciarli non appena la tabella o la pagina dei dati richiesta non è più necessaria, indipendentemente dal completamento della transazione. L'analisi viene eseguita con la stessa semantica di una transazione eseguita con il livello di isolamento SERIALIZABLE. Per altre informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
SNAPSHOT  
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive. 
  
Alla tabella ottimizzata per la memoria si accede con isolamento SNAPSHOT. SNAPSHOT può essere usato solo con tabelle ottimizzate per la memoria (e non con tabelle basate su disco), come nell'esempio seguente. Per altre informazioni, vedere [Introduzione alle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
```sql 
SELECT * 
FROM dbo.Customers AS c WITH (SNAPSHOT)   
LEFT JOIN dbo.[Order History] AS oh   
    ON c.customer_id=oh.customer_id;  
```  
  
SPATIAL_WINDOW_MAX_CELLS = *<integer\_value>*  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
Specifica il numero massimo di celle da usare per la suddivisione a mosaico di un oggetto geografico o di geometria. Il valore di *<integer\_value>* è compreso tra 1 e 8192.  
  
Questa opzione consente l'ottimizzazione dei tempi di esecuzione delle query raggiungendo un compromesso tra il tempo di esecuzione del filtro primario e secondario. Un numero maggiore riduce il tempo di esecuzione del filtro secondario, ma aumenta il tempo di esecuzione del filtro primario e un numero minore diminuisce tempo di esecuzione del filtro primario, ma aumenta l'esecuzione del filtro secondario. Per i dati spaziali più densi, un numero superiore dovrebbe dar luogo a un tempo di esecuzione più rapido fornendo un'approssimazione migliore del filtro primario e riducendo il tempo di esecuzione del filtro secondario. Per i dati di tipo sparse, un numero inferiore diminuirà il tempo di esecuzione del filtro primario.  
  
 Questa opzione funziona per sia per le suddivisioni a mosaico della griglia automatiche che per quelle manuali.  
  
TABLOCK  
Specifica che il blocco acquisito deve essere applicato a livello di tabella. Il tipo di blocco acquisito varia in base all'istruzione eseguita. Un'istruzione SELECT può ad esempio acquisire un blocco condiviso. Se si specifica TABLOCK, il blocco condiviso viene applicato all'intera tabella anziché a livello di riga o di pagina. Se si specifica anche HOLDLOCK, il blocco di tabella viene mantenuto attivo fino al termine della transazione.  
  
Quando si importano dati in un heap tramite l'istruzione `INSERT INTO <target_table> SELECT <columns> FROM <source_table>`, è possibile abilitare la registrazione minima e i blocchi ottimizzati per l'istruzione specificando l'hint TABLOCK per la tabella di destinazione. Il modello di recupero del database deve inoltre essere impostato sul modello con registrazione minima o con registrazione minima delle operazioni bulk. L'hint TABLOCK abilita anche gli inserimenti paralleli negli heap o negli indici columnstore cluster. Per altre informazioni, vedere [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
Se usato in combinazione con il provider di set di righe con lettura bulk [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) per importare dati in una tabella, TABLOCK consente a più client di caricare simultaneamente i dati nella tabella di destinazione ottimizzandone la registrazione e i blocchi. Per altre informazioni, vedere [Prerequisiti per la registrazione minima nell'importazione bulk](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
TABLOCKX  
Specifica l'acquisizione di un blocco esclusivo sulla tabella.  
  
UPDLOCK  
Specifica che devono essere acquisiti blocchi di aggiornamento, i quali dovranno essere mantenuti attivi fino al completamento della transazione. UPDLOCK acquisisce i blocchi di aggiornamento per le operazioni di lettura solo a livello di riga o di pagina. Se UPDLOCK viene usato insieme a TABLOCK oppure se viene acquisito un blocco a livello di tabella per altri motivi, viene acquisito un blocco esclusivo (X).  
  
Quando è specificato UPDLOCK, gli hint del livello di isolamento READCOMMITTED e READCOMMITTEDLOCK vengono ignorati. Se ad esempio il livello di isolamento della sessione è impostato su SERIALIZABLE e una query specifica (UPDLOCK, READCOMMITTED), l'hint READCOMMITTED viene ignorato e la transazione viene eseguita utilizzando il livello di isolamento SERIALIZABLE.  
  
XLOCK  
Specifica che devono essere acquisiti blocchi esclusivi, i quali dovranno essere mantenuti attivi fino al completamento della transazione. Se specificato in combinazione con ROWLOCK, PAGLOCK o TABLOCK, i blocchi esclusivi vengono applicati al livello di granularità appropriato.  
  
## <a name="remarks"></a>Osservazioni  
Gli hint di tabella vengono ignorati se il piano di query non accede alla tabella. Ciò si verifica se Query Optimizer non accede esplicitamente alla tabella oppure perché viene invece eseguito l'accesso a una vista indicizzata. In questo secondo caso è possibile impedire l'accesso a una vista indicizzata utilizzando l'hint per la query OPTION (EXPAND VIEWS).  
  
Tutti gli hint di blocco vengono propagati a tutte le tabelle e viste a cui accede il piano di query, incluse tabelle e viste di riferimento in una vista. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue inoltre le relative verifiche di consistenza dei blocchi.  
  
È possibile che gli hint di blocco ROWLOCK, UPDLOCK e AND XLOCK che acquisiscono blocchi a livello di riga applichino blocchi alle chiavi di indice anziché alle righe di dati effettive. Se, ad esempio, una tabella include un indice non cluster e un'istruzione SELECT che utilizza un hint di blocco viene gestita da un indice di copertura, viene acquisito un blocco sulla chiave dell'indice di copertura anziché sulla riga di dati nella tabella di base.  
  
Se una tabella include colonne calcolate in base a espressioni o funzioni che accedono a colonne in altre tabelle, gli hint di tabella non verranno usati su tali tabelle e non verranno propagati. Ad esempio, un hint di tabella NOLOCK è specificato in una tabella della query che include colonne calcolate tramite una combinazione di espressioni e funzioni che accedono alle colonne di un'altra tabella. Le tabelle a cui le espressioni e le funzioni fanno riferimento non utilizzano l'hint di tabella NOLOCK durante l'accesso.  
  
Per ogni tabella nella clausola FROM in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è consentito impostare più di un hint di tabella appartenente a ognuno dei gruppi riportati di seguito.  
-   Hint di granularità: PAGLOCK, NOLOCK, READCOMMITTEDLOCK, ROWLOCK, TABLOCK o TABLOCKX.  
-   Hint del livello di isolamento: HOLDLOCK, NOLOCK, READCOMMITTED, REPEATABLEREAD, SERIALIZABLE.  
  
## <a name="filtered-index-hints"></a>Hint per l'indice filtrato  
 È possibile usare un hint per l'indice filtrato come hint di tabella; se tuttavia tale indice non include tutte le righe selezionate dalla query, in Query Optimizer viene generato l'errore 8622. Di seguito viene fornito un esempio di hint per l'indice filtrato non valido. Nell'esempio viene creato l'indice filtrato `FIBillOfMaterialsWithComponentID`, che viene quindi usato come hint per l'indice per un'istruzione SELECT. Il predicato dell'indice filtrato include righe di dati per gli elementi ComponentID 533, 324 e 753. Anche il predicato della query include righe di dati per gli elementi ComponentID 533, 324 e 753, ma estende il set di risultati in modo da includere gli elementi ComponentID 855 e 924, non inclusi nell'indice filtrato. In Query Optimizer l'hint per l'indice filtrato non può pertanto essere usato e viene generato l'errore 8622. Per altre informazioni, vedere [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithComponentID'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithComponentID  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithComponentID"  
    ON Production.BillOfMaterials (ComponentID, StartDate, EndDate)  
    WHERE ComponentID IN (533, 324, 753);  
GO  
SELECT StartDate, ComponentID FROM Production.BillOfMaterials  
    WITH( INDEX (FIBillOfMaterialsWithComponentID) )  
    WHERE ComponentID in (533, 324, 753, 855, 924);  
GO  
```  
  
In Query Optimizer non viene considerato alcun hint per l'indice se le opzioni SET non includono i valori necessari per gli indici filtrati. Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="using-noexpand"></a>Utilizzo di NOEXPAND  
L'hint NOEXPAND è applicabile solo alle *viste indicizzate*, ovvero alle viste in cui è stato creato un indice cluster univoco. In Query Optimizer viene usato l'indice nella vista se in una query sono inclusi riferimenti a colonne disponibili sia in una vista indicizzata sia nelle tabelle di base e viene determinato che l'utilizzo della vista indicizzata rappresenta il metodo migliore per l'esecuzione della query. Questa funzionalità viene chiamata *corrispondenza della vista indicizzata*. Prima di [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1, l'uso automatico di una vista indicizzata in Query Optimizer è supportato solo in edizioni specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md), [Funzionalità supportate dalle edizioni di SQL Server 2017](../../SQL-server/editions-and-components-of-SQL-server-2017.md) e [Funzionalità supportate dalle edizioni di SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Tuttavia, affinché Query Optimizer usi le viste indicizzate oppure una vista indicizzata a cui viene fatto riferimento tramite l'hint NOEXPAND per l'esecuzione della corrispondenza, le opzioni SET seguenti devono essere impostate su ON.  

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta l'uso automatico di viste indicizzate senza specificare l'hint NOEXPAND.

- ANSI_NULLS
- ANSI_PADDING
- ANSI_WARNINGS
- ARITHABORT<sup>1</sup>
- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER

<sup>1</sup> ARITHABORT viene impostata in modo implicito su ON se ANSI_WARNINGS è impostata su ON. Non sarà pertanto necessario modificare manualmente questa impostazione.  

L'opzione NUMERIC_ROUNDABORT deve invece essere impostata su OFF.  
  
 Per forzare l'uso di un indice per una vista indicizzata in Query Optimizer, specificare l'opzione NOEXPAND. Questo hint può essere usato solo se la vista è specificata anche nella query. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è disponibile un hint per forzare l'uso di una vista indicizzata specifica in una query in cui tale vista non viene specificata direttamente nella clausola FROM. In Query Optimizer viene tuttavia valutata la possibilità di usare le viste indicizzate, anche se non sono presenti riferimenti diretti a tali viste all'interno della query. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] creerà automaticamente solo le statistiche relative a una vista indicizzata quando si usa un hint di tabella NOEXPAND. L'omissione di questo hint può causare la generazione di avvisi del piano di esecuzione relativi alla perdita di statistiche che non possono essere risolti creando manualmente le statistiche. Durante l'ottimizzazione della query [!INCLUDE[ssde_md](../../includes/ssde_md.md)] userà le statistiche della vista create automaticamente o manualmente quando la query fa riferimento alla vista direttamente e viene usato l'hint NOEXPAND.    
  
## <a name="using-a-table-hint-as-a-query-hint"></a>Utilizzo di un hint di tabella come hint per la query  
 Gli *hint di tabella* possono anche essere specificati come hint per la query tramite la clausola OPTION (TABLE HINT). È consigliabile usare un hint di tabella come hint per la query solo nel contesto di una [guida di piano](../../relational-databases/performance/plan-guides.md). Per le query ad hoc, specificare questi hint solo come hint di tabella. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Gli hint KEEPIDENTITY, IGNORE_CONSTRAINTS e IGNORE_TRIGGERS richiedono l'autorizzazione `ALTER` sulla tabella.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-the-tablock-hint-to-specify-a-locking-method"></a>R. Utilizzo dell'hint TABLOCK per specificare un metodo di blocco  
 Nell'esempio seguente viene specificata l'acquisizione di un blocco condiviso sulla tabella `Production.Product` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] che viene mantenuto fino alla fine dell'istruzione UPDATE.  
  
```sql  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
### <a name="b-using-the-forceseek-hint-to-specify-an-index-seek-operation"></a>B. Utilizzo dell'hint FORCESEEK per specificare un'operazione di ricerca nell'indice  
 Nell'esempio seguente viene utilizzato l'hint FORCESEEK senza specificare un indice per forzare l'esecuzione di un'operazione Index Seek da parte di Query Optimizer nella tabella `Sales.SalesOrderDetail` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT *  
FROM Sales.SalesOrderHeader AS h  
INNER JOIN Sales.SalesOrderDetail AS d WITH (FORCESEEK)  
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
GO  
  
```  
  
 Nell'esempio seguente l'hint FORCESEEK viene usato con un indice affinché in Query Optimizer venga eseguita un'operazione di ricerca nell'indice e nella colonna di indice specificati.  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESEEK (PK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID (SalesOrderID)))   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);   
GO  
  
```  
  
### <a name="c-using-the-forcescan-hint-to-specify-an-index-scan-operation"></a>C. Utilizzo dell'hint FORCESCAN per specificare un'operazione di analisi dell'indice  
 Nell'esempio seguente viene utilizzato l'hint FORCESCAN per forzare l'esecuzione di un'operazione di analisi da parte di Query Optimizer nella tabella `Sales.SalesOrderDetail` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESCAN)   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Hint &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
 [Hint di query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)  
  
