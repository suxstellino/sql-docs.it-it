---
description: DROP SIGNATURE (Transact-SQL)
title: DROP SIGNATURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SIGNATURE
- DROP_SIGNATURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting signatures
- dropping signatures
- DROP SIGNATURE statement
- removing signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 8a1fd8c5-0e75-4b2f-9d3c-c296bed56cc7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: fe872b1860091178dadf181fb47c1b887693079c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210956"
---
# <a name="drop-signature-transact-sql"></a>DROP SIGNATURE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Elimina una firma digitale da una stored procedure, una funzione, un trigger o un assembly.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP [ COUNTER ] SIGNATURE FROM module_name   
    BY <crypto_list> [ ,...n ]  
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | ASYMMETRIC KEY Asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *module_name*  
 Nome di una stored procedure, una funzione, un assembly o un trigger.  
  
 CERTIFICATE *cert_name*  
 Nome di un certificato con cui viene firmato un assembly, una stored procedure, una funzione o un trigger.  
  
 ASYMMETRIC KEY *Asym_key_name*  
 Nome di una chiave asimmetrica con cui viene firmato un assembly, una stored procedure, una funzione o un trigger.  
  
## <a name="remarks"></a>Commenti  
 Le informazioni sulle firme sono visibili nella vista di catalogo sys.crypt_properties.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste l'autorizzazione ALTER per l'oggetto e l'autorizzazione CONTROL per il certificato o la chiave asimmetrica. Se una chiave privata associata è protetta tramite una password, è necessario che anche l'utente disponga della password.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente la firma del certificato `HumanResourcesDP` viene rimossa dalla stored procedure `HumanResources.uspUpdateEmployeeLogin`.  
  
```sql  
USE AdventureWorks2012;  
DROP SIGNATURE FROM HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)   
 [ADD SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/add-signature-transact-sql.md)  
  
  
