---
description: DROP EXTERNAL TABLE (Transact-SQL)
title: DROP EXTERNAL TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 02a6a236-0756-4570-abfa-6f677a7df042
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 26b9a14f07b3e3ebaf7d08484c0dba21e8e2eae4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752871"
---
# <a name="drop-external-table-transact-sql"></a>DROP EXTERNAL TABLE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Rimuove una tabella esterna PolyBase da un database, ma non elimina i dati esterni.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  

## <a name="arguments"></a>Argomenti  
 `[ database_name . [schema_name] . | schema_name . ] table_name`  
 Numero della tabella esterna da rimuovere, composto da una, due o tre parti. Il nome della tabella può includere facoltativamente lo schema o il database e lo schema.  
  
## <a name="permissions"></a>Autorizzazioni  
  
-   Richiede l'autorizzazione **ALTER** per lo schema a cui appartiene la tabella.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 L'eliminazione di una tabella esterna elimina tutti i metadati associati alla tabella. Non elimina i dati esterni.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-basic-syntax"></a>R. Uso della sintassi di base  
  
```sql  
DROP EXTERNAL TABLE SalesPerson;  
DROP EXTERNAL TABLE dbo.SalesPerson;  
DROP EXTERNAL TABLE EasternDivision.dbo.SalesPerson;  
```  
  
### <a name="b-dropping-an-external-table-from-the-current-database"></a>B. Eliminazione di una tabella esterna dal database corrente  
 Nell'esempio seguente vengono eliminati dal database corrente la tabella `ProductVendor1`, e i relativi dati, indici e visualizzazioni dipendenti.  
  
```sql  
DROP EXTERNAL TABLE ProductVendor1;  
```  
  
### <a name="c-dropping-a-table-from-another-database"></a>C. Eliminazione di una tabella da un database diverso da quello corrente  
 Nell'esempio seguente viene eliminata la tabella `SalesPerson` nel database `EasternDivision`.  
  
```sql  
DROP EXTERNAL TABLE EasternDivision.dbo.SalesPerson;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
