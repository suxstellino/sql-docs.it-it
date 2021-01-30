---
description: ALTER CREDENTIAL (Transact-SQL)
title: ALTER CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER CREDENTIAL
- ALTER_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], credentials
- credentials [SQL Server], ALTER CREDENTIAL statement
- modifying credentials
- authentication [SQL Server], credentials
- ALTER CREDENTIAL statement
ms.assetid: b08899a6-c09e-4af4-91aa-a978ada79264
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 3ae2deff1638c7316fdaef9c3923fa5e2108404f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191054"
---
# <a name="alter-credential-transact-sql"></a>ALTER CREDENTIAL (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà di una credenziale.  

> [!IMPORTANT]
> Info "consigliabile" come procedura consigliata; "da fare" per completare l'attività ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
ALTER CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *credential_name*  
 Specifica il nome della credenziale che si desidera modificare.  
  
 IDENTITY **='** _identity_name_*_'_*  
 Specifica il nome dell'account da utilizzare per la connessione all'esterno del server.  
  
 SECRET **='** _secret_*_'_*  
 Specifica il segreto richiesto per l'autenticazione in uscita. *secret* è facoltativo.
  
> [!IMPORTANT]
> Database SQL di Azure supporta solo le identità Azure Key Vault e di firma di accesso condiviso. Le identità utente di Windows non sono supportate.
  
## <a name="remarks"></a>Commenti  
 Quando si modifica una credenziale, i valori di *identity_name* e *secret* vengono reimpostati. Se l'argomento facoltativo SECRET viene omesso, il valore del segreto archiviato verrà impostato su NULL.  
  
 Il segreto viene crittografato tramite la chiave master del servizio. Se la chiave master del servizio viene rigenerata, il segreto verrà crittografato nuovamente tramite la nuova chiave master del servizio.  
  
 Le informazioni sulle credenziali sono visibili nella vista del catalogo **sys.credentials**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY CREDENTIAL. Se la credenziale è una credenziale di sistema, è richiesta l'autorizzazione CONTROL SERVER.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-password-of-a-credential"></a>R. Modifica della password di una credenziale  
 Nell'esempio seguente viene modificato il segreto archiviato nella credenziale denominata `Saddles`. La credenziale include l'account di accesso di Windows `RettigB` e la relativa password. La nuova password viene aggiunta alla credenziale tramite la clausola SECRET.  
  
```sql  
ALTER CREDENTIAL Saddles WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. Rimozione della password da una credenziale  
 Nell'esempio seguente la password viene rimossa da una credenziale denominata `Frames`. La credenziale include l'account di accesso di Windows `Aboulrus8` e una password. Dopo l'esecuzione dell'istruzione, la credenziale includerà una password NULL perché l'opzione SECRET è stata omessa.  
  
```sql  
ALTER CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)   
 [ALTER DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
  
