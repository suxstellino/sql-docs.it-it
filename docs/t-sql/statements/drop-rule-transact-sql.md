---
description: DROP RULE (Transact-SQL)
title: DROP RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_RULE_TSQL
- DROP RULE
dev_langs:
- TSQL
helpviewer_keywords:
- rules [SQL Server], removing
- deleting roles
- DROP RULE statement
- removing roles
- dropping roles
ms.assetid: 8370b730-7fd5-43fe-a7f6-8300b3caa16d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 81dd188416fd57274dbfd2ba587ddd9fdde1f51c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172603"
---
# <a name="drop-rule-transact-sql"></a>DROP RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove una o più regole definite dall'utente dal database corrente.  
  
> [!IMPORTANT]
>  L'istruzione DROP RULE verrà rimossa nella prossima versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non utilizzare l'istruzione DROP RULE e pianificare la modifica delle applicazioni che ne fanno uso. Usare invece i vincoli CHECK, che possono essere creati tramite la parola chiave CHECK nelle istruzioni [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md). Per altre informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP RULE [ IF EXISTS ] { [ schema_name . ] rule_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *IF EXISTS*  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] alla [versione corrente](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Rimuove in modo condizionale la regola solo se esiste già.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la regola.  
  
 *rule*  
 Regola che si desidera rimuovere. I nomi di regola devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome dello schema della regola è facoltativo.  
  
## <a name="remarks"></a>Commenti  
 Per eliminare una regola associata a una colonna o a un tipo di dati alias, è innanzitutto necessario disassociarla. Per disassociare la regola, utilizzare la stored procedure **sp_unbindrule**. Se si tenta di eliminare una regola prima di disassociarla, viene visualizzato un messaggio di errore e l'istruzione DROP RULE viene annullata.  
  
 Dopo l'eliminazione di una regola, i nuovi dati immessi in colonne precedentemente governate dalla regola vengono inseriti senza i vincoli della regola. I dati esistenti non vengono alterati in alcun modo.  
  
 L'istruzione DROP RULE non ha alcun effetto sui vincoli CHECK. Per altre informazioni sull'eliminazione dei vincoli CHECK, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione DROP RULE, è necessario disporre almeno dell'autorizzazione ALTER per lo schema a cui appartiene la regola.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene disassociata e quindi eliminata la regola `VendorID_rule`. 
  
```sql  
sp_unbindrule 'Production.ProductVendor.VendorID'  
DROP RULE VendorID_rule  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  

