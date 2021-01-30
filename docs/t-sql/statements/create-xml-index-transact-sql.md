---
title: CREATE XML INDEX (Transact-SQL)
description: CREATE XML INDEX (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_XML_INDEX_TSQL
- XML INDEX
- CREATE_XML_TSQL
- XML
- CREATE XML
- CREATE XML INDEX
- XML_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- CREATE INDEX statement
- index creation [SQL Server], XML indexes
- XML indexes [SQL Server], creating
ms.assetid: c510cfbc-68be-4736-b3cc-dc5b7aa51f14
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
ms.custom: ''
ms.date: 08/10/2017
ms.openlocfilehash: f58ae25a634b080af7bcfaa752568aba7bd6519d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177842"
---
# <a name="create-xml-index-transact-sql"></a>CREATE XML INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un indice XML in una tabella specificata. L'indice può essere creato prima dell'immissione dei dati nella tabella. È possibile creare indici XML per tabelle di un altro database specificando un nome di database completo.  
  
> [!NOTE]  
>  Per creare un indice relazionale, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md). Per informazioni su come creare un indice spaziale, vedere [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
--Create XML Index   
CREATE [ PRIMARY ] XML INDEX index_name   
    ON <object> ( xml_column_name )  
    [ USING XML INDEX xml_index_name   
        [ FOR { VALUE | PATH | PROPERTY } ] ]  
    [ WITH ( <xml_index_option> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{ database_name.schema_name.table_name | schema_name.table_name | table_name }
  
<xml_index_option> ::=  
{   
    PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 [PRIMARY] XML  
 Crea un indice XML sulla colonna **xml** specificata. Quando viene specificata la parola chiave PRIMARY, viene creato un indice cluster con una chiave cluster costituita dalla chiave di clustering della tabella utente e da un identificatore di nodo XML. Per ogni tabella è possibile creare al massimo 249 indici XML. Quando si crea un indice XML, è necessario tenere presente quanto segue:  
  
-   È necessario che esista un indice cluster sulla chiave primaria della tabella utente.  
  
-   La chiave di clustering della tabella utente può includere al massimo 15 colonne.  
  
-   Per ogni colonna **xml** di una tabella è possibile creare un indice XML primario e più indici XML secondari.  
  
-   Prima di poter creare un indice XML secondario su una colonna **xml**, è necessario che per tale colonna esista un indice XML primario.  
  
-   È possibile creare un indice XML solo su una singola colonna **xml**. Non è possibile creare un indice XML su una colonna non **xml** né creare un indice relazionale su una colonna **xml**.  
  
-   Non è possibile creare un indice XML, primario o secondario, su una colonna **xml** di una vista, su una variabile con valori di tabella con colonne **xml** oppure su variabili di tipo **xml**.  
  
-   Non è possibile creare un indice XML primario su una colonna **xml** calcolata.  
  
-   Le impostazioni delle opzioni SET devono corrispondere a quelle necessarie per le viste indicizzate e gli indici su colonne calcolate. In particolare, l'opzione ARITHABORT deve essere impostata su ON quando viene creato un indice XML e quando vengono inseriti, eliminati o aggiornati valori nella colonna **xml**.  
  
 Per altre informazioni, vedere [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
 *index_name*  
 Nome dell'indice. I nomi degli indici devono essere univoci all'interno di una tabella, ma non all'interno di un database. Devono essere anche conformi alle regole degli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 I nomi di indici XML primari non possono iniziare con i caratteri seguenti: **#** , **##** , **@** o **@@** .  
  
 *xml_column_name*  
 Colonna **xml** su cui l'indice è basato. È possibile specificare una sola colonna **xml** in una singola definizione di indice XML, ma è possibile creare più indici XML secondari su una colonna **xml**.  
  
 USING XML INDEX *xml_index_name*  
 Specifica l'indice XML primario da usare per la creazione di un indice XML secondario.  
  
 FOR { VALUE | PATH | PROPERTY }  
 Specifica il tipo di indice XML secondario.  
  
 VALUE  
 Crea un indice XML secondario su colonne con le colonne chiave (valore di nodo e percorso) dell'indice XML primario.  
  
 PATH  
 Crea un indice XML secondario su colonne compilate in base ai valori di percorso e i valori di nodo dell'indice XML primario. Nell'indice secondario PATH i valori di nodo e di percorso sono colonne chiave che consentono di eseguire le ricerche dei percorsi in modo più efficiente.  
  
 PROPERTY  
 Crea un indice XML secondario su colonne (valore di chiave primaria, percorso e nodo) dell'indice XML primario, dove il valore di chiave primaria è la chiave primaria della tabella di base.  
  
 **\<object>::=**  
  
 Oggetto con nome completo o non completo che si desidera indicizzare.  
  
 *database_name*  
 Nome del database.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella.  
  
 *table_name*  
 Nome della tabella da indicizzare.  
  
 **\<xml_index_option> ::=** 
  
 Specifica le opzioni da usare quando si crea l'indice.  
  
 PAD_INDEX **=** { ON | **OFF** }  
 Specifica il riempimento dell'indice. Il valore predefinito è OFF.  
  
 ON  
 La percentuale di spazio disponibile specificata da *fillfactor* viene applicata alle pagine di livello intermedio dell'indice.  
  
 OFF o *fillfactor* non è specificato  
 Le pagine di livello intermedio vengono riempite poco al di sotto della capacità massima, in modo che lo spazio residuo sia sufficiente per almeno una riga della dimensione massima supportata dall'indice, in base al set di chiavi nelle pagine intermedie.  
  
 L'opzione PAD_INDEX risulta utile solo quando si specifica FILLFACTOR, in quanto PAD_INDEX usano la percentuale specificata in FILLFACTOR. Se la percentuale specificata in FILLFACTOR non consente l'inserimento di una riga, [!INCLUDE[ssDE](../../includes/ssde-md.md)] sostituisce internamente tale percentuale in modo da rendere disponibile lo spazio minimo necessario. Il numero di righe di una pagina intermedia dell'indice non è mai minore di due, indipendentemente dal valore di *fillfactor*.  
  
 FILLFACTOR **=** _fillfactor_  
 Specifica una percentuale che indica il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la ricompilazione dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0. Se *fillfactor* è 100 o 0, tramite [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono creati indici con pagine foglia riempite fino alla capacità massima.  
  
> [!NOTE]  
>  I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.  
  
 L'impostazione di FILLFACTOR viene applicata solo in fase di creazione o di ricompilazione dell'indice. La percentuale specificata di spazio vuoto delle pagine non viene mantenuta in modo dinamico da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per visualizzare l'impostazione del fattore di riempimento, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
>  La creazione di un indice cluster con un valore FILLFACTOR minore di 100 influisce sulla quantità di spazio di archiviazione occupata dai dati perché i dati vengono ridistribuiti da [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione dell'indice cluster.  
  
 Per altre informazioni, vedere [Specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 Specifica se i risultati temporanei dell'ordinamento devono essere archiviati in **tempdb**. Il valore predefinito è OFF.  
  
 ON  
 I risultati intermedi dell'ordinamento usati per la compilazione dell'indice vengono archiviati in **tempdb**. In questo modo si può ridurre il tempo necessario per creare un indice se **tempdb** si trova in un set di dischi diverso rispetto al database utente. La quantità di spazio su disco utilizzata durante la compilazione dell'indice sarà tuttavia maggiore.  
  
 OFF  
 I risultati intermedi dell'ordinamento vengono archiviati nello stesso database dell'indice.  
  
 Oltre allo spazio necessario nel database utente per la creazione dell'indice, in **tempdb** deve essere disponibile una quantità di spazio aggiuntivo pressoché equivalente per l'archiviazione dei risultati intermedi dell'ordinamento. Per altre informazioni, vedere [Opzione SORT_IN_TEMPDB per gli indici](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=OFF**  
 Non ha effetto per gli indici XML perché il tipo di indice non è mai univoco. Non impostare questa opzione su ON. In caso contrario, viene generato un errore.  
  
 DROP_EXISTING **=** { ON | **OFF** }  
 Specifica che è necessario eliminare e quindi ricompilare l'indice XML denominato preesistente. Il valore predefinito è OFF.  
  
 ON  
 L'indice esistente deve essere eliminato e ricompilato. Il nome di indice specificato deve corrispondere a quello dell'indice esistente, mentre la definizione dell'indice può essere modificata. È possibile, ad esempio, specificare valori diversi per le colonne, il tipo di ordinamento, lo schema di partizione o le opzioni dell'indice.  
  
 OFF  
 Se il nome di indice specificato esiste già, viene visualizzato un messaggio di errore.  
  
 Il tipo di indice non può essere modificato tramite l'opzione DROP_EXISTING. Non è inoltre possibile ridefinire un indice XML primario come indice XML secondario o viceversa.  
  
 ONLINE **=OFF**  
 Specifica che le tabelle sottostanti e gli indici associati non sono disponibili per le query e la modifica dei dati durante l'operazione sull'indice. In questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le operazioni di compilazione di indici online non sono supportate per gli indici XML. Se questa opzione è impostata su ON per un indice XML, viene generato un errore. Omettere l'opzione ONLINE o impostare ONLINE su OFF.  
  
 Un'operazione offline sull'indice che crea, ricompila o elimina un indice XML acquisisce un blocco di modifica dello schema (SCH-M) per la tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione.  
  
> [!NOTE]
>  Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.  
  
 ON  
 I blocchi di riga sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga.  
  
 OFF  
 I blocchi di riga non vengono utilizzati.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.  
  
 ON  
 I blocchi a livello di pagina sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina.  
  
 OFF  
 I blocchi a livello di pagina non vengono utilizzati.  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 Esegue l'override dell'[opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) per la durata dell'operazione sugli indici. Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
> [!IMPORTANT]  
>  Sebbene l'opzione MAXDOP sia supportata a livello di sintassi per tutti gli indici XML, per un indice XML primario CREATE XML INDEX usano solo un processore singolo.  
  
 *max_degree_of_parallelism* può essere:  
  
 1  
 Disattiva la generazione di piani paralleli.  
  
 \>1  
 Consente di limitare al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori utilizzati in un'operazione parallela sugli indici.  
  
 0 (predefinito)  
 Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
## <a name="remarks"></a>Osservazioni  
 Le colonne calcolate che derivano da tipi di dati **xml** possono essere indicizzate come colonna chiave o come colonna non chiave inclusa a condizione che il tipo di dati della colonna calcolata sia supportato come colonna chiave o colonna non chiave dell'indice. Non è possibile creare un indice XML primario su una colonna **xml** calcolata.  
  
 Per visualizzare informazioni relative agli indici XML, usare la vista del catalogo [sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md).  
  
 Per altre informazioni sugli indici XML, vedere [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="additional-remarks-on-index-creation"></a>Osservazioni aggiuntive sulla creazione dell'indice  
 Per altre informazioni sulla creazione dell'indice, vedere la sezione "Osservazioni" in [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-primary-xml-index"></a>R. Creazione di un indice XML primario  
 Nell'esempio seguente viene creato un indice XML primario sulla colonna `CatalogDescription` della tabella `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT * FROM sys.indexes  
            WHERE name = N'PXML_ProductModel_CatalogDescription')  
    DROP INDEX PXML_ProductModel_CatalogDescription   
        ON Production.ProductModel;  
GO  
CREATE PRIMARY XML INDEX PXML_ProductModel_CatalogDescription  
    ON Production.ProductModel (CatalogDescription);  
GO  
```  
  
### <a name="b-creating-a-secondary-xml-index"></a>B. Creazione di un indice XML secondario  
 Nell'esempio seguente viene creato un indice XML secondario sulla colonna `CatalogDescription` della tabella `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.indexes  
            WHERE name = N'IXML_ProductModel_CatalogDescription_Path')  
    DROP INDEX IXML_ProductModel_CatalogDescription_Path  
        ON Production.ProductModel;  
GO  
CREATE XML INDEX IXML_ProductModel_CatalogDescription_Path   
    ON Production.ProductModel (CatalogDescription)  
    USING XML INDEX PXML_ProductModel_CatalogDescription FOR PATH ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
 [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
