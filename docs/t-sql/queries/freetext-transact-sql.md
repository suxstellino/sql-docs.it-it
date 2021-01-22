---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c4ed81c783dcd8f3fc850432f89b3f7498c16664
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596233"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Predicato usato nella clausola [WHERE](../../t-sql/queries/where-transact-sql.md) di [!INCLUDE[tsql](../../includes/tsql-md.md)] relativa a un'istruzione SELECT di [!INCLUDE[tsql](../../includes/tsql-md.md)] per eseguire una ricerca full-text di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in colonne indicizzate full-text che contengono tipi di dati basati su caratteri. Questo predicato esegue la ricerca dei valori che corrispondono al significato e non solo all'esatta formulazione delle parole nella condizione di ricerca. Se si usa FREETEXT, il motore delle query full-text esegue internamente le azioni di seguito elencate in *freetext_string*, assegna un peso a ogni termine e quindi cerca le corrispondenze:  
  
-   Separazione della stringa in singole parole in base ai delimitatori di parola (word breaking).  
  
-   Generazione di forme flessive delle parole (stemming).  
  
-   Identificazione di una lista di espansioni o sostituzioni dei termini in base alle corrispondenze nel thesaurus.  
  
> [!NOTE]  
>  Per informazioni sulle forme di ricerca full-text supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md).  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *column_name*  
 Nome di una o più colonne indicizzate full-text della tabella specificata nella clausola FROM. La colonna o le colonne possono essere di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** r **varbinary(max)** .  
  
 *column_list*  
 Viene indicato che è possibile specificare più colonne, separate da virgola. *column_list* deve essere racchiuso tra parentesi. La lingua di tutte le colonne di *column_list* deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 \*  
 Specifica che la ricerca della stringa specificata in *freetext_string* deve essere eseguita in tutte le colonne registrate per la ricerca full-text. Se nella clausola FROM sono specificate più tabelle, è necessario qualificare il simbolo \* con il nome della tabella. La lingua di tutte le colonne della tabella deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 *freetext_string*  
 Testo da cercare nella colonna specificata in *column_name*. È possibile specificare qualsiasi testo, comprese parole e frasi. Vengono generate corrispondenze se nell'indice full-text viene trovato un termine o vengono trovate le forme di un termine.  
  
 A differenza di quanto avviene nella condizione di ricerca CONTAINS e CONTAINSTABLE in cui AND è una parola chiave, se usata in *freetext_string* la parola 'and' viene considerata una [parola non significativa](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) e verrà ignorata.  
  
 Non è consentito utilizzare WEIGHT, FORMSOF, caratteri jolly, NEAR e altra sintassi. *freetext_string* viene sottoposta alla sillabazione, all'analisi morfologica e al thesaurus.  
  
 *freetext_string* è **nvarchar**. Viene eseguita una conversione implicita quando si utilizza come input un tipo di dati character diverso. Non è possibile usare varchar (max) e nvarchar (max) per tipi di dati di stringa di grandi dimensioni. Nell'esempio seguente la variabile `@SearchWord`, definita come `varchar(30)`, causa una conversione implicita nel predicato `FREETEXT`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Poiché non è possibile usare l'analisi dei parametri nella conversione, usare **nvarchar** per migliorare le prestazioni. Nell'esempio dichiarare `@SearchWord` come `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 È inoltre possibile utilizzare l'hint per la query OPTIMIZE FOR nei casi in cui venga generato un piano non ottimale.  
  
 LANGUAGE *language_term*  
 Lingua le cui risorse verranno utilizzate per il word breaking, lo stemming, il thesaurus e la rimozione di parole non significative come parte della query. Questo parametro è facoltativo e può essere specificato come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) di una lingua. Se si specifica *language_term*, la lingua rappresentata dall'argomento verrà applicata a tutti gli elementi della condizione di ricerca. Se non si specifica alcun valore, verrà utilizzata la lingua full-text della colonna.  
  
 Se documenti di lingue diverse vengono archiviati insieme come oggetti BLOB in una singola colonna, l'identificatore delle impostazioni locali (LCID) di un documento specifico determina la lingua da utilizzare per indicizzarne il contenuto. Se quando si esegue una query su una colonna di questo tipo si specifica LANGUAGE *language_term*, è possibile aumentare la probabilità di una corrispondenza soddisfacente.  
  
 Se l'argomento *language_term* viene specificato come stringa, corrisponde al valore della colonna **alias** nella vista di compatibilità [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).  La stringa deve essere racchiusa tra virgolette singole chiuse, come in '*language_term*'. Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale di LCID. Il valore esadecimale non deve superare le otto cifre, inclusi gli zeri iniziali.  
  
 Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirà in Unicode.  
  
 Se la lingua specificata non è valida o non vi sono risorse installate corrispondenti a tale lingua, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore. Per usare le risorse della lingua neutra, specificare 0x0 per *language_term*.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 I predicati e le funzioni full-text possono essere utilizzati in una singola tabella, specificata in modo implicito nel predicato FROM. Per cercare in più tabelle, utilizzare una tabella unita in join nella clausola FROM, che consente di eseguire una ricerca in un set di risultati prodotto da due o più tabelle.  
  
Le query full-text che utilizzano il predicato FREETEXT sono meno precise delle query che utilizzano il predicato CONTAINS. Il motore di ricerca full-text di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifica le parole e le frasi importanti. Alle parole chiave riservate e ai caratteri jolly che hanno generalmente significato nel parametro \<contains_search_condition> del predicato CONTAINS non viene associato alcun significato speciale.
  
 I predicati full-text non sono consentiti nella [clausola OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) quando il livello di compatibilità del database è impostato su 100.  
  
> [!NOTE]  
>  La funzione FREETEXTTABLE è utile per gli stessi tipi di corrispondenze del predicato FREETEXT. È possibile fare riferimento a questa funzione come un normale nome di tabella nella [clausola FROM](../../t-sql/queries/from-transact-sql.md) di un'istruzione SELECT. Per altre informazioni, vedere [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="querying-remote-servers"></a>Esecuzione di query in server remoti  
 È possibile usare un nome in quattro parti nel predicato [CONTAINS](../../t-sql/queries/contains-transact-sql.md) o FREETEXT per eseguire query di colonne con indicizzazione full-text delle tabelle di destinazione in un server collegato. Per preparare un server remoto alla ricezione di query full-text, creare un indice full-text delle tabelle di destinazione e delle colonne nel server remoto, quindi aggiungere il server remoto come server collegato.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Confronto tra LIKE e la ricerca full-text  
 Contrariamente alla ricerca full-text, il predicato [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] funziona unicamente con i modelli di caratteri. Non è inoltre possibile utilizzare il predicato LIKE per eseguire query su dati binari formattati. Inoltre, l'esecuzione di una query LIKE su una grande quantità di dati di testo non strutturati è molto più lenta dell'esecuzione di una query full-text equivalente sugli stessi dati. Una query LIKE eseguita su milioni di righe di dati di testo può richiedere diversi minuti, mentre per una query full-text sugli stessi dati possono essere necessari al massimo pochi secondi, a seconda del numero di righe restituite.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>R. Utilizzo di FREETEXT per la ricerca di parole contenenti valori di carattere specificati  
 Nell'esempio seguente viene eseguita la ricerca di tutti i documenti contenenti le parole associate a "vital", "safety" e "components".  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>B. Utilizzo di FREETEXT con variabili  
 Nell'esempio seguente viene utilizzata una variabile anziché un termine di ricerca specifico.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alla ricerca full-text](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Creare e gestire cataloghi full-text](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Creazione e gestione di indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Creare query di ricerca full-text &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
