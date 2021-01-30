---
description: sp_fulltext_keymappings (Transact-SQL)
title: sp_fulltext_keymappings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_keymappings_TSQL
- sp_fulltext_keymappings
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], key column
- sp_fulltext_keymappings
- full-text indexes [SQL Server], troubleshooting
ms.assetid: 2818fa42-072d-4664-a2f7-7ec363b51d81
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d9520a47fb519988e252e251e7e4449f7a425079
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185423"
---
# <a name="sp_fulltext_keymappings-transact-sql"></a>sp_fulltext_keymappings (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Restituisce i mapping tra gli identificatori del documento (DocIds) e i valori della chiave full-text. La colonna DocId contiene valori per un intero **bigint** mappato a un particolare valore della chiave full-text in una tabella con indicizzazione full-text. I valori DocId che soddisfano una condizione di ricerca vengono passati dal motore di ricerca full-text al Motore di database dove vengono sottoposti al mapping ai valori della chiave full-text della tabella di base su cui viene eseguita la query. La colonna chiave full-text rappresenta l'indice univoco necessario in una colonna della tabella.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_fulltext_keymappings { table_id | table_id, docid | table_id, NULL, key }  
```  
  
#### <a name="parameters"></a>Parametri  
 *table_id*  
 ID oggetto della tabella con indicizzazione full-text. Se si specifica un *table_id* non valido, viene restituito un errore. Per informazioni su come ottenere l'ID oggetto di una tabella, vedere [OBJECT_ID &#40;&#41;Transact-SQL ](../../t-sql/functions/object-id-transact-sql.md).  
  
 *docid*  
 Identificatore interno del documento (DocID) corrispondente al valore della chiave. Se si specifica un valore *docid* non valido, non viene restituito alcun risultato.  
  
 *key*  
 Valore di chiave full-text dalla tabella specificata. Se si specifica un valore *key* non valido, non viene restituito alcun risultato. Per informazioni sui valori della chiave full-text, vedere [manage Full-Text Indexes](../search/create-and-manage-full-text-indexes.md).  
  
> [!IMPORTANT]  
>  Per informazioni sull'utilizzo di uno, due o tre parametri, vedere la sezione "Osservazioni" più avanti in questo argomento.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuna.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|DocId|**bigint**|Colonna dell'identificatore interno del documento (DocID) corrispondente al valore della chiave.|  
|Chiave|*|Valore di chiave full-text dalla tabella specificata.<br /><br /> Se nella tabella di mapping non esiste alcuna chiave full-text, viene restituito un set di righe vuoto.|  
  
 <sup>*</sup> Il tipo di dati della chiave è uguale al tipo di dati della colonna chiave full-text nella tabella di base.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa funzione è pubblica e non richiede autorizzazioni speciali.  
  
## <a name="remarks"></a>Commenti  
 Nella tabella seguente viene descritto l'effetto ottenuto nel caso si utilizzino uno, due o tre parametri.  
  
|Questo elenco di parametri...|Risultato...|  
|--------------------------|----------------------|  
|*table_id*|Quando viene richiamato solo con il parametro *table_id* , sp_fulltext_keymappings restituisce tutti i valori della chiave full-text (chiave) dalla tabella di base specificata, insieme a DocId che corrisponde a ogni chiave. Le chiavi in sospeso vengono eliminate.<br /><br /> Questa funzione consente di risolvere numerosi problemi ed è particolarmente utile per visualizzare il contenuto dell'indice full-text quando il tipo di dati della chiave full-text selezionata non è Integer. Questa operazione implica l'Unione dei risultati di sp_fulltext_keymappings con i risultati della **sys.dm_fts_index_keywords_by_document**. Per ulteriori informazioni, vedere [sys.dm_fts_index_keywords_by_document &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md).<br /><br /> In generale, quando possibile, è consigliabile tuttavia eseguire sp_fulltext_keymappings con parametri che indicano una chiave full-text o un valore DocId specifico Questa operazione risulta molto più efficiente rispetto alla restituzione di un'intera mappa di chiavi, soprattutto per una tabella di dimensioni elevate per cui il costo in termini di prestazioni della restituzione dell'intera mappa di chiavi potrebbe essere significativo.|  
|*table_id*, *docid*|Se vengono specificati solo il *table_id* e *docid* , *docid* deve essere diverso da null e specificare un DocID valido nella tabella specificata. Questa funzione è utile per isolare la chiave full-text personalizzata della tabella di base corrispondente al valore DocId di un determinato indice full-text.|  
|*table_id*, null, *Key*|Se sono presenti tre parametri, il secondo parametro deve essere NULL e la *chiave* deve essere non null e specificare un valore valido per la chiave full-text dalla tabella specificata. Questa funzione è utile per isolare il valore DocId corrispondente a una chiave full-text specifica della tabella di base.|  
  
 Se si verifica una delle condizioni seguenti, viene restituito un errore:  
  
-   Si specifica un *table_id* non valido.  
  
-   La tabella non è una tabella con indicizzazione full-text.  
  
-   Viene rilevato il valore NULL per un parametro che può essere diverso da NULL.  
  
## <a name="examples"></a>Esempio  
  
> [!NOTE]  
>  Negli esempi riportati in questa sezione utilizzare la tabella `Production.ProductReview` del database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] . È possibile creare questo indice eseguendo l'esempio fornito per la `ProductReview` tabella in [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
### <a name="a-obtaining-all-the-key-and-docid-values"></a>R. Recupero di tutti i valori Key e DocId  
 Nell'esempio seguente viene utilizzata un'istruzione [Declare](../../t-sql/language-elements/declare-local-variable-transact-sql.md) per creare una variabile locale `@table_id` e per assegnare l'ID della `ProductReview` tabella come valore. Nell'esempio viene eseguito **sp_fulltext_keymappings** specificando `@table_id` per il parametro *table_id* .  
  
> [!NOTE]  
>  L'utilizzo di **sp_fulltext_keymappings** solo con il parametro *table_id* è adatto per le tabelle di piccole dimensioni.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @table_id int = OBJECT_ID(N'Production.ProductReview');  
EXEC sp_fulltext_keymappings @table_id;  
GO  
  
```  
  
 In questo esempio vengono restituite tutte i valori di DocId e le chiavi full-text dalla tabella come segue:  
  
| TABLE | docid | key |
| ----- | ----- | --- |
|`1`|`1`|`1`|  
|`2`|`2`|`2`|  
|`3`|`3`|`3`|  
|`4`|`4`|`4`|  
  
### <a name="b-obtaining-the-docid-value-for-a-specific-key-value"></a>B. Acquisizione del valore DocId per un valore Key specifico  
 Nell'esempio seguente viene utilizzata un'istruzione DECLARE per creare una variabile locale, `@table_id`, e assegnare l'ID della tabella `ProductReview` come valore. Nell'esempio viene eseguito **sp_fulltext_keymappings** specificando `@table_id` per il parametro *table_id* , null per il parametro *docid* e 4 per il parametro *Key* .  
  
> [!NOTE]  
>  L'utilizzo di **sp_fulltext_keymappings** solo con il parametro *table_id* è adatto per le tabelle di piccole dimensioni.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @table_id int = OBJECT_ID(N'Production.ProductReview');  
EXEC sp_fulltext_keymappings @table_id, NULL, 4;  
GO  
  
```  
  
 Nell'esempio vengono restituiti i risultati seguenti.  
  
| TABLE | docid | key |
| ----- | ----- | --- |
|`4`|`4`|`4`|  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
