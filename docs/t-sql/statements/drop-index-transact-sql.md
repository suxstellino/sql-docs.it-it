---
description: DROP INDEX (Transact-SQL)
title: DROP INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_INDEX_TSQL
- DROP INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- nonclustered indexes [SQL Server], removing
- MAXDOP index option, DROP INDEX statement
- index removal [SQL Server]
- spatial indexes [SQL Server], dropping
- removing indexes
- deleting indexes
- dropping indexes
- MOVE TO clause
- clustered indexes, removing
- indexes [SQL Server], dropping
- filtered indexes [SQL Server], dropping
- ONLINE option
- indexes [SQL Server], moving
- XML indexes [SQL Server], dropping
- DROP INDEX statement
ms.assetid: 2b1464c8-934c-405f-8ef7-2949346b5372
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e5b63c83c7081819d5020e79c8d199f2a263dedd
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689063"
---
# <a name="drop-index-transact-sql"></a>DROP INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove uno o più indici relazionali, spaziali, filtrati o XML dal database corrente. È possibile eliminare un indice cluster e spostare la tabella risultante in un altro filegroup o schema di partizione in una singola transazione specificando l'opzione MOVE TO.  
  
 L'istruzione DROP INDEX non è valida per gli indici creati tramite i vincoli PRIMARY KEY o UNIQUE. Per rimuovere il vincolo e l'indice corrispondente, usare [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) con la clausola DROP CONSTRAINT.  
  
> [!IMPORTANT]
>  La sintassi definita in `<drop_backward_compatible_index>` verrà rimossa in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarla in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Utilizzare in alternativa la sintassi specificata in `<drop_relational_or_xml_index>`. Gli indici XML non possono essere eliminati utilizzando la sintassi compatibile con le versioni precedenti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server (All options except filegroup and filestream apply to Azure SQL Database.)  
  
DROP INDEX [ IF EXISTS ]   
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
| <drop_backward_compatible_index> [ ,...n ]  
}  
  
<drop_relational_or_xml_or_spatial_index> ::=  
    index_name ON <object>   
    [ WITH ( <drop_clustered_index_option> [ ,...n ] ) ]  
  
<drop_backward_compatible_index> ::=  
    [ owner_name. ] table_or_view_name.index_name  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_clustered_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
  | ONLINE = { ON | OFF }  
  | MOVE TO { partition_scheme_name ( column_name )   
            | filegroup_name  
            | "default"   
            }  
  [ FILESTREAM_ON { partition_scheme_name   
            | filestream_filegroup_name   
            | "default" } ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database  
  
DROP INDEX  
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
}  
  
<drop_relational_or_xml_or_spatial_index> ::=   
    index_name ON <object>  
  
<object> ::=   
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP INDEX index_name ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *IF EXISTS*  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Rimuove in modo condizionale l'indice solo se esiste già.  
  
 *index_name*  
 Nome dell'indice che si desidera eliminare.  
  
 *database_name*  
 Nome del database.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella o la vista.  
  
 *table_or_view_name*  
 Nome della tabella o della vista associata all'indice. Gli indici spaziali sono supportati solo nelle tabelle.  
  
 Per visualizzare un report degli indici di un oggetto, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Il database SQL di Azure supporta il formato del nome in tre parti, nome_database.[nome_schema].nome_oggetto, quando nome_database è il database corrente oppure nome_database è tempdb e nome_oggetto inizia con #.  
  
 \<drop_clustered_index_option>  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Controlla le opzioni degli indici cluster. Non è possibile utilizzare queste opzioni con altri tipi di indice.  
  
 MAXDOP = *max_degree_of_parallelism*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (solo livelli di prestazioni P2 e P3).  
  
 Sostituisce l'opzione di configurazione **max degree of parallelism** per la durata dell'operazione sull'indice. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
> [!IMPORTANT]  
>  MAXDOP non è consentito per indici spaziali o XML.  
  
 *max_degree_of_parallelism* può essere:  
  
 1  
 Disattiva la generazione di piani paralleli.  
  
 \>1  
 Limita il numero massimo di processori utilizzati in un'operazione parallela sull'indice in base al numero specificato.  
  
 0 (predefinito)  
 Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]  
>  Le operazioni sugli indici parallele sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ONLINE = ON | **OFF**  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Specifica se le tabelle sottostanti e gli indici associati sono disponibili per le query e la modifica dei dati durante l'operazione sugli indici. Il valore predefinito è OFF.  
  
 ON  
 I blocchi di tabella a lungo termine non vengono mantenuti. Ciò consente il proseguimento delle query o degli aggiornamenti nella tabella sottostante.  
  
 OFF  
 Vengono applicati blocchi di tabella e la tabella non è disponibile per la durata dell'operazione sull'indice.  
  
 L'opzione ONLINE può essere specificata solo quando si eliminano gli indici cluster. Per altre informazioni, vedere la sezione Osservazioni.  
  
> [!NOTE]  
>  Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ **)**  | _filegroup\_name_ |  **"** default **"**  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive. Il [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] supporta "default" come nome del filegroup.  
  
 Specifica una posizione in cui spostare le righe dei dati attualmente nel livello foglia dell'indice cluster. I dati vengono spostati nella nuova posizione sotto forma di heap. È possibile specificare come nuova posizione uno schema di partizione o un filegroup, ma questo deve essere già esistente. MOVE TO non può essere utilizzata per viste indicizzate o indici non cluster. Se uno schema di partizione o filegroup non viene specificato, la tabella risultante si troverà nello stesso schema di partizione o filegroup definito per l'indice cluster.  
  
 Se un indice cluster viene eliminato tramite MOVE TO, tutti gli indici non cluster nella tabella di base vengono ricostruiti, ma rimangono nei filegroup o negli schemi di partizione originali. Se la tabella di base viene spostata in un filegroup o in uno schema di partizione diverso, gli indici non cluster non vengono spostati in base alla nuova posizione della tabella di base (heap). Pertanto, anche se in precedenza gli indici non cluster erano allineati con gli indici cluster, potrebbero non essere più allineati con l'heap. Per altre informazioni sull'allineamento degli indici partizionati, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 _partition_scheme_name_ **(** _column_name_ **)**  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Specifica uno schema di partizione come posizione per la tabella risultante. È necessario che lo schema di partizione sia già stato creato tramite [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Se non viene specificata alcuna posizione e la tabella è partizionata, la tabella viene inclusa nello stesso schema di partizione dell'indice cluster esistente.  
  
 Il nome di colonna nello schema non è limitato alle colonne nella definizione dell'indice. È possibile specificare qualsiasi colonna nella tabella di base.  
  
 *filegroup_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica un filegroup come posizione per la tabella risultante. Se non viene specificata una posizione e la tabella non è partizionata, la tabella risultante viene inclusa nello stesso filegroup dell'indice cluster. Il filegroup deve essere già esistente.  
  
 **"** default **"**  
 Specifica la posizione predefinita per la tabella risultante.  
  
> [!NOTE]
>  In questo contesto, default non è una parola chiave, ma un identificatore per il filegroup predefinito e deve essere delimitato, come in MOVE TO **"** default **"** o MOVE TO **[** default **]** . Se si specifica **"** default **"** , l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 FILESTREAM_ON { *partition_scheme_name* | *filestream_filegroup_name* |  **"** default **"** }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica una posizione in cui spostare la tabella FILESTREAM che attualmente si trova al livello foglia dell'indice cluster. I dati vengono spostati nella nuova posizione sotto forma di heap. È possibile specificare come nuova posizione uno schema di partizione o un filegroup, ma questo deve essere già esistente. FILESTREAM ON non è valido per viste indicizzate o indici non cluster. Se non viene specificato uno schema di partizione, i dati verranno inclusi nello stesso schema di partizione definito per l'indice cluster.  
  
 *partition_scheme_name*  
 Specifica uno schema di partizione per i dati FILESTREAM. È necessario che lo schema di partizione sia già stato creato tramite [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Se non viene specificata alcuna posizione e la tabella è partizionata, la tabella viene inclusa nello stesso schema di partizione dell'indice cluster esistente.  
  
 Se si specifica un schema di partizione per MOVE TO, è necessario utilizzare lo stesso schema di partizione per FILESTREAM ON.  
  
 *filestream_filegroup_name*  
 Specifica un filegroup FILESTREAM per i dati FILESTREAM. Se non viene specificata una posizione e la tabella non è partizionata, i dati vengono inclusi nel filegroup FILESTREAM predefinito.  
  
 **"** default **"**  
 Specifica la posizione predefinita dei dati FILESTREAM.  
  
> [!NOTE]
>  In questo contesto, default non è una parola chiave, ma un identificatore per il filegroup predefinito e deve essere delimitato, come in MOVE TO **"** default **"** o MOVE TO **[** default **]** . Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 Quando si elimina un indice non cluster, la definizione dell'indice viene rimossa dai metadati e le pagine dei dati dell'indice (albero B) vengono rimosse dai file del database. Quando viene eliminato un indice cluster, la definizione dell'indice viene rimossa dai metadati e le righe di dati precedentemente archiviate nel livello foglia dell'indice cluster vengono archiviate nella tabella non ordinata risultante, o heap. Tutto lo spazio occupato in precedenza dall'indice viene recuperato, e può essere quindi utilizzato per qualsiasi oggetto di database.  
  
 Non è possibile eliminare un indice se il filegroup in cui si trova è offline oppure è in sola lettura.  
  
 Quando si elimina l'indice cluster di una vista indicizzata, tutte le statistiche create automaticamente e tutti gli indici non cluster della stessa vista vengono eliminati automaticamente. Le statistiche create manualmente non vengono eliminate.  
  
 La sintassi _table\_or\_view\_name_ **.** _index\_name_ è stata mantenuta per la compatibilità con le versioni precedenti. Non è possibile eliminare un indice XML o spaziale tramite la sintassi compatibile con le versioni precedenti.  
  
 Quando vengono eliminati gli indici con 128 o più extent, [!INCLUDE[ssDE](../../includes/ssde-md.md)] posticipa le deallocazioni effettive delle pagine e dei relativi blocchi associati fino al termine dell'esecuzione del commit della transazione.  
  
 Talvolta gli indici vengono eliminati e ricreati per riorganizzare o ricompilare l'indice, ad esempio per applicare un nuovo fattore di riempimento o riorganizzare i dati dopo un'operazione di caricamento bulk. Per eseguire questa operazione, l'uso di [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) risulta più efficace, in particolare per gli indici cluster. Per ALTER INDEX REBUILD sono disponibili ottimizzazioni per impedire che gli indici non cluster vengano ricompilati.  
  
## <a name="using-options-with-drop-index"></a>Utilizzo delle opzioni con DROP INDEX  
 Quando si elimina un indice cluster, è possibile impostare le opzioni di indice MAXDOP, ONLINE e MOVE TO.  
  
 Utilizzare l'opzione MOVE TO per eliminare l'indice cluster e spostare la tabella risultante in un altro filegroup o schema di partizione in una singola transazione.  
  
 Quando si specifica ONLINE = ON, le query e le modifiche ai dati sottostanti e agli indici non cluster associati non vengono bloccate dalla transazione DROP INDEX. È possibile eliminare un solo indice cluster online alla volta. Per una descrizione completa dell'opzione ONLINE, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Non è possibile eliminare un indice cluster online se l'indice è disabilitato in una vista o contiene colonne di tipo **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** o **xml** nelle righe di dati a livello foglia.  
  
 Per l'utilizzo delle opzioni ONLINE = ON e MOVE TO è necessario spazio su disco temporaneo aggiuntivo.  
  
 Dopo aver eliminato un indice, l'heap risultante viene visualizzato nella vista del catalogo **sys.indexes** con un valore NULL nella colonna **name**. Per visualizzare il nome della tabella, creare un join tra **sys.indexes** e **sys.tables** su **object_id**. Per una query di esempio, vedere l'esempio D.  
  
 Nei computer multiprocessore in cui è in esecuzione [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] o versione successiva l'istruzione DROP INDEX, allo stesso modo di altre query, può utilizzare più processori per eseguire le operazioni di analisi e di ordinamento associate all'eliminazione dell'indice cluster. È possibile configurare manualmente il numero di processori utilizzati per eseguire l'istruzione DROP INDEX mediante l'opzione di indice MAXDOP. Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
 Quando un indice cluster viene eliminato, le partizioni di heap corrispondenti mantengono l'impostazione di compressione dei dati, a meno che lo schema di partizione non venga modificato. In tal caso, tutte le partizioni vengono ricostruite in un stato non compresso (DATA_COMPRESSION = NONE). Per eliminare un indice cluster e modificare lo schema di partizione, sono necessari i due passaggi seguenti:  
  
1.  Eliminare l'indice cluster.  
  
2.  Modificare la tabella usando un'opzione ALTER TABLE ... REBUILD... e specificando l'opzione di compressione.  
  
Quando un indice cluster viene eliminato offline, vengono rimossi solo i livelli superiori degli indici cluster, pertanto l'operazione è piuttosto veloce. Quando un indice cluster viene eliminato ONLINE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricompila l'heap due volte, una volta per l'operazione descritta nel passaggio 1 e una volta per quella descritta nel passaggio 2. Per altre informazioni sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
## <a name="xml-indexes"></a>Indici XML  
 Quando si elimina un indice XML non è possibile specificare opzioni né usare la sintassi _table\_or\_view\_name_ **.** _index\_name_. Quando viene eliminato un indice XML primario, tutti gli indici XML secondari associati vengono eliminati automaticamente. Per altre informazioni, vedere [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="spatial-indexes"></a>Indici spaziali  
 Gli indici spaziali sono supportati solo nelle tabelle. Quando si elimina un indice spaziale non è possibile specificare alcuna opzione né usare **.** _index\_name_. La sintassi corretta è la seguente:  
  
 DROP INDEX *spatial_index_name* ON *spatial_table_name*;  
  
 Per altre informazioni sugli indici spaziali, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire DROP INDEX è necessario disporre almeno dell'autorizzazione ALTER sulla tabella o sulla vista. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_ddladmin** e **db_owner** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-an-index"></a>R. Eliminazione di un indice  
 Nell'esempio seguente viene eliminato l'indice `IX_ProductVendor_VendorID` nella tabella `ProductVendor` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  
```  
  
### <a name="b-dropping-multiple-indexes"></a>B. Eliminazione di più indici  
 Nell'esempio seguente vengono eliminati due indici in una transazione singola nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX  
    IX_PurchaseOrderHeader_EmployeeID ON Purchasing.PurchaseOrderHeader,  
    IX_Address_StateProvinceID ON Person.Address;  
GO  
```  
  
### <a name="c-dropping-a-clustered-index-online-and-setting-the-maxdop-option"></a>C. Eliminazione di un indice cluster online e impostazione dell'opzione MAXDOP  
 Nell'esempio seguente viene eliminato un indice cluster con l'opzione `ONLINE` impostata su `ON` e l'opzione `MAXDOP` impostata su `8`. Poiché l'opzione MOVE TO non è stata specificata, la tabella risultante viene archiviata nello stesso filegroup dell'indice. In questo esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
```sql  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials WITH (ONLINE = ON, MAXDOP = 2);  
GO  
```  
  
### <a name="d-dropping-a-clustered-index-online-and-moving-the-table-to-a-new-filegroup"></a>D. Eliminazione di un indice cluster online e spostamento di una tabella in un nuovo filegroup  
 Nell'esempio seguente viene eliminato un indice cluster online e la tabella risultante (heap) viene spostata nel filegroup `NewGroup` tramite la clausola `MOVE TO` . Vengono eseguite query sulle viste del catalogo `sys.indexes`, `sys.tables`e `sys.filegroups` per verificare la posizione dell'indice e della tabella nei filegroup prima e dopo lo spostamento. A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] è possibile usare la sintassi DROP INDEX IF EXISTS.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
--Create a clustered index on the PRIMARY filegroup if the index does not exist.  
CREATE UNIQUE CLUSTERED INDEX  
    AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
        ON Production.BillOfMaterials (ProductAssemblyID, ComponentID,   
        StartDate)  
    ON 'PRIMARY';  
GO  
-- Verify filegroup location of the clustered index.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U')  
GO  
--Create filegroup NewGroup if it does not exist.  
IF NOT EXISTS (SELECT name FROM sys.filegroups  
                WHERE name = N'NewGroup')  
    BEGIN  
    ALTER DATABASE AdventureWorks2012  
        ADD FILEGROUP NewGroup;  
    ALTER DATABASE AdventureWorks2012  
        ADD FILE (NAME = File1,  
            FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\File1.ndf')  
        TO FILEGROUP NewGroup;  
    END  
GO  
--Verify new filegroup  
SELECT * from sys.filegroups;  
GO  
-- Drop the clustered index and move the BillOfMaterials table to  
-- the Newgroup filegroup.  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials   
    WITH (ONLINE = ON, MOVE TO NewGroup);  
GO  
-- Verify filegroup location of the moved table.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U');  
GO  
```  
  
### <a name="e-dropping-a-primary-key-constraint-online"></a>E. Eliminazione di un vincolo PRIMARY KEY online  
 Gli indici creati come risultato della creazione dei vincoli PRIMARY KEY o UNIQUE non possono essere eliminati tramite DROP INDEX, ma vengono eliminati tramite l'istruzione ALTER TABLE DROP CONSTRAINT. Per altre informazioni, vedere [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 Nell'esempio seguente viene eliminato un indice cluster con un vincolo PRIMARY KEY eliminando il vincolo. La tabella `ProductCostHistory` non ha vincoli FOREIGN KEY. In caso contrario, sarebbe stato necessario rimuovere prima i vincoli.  
  
```sql  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
ALTER TABLE Production.TransactionHistoryArchive  
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID  
WITH (ONLINE = ON);  
```  
  
### <a name="f-dropping-an-xml-index"></a>F. Eliminazione di un indice XML  
 Nell'esempio seguente viene eliminato un indice XML nella tabella `ProductModel` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX PXML_ProductModel_CatalogDescription   
    ON Production.ProductModel;  
```  
  
### <a name="g-dropping-a-clustered-index-on-a-filestream-table"></a>G. Eliminazione di un indice cluster in una tabella FILESTREAM  
 Nell'esempio seguente viene eliminato un indice cluster online e la tabella risultante (heap) e i dati FILESTREAM vengono spostati nello schema di partizione `MyPartitionScheme` utilizzando sia la clausola `MOVE TO` che la clausola `FILESTREAM ON`.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
DROP INDEX PK_MyClusteredIndex   
    ON dbo.MyTable   
    WITH (MOVE TO MyPartitionScheme,  
          FILESTREAM_ON MyPartitionScheme);  
GO  
```  

  
## <a name="see-also"></a>Vedere anche  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
