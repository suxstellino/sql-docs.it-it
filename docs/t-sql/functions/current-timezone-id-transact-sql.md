---
description: CURRENT_TIMEZONE_ID (Transact-SQL)
title: CURRENT_TIMEZONE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TIMEZONE)ID
- CURRENT_TIMEZONE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- current time zone id [SQL Server]
- current timezoneid [SQL Server]
- system time zone id[SQL Server]
- system timezone id[SQL Server]
- functions [SQL Server], time zone id
- functions [SQL Server], timezoneid
- timezoneid [SQL Server], functions
- time zone id [SQL Server], functions
- CURRENT_TIMEZONE_ID function [SQL Server]
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 70f4fbb5e683aa7bc6436b27a727f2c3952b6592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184053"
---
# <a name="current_timezone_id-transact-sql"></a>CURRENT_TIMEZONE_ID (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Questa funzione restituisce l'ID del fuso orario osservato da un server o un'istanza. Per Istanza gestita di Azure SQL, il valore restituito è basato sul fuso orario dell'istanza stessa assegnato durante la creazione dell'istanza e non sul fuso orario del sistema operativo sottostante.
  
> [!NOTE]  
> Per i database SQL, il fuso orario è sempre impostato su UTC e `CURRENT_TIMEZONE_ID` restituisce l'ID del fuso orario UTC.
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CURRENT_TIMEZONE_ID ( )  
```
  
## <a name="arguments"></a>Argomenti

Questa funzione non accetta argomenti
  
## <a name="return-type"></a>Tipo restituito  

**varchar**
  
## <a name="remarks"></a>Osservazioni  

`CURRENT_TIMEZONE_ID` è una funzione non deterministica. Le viste e le espressioni in cui viene fatto riferimento a questa colonna non sono indicizzabili.
  
## <a name="example"></a>Esempio

Il valore restituito riflette il fuso orario effettivo e le impostazioni della lingua del server o dell'istanza.

```sql
SELECT CURRENT_TIMEZONE_ID();  
/* Returned:  
W. Europe Standard Time
*/
```  
  
## <a name="see-also"></a>Vedere anche

[Fuso orario di Istanza gestita di SQL](/azure/sql-database/sql-database-managed-instance-timezone)

[CURRENT_TIMEZONE()](./current-timezone-transact-sql.md)