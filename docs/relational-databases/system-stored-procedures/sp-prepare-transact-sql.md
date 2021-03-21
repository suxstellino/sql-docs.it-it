---
description: sp_prepare (Transact SQL)
title: sp_prepare (Transact SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepare
ms.assetid: f328c9eb-8211-4863-bafa-347e1bf7bb3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 017009d5dd3d22c8bba1eed67f5a8ebe01d87ed7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754981"
---
# <a name="sp_prepare-transact-sql"></a>sp_prepare (Transact SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Prepara un'istruzione con parametri [!INCLUDE[tsql](../../includes/tsql-md.md)] e restituisce un *handle* di istruzione per l'esecuzione.  `sp_prepare` viene richiamato specificando ID = 11 in un pacchetto TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
sp_prepare handle OUTPUT, params, stmt, options  
```  
  
## <a name="arguments"></a>Argomenti  
 *gestire*  
 Identificatore dell' *handle preparato* generato da SQL Server. *handle* è un parametro obbligatorio con un valore restituito **int** .  
  
 *params*  
 Identifica le istruzioni con parametri. La definizione *params* delle variabili viene sostituita per i marcatori di parametro nell'istruzione. *params* è un parametro obbligatorio che richiede un valore di input **ntext**, **nchar** o **nvarchar** . Immettere un valore NULL se l'istruzione non è con parametri.  
  
 *stmt*  
 Definisce il set di risultati del cursore. Il parametro *stmt* è obbligatorio e richiede un valore di input **ntext**, **nchar** o **nvarchar** .  
  
 *options*  
 Parametro facoltativo tramite cui viene restituita una descrizione delle colonne dei set di risultati del cursore. *options* richiede il valore di input int seguente:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
## <a name="examples"></a>Esempi  
R. Nell'esempio seguente viene preparata ed eseguita un'istruzione semplice.  
  
```sql  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name=@P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```

B. Nell'esempio seguente viene preparata un'istruzione nel database AdventureWorks2016 e successivamente viene eseguita utilizzando l'handle.

```sql
-- Prepare query
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@Param INT',  
    N'SELECT *
FROM Sales.SalesOrderDetail AS sod
INNER JOIN Production.Product AS p ON sod.ProductID = p.ProductID
WHERE SalesOrderID = @Param
ORDER BY Style DESC;';  

-- Return handle for calling application
SELECT @P1;
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
-----------
1

(1 row affected)
```

L'applicazione esegue quindi la query due volte usando il valore di handle 1, prima di scartare il piano preparato.

```sql
EXEC sp_execute 1, 49879;  
GO

EXEC sp_execute 1, 48766;
GO

EXEC sp_unprepare 1; 
GO
```
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

