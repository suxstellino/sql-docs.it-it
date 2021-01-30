---
description: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER DATABASE SCOPED CREDENTIAL
- ALTER_DATABASE_SCOPED_CREDENTIAL_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], ALTER DATABASE SCOPED CREDENTIAL statement
ms.assetid: 966b75b5-ca87-4203-8bf9-95c4e00cb0b5
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 245e99b6a377b1026c6865ac16989daf8080960c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179212"
---
# <a name="alter-database-scoped-credential-transact-sql"></a>ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica le proprietà di una credenziale con ambito database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *credential_name*  
 Specifica il nome della credenziale con ambito database che si vuole modificare.  
  
 IDENTITY **='** _identity_name_*_'_*  
 Specifica il nome dell'account da utilizzare per la connessione all'esterno del server. Per importare un file dall'archiviazione BLOB di Azure, il nome dell'identità deve essere `SHARED ACCESS SIGNATURE`.  Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso](/azure/storage/storage-dotnet-shared-access-signature-part-1).  
    
  
 SECRET **='** _secret_*_'_*  
 Specifica il segreto richiesto per l'autenticazione in uscita. È necessario specificare *secret* per importare un file dall'archiviazione BLOB di Azure. *secret* può essere facoltativo per altri scopi.   
> [!WARNING]
>  Il valore della chiave di firma di accesso condiviso può iniziare con "?" (punto interrogativo). Quando si usa la chiave di firma di accesso condiviso, è necessario rimuovere il carattere "?" iniziale, altrimenti potrebbe verificarsi un blocco.    
  
## <a name="remarks"></a>Osservazioni  
 Quando si modifica una credenziale con ambito database, i valori di *identity_name* e *secret* vengono reimpostati. Se l'argomento facoltativo SECRET viene omesso, il valore del segreto archiviato verrà impostato su NULL.  
  
 Il segreto viene crittografato tramite la chiave master del servizio. Se la chiave master del servizio viene rigenerata, il segreto verrà crittografato nuovamente tramite la nuova chiave master del servizio.  
  
 Altre informazioni sulle credenziali con ambito database sono disponibili nella vista del catalogo [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione `ALTER` per la credenziale.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-password-of-a-database-scoped-credential"></a>R. Modifica della password di una credenziale con ambito database  
 Nell'esempio seguente viene modificato il segreto archiviato nella credenziale con ambito database denominata `Saddles`. La credenziale con ambito database include l'account di accesso Windows `RettigB` e la relativa password. La nuova password viene aggiunta alla credenziale con ambito database tramite la clausola SECRET.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. Rimozione della password da una credenziale  
 Nell'esempio seguente la password viene rimossa da una credenziale con ambito database denominata `Frames`. La credenziale con ambito database include l'account di accesso Windows `Aboulrus8` e una password. Dopo l'esecuzione dell'istruzione, la credenziale con ambito database includerà una password NULL perché l'opzione SECRET è stata omessa.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)   
 [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)   
 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
