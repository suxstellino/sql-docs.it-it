---
description: CREATE FULLTEXT STOPLIST (Transact-SQL)
title: CREATE FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STOPLIST_TSQL
- FULLTEXT STOPLIST
- STOPLIST
- FULLTEXT_STOPLIST_TSQL
- CREATE FULLTEXT STOPLIST
- CREATE_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- CREATE FULLTEXT STOPLIST statement
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 0669b1d0-46cc-4fac-8df7-5f7fa7af5db4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: feef9cf677b1d897a1a9cfc681791c0aba8fdf60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191034"
---
# <a name="create-fulltext-stoplist-transact-sql"></a>CREATE FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un nuovo elenco di parole non significative full-text nel database corrente.  
  
 Le parole non significative vengono gestite nei database usando oggetti denominati *elenchi di parole non significative*. Un elenco di parole non significative è un elenco che, quando associato a un indice full-text, viene applicato alle query full-text su tale indice. Per altre informazioni, vedere [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
> [!IMPORTANT]  
>  CREATE FULLTEXT STOPLIST, ALTER FULLTEXT STOPLIST e DROP FULLTEXT STOPLIST sono supportate solo con il livello di compatibilità 100. Con livelli di compatibilità 80 e 90, queste istruzioni non sono supportate. Con tutti i livelli di compatibilità, tuttavia, l'elenco di parole non significative di sistema viene automaticamente associato ai nuovi indici full-text.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
CREATE FULLTEXT STOPLIST stoplist_name  
[ FROM { [ database_name.]source_stoplist_name } | SYSTEM STOPLIST ]  
[ AUTHORIZATION owner_name ]  
;  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *stoplist_name*  
 Nome dell'elenco di parole non significative. *stoplist_name* può essere composto da un massimo di 128 caratteri. *stoplist_name* deve essere univoco tra tutti gli elenchi di parole non significative nel database corrente e conforme alle regole per gli identificatori.  
  
 *stoplist_name* verrà usato dopo la creazione dell'indice full-text.  
  
 *database_name*  
 Nome del database in cui si trova l'elenco di parole non significative specificato da *source_stoplist_name*. Se l'argomento *database_name* non viene specificato, il valore predefinito è il database corrente.  
  
 *source_stoplist_name*  
 Specifica che il nuovo elenco di parole non significative viene creato copiando un elenco di parole non significative esistente. Se *source_stoplist_name* non esiste o l'utente del database non dispone di autorizzazioni corrette, CREATE FULLTEXT STOPLIST ha esito negativo e viene generato un errore. Se qualsiasi lingua specificata nelle parole non significative dell'elenco di parole non significative di origine non è registrata nel database corrente, CREATE FULLTEXT STOPLIST ha esito positivo, ma vengono restituiti avvisi e le parole non significative corrispondenti non vengono aggiunte.  
  
 SYSTEM STOPLIST  
 Specifica che il nuovo elenco di parole non significative viene creato dall'elenco di parole non significative esistente per impostazione predefinita nel [database delle risorse](../../relational-databases/databases/resource-database.md).  
  
 AUTHORIZATION *owner_name*  
 Specifica il nome di un'entità di database come proprietario dell'elenco di parole non significative. *owner_name* deve essere il nome di un'entità di cui l'utente corrente è membro oppure l'utente corrente deve avere l'autorizzazione IMPERSONATE per *owner_name*. Se viene omesso, la proprietà viene assegnata all'utente corrente.  
  
## <a name="remarks"></a>Commenti  
 L'autore di un elenco di parole non significative è il proprietario dell'elenco.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per creare un elenco di parole non significative sono necessarie le autorizzazioni CREATE FULLTEXT CATALOG. Il proprietario dell'elenco di parole non significative può concedere in modo esplicito l'autorizzazione CONTROL per un elenco per consentire agli utenti di aggiungere e rimuovere parole e di eliminare l'elenco.  
  
> [!NOTE]  
>  Per l'utilizzo di un elenco di parole non significative con un indice full-text è necessaria l'autorizzazione REFERENCE.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-new-full-text-stoplist"></a>R. Creazione di un nuovo elenco di parole non significative full-text  
 Nell'esempio seguente viene creato un nuovo elenco di parole non significative full-text denominato `myStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist;  
GO  
```  
  
### <a name="b-copying-a-full-text-stoplist-from-an-existing-full-text-stoplist"></a>B. Copia di un elenco di parole non significative full-text da un elenco di parole non significative full-text esistente  
 Nell'esempio seguente viene creato un nuovo elenco di parole non significative full-text denominato `myStoplist2` copiando un elenco di parole non significative di AdventureWorks esistente denominato `Customers.otherStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist2 FROM AdventureWorks.otherStoplist;  
GO  
```  
  
### <a name="c-copying-a-full-text-stoplist-from-the-system-full-text-stoplist"></a>C. Copia di un elenco di parole non significative full-text dall'elenco di parole non significative full-text di sistema  
 Nell'esempio seguente viene creato un nuovo elenco di parole non significative full-text denominato `myStoplist3` copiando l'elenco di parole non significative di sistema.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist3 FROM SYSTEM STOPLIST;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
