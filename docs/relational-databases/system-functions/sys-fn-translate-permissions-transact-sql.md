---
description: sys.fn_translate_permissions (Transact-SQL)
title: sys.fn_translate_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_translate_permissions
- sys.fn_translate_permissions_TSQL
- fn_translate_permissions
- fn_translate_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions bitmask [SQL Server]
- sys.fn_translate_permissions function
- fn_translate_permissions function
ms.assetid: ac97121f-2bd0-4f71-8e45-42c8584edbc5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e4d00e0b945012cb8a1b583921328009d12a34ad
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094943"
---
# <a name="sysfn_translate_permissions-transact-sql"></a>sys.fn_translate_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Converte la maschera di bit delle autorizzazioni restituita da Traccia SQL in una tabella di nomi delle autorizzazioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_translate_permissions ( level , perms )  
```  
  
## <a name="arguments"></a>Argomenti  
 *level*  
 Tipo di entità a protezione diretta a cui viene applicata l'autorizzazione. *Level* è di **tipo nvarchar (60)**.  
  
 *perms*  
 Maschera di bit restituita nella colonna delle autorizzazioni. *perms* è di tipo **varbinary (16)**.  
  
## <a name="returns"></a>Restituisce  
 **tabella**  
  
## <a name="remarks"></a>Osservazioni  
 Il valore restituito nella colonna **autorizzazioni** di una traccia SQL è una rappresentazione Integer di una maschera di maschera utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per calcolare le autorizzazioni valide. Tutti i 25 tipi di entità a protezione diretta dispongono di un proprio set di autorizzazioni con valori numerici corrispondenti. **sys.fn_translate_permissions** converte questa maschera di maschera in una tabella di nomi di autorizzazioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="example"></a>Esempio  
 La query seguente usa `sys.fn_builtin_permissions` per visualizzare le autorizzazioni valide per i certificati, quindi usa `sys.fn_translate_permissions` per restituire i risultati della maschera di maschera delle autorizzazioni.  
  
```  
SELECT * FROM sys.fn_builtin_permissions('CERTIFICATE');  
SELECT '0001' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0001);  
SELECT '0010' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0010);  
SELECT '0011' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0011);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
