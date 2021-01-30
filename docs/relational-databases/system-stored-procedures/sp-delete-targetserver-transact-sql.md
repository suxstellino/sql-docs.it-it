---
description: sp_delete_targetserver (Transact-SQL)
title: sp_delete_targetserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_targetserver
- sp_delete_targetserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_targetserver
ms.assetid: cc438701-ad91-419d-9f23-ebc4c548c700
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2f9e77a548e32e32c41d7ed6b45621ac5f4a2594
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199146"
---
# <a name="sp_delete_targetserver-transact-sql"></a>sp_delete_targetserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove il server specificato dall'elenco dei server di destinazione disponibili.  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_targetserver [ @server_name = ] 'server'   
     [ , [ @clear_downloadlist = ] clear_downloadlist ]  
     [ , [ @post_defection = ] post_defection ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @server_name = ] 'server'` Nome del server da rimuovere come server di destinazione disponibile. il *Server* è di **tipo nvarchar (30)** e non prevede alcun valore predefinito.  
  
`[ @clear_downloadlist = ] clear_downloadlist` Specifica se cancellare l'elenco di download per il server di destinazione. *clear_downloadlist* è di tipo **bit** e il valore predefinito è **1**. Quando *clear_downloadlist* è **1**, la procedura cancella l'elenco di download per il server prima di eliminare il server. Quando *clear_downloadlist* è **0**, l'elenco di download non viene cancellato.  
  
`[ @post_defection = ] post_defection` Specifica se inviare un'istruzione di esclusione al server di destinazione. *post_defection* è di tipo **bit** e il valore predefinito è 1. Quando *post_defection* è **1**, la procedura Invia un'istruzione di esclusione al server di destinazione prima di eliminare il server. Quando *post_defection* è **0**, la procedura non invia un'istruzione di esclusione al server di destinazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Il modo normale per eliminare un server di destinazione consiste nel chiamare **sp_msx_defect** nel server di destinazione. Usare **sp_delete_targetserver** solo quando è necessario un errore manuale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che agli utenti venga concesso il ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il server `LONDON1` viene rimosso dall'elenco dei server di processo disponibili.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_targetserver  
  @server_name = N'LONDON1' ;  
GO  
```  
  
## <a name="see-also"></a>Vedi anche  
 [sp_help_targetserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)   
 [sp_msx_defect &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
