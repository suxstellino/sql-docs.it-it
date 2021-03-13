---
description: DROP ASYMMETRIC KEY (Transact-SQL)
title: DROP ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ASYMMETRIC KEY
- DROP_ASYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], removing
- removing asymmetric keys
- encryption [SQL Server], asymmetric keys
- DROP ASYMMETRIC KEY statement
- dropping asymmetric keys
- deleting asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: bf94ac07-9b62-4318-b55b-1eed8f3a1ac6
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 072e09f359ce45a2ede9531dd4e622fa71ea0df7
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103422039"
---
# <a name="drop-asymmetric-key-transact-sql"></a>DROP ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Rimuove una chiave asimmetrica dal database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP ASYMMETRIC KEY key_name [ REMOVE PROVIDER KEY ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *key_name*  
 Nome della chiave asimmetrica da rimuovere dal database.  
  
 REMOVE PROVIDER KEY  
 Rimuove una chiave EKM (Extensible Key Management ) da un dispositivo EKM. Per altre informazioni su Extensible Key Management, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Commenti  
 Non è possibile rimuovere una chiave asimmetrica con cui è stata crittografata una chiave simmetrica nel database o a cui è stato eseguito il mapping di un utente o un account di accesso. Prima di rimuovere questo tipo di chiave, è necessario rimuovere qualsiasi utente o account di accesso di cui è eseguito il mapping alla chiave, nonché rimuovere o modificare qualsiasi chiave simmetrica crittografata con la chiave asimmetrica. È possibile usare l'opzione DROP ENCRYPTION di [ALTER SYMMETRIC KEY](../../t-sql/statements/alter-symmetric-key-transact-sql.md) per rimuovere la crittografia eseguita con una chiave asimmetrica.  
  
 È possibile accedere ai metadati delle chiavi asimmetriche usando la vista del catalogo [sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md). Le chiavi non possono essere visualizzate direttamente dall'interno del database.  
  
 Se sulla chiave asimmetrica viene eseguito il mapping a una chiave EKM (Extensible Key Management) in un dispositivo EKM e l'opzione REMOVE PROVIDER KEY non è specificata, la chiave verrà rimossa dal database ma non dal dispositivo e verrà generato un avviso.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per la chiave asimmetrica.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene rimossa la chiave asimmetrica `MirandaXAsymKey6` dal database `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
DROP ASYMMETRIC KEY MirandaXAsymKey6;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)  
  
  
