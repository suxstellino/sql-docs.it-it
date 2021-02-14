---
description: DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
title: DROP SENSITIVITY CLASSIFICATION (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
ms.custom: ''
ms.author: giladm
author: giladmit
f1_keywords:
- DROP SENSITIVITY CLASSIFICATION
- DROP_SENSITIVITY_CLASSIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SENSITIVITY CLASSIFICATION statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
monikerRange: " >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 4c1eef43401cd55ef7983e620d3d2f73f221809c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340947"
---
# <a name="drop-sensitivity-classification-transact-sql"></a>DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Elimina i metadati di classificazione della riservatezza da una o più colonne di database.

## <a name="syntax"></a>Sintassi

```syntaxsql
DROP SENSITIVITY CLASSIFICATION FROM
    <object_name> [, ...n ]

<object_name> ::=
{
    [schema_name.]table_name.column_name
}
```  

## <a name="arguments"></a>Argomenti  

*object_name* ([schema_name.]table_name.column_name)

Si tratta del nome della colonna del database da cui rimuovere la classificazione. Attualmente è supportata solo la classificazione delle colonne.
    - *schema_name* (facoltativo): si tratta del nome dello schema a cui appartiene la colonna classificata.
    - *table_name*: si tratta del nome della tabella a cui appartiene la colonna classificata.
    - *column_name*: si tratta del nome della colonna da cui eliminare la classificazione.

## <a name="remarks"></a>Osservazioni  

- È possibile eliminare più classificazioni di oggetti usando una sola istruzione 'DROP SENSITIVITY CLASSIFICATION'.

## <a name="permissions"></a>Autorizzazioni  

È necessaria l'autorizzazione ALTER ANY SENSITIVITY CLASSIFICATION. L'autorizzazione ALTER ANY SENSITIVITY CLASSIFICATION è implicita nell'autorizzazione del database ALTER o nell'autorizzazione del server CONTROL SERVER.


## <a name="examples"></a>Esempi  


### <a name="a-dropping-classification-from-a-single-column"></a>R. Eliminazione della classificazione da una singola colonna

Nell'esempio seguente la classificazione viene rimossa dalla colonna `dbo.sales.price`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price
```

### <a name="b-dropping-classification-from-multiple-columns"></a>B. Eliminazione della classificazione da più colonne

Nell'esempio seguente la classificazione viene rimossa dalle colonne `dbo.sales.price`, `dbo.sales.discount` e `SalesLT.Customer.Phone`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price, dbo.sales.discount, SalesLT.Customer.Phone  
```

## <a name="see-also"></a>Vedere anche  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)

[Introduzione a SQL Information Protection](/azure/azure-sql/database/data-discovery-and-classification-overview)