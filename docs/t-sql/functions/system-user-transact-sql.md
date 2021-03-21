---
description: SYSTEM_USER (Transact-SQL)
title: SYSTEM_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSTEM_USER_TSQL
- SYSTEM_USER
dev_langs:
- TSQL
helpviewer_keywords:
- current user names
- system-supplied user names [SQL Server]
- users [SQL Server], logins
- logins [SQL Server], identification name
- current system user names
- SYSTEM_USER function
- inserting system user name into table
- system usernames [SQL Server]
- users [SQL Server], names
ms.assetid: 565984cd-60c6-4df7-83ea-2349b838ccb2
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8a718ab0e86ed0fa8bafdca88968760a3c103cca
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754501"
---
# <a name="system_user-transact-sql"></a>SYSTEM_USER (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Consente di inserire in una tabella un valore fornito dal sistema per l'account di accesso corrente quando non è specificato alcun valore predefinito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SYSTEM_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare la funzione SYSTEM_USER in combinazione con i vincoli DEFAULT nelle istruzioni CREATE TABLE e ALTER TABLE, nonché come qualsiasi funzione standard.  
  
 Se il nome utente e il nome dell'account di accesso sono diversi, SYSTEM_USER restituisce il nome dell'account di accesso.  
  
 Se l'utente corrente ha eseguito l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando l'autenticazione di Windows, SYSTEM_USER restituisce il nome di identificazione dell'account di accesso Windows nel formato: *DOMAIN*\\*nome_account_utente*. Se l'utente invece è connesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'autenticazione di SQL Server, SYSTEM_USER restituisce il nome dell'identificazione dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio `WillisJo` per un utente connesso come `WillisJo`.  
  
 SYSTEM_USER restituisce il nome del contesto di esecuzione corrente. Se l'istruzione EXECUTE AS è stata utilizzata per cambiare contesto, SYSTEM_USER restituirà il nome del contesto rappresentato.  

 Non è possibile usare EXECUTE AS per SYSTEM_USER.

## <a name="examples"></a>Esempi  
  
### <a name="a-using-system_user-to-return-the-current-system-user-name"></a>R. Utilizzo di SYSTEM_USER per recuperare il nome utente di sistema corrente  
 Nell'esempio seguente viene dichiarata una variabile `char`, il valore corrente di `SYSTEM_USER` viene archiviato nella variabile, quindi viene visualizzato il valore archiviato nella variabile.  
  
```sql
DECLARE @sys_usr CHAR(30);  
SET @sys_usr = SYSTEM_USER;  
SELECT 'The current system user is: '+ @sys_usr;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
----------------------------------------------------------
The current system user is: WillisJo

(1 row(s) affected)
 ```  
  
### <a name="b-using-system_user-with-default-constraints"></a>B. Utilizzo di SYSTEM_USER con vincoli DEFAULT  
 Nell'esempio seguente viene creata una tabella con `SYSTEM_USER` come vincolo `DEFAULT` per la colonna `SRep_tracking_user`.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.Sales_Tracking  
(  
    Territory_id INT IDENTITY(2000, 1) NOT NULL,  
    Rep_id INT NOT NULL,  
    Last_sale DATETIME NOT NULL DEFAULT GETDATE(),  
    SRep_tracking_user VARCHAR(30) NOT NULL DEFAULT SYSTEM_USER  
);  
GO  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (151);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (293, '19980515');  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (27882, '19980620');  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (21392);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (24283, '19981130');  
GO  
```  
  
 La query seguente consente di selezionare tutte le informazioni della tabella `Sales_Tracking`.  
  
```sql
SELECT * FROM Sales_Tracking ORDER BY Rep_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Territory_id Rep_id Last_sale            SRep_tracking_user
-----------  ------ -------------------- ------------------
2000         151    Mar 4 1998 10:36AM   ArvinDak
2001         293    May 15 1998 12:00AM  ArvinDak
2003         21392  Mar 4 1998 10:36AM   ArvinDak
2004         24283  Nov 3 1998 12:00AM   ArvinDak
2002         27882  Jun 20 1998 12:00AM  ArvinDak
  
(5 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-system_user-to-return-the-current-system-user-name"></a>C: Utilizzo di SYSTEM_USER per recuperare il nome utente di sistema corrente  
 L'esempio seguente restituisce il valore corrente di `SYSTEM_USER`.  
  
```sql
SELECT SYSTEM_USER;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [USER &#40;Transact-SQL&#41;](../../t-sql/functions/user-transact-sql.md)  
  
  

