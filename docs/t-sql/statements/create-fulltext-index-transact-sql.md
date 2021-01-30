---
description: CREATE FULLTEXT INDEX (Transact-SQL)
title: CREATE FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FULLTEXT_INDEX_TSQL
- CREATE_FULLTEXT_INDEX_TSQL
- CREATE FULLTEXT INDEX
- FULLTEXT INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], creating
- index creation [SQL Server], CREATE FULLTEXT INDEX statement
- CREATE FULLTEXT INDEX statement
ms.assetid: 8b80390f-5f8b-4e66-9bcc-cabd653c19fd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 780f0f1471f0ce7a03a4aa800e4e98cc6a533dbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179176"
---
# <a name="create-fulltext-index-transact-sql"></a>CREATE FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un indice full-text per una tabella o una vista indicizzata di un database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È consentito un solo indice full-text per tabella o vista indicizzata e ogni indice full-text viene applicato a una singola tabella o vista indicizzata. Un indice full-text può contenere fino a 1024 colonne.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE FULLTEXT INDEX ON table_name  
   [ ( { column_name   
             [ TYPE COLUMN type_column_name ]  
             [ LANGUAGE language_term ]   
             [ STATISTICAL_SEMANTICS ]  
        } [ ,...n]   
      ) ]  
    KEY INDEX index_name   
    [ ON <catalog_filegroup_option> ]  
    [ WITH [ ( ] <with_option> [ ,...n] [ ) ] ]  
[;]  
  
<catalog_filegroup_option>::=  
 {  
    fulltext_catalog_name   
 | ( fulltext_catalog_name, FILEGROUP filegroup_name )  
 | ( FILEGROUP filegroup_name, fulltext_catalog_name )  
 | ( FILEGROUP filegroup_name )  
 }  
  
<with_option>::=  
 {  
   CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF [, NO POPULATION ] }   
 | STOPLIST [ = ] { OFF | SYSTEM | stoplist_name }  
 | SEARCH PROPERTY LIST [ = ] property_list_name   
 }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*table_name*       
Nome della tabella o della vista indicizzata contenente la colonna o le colonne incluse nell'indice full-text.  
  
*column_name*       
Nome della colonna inclusa nell'indice full-text. Per la ricerca full-text è possibile indicizzare solo colonne di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml** e **varbinary(max)**. Per specificare più colonne, ripetere la clausola *column_name* come indicato di seguito:  
  
CREATE FULLTEXT INDEX ON *table_name* (*column_name1* [...], *column_name2* [...]) ...  
  
TYPE COLUMN *type_column_name*       
Specifica il nome di una colonna della tabella, *type_column_name*, usato per contenere il tipo di documento per un documento **varbinary(max)** o **image**. Questa colonna, nota come colonna di tipo, contiene un'estensione di file fornita dall'utente (doc, pdf, xls e così via). La colonna del tipo deve essere di tipo **char**, **nchar**, **varchar** o **nvarchar**.  
  
Specificare TYPE COLUMN *type_column_name* solo se *column_name* specifica una colonna **varbinary(max)** o **image** in cui i dati vengono archiviati come dati binari; in caso contrario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore.  
  
> [!NOTE]  
> Durante l'indicizzazione il motore di ricerca full-text usa l'abbreviazione nella colonna di tipo di ogni riga di tabella per identificare il filtro di ricerca full-text da usare per il documento in *column_name*. Il filtro carica il documento come flusso binario, rimuove le informazioni sulla formattazione e invia il testo dal documento al componente word breaker. Per altre informazioni, vedere [Configurazione e gestione di filtri per la ricerca](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
LANGUAGE *language_term*       
È la lingua dei dati archiviati in *column_name*.  
  
*language_term* è facoltativo e può essere specificato come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) di una lingua. Se non viene specificato alcun valore, viene utilizzata la lingua predefinita dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Se *language_term* è specificato, la lingua rappresentata verrà usata per indicizzare i dati archiviati nelle colonne **char**, **nchar**, **varchar**, **nvarchar**, **text** e **ntext**. Questa lingua corrisponde alla lingua predefinita usata in fase di query se non viene specificato *language_term* in un predicato full-text per la colonna.  
  
Se l'argomento *language_term* viene specificato come stringa, corrisponde al valore della colonna alias nella tabella di sistema syslanguages. La stringa deve essere racchiusa tra virgolette singole, come in **'** _language\__term_ **'** . Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale dell'LCID. Il valore esadecimale deve essere composto al massimo da otto cifre, zero iniziali inclusi.  
  
Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirà in Unicode.  
  
Per la lingua specificata in *language_term* è necessario abilitare risorse quali word breaker e stemmer. Se tali risorse non supportano la lingua specificata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituirà un errore.  
  
Usare la stored procedure sp_configure per accedere alle informazioni sulla lingua full-text predefinita dell'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Per le colonne di tipo non BLOB e non XML che contengono dati di testo in più lingue oppure nei casi in cui la lingua del testo archiviato nella colonna è sconosciuta, potrebbe essere appropriato utilizzare la risorsa per la lingua neutra (0x0). È tuttavia necessario considerare prima le possibili conseguenze derivanti dall'utilizzo di tale risorsa. Per informazioni sulle possibili soluzioni e conseguenze dell'uso della risorsa per la lingua neutra (0x0), vedere [Scegliere una lingua durante la creazione di un indice full-text](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
Per i documenti archiviati in colonne di tipo XML o BLOB, in fase di indicizzazione verrà utilizzata la codifica di lingua del documento. Nelle colonne XML, ad esempio, la lingua è identificata dall'attributo **xml:lang** nei documenti XML. In fase di query, il valore precedentemente specificato in *language_term* diventa la lingua predefinita per le query full-text, a meno che non si specifichi *language_term* all'interno di una query full-text.  
  
STATISTICAL_SEMANTICS       
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) 
  
Consente di creare la frase chiave aggiuntiva e gli indici di somiglianza dei documenti che fanno parte dell'indicizzazione semantica statistica. Per altre informazioni, vedere [Ricerca semantica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
KEY INDEX *index_name*       
Nome dell'indice di chiave univoca in *table_name*. KEY INDEX deve essere una colonna chiave singola univoca che non ammette i valori Null. Selezionare l'indice di chiave univoca più piccolo possibile per la chiave univoca full-text.  Per prestazioni ottimali, si consiglia di utilizzare un tipo di dati integer per la chiave full-text.  
  
*fulltext_catalog_name*       
Catalogo full-text utilizzato per l'indice full-text. Il catalogo deve esistere nel database corrente. Questa clausola è facoltativa. Se non viene specificata, viene utilizzato un catalogo predefinito. Se non esiste alcun catalogo predefinito, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore.  
  
FILEGROUP *filegroup_name*       
Crea l'indice full-text specificato nel filegroup specificato. Il filegroup deve essere già esistente. Se la clausola FILEGROUP non viene specificata, l'indice full-text viene posizionato nello stesso filegroup della tabella o della vista di base nel caso di una tabella non partizionata o nel filegroup primario nel caso di una tabella partizionata.  
  
 CHANGE_TRACKING [ = ] { MANUAL | **AUTO** | OFF [ , NO POPULATION ] }       
 Specifica se le modifiche (aggiornamenti, eliminazioni o inserimenti) apportate alle colonne della tabella coperte dall'indice full-text verranno propagate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'indice full-text. Le modifiche apportate ai dati tramite WRITETEXT e UPDATETEXT non vengono riflesse nell'indice full-text, pertanto non vengono registrate dalla funzione di rilevamento delle modifiche.  
  
MANUAL       
Specifica che le modifiche rilevate devono essere propagate manualmente chiamando l'istruzione ALTER FULLTEXT INDEX ... START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*popolamento manuale*). Per chiamare questa istruzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] periodicamente, è possibile utilizzare [!INCLUDE[tsql](../../includes/tsql-md.md)] Agent.  
  
**AUTO**       
Specifica che le modifiche rilevate verranno propagate automaticamente quando i dati nella tabella di base vengono modificati (*popolamento automatico*). Sebbene le modifiche vengano propagate automaticamente, tali modifiche potrebbero non risultare immediatamente nell'indice full-text. AUTO è l'impostazione predefinita.  
  
OFF [ `,` NO POPULATION]       
Specifica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non mantiene un elenco delle modifiche apportate ai dati indicizzati. Se non si specifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] popola l'indice completamente dopo che è stato creato.  
  
L'opzione NO POPULATION può essere utilizzata solo se l'impostazione di CHANGE_TRACKING è OFF. Se si specifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non popola un indice dopo che è stato creato. L'indice viene popolato solo dopo avere eseguito il comando ALTER FULLTEXT INDEX con la clausola START FULL POPULATION o START INCREMENTAL POPULATION.  
  
STOPLIST [ = ] { OFF | **SYSTEM** | *stoplist_name* }       
Associa un elenco di parole non significative full-text all'indice. L'indice non viene popolato con alcun token che faccia parte dell'elenco di parole non significative specificato. Se non si specifica STOPLIST, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associa l'elenco di parole non significative full-text di sistema all'indice.  
  
OFF       
Specifica che all'indice full-text non deve essere associato alcun elenco di parole non significative.  
  
**SYSTEM**       
Specifica che per l'indice full-text deve essere utilizzato l'elemento STOPLIST full-text di sistema predefinito.  
  
*stoplist_name*       
Specifica il nome dell'elenco di parole non significative da associare all'indice full-text.  
  
SEARCH PROPERTY LIST [ = ] *property_list_name*       
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive)  
  
Associa un elenco di proprietà di ricerca all'indice.  
 
OFF       
Specifica che all'indice full-text non deve essere associato alcun elenco di proprietà.  
  
*property_list_name*       
Specifica il nome dell'elenco delle proprietà di ricerca da associare all'indice full-text.  
  
## <a name="remarks"></a>Osservazioni  
Per altre informazioni sugli indici full-text, vedere [Creare e gestire indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md).  
  
Nelle colonne **xml** è possibile creare un indice full-text per indicizzare il contenuto degli elementi XML ignorando il markup XML. Ai valori di attributo viene applicata l'indicizzazione full-text a meno che non siano valori numerici. I tag degli elementi vengono utilizzati come limiti dei token. Sono supportati documenti e frammenti XML o HTML ben formati e contenenti più lingue. Per altre informazioni, vedere [Utilizzo della ricerca full-text con colonne XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
Si consiglia di utilizzare un tipo di dati integer per la colonna chiave indice per consentire l'ottimizzazione durante l'esecuzione della query.  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a>Interazioni del rilevamento delle modifiche con NO POPULATION  
 Il popolamento dell'indice full-text dipende dal fatto che il rilevamento delle modifiche sia o meno abilitato e che si specifichi o meno WITH NO POPULATION nell'istruzione ALTER FULLTEXT INDEX. Nella tabella seguente è riepilogato il risultato di tale interazione.  
  
|Rilevamento modifiche|WITH NO POPULATION|Risultato|  
|---------------------|------------------------|------------|  
|Non abilitato|Non specificato|Viene eseguito un popolamento completo dell'indice.|  
|Non abilitato|Specificata|L'indice non viene popolato fino a quando non viene eseguita un'istruzione ALTER FULLTEXT INDEX...START POPULATION.|  
|Attivato|Specificata|Viene generato un errore e l'indice non viene modificato.|  
|Attivato|Non specificato|Viene eseguito un popolamento completo dell'indice.|  
  
 Per altre informazioni sul popolamento degli indici full-text, vedere [Popolare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve avere l'autorizzazione `REFERENCES` per il catalogo full-text e dell'autorizzazione `ALTER` per la tabella o la vista indicizzata oppure deve essere membro del ruolo predefinito del server `sysadmin` o del ruolo predefinito del database `db_owner` o `db_ddladmin`.  
  
Se si specifica `SET STOPLIST`, l'utente deve avere l'autorizzazione REFERENCES per l'elenco di parole non significative specificato. Questa autorizzazione può essere concessa dal proprietario di STOPLIST.  
  
> [!NOTE]  
> Agli utenti viene concessa l'autorizzazione REFERENCE per l'elenco di parole non significative predefinito fornito con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-unique-index-a-full-text-catalog-and-a-full-text-index"></a>R. Creazione di un indice univoco, un catalogo full-text e un indice full-text  
 Nell'esempio seguente viene creato un indice univoco sulla colonna `JobCandidateID` della tabella `HumanResources.JobCandidate` del database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Viene quindi creato un catalogo full-text predefinito, `ft` e infine viene creato un indice full-text sulla colonna `Resume` utilizzando il catalogo `ft` e l'elenco di parole non significative del sistema.  
  
```sql  
CREATE UNIQUE INDEX ui_ukJobCand ON HumanResources.JobCandidate(JobCandidateID);  
CREATE FULLTEXT CATALOG ft AS DEFAULT;  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume)   
   KEY INDEX ui_ukJobCand   
   WITH STOPLIST = SYSTEM;  
GO  
```  
  
### <a name="b-creating-a-full-text-index-on-several-table-columns"></a>B. Creazione di un indice full-text su diverse colonne della tabella  
 Nell'esempio seguente viene creato un catalogo full-text, `production_catalog`, nel database di esempio `AdventureWorks`. Nell'esempio viene quindi creato un indice full-text che utilizza questo nuovo catalogo. L'indice full-text è sulle colonne `ReviewerName`, `EmailAddress` e `Comments` di `Production.ProductReview`. Per ogni colonna, nell'esempio viene specificato l'identificatore LCID dell'inglese, `1033`, che identifica la lingua dei dati nelle colonne. Questo indice full-text utilizza un indice della chiave univoca esistente, `PK_ProductReview_ProductReviewID`. Come consigliato, questa chiave di indice è in una colonna di valori interi, `ProductReviewID`.  
  
```sql  
CREATE FULLTEXT CATALOG production_catalog;  
GO  
CREATE FULLTEXT INDEX ON Production.ProductReview  
 (   
  ReviewerName  
     Language 1033,  
  EmailAddress  
     Language 1033,  
  Comments   
     Language 1033       
 )   
  KEY INDEX PK_ProductReview_ProductReviewID   
      ON production_catalog;   
GO  
```  
  
### <a name="c-creating-a-full-text-index-with-a-search-property-list-without-populating-it"></a>C. Creazione di un indice full-text con un elenco di proprietà di ricerca senza popolarlo  
 Nell'esempio seguente viene creato un indice full-text sulle colonne `Title`, `DocumentSummary` e `Document` della tabella `Production.Document`. Nell'esempio viene specificato l'identificatore LCID dell'inglese, `1033`, che identifica la lingua dei dati delle colonne. Questo indice full-text utilizza il catalogo full-text predefinito e un indice della chiave univoca esistente, `PK_Document_DocumentID`. Come consigliato, questa chiave di indice è in una colonna di valori interi, `DocumentID`.  
  
 Nell'esempio viene inoltre specificato l'elenco di parole non significative SYSTEM. Specifica anche un elenco di proprietà di ricerca, `DocumentPropertyList`; per un esempio in cui viene creato questo elenco di proprietà, vedere [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
 Nell'esempio viene specificato che il rilevamento delle modifiche è disabilitato e non viene eseguito il popolamento. In un secondo momento, durante le ore di minore attività, nell'esempio viene utilizzata un'istruzione ALTER FULLTEXT INDEX per avviare il popolamento completo del nuovo indice e abilitare il rilevamento delle modifiche automatico.  
  
```sql  
CREATE FULLTEXT INDEX ON Production.Document  
  (   
  Title  
      Language 1033,   
  DocumentSummary  
      Language 1033,   
  Document   
      TYPE COLUMN FileExtension  
      Language 1033   
  )  
  KEY INDEX PK_Document_DocumentID  
          WITH STOPLIST = SYSTEM, SEARCH PROPERTY LIST = DocumentPropertyList, CHANGE_TRACKING OFF, NO POPULATION;  
   GO  
```  
  
 In un secondo momento, in un orario di minore attività, l'indice viene popolato:  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document SET CHANGE_TRACKING AUTO;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e gestione di indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md)       
[ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)       
[DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)       
[Ricerca full-text](../../relational-databases/search/full-text-search.md)       
[GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)       
[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)       
[Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)       
  
  
