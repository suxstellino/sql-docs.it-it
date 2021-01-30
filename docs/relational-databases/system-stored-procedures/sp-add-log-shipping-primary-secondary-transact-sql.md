---
description: sp_add_log_shipping_primary_secondary (Transact-SQL)
title: sp_add_log_shipping_primary_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_secondary_TSQL
- sp_add_log_shipping_primary_secondary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_secondary
ms.assetid: 23b3e100-5318-410e-b8f3-51c89b2dd777
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 46c33fa71e9f0c3b49a693896b7349d3f5bdf7bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186032"
---
# <a name="sp_add_log_shipping_primary_secondary-transact-sql"></a>sp_add_log_shipping_primary_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa stored procedure aggiunge una voce per un database secondario nel server primario.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_log_shipping_primary_secondary  
[ @primary_database = ] 'primary_database',  
[ @secondary_server = ] 'secondary_server',   
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @primary_database = ] 'primary_database'` Nome del database nel server primario. *primary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @secondary_server = ] 'secondary_server',` Nome del server secondario. *secondary_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @secondary_database = ] 'secondary_database'` Nome del database secondario. *secondary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_log_shipping_primary_secondary** deve essere eseguito dal database **Master** nel server primario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Questo esempio illustra l'uso di **sp_add_log_shipping_primary_secondary** per aggiungere una voce per il database secondario **LogShipAdventureWorks** al server secondario Flatiron.  
  
```  
EXEC master.dbo.sp_add_log_shipping_primary_secondary   
@primary_database = N'AdventureWorks'   
, @secondary_server = N'flatiron'   
, @secondary_database = N'LogShipAdventureWorks' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
