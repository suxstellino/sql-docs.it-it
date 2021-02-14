---
description: Creazione e gestione di indici full-text
title: Creare e gestire indici full-text | Microsoft Docs
ms.custom: ''
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], about
ms.assetid: f8a98486-5438-44a8-b454-9e6ecbc74f83
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a3c3792fa2aca009e02fc8fcdce6bf613b5e370
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341687"
---
# <a name="create-and-manage-full-text-indexes"></a>Creazione e gestione di indici full-text
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Questo argomento descrive come creare, compilare e gestire gli indici full-text in SQL Server.
  
## <a name="prerequisite---create-a-full-text-catalog"></a>Prerequisito - Creare un catalogo full-text
Prima di poter creare un indice full-text è necessario che sia disponibile un catalogo full-text. Il catalogo è un contenitore virtuale per uno o più indici full-text. Per altre informazioni, vedere [Creare e gestire cataloghi full-text](../../relational-databases/search/create-and-manage-full-text-catalogs.md).
  
##  <a name="create-alter-or-drop-a-full-text-index"></a><a name="tasks"></a> Creare, modificare o eliminare un indice full-text  
### <a name="create-a-full-text-index"></a>Creare un indice full-text  
  
-   [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
### <a name="alter-a-full-text-index"></a>Modificare un indice full-text
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
### <a name="drop-a-full-text-index"></a>Eliminare un indice full-text 
  
-   [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)

## <a name="populate-a-full-text-index"></a>Popolare un indice full-text
Il processo di creazione e gestione di un indice full-text è definito *popolamento* (noto anche come *ricerca per indicizzazione*). Esistono tre tipi di popolamento dell'indice full-text:
-   Popolamento completo
-   Popolamento basato sul rilevamento delle modifiche
-   Popolamento incrementale basato su timestamp.

Per altre informazioni, vedere [Popolare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).

##  <a name="view-the-properties-of-a-full-text-index"></a><a name="view"></a> Visualizzare le proprietà di un indice full-text
### <a name="view-the-properties-of-a-full-text-index-with-transact-sql"></a>Visualizzare le proprietà di un indice full-text con Transact-SQL

|Catalogo o vista a gestione dinamica|Descrizione|  
|----------------------------------------|-----------------|  
|[sys.fulltext_index_catalog_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)|Restituisce una riga per ogni catalogo full-text in riferimento all'indice full-text.|  
|[sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)|Contiene una riga per ogni colonna che fa parte di un indice full-text.|  
|[sys.fulltext_index_fragments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)|Un indice full-text utilizza tabelle interne denominate frammenti di indice full-text per archiviare i dati dell'indice invertito. Questa vista può essere utilizzata per eseguire una query sui metadati relativi a tali frammenti. Nella vista è contenuta una riga per ogni frammento di indice full-text presente in ogni tabella contenente un indice full-text.|  
|[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)|Contiene una riga per indice full-text di un oggetto in formato di tabella.|  
|[sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)|Restituisce informazioni sul contenuto di un indice full-text per la tabella specificata.|  
|[sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)|Restituisce informazioni sul contenuto a livello di documento di un indice full-text per la tabella specificata. Una determinata parola chiave può essere inclusa in diversi documenti.|  
|[sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)|Restituisce informazioni sui popolamenti di indici full-text in corso.|  
 
### <a name="view-the-properties-of-a-full-text-index-with-management-studio"></a>Visualizzare le proprietà di un indice full-text con Management Studio 
1.  In Management Studio espandere il server in Esplora oggetti.  
  
2.  Espandere **Database** e quindi il database contenente l'indice full-text.  
  
3.  Espandere **Tabelle**.  
  
4.  Fare clic con il pulsante destro del mouse sulla tabella in cui viene definito l'indice full-text, scegliere **Indice full-text** e quindi **Proprietà** dal menu di scelta rapida **Indice full-text**. Verrà visualizzata la finestra di dialogo **Proprietà indice full-text** .  
  
5.  Nel riquadro **Seleziona una pagina** è possibile selezionare le pagine seguenti:  
  
    |Pagina|Descrizione|  
    |----------|-----------------|  
    |**Generale**|Sono contenute le proprietà di base dell'indice full-text, incluse diverse proprietà modificabili e alcune proprietà non modificabili quali il nome del database, il nome della tabella e il nome della colonna chiave full-text. Le proprietà modificabili sono le seguenti:<br /><br /> **Elenco di parole non significative indice full-text**<br /><br /> **Indicizzazione full-text abilitata**<br /><br /> **Rilevamento delle modifiche**<br /><br /> **Elenco delle proprietà di ricerca**|  
    |**Colonne**|Consente di visualizzare le colonne della tabella disponibili per l'indicizzazione full-text. La colonna o le colonne selezionate contengono indici full-text. È possibile selezionare il numero desiderato di colonne disponibili da includere nell'indice full-text. Per altre informazioni, vedere [Popolare gli indici full-text](populate-full-text-indexes.md).|
    |**Pianificazioni**|Utilizzare questa pagina per creare o gestire le pianificazioni per un processo di SQL Server Agent che consente di avviare un popolamento incrementale della tabella per i popolamenti dell'indice full-text. Per altre informazioni, vedere [Popolare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Nota: Dopo avere chiuso la finestra di dialogo **Proprietà indice full-text**, eventuali nuove pianificazioni vengono associate a un processo di SQL Server Agent (Avvio del popolamento incrementale della tabella in *nome_database*.*nome_tabella*).|  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] per salvare le modifiche e uscire dalla finestra di dialogo **Proprietà indice full-text**.  
  
##  <a name="view-the-properties-of-indexed-tables-and-columns"></a><a name="props"></a> Visualizzare le proprietà di tabelle e colonne indicizzate  
 Per ottenere il valore di diverse proprietà di indicizzazione full-text, è possibile utilizzare varie funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] come OBJECTPROPERTYEX. Queste informazioni sono utili per l'amministrazione e la risoluzione dei problemi relativi alla ricerca full-text.  
  
 Nella tabella seguente sono elencate le proprietà full-text relative a tabelle e colonne indicizzate e le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] correlate.  
  
|Proprietà|Descrizione|Funzione|  
|--------------|-----------------|--------------|  
|**FullTextTypeColumn**|TYPE COLUMN nella tabella in cui sono contenute le informazioni sui tipi di documenti della colonna.|[COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)|  
|**IsFulltextIndexed**|Indica se una colonna è stata abilitata per l'indicizzazione full-text.|COLUMNPROPERTY|  
|**IsFulltextKey**|Indica se l'indice è la chiave full-text di una tabella.|[INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)|  
|**TableFulltextBackgroundUpdateIndexOn**|Indica se per una tabella è stata impostata l'indicizzazione full-text degli aggiornamenti in background.|[OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)|  
|**TableFulltextCatalogId**|ID del catalogo full-text contenente i dati dell'indice full-text per la tabella.|OBJECTPROPERTYEX|  
|**TableFulltextChangeTrackingOn**|Indica se per la tabella è abilitato il rilevamento delle modifiche full-text.|OBJECTPROPERTYEX|  
|**TableFulltextDocsProcessed**|Numero di righe elaborate dopo l'avvio dell'indicizzazione full-text.|OBJECTPROPERTYEX|  
|**TableFulltextFailCount**|Numero di righe non indicizzate dalla ricerca full-text.|OBJECTPROPERTYEX|  
|**TableFulltextItemCount**|Numero di righe per cui l'indicizzazione full-text ha avuto esito positivo.|OBJECTPROPERTYEX|  
|**TableFulltextKeyColumn**|ID della colonna chiave univoca full-text.|OBJECTPROPERTYEX|  
|**TableFullTextMergeStatus**|Indica se in una tabella che dispone di un indice full-text è attualmente in corso un'operazione di unione.|OBJECTPROPERTYEX|  
|**TableFulltextPendingChanges**|Numero di voci in sospeso del rilevamento delle modifiche da elaborare.|OBJECTPROPERTYEX|  
|**TableFulltextPopulateStatus**|Stato popolamento di una tabella full-text.|OBJECTPROPERTYEX|  
|**TableHasActiveFulltextIndex**|Indica se una tabella include un indice full-text attivo.|OBJECTPROPERTYEX|  
  
##  <a name="get-info-about-the-full-text-key-column"></a><a name="key"></a> Ottenere informazioni sulla colonna chiave full-text  
 In genere, il risultato della funzione con valori del set di righe CONTAINSTABLE o FREETEXTTABLE deve essere unito in join alla tabella di base. In questi casi, è necessario conoscere il nome della colonna chiave univoca. È possibile verificare se un determinato indice univoco viene utilizzato come chiave full-text e ottenere l'identificatore della colonna chiave full-text.  
  
### <a name="determine-whether-a-given-unique-index-is-used-as-the-full-text-key-column"></a>Determinare se un determinato indice univoco viene usato come colonna chiave full-text  
  
Usare un'istruzione [SELECT](../../t-sql/queries/select-transact-sql.md) per chiamare la funzione [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md). Nella chiamata alla funzione usare la funzione OBJECT_ID per convertire il nome della tabella (*table_name*) nell'ID tabella, specificare il nome di un indice univoco per la tabella, quindi specificare la proprietà di indice **IsFulltextKey** come illustrato di seguito:  
  
```sql  
SELECT INDEXPROPERTY( OBJECT_ID('table_name'), 'index_name',  'IsFulltextKey' );  
```  
  
 L'istruzione restituisce 1 se l'indice viene utilizzato per applicare l'unicità della colonna chiave full-text e 0 in caso contrario.  
  
 **Esempio**  
  
 Nell'esempio seguente viene illustrato come verificare se l'indice `PK_Document_DocumentNode` viene utilizzato per applicare l'univocità della colonna chiave full-text:  
  
```sql  
USE AdventureWorks  
GO  
SELECT INDEXPROPERTY ( OBJECT_ID('Production.Document'), 'PK_Document_DocumentNode',  'IsFulltextKey' )  
```  
  
 In questo esempio viene restituito 1 se l'indice `PK_Document_DocumentNode` viene utilizzato per applicare l'univocità della colonna chiave full-text. In caso contrario, viene restituito 0 o NULL. NULL indica che è in uso un nome di indice non valido, il nome dell'indice non corrisponde alla tabella, la tabella non esiste e così via.  
  
### <a name="find-the-identifier-of-the-full-text-key-column"></a>Trovare l'identificatore della colonna chiave full-text  
  
Ogni tabella abilitata per la funzionalità full-text include una colonna che viene usata per applicare righe univoche per la tabella (la *colonna chiave**univoca*). La proprietà **TableFulltextKeyColumn**, ottenuta dalla funzione OBJECTPROPERTYEX contiene l'ID della colonna chiave univoca.  
 
Per ottenere questo identificatore, è possibile utilizzare un'istruzione SELECT per chiamare la funzione OBJECTPROPERTYEX. Usare la funzione OBJECT_ID per convertire il nome della tabella (*table_name*) nell'ID tabella e specificare la proprietà **TableFulltextKeyColumn** come illustrato di seguito:  
  
```sql  
SELECT OBJECTPROPERTYEX(OBJECT_ID( 'table_name'), 'TableFulltextKeyColumn' ) AS 'Column Identifier';  
```  
  
 **esempi**  
  
 Nell'esempio seguente viene restituito l'identificatore della colonna chiave full-text o NULL. NULL indica che è in uso un nome di indice non valido, il nome dell'indice non corrisponde alla tabella, la tabella non esiste e così via.  
  
```sql
USE AdventureWorks;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('Production.Document'), 'TableFulltextKeyColumn');  
GO  
```  
  
 Nell'esempio seguente viene illustrato come utilizzare l'identificatore della colonna chiave univoca per ottenere il nome della colonna.  
  
```sql  
USE AdventureWorks;  
GO  
DECLARE @key_column sysname  
SET @key_column = Col_Name(Object_Id('Production.Document'),  
ObjectProperty(Object_id('Production.Document'),  
'TableFulltextKeyColumn')   
)  
SELECT @key_column AS 'Unique Key Column';  
GO  
```  
  
 Nell'esempio viene restituita una colonna del set di risultati denominata `Unique Key Column`in cui viene visualizzata una sola riga contenente il nome della colonna chiave univoca della tabella Document, DocumentNode. Si noti che se questa query contenesse un nome di indice non valido, il nome di indice non corrispondesse alla tabella, la tabella non esistesse e così via, il risultato restituito sarebbe NULL.  

## <a name="index-varbinarymax-and-xml-columns"></a>Indicizzare colonne varbinary(max) e xml  
 Se una colonna **varbinary(max)** , **varbinary** o **xml** viene sottoposta a indicizzazione full-text, le query su questa colonna possono essere eseguite usando predicati (CONTAINS e FREETEXT) e funzioni (CONTAINSTABLE e FREETEXTTABLE) full-text, proprio come su ogni altra colonna con indicizzazione full-text.
   
### <a name="index-varbinarymax-or-varbinary-data"></a>Indicizzare colonne varbinary(max) o varbinary  
 In una singola colonna **varbinary(max)** o **varbinary** possono essere archiviati molti tipi di documenti. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta qualsiasi tipo di documento per cui viene installato un filtro e disponibile nel sistema operativo. Il tipo di ogni documento è identificato dall'estensione file relativa. Per un'estensione file doc, ad esempio, la ricerca full-text utilizza il filtro che supporta i documenti di Microsoft Word. Per un elenco dei tipi di documento disponibili, eseguire una query sulla vista del catalogo [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) .  
  
Si noti che il motore di ricerca full-text può utilizzare i filtri esistenti installati nel sistema operativo. Prima di poter utilizzare i filtri, i word breaker e gli stemmer del sistema operativo, è necessario caricarli nell'istanza del server, come illustrato di seguito:  
  
```sql  
EXEC sp_fulltext_service @action='load_os_resources', @value=1  
```  
  
Per creare un indice full-text in una colonna **varbinary(max)** , il motore di ricerca full-text deve accedere alle estensioni file dei documenti nella colonna **varbinary(max)** . Queste informazioni devono essere archiviate in una colonna di tabella, denominata colonna del tipo, che deve essere associata alla colonna **varbinary(max)** nell'indice full-text. Quando si esegue l'indicizzazione di un documento, il motore di ricerca full-text utilizza l'estensione del file nella colonna del tipo per identificare il filtro da utilizzare.  
   
### <a name="index-xml-data"></a>Indicizzare dati xml  
 In una colonna del tipo di dati **xml** vengono archiviati esclusivamente documenti e frammenti XML e per i documenti viene usato solo il filtro XML. Una colonna del tipo non è pertanto necessaria. Nelle colonne **xml** , l'indice full-text indicizza il contenuto degli elementi XML, ma ignora il markup XML. Ai valori di attributo viene applicata l'indicizzazione full-text a meno che non siano valori numerici. I tag degli elementi vengono utilizzati come limiti dei token. Sono supportati documenti e frammenti XML o HTML ben formati e contenenti più lingue.  
  
 Per altre informazioni sull'indicizzazione e l'esecuzione di query su una colonna **xml** , vedere [Usare la ricerca full-text con colonne XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
##  <a name="disable-or-re-enable-full-text-indexing-for-a-table"></a><a name="disable"></a> Disabilitare o riabilitare l'indicizzazione full-text per una tabella   
 Per impostazione predefinita, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tutti i database creati dall'utente sono abilitati per la funzionalità full-text. Una tabella viene inoltre abilitata automaticamente per l'indicizzazione full-text dopo la creazione di un indice full-text nella tabella e l'aggiunta di una colonna all'indice. L'indicizzazione full-text viene disabilitata automaticamente nella tabella quando l'ultima colonna viene eliminata dall'indice full-text.  
  
 In una tabella che dispone di un indice full-text è possibile disabilitare o riabilitare manualmente una tabella per indicizzazione full-text utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

1.  Espandere il gruppo di server, espandere **Database**, quindi il database contenente la tabella che si vuole abilitare per l'indicizzazione full-text.  
  
2.  Espandere **Tabelle** e fare clic con il pulsante destro del mouse sulla tabella che si vuole disabilitare o riabilitare per l'indicizzazione full-text.  
  
3.  Scegliere **Indice full-text**, quindi fare clic su **Disabilita indicizzazione full-text** o **Abilita indicizzazione full-text**.  
  
##  <a name="remove-a-full-text-index-from-a-table"></a><a name="remove"></a> Rimuovere un indice full-text da una tabella  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sulla tabella contenente l'indice full-text che si desidera eliminare.  
  
2.  Selezionare **Elimina indice full-text**.  
  
3.  Quando richiesto, fare clic su **OK** per confermare l'eliminazione dell'indice full-text.  
  
  
