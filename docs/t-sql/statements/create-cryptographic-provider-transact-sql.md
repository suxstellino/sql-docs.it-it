---
description: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_CRYPTOGRAPHIC_TSQL
- CRYPTOGRAPHIC PROVIDER
- PROVIDER_TSQL
- CREATE CRYPTOGRAPHIC
- CREATE CRYPTOGRAPHIC PROVIDER
- CRYPTOGRAPHIC_PROVIDER_TSQL
- CREATE_CRYPTOGRAPHIC_PROVIDER_TSQL
- PROVIDER
dev_langs:
- TSQL
helpviewer_keywords:
- 33085 (Database Engine error)
- CREATE CRYPTOGRAPHIC PROVIDER statement
- 33032 (Database Engine error)
ms.assetid: 059a39a6-9d32-4d3f-965b-0a1ce75229c7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 15793074e9aca337f2d1b1fe93ae3e603b518c01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192750"
---
# <a name="create-cryptographic-provider-transact-sql"></a>CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un provider del servizio di crittografia in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un provider EKM (Extensible Key Management).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
CREATE CRYPTOGRAPHIC PROVIDER provider_name   
    FROM FILE = path_of_DLL  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *provider_name*  
 Nome del provider EKM.  
  
 *path_of_DLL*  
 Percorso del file dll che implementa l'interfaccia EKM di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando si usa **Connettore SQL Server per Microsoft Azure Key Vault**, il percorso predefinito è **"C:\Programmi\Microsoft SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"**.  
  
## <a name="remarks"></a>Osservazioni  
 Tutte le chiavi create da un provider faranno riferimento al provider attraverso il GUID. Il GUID viene mantenuto per tutte le versioni della DLL.  
  
 Alla DLL che implementa l'interfaccia SQLEKM deve essere applicata una firma digitale usando qualsiasi certificato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verificherà la firma. Include la catena di certificati, la cui radice deve essere installata nel percorso **Autorità di certificazione radice attendibili** in un sistema Windows. Se la firma non viene verificata correttamente, l'istruzione CREATE CRYPTOGRAPHIC PROVIDER avrà esito negativo. Per altre informazioni sui certificati e sulle catene di certificati, vedere [Certificati SQL Server e chiavi simmetriche](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
 Quando la DLL di un provider EKM non implementa tutti i metodi necessari, CREATE CRYPTOGRAPHIC PROVIDER può restituire l'errore 33085:  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
 Quando il file di intestazione utilizzato per creare la DLL del provider EKM non è aggiornato, CREATE CRYPTOGRAPHIC PROVIDER può restituire l'errore 33032:  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL SERVER o l'appartenenza al ruolo predefinito del server **sysadmin**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creato un provider del servizio di crittografia denominato `SecurityProvider` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un file DLL. Il file dll è denominato `c:\SecurityProvider\SecurityProvider_v1.dll` ed è installato nel server. Il certificato del provider deve prima essere installato nel server.  
  
```sql  
-- Install the provider  
CREATE CRYPTOGRAPHIC PROVIDER SecurityProvider  
    FROM FILE = 'C:\SecurityProvider\SecurityProvider_v1.dll';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [ALTER CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Configurare Extensible Key Management TDE di SQL Server usando Azure Key Vault](../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)  
 [sys.cryptographic_providers](../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)  
 [sys.dm_cryptographic_provider_properties](../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-properties-transact-sql.md)
  
  
