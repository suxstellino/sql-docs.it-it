---
description: CREATE SEARCH PROPERTY LIST (Transact-SQL)
title: CREATE SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SEARCH_PROPERTY_LIST_TSQL
- CREATE SEARCH
- CREATE_SEARCH_TSQL
- CREATE_SEARCH_PROPERTY_TSQL
- CREATE SEARCH PROPERTY
- CREATE SEARCH PROPERTY LIST
- sql13.swb.spl.newsearchpropertylist.f1
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], creating
- CREATE SEARCH PROPERTY LIST statement
ms.assetid: 5440cbb8-3403-4d27-a2f9-8e1f5a1bc12b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 31e938250b543fbb6bb3e3443138677f8ee3befd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192700"
---
# <a name="create-search-property-list-transact-sql"></a>CREATE SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea un nuovo elenco delle proprietà di ricerca. Un elenco delle proprietà di ricerca viene utilizzato per specificare una o più proprietà di ricerca da includere in un indice full-text.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
CREATE SEARCH PROPERTY LIST new_list_name  
   [ FROM [ database_name. ] source_list_name ]  
   [ AUTHORIZATION owner_name ]  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *new_list_name*  
 Nome del nuovo elenco delle proprietà di ricerca. *new_list_name* è un identificatore composto da un massimo di 128 caratteri. *new_list_name* deve essere univoco in tutti gli elenchi di proprietà nel database corrente e conforme alle regole per gli identificatori. *new_list_name* viene usato dopo la creazione dell'indice full-text.  
  
 *database_name*  
 Nome del database in cui si trova l'elenco di proprietà specificato da *source_list_name*. Se l'argomento *database_name* non viene specificato, il valore predefinito è il database corrente.  
  
 *database_name* deve specificare il nome di un database esistente. L'account di accesso per la connessione corrente deve essere associato a un ID utente esistente nel database specificato da *database_name*. Occorre inoltre avere le [autorizzazioni](#Permissions) necessarie nel database.  
  
 *source_list_name*  
 Specifica che il nuovo elenco di proprietà viene creato copiando un elenco di proprietà esistente da *database_name*. Se *source_list_name* non esiste, CREATE SEARCH PROPERTY LIST ha esito negativo e restituisce un errore. Le proprietà di ricerca in *source_list_name* vengono ereditate da *new_list_name*.  
  
 AUTHORIZATION *owner_name*  
 Specifica il nome di un utente o di un ruolo proprietario dell'elenco di proprietà. *owner_name* deve essere il nome di un ruolo di cui l'utente corrente è membro oppure l'utente corrente deve avere l'autorizzazione IMPERSONATE per *owner_name*. Se viene omesso, la proprietà viene assegnata all'utente corrente.  
  
> [!NOTE]  
>  Il proprietario può essere modificato usando l'istruzione [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="remarks"></a>Commenti  
  
> [!NOTE]  
>  Per informazioni generali sugli elenchi delle proprietà, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 Per impostazione predefinita, un nuovo elenco delle proprietà di ricerca è vuoto ed è necessario modificarlo manualmente per aggiungere una o più proprietà di ricerca. In alternativa, è possibile copiare un elenco delle proprietà di ricerca esistente. In questo caso, il nuovo elenco eredita le proprietà di ricerca dell'origine, ma è possibile modificare il nuovo elenco per aggiungere o rimuovere le proprietà di ricerca. Tutte le proprietà nell'elenco delle proprietà di ricerca al momento del popolamento completo successivo vengono incluse nell'indice full-text.  
  
 Un istruzione CREATE SEARCH PROPERTY LIST ha esito negativo se si verifica una delle condizioni seguenti:  
  
-   Se il database specificato da *database_name* non esiste.  
  
-   Se l'elenco specificato da *source_list_name* non esiste.  
  
-   Se non sono disponibili le autorizzazioni corrette.  
  
 **Per aggiungere o rimuovere proprietà da un elenco**  
  
-   [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   **Per eliminare un elenco di proprietà**  
  
-   [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono richieste autorizzazioni CREATE FULLTEXT CATALOG nel database corrente e autorizzazioni REFERENCES in qualsiasi database da cui viene copiato un elenco delle proprietà di origine.  
  
> [!NOTE]  
>  L'autorizzazione REFERENCES è necessaria per associare l'elenco a un indice full-text. L'autorizzazione CONTROL è necessaria per aggiungere e rimuovere proprietà o eliminare l'elenco. Il proprietario dell'elenco di proprietà può concedere autorizzazioni REFERENCES o CONTROL sull'elenco. Gli utenti con l'autorizzazione CONTROL possono inoltre concedere l'autorizzazione REFERENCES ad altri utenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-an-empty-property-list-and-associating-it-with-an-index"></a>R. Creazione di un elenco di proprietà vuoto e relativa associazione a un indice  
 Nell'esempio seguente viene creato un nuovo elenco delle proprietà di ricerca denominato `DocumentPropertyList`. Nell'esempio viene quindi usata un'istruzione [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) per associare il nuovo elenco di proprietà all'indice full-text della tabella `Production.Document` nel database `AdventureWorks`, senza avviare un'operazione di popolamento.  
  
> [!NOTE]  
>  Per un esempio in cui vengono aggiunte diverse proprietà di ricerca note e predefinite a questo elenco delle proprietà di ricerca, vedere [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md). Una volta aggiunte le proprietà di ricerca all'elenco, l'amministratore del database dovrebbe utilizzare un'altra istruzione ALTER FULLTEXT INDEX con la clausola START FULL POPULATION.  
  
```sql 
CREATE SEARCH PROPERTY LIST DocumentPropertyList;  
GO  
USE AdventureWorks2012;  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList  
   WITH NO POPULATION;   
GO   
```  
  
### <a name="b-creating-a-property-list-from-an-existing-one"></a>B. Creazione di un elenco di proprietà da uno esistente  
 Nell'esempio seguente viene creato un nuovo l'elenco delle proprietà di ricerca, `JobCandidateProperties`, dall'elenco creato nell'esempio A, `DocumentPropertyList`, associato a un indice full-text nel database `AdventureWorks2012`. Nell'esempio viene quindi utilizzata un'istruzione ALTER FULLTEXT INDEX per associare il nuovo elenco di proprietà all'indice full-text della tabella `HumanResources.JobCandidate` nel database `AdventureWorks2012`. Questa istruzione ALTER FULLTEXT INDEX avvia un popolamento completo, che corrisponde al comportamento predefinito della clausola SET SEARCH PROPERTY LIST.  
  
```sql  
CREATE SEARCH PROPERTY LIST JobCandidateProperties 
FROM AdventureWorks2012.DocumentPropertyList;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   SET SEARCH PROPERTY LIST JobCandidateProperties;  
GO
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Trovare GUID del set di proprietà e ID di tipo integer delle proprietà per le proprietà di ricerca](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
