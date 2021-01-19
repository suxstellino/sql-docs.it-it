---
description: CREATE VIEW (Transact-SQL)
title: CREATE VIEW (Transact-SQL)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 781dc13180d75e771402822259ad5ef6a913ac28
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170313"
---
# <a name="create-view-transact-sql"></a>CREATE VIEW (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea una tabella virtuale il cui contenuto (colonne e righe) è definito da una query. Utilizzare questa istruzione per creare una vista dei dati in una o più tabelle nel database. Ad esempio, è possibile utilizzare una vista per gli scopi seguenti:  
  
-   Per analizzare, semplificare e personalizzare la visualizzazione del database per ogni utente.  
  
-   Come meccanismo di sicurezza grazie al quale è possibile consentire agli utenti di accedere ai dati tramite una vista, senza concedere loro le autorizzazioni di accesso alle tabelle di base sottostanti.  
  
-   Per fornire un'interfaccia compatibile con le versioni precedenti con cui emulare una tabella il cui schema è stato modificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE [ OR ALTER ] VIEW [ schema_name . ] view_name [ (column [ ,...n ] ) ]
[ WITH <view_attribute> [ ,...n ] ]
AS select_statement
[ WITH CHECK OPTION ]
[ ; ]  
  
<view_attribute> ::=
{  
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]
}
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

OR ALTER  
 **Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1).   
  
 Modifica in modo condizionale la vista solo se esiste già. 
 
 *schema_name*  
 Nome dello schema a cui appartiene la vista.  
  
 *view_name*  
 Nome della vista. I nomi di vista devono essere conformi alle regole per gli identificatori. Il nome del proprietario della vista è facoltativo.  
  
 *column*  
 Nome da assegnare a una colonna della vista. Il nome della colonna è necessario solo quando una colonna deriva da un'espressione aritmetica, una funzione o una costante, quando due o più colonne potrebbero altrimenti avere lo stesso nome (in genere a causa di un join) oppure quando a una colonna di una vista viene assegnato un nome diverso rispetto alla colonna da cui deriva. È inoltre possibile assegnare nomi di colonna nell'istruzione SELECT.  
  
 Se *column* viene omesso, le colonne della vista acquisiscono gli stessi nomi delle colonne dell'istruzione SELECT.  
  
> [!NOTE]  
>  Nelle colonne di una vista, le autorizzazioni assegnate per un nome di colonna rimangono valide tra istruzioni CREATE VIEW e ALTER VIEW, indipendentemente dall'origine dei dati sottostanti. Se, ad esempio, vengono concesse autorizzazioni per la colonna **SalesOrderID** in un'istruzione CREATE VIEW, un'istruzione ALTER VIEW può assegnare alla colonna **SalesOrderID** un nome diverso, ad esempio **OrderRef**, e mantenere le autorizzazioni associate alla vista che usa **SalesOrderID**.  
  
 AS  
 Specifica le operazioni che la vista deve eseguire.  
  
 *select_statement*  
 Istruzione SELECT che definisce la vista. Nell'istruzione è possibile specificare più di una tabella e altre viste. Per selezionare gli oggetti a cui viene fatto riferimento nella clausola SELECT della vista creata è necessario disporre di autorizzazioni appropriate.  
  
 Non è necessario che una vista sia un semplice subset delle righe e delle colonne di una tabella specifica. È possibile creare una vista che utilizza più tabelle o altre viste tramite una clausola SELECT del grado di complessità desiderato.  
  
 In una definizione di vista indicizzata l'istruzione SELECT deve essere riferita a una sola tabella oppure includere un JOIN tra più tabelle con aggregazioni facoltative.  
  
 Le clausole SELECT utilizzate in una definizione di vista non possono includere gli elementi seguenti:  
  
-   Una clausola ORDER BY, a meno che nell'elenco di selezione dell'istruzione SELECT non sia presente anche una clausola TOP  
  
    > [!IMPORTANT]  
    >  La clausola ORDER BY viene utilizzata esclusivamente per determinare le righe restituite dalla clausola TOP oppure OFFSET nella definizione della vista. La clausola ORDER BY non garantisce risultati ordinati in caso di query sulla vista, a meno che tale clausola non venga specificata anche nella query.  
  
-   Parola chiave INTO  
  
-   Clausola OPTION  
  
-   Un riferimento a una tabella temporanea o a una variabile di tabella  
  
 Poiché *select_statement* usa l'istruzione SELECT, è possibile usare gli hint \<join_hint> e \<table_hint> come specificato nella clausola FROM. Per altre informazioni, vedere [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md) e [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md). 
  
 In *select_statement* è possibile usare funzioni e più istruzioni SELECT separate dall'operatore UNION o UNION ALL.  
  
 CHECK OPTION  
 Forza il rispetto del set di criteri specificato in *select_statement* per tutte le istruzioni di modifica dei dati eseguite sulla vista. Quando una riga viene modificata tramite una vista, la clausola WITH CHECK OPTION assicura che i dati rimangano visibili nella vista dopo il commit della modifica.  
  
> [!NOTE]
>  CHECK OPTION si applica solo agli aggiornamenti eseguiti tramite la vista. Non è applicabile agli aggiornamenti eseguiti direttamente nelle tabelle sottostanti di una vista.  
  
 ENCRYPTION  
 **SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Esegue la crittografia delle voci della tabella [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) contenenti il testo dell'istruzione CREATE VIEW. Se si utilizza WITH ENCRYPTION, la vista non viene pubblicata nell'ambito della replica di SQL Server.  
  
 SCHEMABINDING  
 Associa la vista allo schema della tabella o delle tabelle sottostanti. Quando la clausola SCHEMABINDING viene specificata, non è possibile apportare alla tabella o alle tabelle di base modifiche che hanno effetto sulla definizione della vista. È necessario prima modificare o eliminare la definizione della vista per rimuovere le dipendenze dalla tabella da modificare. Quando si usa SCHEMABINDING, l'argomento *select_statement* deve includere i nomi composti da due parti (_schema_ **.** _object_) delle tabelle, delle viste o delle funzioni definite dall'utente a cui viene fatto riferimento. Tutti gli oggetti a cui viene fatto riferimento devono essere presenti nello stesso database.  
  
 Le viste o le tabelle che fanno parte di una vista creata con la clausola SCHEMABINDING non possono essere eliminate, a meno che la vista non venga eliminata o modificata in modo che non sia più associata a uno schema. In caso contrario, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene generato un errore. Inoltre, l'esecuzione di istruzioni ALTER TABLE su tabelle che fanno parte di viste associate a schema ha esito negativo se tali istruzioni modificano la definizione della vista.  
  
 VIEW_METADATA  
 Specifica che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dovrà restituire alle API DB-Library, ODBC e OLE DB le informazioni sui metadati relativi alla vista anziché alla tabella o alle tabelle di base quando vengono richiesti metadati in modalità browse per una query che fa riferimento alla vista. I metadati in modalità browse sono metadati aggiuntivi che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce a queste API sul lato client. Questi metadati consentono alle API sul lato client di implementare cursori sul lato client aggiornabili. I metadati in modalità browse includono informazioni sulla tabella di base a cui appartengono le colonne del set di risultati.  
  
 Per le viste create con VIEW_METADATA, i metadati in modalità browse restituiscono il nome della vista anziché i nomi delle tabelle di base nella descrizione delle colonne della vista nel set di risultati.  
  
 Quando si crea una vista con la clausola WITH VIEW_METADATA, tutte le relative colonne, escluse quelle di tipo **timestamp**, sono aggiornabili se la vista include trigger INSTEAD OF INSERT o INSTEAD OF UPDATE. Per ulteriori informazioni sulle viste aggiornabili, vedere la sezione Osservazioni.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile creare una vista solo nel database corrente. CREATE VIEW deve essere la prima istruzione di un batch di query. Una vista può includere al massimo 1.024 colonne.  
  
 Quando si esegue una query tramite una vista, [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica che tutti gli oggetti di database a cui viene fatto riferimento nell'istruzione esistano e siano validi nel contesto dell'istruzione e che le istruzioni di modifica dei dati non violino le regole di integrità dei dati. Se la verifica ha esito negativo, viene visualizzato un messaggio di errore. In caso contrario, l'azione viene convertita automaticamente in un'operazione sulla tabella o sulle tabelle sottostanti.  
  
 Se si tenta di utilizzare una vista che dipende da una tabella o una vista eliminata, [!INCLUDE[ssDE](../../includes/ssde-md.md)] visualizza un messaggio di errore. Se viene creata una nuova tabella o vista per sostituire quella eliminata e la struttura della nuova tabella non è diversa rispetto alla tabella di base precedente, la vista risulta di nuovo utilizzabile. Se la struttura della nuova tabella o vista è diversa, la vista deve essere eliminata e ricreata.  
  
 Se una vista non viene creata con la clausola SCHEMABINDING, eseguire [sp_refreshview](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md) quando vengono apportate modifiche agli oggetti sottostanti la vista che influiscono sulla definizione di tale vista. In caso contrario, le query sulla vista possono generare risultati imprevisti.  
  
 Quando si crea una vista, le relative informazioni vengono archiviate nelle viste del catalogo seguenti: [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md), [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) e [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md). Il testo dell'istruzione CREATE VIEW viene archiviato nella vista del catalogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Una query che usa un indice su una vista definita tramite espressioni **numeric** o **float** può generare un risultato diverso rispetto a una query simile che non usa l'indice sulla vista. Tale differenza può essere dovuta a errori di arrotondamento generati durante l'esecuzione di operazioni INSERT, DELETE o UPDATE sulle tabelle sottostanti.  
  
 In [!INCLUDE[ssDE](../../includes/ssde-md.md)] le impostazioni delle opzioni SET QUOTED_IDENTIFIER e SET ANSI_NULLS vengono salvate in fase di creazione di una vista. Queste impostazioni originali vengono utilizzate per analizzare la vista quando viene utilizzata. Pertanto, tutte le impostazioni di sessione del client per SET QUOTED_IDENTIFIER e SET ANSI_NULLS non hanno effetto sulla definizione della vista durante l'accesso alla vista.  
  
## <a name="updatable-views"></a>Viste aggiornabili  
 È possibile modificare i dati di una tabella di base sottostante tramite una vista solo se vengono soddisfatti i requisiti seguenti:  
  
-   Tutte le modifiche, incluse le istruzioni UPDATE, INSERT e DELETE, devono fare riferimento a colonne di una sola tabella di base.  
  
-   Le colonne modificate nella vista devono fare riferimento direttamente ai dati sottostanti nelle colonne della tabella. Le colonne non possono essere derivate in altro modo, ad esempio tramite:  
  
    -   Una funzione di aggregazione: AVG, COUNT, SUM, MIN, MAX, GROUPING, STDEV, STDEVP, VAR e VARP.  
  
    -   Un calcolo. La colonna non può essere calcolata tramite un'espressione che utilizza altre colonne. Le colonne create mediante gli operatori sui set UNION, UNION ALL, CROSSJOIN, EXCEPT e INTERSECT sono considerate un calcolo e non sono aggiornabili.  
  
-   Le colonne da modificare non sono interessate da clausole GROUP BY, HAVING o DISTINCT.  
  
-   La clausola TOP non può essere usata in tutte le posizioni dell'argomento *select_statement* della vista in combinazione con la clausola WITH CHECK OPTION.  
  
 Le restrizioni sopra indicate sono valide sia per qualsiasi sottoquery nella clausola FROM della vista sia per la vista stessa. In genere, [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve essere in grado di tracciare senza ambiguità le modifiche dalla definizione della vista a una tabella di base. Per altre informazioni, vedere [Modificare i dati tramite una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
 Se le restrizioni sopra indicate non consentono di modificare i dati direttamente tramite una vista, è possibile ricorrere alle soluzioni seguenti:  
  
-   **Trigger INSTEAD OF**  
  
     È possibile creare trigger INSTEAD OF per una vista in modo da renderla aggiornabile. Questo trigger viene eseguito in sostituzione dell'istruzione di modifica dei dati in cui è definito e consente all'utente di specificare il set di azioni che devono essere eseguite per elaborare l'istruzione di modifica dei dati. Pertanto, se è disponibile un trigger INSTEAD OF per una vista in un'istruzione di modifica dei dati specifica (INSERT, UPDATE o DELETE), la vista corrispondente risulta aggiornabile tramite l'istruzione. Per altre informazioni sui trigger INSTEAD OF, vedere [Trigger DML](../../relational-databases/triggers/dml-triggers.md).  
  
-   **Viste partizionate**  
  
     Se la vista è partizionata, è aggiornabile ma con particolari restrizioni. Quando necessario, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] effettua una distinzione tra viste partizionate locali, ovvero le viste nelle quali tutte le tabelle coinvolte e la vista stessa si trovano nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], e viste partizionate distribuite, ovvero le viste nelle quali almeno una tabella della vista risiede in un server diverso o remoto.  
  
## <a name="partitioned-views"></a>Viste partizionate  
 Una vista partizionata è una vista definita tramite un'istruzione UNION ALL eseguita su tabelle membro con la stessa struttura, ma archiviate separatamente sotto forma di più tabelle nello stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure in un gruppo di istanze autonome di server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominato federazione di server di database.  
  
> [!NOTE]  
>  Il metodo consigliato per il partizionamento locale dei dati in un server prevede l'utilizzo di tabelle partizionate. Per ulteriori informazioni, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 Durante la progettazione di uno schema di partizione, è necessario individuare con chiarezza i dati appartenenti a ogni partizione. Ad esempio, i dati della tabella `Customers` vengono distribuiti in tre tabelle membro su tre server: `Customers_33` su `Server1`, `Customers_66` su `Server2` e `Customers_99` su `Server3`.  
  
 Una vista partizionata in `Server1` viene definita nel modo seguente:  
  
```sql
--Partitioned view as defined on Server1  
CREATE VIEW Customers  
AS  
--Select from local member table.  
SELECT *  
FROM CompanyData.dbo.Customers_33  
UNION ALL  
--Select from member table on Server2.  
SELECT *  
FROM Server2.CompanyData.dbo.Customers_66  
UNION ALL  
--Select from member table on Server3.  
SELECT *  
FROM Server3.CompanyData.dbo.Customers_99;  
```  
  
 In generale, una vista viene definita partizionata se utilizza il formato seguente:  
  
```syntaxsql
SELECT <select_list1>  
FROM T1  
UNION ALL  
SELECT <select_list2>  
FROM T2  
UNION ALL  
...  
SELECT <select_listn>  
FROM Tn;  
```  
  
## <a name="conditions-for-creating-partitioned-views"></a>Requisiti per la creazione di viste partizionate  
  
1.  Elenchi di selezione (select `list`)  
  
    -   Nell'elenco di colonne della definizione della vista, selezionare tutte le colonne nelle tabelle membro.  
  
    -   Assicurarsi che le colonne nella stessa posizione ordinale in ogni oggetto `select list` siano dello stesso tipo, incluse le regole di confronto. Non è sufficiente che il tipo di dati delle colonne supporti la conversione implicita, come nel caso di operazioni UNION.  
  
         Inoltre, è necessario che almeno una colonna, ad esempio `<col>`, sia presente in tutti gli elenchi di selezione nella stessa posizione ordinale. Definire `<col>` in modo che le tabelle membro `T1, ..., Tn` abbiano, rispettivamente, vincoli CHECK `C1, ..., Cn` definiti in `<col>`.  
  
         Il vincolo `C1` definito nella tabella `T1` deve avere il formato seguente:  
  
        ```syntaxsql
        C1 ::= < simple_interval > [ OR < simple_interval > OR ...]  
        < simple_interval > :: =   
        < col > { < | > | \<= | >= | = < value >}   
        | < col > BETWEEN < value1 > AND < value2 >  
        | < col > IN ( value_list )  
        | < col > { > | >= } < value1 > AND  
        < col > { < | <= } < value2 >  
        ```
  
    -   I vincoli devono essere tali per cui qualsiasi valore di `<col>` specificato possa soddisfare al massimo uno dei vincoli `C1, ..., Cn`, in modo che i vincoli formino un set di intervalli disgiunti o non sovrapposti. La colonna `<col>` in cui sono definiti i vincoli disgiunti è denominata colonna di partizionamento. Si noti che nelle tabelle sottostanti tale colonna può avere nomi diversi. Per soddisfare le condizioni precedenti della colonna di partizionamento, i vincoli devono essere abilitati e attendibili. Se i vincoli sono disabilitati, è necessario riabilitare il controllo dei vincoli tramite l'opzione CHECK CONSTRAINT *constraint_name* dell'istruzione ALTER TABLE e convalidarli tramite l'opzione WITH CHECK.  
  
         Negli esempi seguenti vengono illustrati alcuni set di vincoli validi:  
  
        ```syntaxsql
        { [col < 10], [col between 11 and 20] , [col > 20] }  
        { [col between 11 and 20], [col between 21 and 30], [col between 31 and 100] }  
        ```  
  
    -   Nell'elenco di selezione non è consentito utilizzare più volte la stessa colonna.  
  
2.  Colonna di partizionamento  
  
    -   Deve fare parte della colonna PRIMARY KEY della tabella.  
  
    -   Non può essere una colonna calcolata, Identity, predefinita o **timestamp**.  
  
    -   Se una colonna di una tabella membro ha più vincoli, vengono ignorati tutti i vincoli, anche quando è necessario determinare se la vista è partizionata. Per soddisfare le condizioni della vista partizionata, assicurarsi che ci sia un solo vincolo di partizionamento nella colonna di partizionamento.  
  
    -   Non sono previste restrizioni per quanto concerne l'aggiornabilità della colonna di partizionamento.  
  
3.  Tabelle membro o tabelle sottostanti `T1, ..., Tn`  
  
    -   Possono essere tabelle locali o tabelle presenti in altri computer che eseguono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene fatto riferimento tramite un nome composto da quattro parti oppure un nome basato su OPENDATASOURCE o OPENROWSET. La sintassi di OPENDATASOURCE e OPENROWSET consente di specificare un nome di tabella, ma non una query pass-through. Per altre informazioni, vedere [OPENDATASOURCE &#40;Transact-SQL&#41; ](../../t-sql/functions/opendatasource-transact-sql.md) e [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
         Se una o più tabelle membro sono remote, la vista viene definita vista partizionata distribuita ed è necessario rispettare alcuni ulteriori requisiti descritti di seguito in questa sezione.  
  
    -   Non è possibile inserire una stessa tabella due volte nel set di tabelle combinate tramite l'istruzione UNION ALL.  
  
    -   Le tabelle membro non possono includere indici creati su colonne calcolate della tabella.  
  
    -   Le tabelle membro hanno i vincoli PRIMARY KEY nello stesso numero di colonne.  
  
    -   Tutte le tabelle membro della vista hanno la stessa impostazione di riempimento ANSI. Questa impostazione può essere configurata tramite l'opzione **user options** della stored procedure **sp_configure** oppure tramite l'istruzione SET.  
  
## <a name="conditions-for-modifying-data-in-partitioned-views"></a>Requisiti per la modifica dei dati in viste partizionate  
 Le istruzioni che modificano i dati in viste partizionate sono soggette alle restrizioni seguenti:  
  
-   L'istruzione INSERT fornisce valori per tutte le colonne della vista, anche se le tabelle membro sottostanti hanno un vincolo DEFAULT per tali colonne o ammettono valori Null. Per le colonne delle tabelle membro che includono definizioni DEFAULT, nelle istruzioni non è consentito specificare la parola chiave DEFAULT in modo esplicito.  
  
-   Il valore inserito nella colonna di partizionamento soddisfa almeno uno dei vincoli sottostanti. In caso contrario, l'operazione di inserimento provoca una violazione di vincolo e ha esito negativo.  
  
-   Nelle istruzioni UPDATE non è possibile specificare la parola chiave DEFAULT come valore nella clausola SET, anche se alla colonna è associato un valore DEFAULT definito nella tabella membro corrispondente.  
  
-   Le colonne Identity della vista incluse in una o più tabelle membro non possono essere modificate tramite un'istruzione INSERT o UPDATE.  
  
-   Se una delle tabelle membro include una colonna di tipo **timestamp**, non è possibile modificare i dati tramite un'istruzione INSERT o UPDATE.  
  
-   Se una delle tabelle membro include un trigger oppure un vincolo ON UPDATE CASCADE/SET NULL/SET DEFAULT o ON DELETE CASCADE/SET NULL/SET DEFAULT, la vista non può essere modificata.  
  
-   Le operazioni INSERT, UPDATE e DELETE eseguite su una vista partizionata non sono consentite se includono un self join con tale vista o con una delle tabelle membro.  
  
-   L'importazione in blocco di dati in una vista partizionata non è supportata da **bcp** o dalle istruzioni BULK INSERT e INSERT... SELECT * FROM OPENROWSET(BULK...). È tuttavia possibile inserire più righe in una vista partizionata usando l'istruzione [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
    > [!NOTE]  
    >  Per aggiornare una vista partizionata, l'utente deve disporre delle autorizzazioni INSERT, UPDATE e DELETE per le tabelle membro.  
  
## <a name="additional-conditions-for-distributed-partitioned-views"></a>Requisiti aggiuntivi per le viste partizionate distribuite  
 Per le viste partizionate distribuite, ovvero le viste nelle quali una o più tabelle membro sono remote, devono essere soddisfatti i requisiti aggiuntivi seguenti:  
  
-   Per garantire l'atomicità in tutti i nodi interessati dall'aggiornamento, viene avviata una transazione distribuita.  
  
-   Per consentire il corretto funzionamento delle istruzioni INSERT, UPDATE o DELETE, impostare l'opzione XACT_ABORT SET su ON.  
  
-   Per tutte le colonne delle tabelle remote di tipo **smallmoney** a cui viene fatto riferimento in una vista partizionata viene eseguito il mapping come **money**. Pertanto, anche le colonne corrispondenti delle tabelle locali, ovvero le colonne che occupano la stessa posizione ordinale nell'elenco di selezione, devono essere di tipo **money**.  
  
-   Con il livello di compatibilità del database 110 o superiore, viene eseguito il mapping come **smalldatetime** di tutte le colonne delle tabelle remote di tipo **smalldatetime** a cui viene fatto riferimento in una vista partizionata. Le colonne corrispondenti delle tabelle locali, ovvero le colonne che occupano la stessa posizione ordinale nell'elenco di selezione, devono essere di tipo **smalldatetime**. Questo comportamento differisce dalle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene eseguito il mapping come **datetime** di tutte le colonne delle tabelle remote di tipo **smalldatetime** a cui viene fatto riferimento in una vista partizionata e le colonne corrispondenti delle tabelle locali devono essere di tipo **datetime**. Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
-   Qualsiasi server collegato nella vista partizionata non può essere di tipo loopback, ovvero non deve puntare alla stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 L'impostazione dell'opzione SET ROWCOUNT viene ignorata per le operazioni INSERT, UPDATE e DELETE che coinvolgono viste partizionate aggiornabili e tabelle remote.  
  
 Quando sono disponibili le tabelle membro e la definizione della vista partizionata, nell'utilità Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono compilati automaticamente piani appropriati in cui le query vengono utilizzate in modo efficiente per l'accesso ai dati di tabelle membro. Con le definizioni di vincoli CHECK, in Query Processor viene eseguito il mapping della distribuzione dei valori chiave nelle tabelle membro. Quando un utente inoltra una query, viene eseguito un confronto tra il mapping e i valori specificati nella clausola WHERE, quindi viene compilato un piano di esecuzione che comporta il trasferimento di una quantità minima di dati tra i server membri. Sebbene sia possibile che alcune tabelle membro si trovino in server remoti, le query distribuite vengono risolte dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che la quantità di dati distribuiti da trasferire sia minima.  
  
## <a name="considerations-for-replication"></a>Requisiti per la replica  
 Per creare viste partizionate di tabelle membro coinvolte nella replica, devono essere soddisfatti i requisiti seguenti:  
  
-   Se le tabelle sottostanti sono coinvolte nella replica transazionale o di tipo merge con sottoscrizioni aggiornabili, assicurarsi che nell'elenco di selezione sia presente anche la colonna **uniqueidentifier**. 
  
     Le operazioni INSERT eseguite nella vista partizionata devono specificare un valore NEWID() per la colonna **uniqueidentifier**. Le operazioni UPDATE eseguite sulla colonna **uniqueidentifier** devono specificare il valore NEWID() in quanto non è possibile usare la parola chiave DEFAULT.  
  
-   La replica di aggiornamenti tramite la vista equivale alla replica di tabelle in due database distinti, ovvero le tabelle vengono gestite da agenti di replica diversi e l'ordine degli aggiornamenti non è prevedibile.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste l'autorizzazione CREATE VIEW per il database e l'autorizzazione ALTER per lo schema in cui viene creata la vista.  
  
## <a name="examples"></a>Esempi  

Negli esempi seguenti vengono usati i database AdventureWorks 2012 o AdventureWorksDW.  

### <a name="a-using-a-simple-create-view"></a>R. Utilizzo di un'istruzione CREATE VIEW semplice  
 Nell'esempio seguente viene creata una vista tramite l'utilizzo di un'istruzione `SELECT` semplice. Una vista semplice risulta utile quando vengono eseguite query frequenti su una combinazione di colonne. I dati di questa vista derivano dalle tabelle `HumanResources.Employee` e `Person.Person` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. I dati contengono informazioni sul nome e la data di assunzione dei dipendenti di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]. È possibile creare la vista per la persona incaricata di tenere traccia del periodo di assunzione dei dipendenti, senza però consentire a tale utente di accedere a tutti i dati di queste tabelle.  
  
```sql
CREATE VIEW hiredate_view  
AS   
SELECT p.FirstName, p.LastName, e.BusinessEntityID, e.HireDate  
FROM HumanResources.Employee e   
JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
  
```  
  
### <a name="b-using-with-encryption"></a>B. Utilizzo della clausola WITH ENCRYPTION  
 Nell'esempio seguente viene utilizzata l'opzione `WITH ENCRYPTION` e vengono illustrate colonne calcolate, colonne rinominate e più colonne.  
  
**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql
CREATE VIEW Purchasing.PurchaseOrderReject  
WITH ENCRYPTION  
AS  
SELECT PurchaseOrderID, ReceivedQty, RejectedQty,   
    RejectedQty / ReceivedQty AS RejectRatio, DueDate  
FROM Purchasing.PurchaseOrderDetail  
WHERE RejectedQty / ReceivedQty > 0  
AND DueDate > CONVERT(DATETIME,'20010630',101) ;  
GO  
  
```  
  
### <a name="c-using-with-check-option"></a>C. Utilizzo della clausola WITH CHECK OPTION  
 Nell'esempio seguente viene illustrata una vista denominata `SeattleOnly` che fa riferimento a cinque tabelle e consente di applicare modifiche ai dati solo per i dipendenti che risiedono a Seattle.  
  
```sql
CREATE VIEW dbo.SeattleOnly  
AS  
SELECT p.LastName, p.FirstName, e.JobTitle, a.City, sp.StateProvinceCode  
FROM HumanResources.Employee e  
INNER JOIN Person.Person p  
ON p.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN Person.BusinessEntityAddress bea   
    ON bea.BusinessEntityID = e.BusinessEntityID   
    INNER JOIN Person.Address a   
    ON a.AddressID = bea.AddressID  
    INNER JOIN Person.StateProvince sp   
    ON sp.StateProvinceID = a.StateProvinceID  
WHERE a.City = 'Seattle'  
WITH CHECK OPTION ;  
GO  
```  
  
### <a name="d-using-built-in-functions-within-a-view"></a>D. Utilizzo di funzioni predefinite in una vista  
 Nell'esempio seguente viene illustrata una definizione di vista che include una funzione predefinita. Quando si utilizzano le funzioni, è necessario specificare un nome di colonna per la colonna derivata.  
  
```sql
CREATE VIEW Sales.SalesPersonPerform  
AS  
SELECT TOP (100) SalesPersonID, SUM(TotalDue) AS TotalSales  
FROM Sales.SalesOrderHeader  
WHERE OrderDate > CONVERT(DATETIME,'20001231',101)  
GROUP BY SalesPersonID;  
GO  

```  
  
### <a name="e-using-partitioned-data"></a>E. Utilizzo di dati partizionati  
 Nell'esempio seguente vengono utilizzate le tabelle denominate `SUPPLY1`, `SUPPLY2`, `SUPPLY3` e `SUPPLY4`, corrispondenti alle tabelle dei fornitori di quattro uffici dislocati in aree geografiche diverse.  
  
```sql
--Create the tables and insert the values.  
CREATE TABLE dbo.SUPPLY1 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 1 and 150),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY2 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 151 and 300),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY3 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 301 and 450),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY4 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 451 and 600),  
supplier CHAR(50)  
);  
GO  
--Create the view that combines all supplier tables.  
CREATE VIEW dbo.all_supplier_view  
WITH SCHEMABINDING  
AS  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY1  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY2  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY3  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY4;  
GO
INSERT dbo.all_supplier_view VALUES ('1', 'CaliforniaCorp'), ('5', 'BraziliaLtd')    
, ('231', 'FarEast'), ('280', 'NZ')  
, ('321', 'EuroGroup'), ('442', 'UKArchip')  
, ('475', 'India'), ('521', 'Afrique');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-creating-a-simple-view"></a>F. Creazione di una vista semplice  
 Nell'esempio seguente viene creata una vista selezionando solo alcune colonne della tabella di origine.  
  
```sql
CREATE VIEW DimEmployeeBirthDates AS  
SELECT FirstName, LastName, BirthDate   
FROM DimEmployee;  
```  
  
### <a name="g-create-a-view-by-joining-two-tables"></a>G. Creare una vista creando un join di due tabelle  
 Nell'esempio seguente viene creata una vista tramite un'istruzione `SELECT` semplice con un elemento `OUTER JOIN`. I risultati della query join popolano la vista.  
  
```sql
CREATE VIEW view1  
AS 
SELECT fis.CustomerKey, fis.ProductKey, fis.OrderDateKey, 
  fis.SalesTerritoryKey, dst.SalesTerritoryRegion  
FROM FactInternetSales AS fis   
LEFT OUTER JOIN DimSalesTerritory AS dst   
ON (fis.SalesTerritoryKey=dst.SalesTerritoryKey);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Creazione di una stored procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_refreshview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

