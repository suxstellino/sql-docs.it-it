---
description: SUSER_SID (Transact-SQL)
title: SUSER_SID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_SID
- SUSER_SID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], users
- SIDs [SQL Server]
- security identifiers [SQL Server]
- users [SQL Server], logins
- 15401 (Database Engine error)
- IDs [SQL Server], logins
- identification numbers [SQL Server], logins
- SUSER_SID function
ms.assetid: 57b42a74-94e1-4326-85f1-701b9de53c7d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 542c742b1d787a8b7991217a1a73565ac6b58ab2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207919"
---
# <a name="suser_sid-transact-sql"></a>SUSER_SID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce l'ID di sicurezza (SID) per il nome dell'account di accesso specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SUSER_SID ( [ 'login' ] [ , Param2 ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *login* **'**  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Nome dell'account di accesso dell'utente. *login* è di tipo **sysname**. Il parametro facoltativo *login* può essere un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure un utente o un gruppo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Se *login* viene omesso, vengono restituite informazioni sul contesto di sicurezza corrente. Se nel parametro è inclusa la parola NULL, verrà restituito NULL.  
  
 *Param2*  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
 Specifica se il nome di accesso viene convalidato. *Param2* è di tipo **int** ed è facoltativo. Se *Param2* è 0, il nome di account di accesso non è convalidato. Se per *Param2* non è specificato 0, il nome di accesso di Windows viene considerato come identico al nome di account di accesso archiviato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-types"></a>Tipi restituiti  
 **varbinary(85)**  
  
## <a name="remarks"></a>Osservazioni  
 La funzione SUSER_SID può essere utilizzata come vincolo DEFAULT nell'istruzione ALTER TABLE o CREATE TABLE. È possibile utilizzare SUSER_SID in un elenco di selezione, in una clausola WHERE e in tutti i casi in cui è consentita un'espressione. SUSER_SID deve essere sempre seguita dalle parentesi, anche in assenza di parametri.  
  
 Se viene richiamata senza alcun argomento, la funzione SUSER_SID restituisce il SID del contesto di sicurezza corrente. Se richiamata senza alcun argomento all'interno di un batch per il quale il contesto è stato cambiato tramite EXECUTE AS, la funzione SUSER_SID restituisce il SID del contesto rappresentato. Se chiamata da un contesto rappresentato, SUSER_SID(ORIGINAL_LOGIN()) restituisce il SID del contesto originale.  
  
 Se le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quelle di Windows sono diverse, è possibile che SUSER_SID abbia esito negativo, qualora in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Windows l'account di accesso venga archiviato in un diverso formato. Se ad esempio l'account di accesso del computer Windows TestComputer è User e in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'account di accesso è archiviato come TESTCOMPUTER\User, è possibile che il nome di account di accesso non venga risolto correttamente durante la ricerca dell'account di accesso TestComputer\User. Per ignorare questa convalida del nome account di accesso, usare *Param2*. L'assegnazione di regole di confronto diverse spesso determina l'errore 15401 di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 `Windows NT user or group '%s' not found. Check the name again.`  
  
## <a name="sssdsfull-remarks"></a>[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Osservazioni  
 SUSER_SID restituisce sempre il SID dell'account di accesso per il contesto di sicurezza corrente. Usare [sys. database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) per ottenere il SID di un account di accesso diverso.
  
 L'istruzione SUSER_SID non supporta l'esecuzione con un contesto di sicurezza rappresentato tramite EXECUTE AS.  

## <a name="examples"></a>Esempi  
  
### <a name="a-using-suser_sid"></a>R. Utilizzo della funzione SUSER_SID  
 Nell'esempio seguente viene restituito l'ID di sicurezza (SID) per il contesto di sicurezza corrente.  
  
```sql
SELECT SUSER_SID();  
```  
  
### <a name="b-using-suser_sid-with-a-specific-login"></a>B. Utilizzo di SUSER_SID con un account di accesso specifico  
 Nell'esempio seguente viene restituito l'ID di sicurezza (SID) per l'account di accesso `sa` di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
```sql
SELECT SUSER_SID('sa');  
GO  
```  
  
### <a name="c-using-suser_sid-with-a-windows-user-name"></a>C. Utilizzo della funzione SUSER_SID con un nome utente di Windows  
 Nell'esempio seguente viene restituito l'ID di sicurezza (SID) per l'utente di Windows `London\Workstation1`.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
```sql
SELECT SUSER_SID('London\Workstation1');  
GO  
```  
  
### <a name="d-using-suser_sid-as-a-default-constraint"></a>D. Utilizzo della funzione SUSER_SID come vincolo DEFAULT  
 Nell'esempio seguente la funzione `SUSER_SID` viene utilizzata come vincolo `DEFAULT` in un'istruzione `CREATE TABLE`.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE sid_example  
(  
login_sid   VARBINARY(85) DEFAULT SUSER_SID(),  
login_name  VARCHAR(30) DEFAULT SYSTEM_USER,  
login_dept  VARCHAR(10) DEFAULT 'SALES',  
login_date  DATETIME DEFAULT GETDATE()  
);   
GO  
INSERT sid_example DEFAULT VALUES;  
GO  
```  
  
### <a name="e-comparing-the-windows-login-name-to-the-login-name-stored-in-sql-server"></a>E. Confronto tra il nome di account di accesso di Windows e il nome di account di accesso archiviato in SQL Server  
 Nell'esempio seguente viene illustrato come usare *Param2* per ottenere il SID da Windows e usarlo come input per la funzione `SUSER_SNAME`. In questo esempio viene fornito l'account di accesso nel formato archiviato in Windows (`TestComputer\User`) e viene restituito l'account di accesso nel formato nel quale è archiviato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`TESTCOMPUTER\User`).  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
```sql
SELECT SUSER_SNAME(SUSER_SID('TestComputer\User', 0));  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ORIGINAL_LOGIN &#40;Transact-SQL&#41;](../../t-sql/functions/original-login-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [binary e varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
