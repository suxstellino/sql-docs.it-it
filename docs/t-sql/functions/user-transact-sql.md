---
description: USER (Transact-SQL)
title: USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER
- USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- system-supplied usernames [SQL Server]
- USER function
- users [SQL Server], database names
- names [SQL Server], database users
- database usernames [SQL Server]
ms.assetid: 82bbbd94-870c-4c43-9ed9-d9abc767a6be
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e81222292aa0bae66b96f5864d2ac7957f9c7742
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210645"
---
# <a name="user-transact-sql"></a>USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Consente l'inserimento in una tabella di un valore fornito dal sistema per il nome utente di database dell'utente corrente quando non è stato specificato alcun valore predefinito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
USER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **nvarchar(128)**  
  
## <a name="remarks"></a>Commenti  
 USER è caratterizzata dalle stesse funzionalità della funzione di sistema USER_NAME.  
  
 Utilizzare la funzione USER con vincoli DEFAULT nell'istruzione CREATE TABLE o ALTER TABLE oppure come qualsiasi funzione standard.  
  
 USER restituisce sempre il nome del contesto corrente. Se richiamata dopo un'istruzione EXECUTE AS, la funzione USER restituisce il nome del contesto rappresentato.  
  
 Se un'entità di Windows accede al database in base all'appartenenza a un gruppo, la funzione USER restituisce il nome dell'entità di Windows anziché il nome del gruppo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-user-to-return-the-database-user-name"></a>R. Utilizzo della funzione USER per recuperare il nome dell'utente del database  
 Nell'esempio seguente viene dichiarata una variabile di tipo `char`, viene assegnato il valore corrente di USER alla variabile e vengono restituite la variabile e una descrizione.  
  
```sql
DECLARE @usr CHAR(30)  
SET @usr = user  
SELECT 'The current user''s database username is: '+ @usr  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------------------------------------------------------  
The current user's database username is: dbo  
  
(1 row(s) affected)
```  
  
### <a name="b-using-user-with-default-constraints"></a>B. Utilizzo della funzione USER con vincoli DEFAULT  
 Nell'esempio seguente viene creata una tabella tramite l'utilizzo della funzione `USER` come vincolo `DEFAULT` per la colonna relativa al rappresentante di una riga di dati sulle vendite.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE inventory22  
(  
 part_id INT IDENTITY(100, 1) NOT NULL,  
 description VARCHAR(30) NOT NULL,  
 entry_person VARCHAR(30) NOT NULL DEFAULT USER   
)  
GO  
INSERT inventory22 (description)  
VALUES ('Red pencil')  
INSERT inventory22 (description)  
VALUES ('Blue pencil')  
INSERT inventory22 (description)  
VALUES ('Green pencil')  
INSERT inventory22 (description)  
VALUES ('Black pencil')  
INSERT inventory22 (description)  
VALUES ('Yellow pencil')  
GO  
```  
  
 Di seguito è riportata la query per la selezione di tutte le informazioni della tabella `inventory22`.  
  
```sql
SELECT * FROM inventory22 ORDER BY part_id;  
GO  
```  
  
 Set di risultati (si noti il valore `entry-person`).  
  
 ```
part_id     description                    entry_person
----------- ------------------------------ -------------------------
100         Red pencil                     dbo
101         Blue pencil                    dbo
102         Green pencil                   dbo
103         Black pencil                   dbo
104         Yellow pencil                  dbo
  
(5 row(s) affected)
```  
  
### <a name="c-using-user-in-combination-with-execute-as"></a>C. Utilizzo della funzione USER in combinazione con EXECUTE AS  
 Nell'esempio seguente viene illustrato il funzionamento di `USER` se chiamata all'interno di una sessione rappresentata.  
  
```sql
SELECT USER;  
GO  
EXECUTE AS USER = 'Mario';  
GO  
SELECT USER;  
GO  
REVERT;  
GO  
SELECT USER;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO
Mario
DBO
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)  
  
  

