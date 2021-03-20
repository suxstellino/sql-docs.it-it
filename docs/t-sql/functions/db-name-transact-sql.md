---
description: DB_NAME (Transact-SQL)
title: DB_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DB_NAME
- DB_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database names [SQL Server], DB_NAME
- names [SQL Server], databases
- viewing database names
- displaying database names
- DB_NAME function
ms.assetid: e21fb33a-a3ea-49b0-bb6b-8f789a675a0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 111827a33c9e7b65a729f09616ce863e1c658cf4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752051"
---
# <a name="db_name-transact-sql"></a>DB_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il nome di un database specificato.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DB_NAME ( [ database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*database_id*  

Numero di identificazione (ID) del database di cui verrà restituito il nome `DB_NAME`. Se la chiamata a `DB_NAME` omette *database_id*, `DB_NAME` restituisce il nome del database corrente.
  
## <a name="return-types"></a>Tipi restituiti
**nvarchar(128)**
  
## <a name="permissions"></a>Autorizzazioni  

Se il chiamante di `DB_NAME` non è proprietario di un database non **master** o non **tempdb** specifico, sono necessarie almeno le autorizzazioni a livello di server `ALTER ANY DATABASE` o `VIEW ANY DATABASE` per visualizzare la riga `DB_ID` corrispondente. Per il database **master**, `DB_ID` necessita almeno dell'autorizzazione `CREATE DATABASE`. Il database a cui si connette il chiamante verrà sempre visualizzato in **sys.databases**.
  
> [!IMPORTANT]  
>  Per impostazione predefinita, il ruolo public ha l'autorizzazione `VIEW ANY DATABASE`, che consente a tutti gli account di accesso di visualizzare informazioni sul database. Per impedire a un account di accesso di rilevare un database, usare `REVOKE` per revocare l'autorizzazione `VIEW ANY DATABASE` da public o `DENY` per negare l'autorizzazione `VIEW ANY DATABASE` per i singoli account di accesso.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-current-database-name"></a>R. Restituzione del nome del database corrente  
Questo esempio restituisce il nome del database corrente.
  
```sql
SELECT DB_NAME() AS [Current Database];  
GO  
```  
  
### <a name="b-returning-the-database-name-of-a-specified-database-id"></a>B. Restituzione del nome di un database con l'ID specificato  
Questo esempio restituisce il nome del database con l'ID `3`.
  
```sql
USE master;  
GO  
SELECT DB_NAME(3) AS [Database Name];  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-return-the-current-database-name"></a>C. Restituzione del nome del database corrente  
  
```sql
SELECT DB_NAME() AS [Current Database];  
```  
  
### <a name="d-return-the-name-of-a-database-by-using-the-database-id"></a>D. Restituzione del nome di un database usando l'ID del database  
Questo esempio restituisce il nome e il valore database_id di ogni database.
  
```sql
SELECT DB_NAME(database_id) AS [Database], database_id  
FROM sys.databases;  
```  
  
## <a name="see-also"></a>Vedi anche
[DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)  
[Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
  
  

