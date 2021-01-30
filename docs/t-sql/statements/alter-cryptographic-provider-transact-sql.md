---
description: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CRYPTOGRAPHIC_PROVIDER_TSQL
- ALTER CRYPTOGRAPHIC PROVIDER
- ALTER_CRYPTOGRAPHIC_TSQL
- ALTER CRYPTOGRAPHIC
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER CRYPTOGRAPHIC PROVIDER
ms.assetid: 876b6348-fb29-49e1-befc-4217979f6416
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6abe4053b87b6417bb1c55708815bc4cc91b2ba0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201152"
---
# <a name="alter-cryptographic-provider-transact-sql"></a>ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica un provider di crittografia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un provider EKM (Extensible Key Management).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
ALTER CRYPTOGRAPHIC PROVIDER provider_name   
    [ FROM FILE = path_of_DLL ]  
    ENABLE | DISABLE  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *provider_name*  
 Nome del provider EKM.  
  
 *Path_of_DLL*  
 Percorso del file DLL che implementa l'interfaccia EKM di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ENABLE | DISABLE  
 Abilita o disabilita un provider.  
  
## <a name="remarks"></a>Commenti  
 Se il provider modifica il file DLL utilizzato per l'implementazione di EKM (Extensible Key Management) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario utilizzare l'istruzione ALTER CRYPTOGRAPHIC PROVIDER.  
  
 Quando il percorso del file DLL viene aggiornato utilizzando l'istruzione ALTER CRYPTOGRAPHIC PROVIDER, tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono effettuate le azioni seguenti:  
-   Disabilitazione del provider.  
-   Verifica della firma DLL e del fatto che il GUID del file DLL sia uguale a quello registrato nel catalogo.  
-   Aggiornamento della versione DLL nel catalogo.  
  

Quando un provider EKM viene impostato su DISABLE, qualsiasi tentativo effettuato su nuove connessioni di utilizzare il provider con istruzioni di crittografia avrà esito negativo.  
  
Per disabilitare un provider, è necessario terminare tutte le sessioni che lo utilizzano.  
  
Quando la dll di un provider EKM non implementa tutti i metodi necessari, ALTER CRYPTOGRAPHIC PROVIDER può restituire l'errore 33085:  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
Quando il file di intestazione utilizzato per creare la dll del provider EKM non è aggiornato, ALTER CRYPTOGRAPHIC PROVIDER può restituire l'errore 33032:  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il provider di crittografia.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificato un provider di crittografia denominato `SecurityProvider` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una versione più recente di un file DLL. La nuova versione è denominata `c:\SecurityProvider\SecurityProvider_v2.dll` e viene installata nel server. Il certificato del provider deve essere installato nel server.  
  
1. Per eseguire l'aggiornamento, disabilitare il provider. Tutte le sessioni di crittografia aperte verranno terminate.  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
DISABLE;  
GO  
```  

2. Aggiornare il file DLL del provider. Il GUID deve corrispondere a quello della versione precedente, ma la versione può essere diversa.  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider  
FROM FILE = 'c:\SecurityProvider\SecurityProvider_v2.dll';  
GO  
```  

3. Abilitare il provider aggiornato.   
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
ENABLE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
