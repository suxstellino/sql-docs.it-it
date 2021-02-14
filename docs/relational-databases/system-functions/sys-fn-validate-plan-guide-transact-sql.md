---
description: sys.fn_validate_plan_guide (Transact-SQL)
title: sys.fn_validate_plan_guide (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_validate_plan_guide
- sys.fn_validate_plan_guide_TSQL
- fn_validate_plan_guide
- fn_validate_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_validate_plan_guide function
- sys.fn_validate_plan_guide function
ms.assetid: 3af8b47a-936d-4411-91d1-d2d16dda5623
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 557cbe292f5bfe901acc9561f346611005d391f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338131"
---
# <a name="sysfn_validate_plan_guide-transact-sql"></a>sys.fn_validate_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verifica la validità della guida di piano specificata. La funzione sys.fn_validate_plan_guide restituisce il primo messaggio di errore rilevato quando la guida di piano viene applicata alla query. Se la guida di piano è valida viene restituito un set di righe vuoto. Le guide di piano possono diventare non valide dopo aver apportato modifiche alla progettazione fisica del database. Ad esempio, se una guida di piano specifica un particolare indice che viene successivamente eliminato, la query non sarà più in grado di utilizzare la guida di piano.  
  
 Convalidando una guida di piano, è possibile determinare se la guida può essere utilizzata dall'ottimizzatore senza modifiche. In base ai risultati della funzione, è possibile decidere di eliminare la guida di piano e regolare la query oppure modificare la progettazione del database, ad esempio ricreando l'indice specificato nella guida di piano.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.fn_validate_plan_guide ( plan_guide_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *plan_guide_id*  
 ID della Guida di piano come riportato nella vista del catalogo [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) . *plan_guide_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|msgnum|**int**|ID del messaggio di errore.|  
|severity|**tinyint**|Livello di gravità del messaggio, compreso tra 1 e 25.|  
|state|**smallint**|Numero di contesto dell'errore indicante il punto nel codice in cui si è verificato l'errore.|  
|message|**nvarchar(2048)**|Testo del messaggio di errore.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le guide di piano definite a livello di ambito di OBJECT richiedono l'autorizzazione VIEW DEFINITION o ALTER nell'oggetto a cui si fa riferimento e autorizzazioni per compilare la query o il batch forniti nella guida di piano. Ad esempio, se un batch contiene istruzioni SELECT, sono richieste autorizzazioni SELECT per gli oggetti a cui si fa riferimento.  
  
 Le guide di piano definite a livello di ambito di SQL o TEMPLATE richiedono l'autorizzazione ALTER per il database e autorizzazioni per compilare la query o il batch forniti nella guida di piano. Ad esempio, se un batch contiene istruzioni SELECT, sono richieste autorizzazioni SELECT per gli oggetti a cui si fa riferimento.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-validating-all-plan-guides-in-a-database"></a>R. Convalida di tutte le guide di piano in un database  
 Nell'esempio seguente viene verificata la validità di tutte le guide di piano nel database corrente. Se viene restituito un set di risultati vuoto, sono valide tutte le guide di piano.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT plan_guide_id, msgnum, severity, state, message  
FROM sys.plan_guides  
CROSS APPLY fn_validate_plan_guide(plan_guide_id);  
GO  
```  
  
### <a name="b-testing-plan-guide-validation-before-implementing-a-change-to-the-database"></a>B. Test della convalida della guida di piano prima di implementare una modifica nel database  
 Nell'esempio seguente viene utilizzata una transazione esplicita per eliminare un indice. `sys.fn_validate_plan_guide`Viene eseguita la funzione per determinare se questa azione invalida le guide di piano nel database. In base ai risultati della funzione, viene eseguito il commit dell'istruzione `DROP INDEX` o il rollback della transazione, l'indice non viene eliminato.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DROP INDEX IX_SalesOrderHeader_CustomerID ON Sales.SalesOrderHeader;  
-- Check for invalid plan guides.  
IF EXISTS (SELECT plan_guide_id, msgnum, severity, state, message  
           FROM sys.plan_guides  
           CROSS APPLY sys.fn_validate_plan_guide(plan_guide_id))  
    ROLLBACK TRANSACTION;  
ELSE  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Guide di piano](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
  
