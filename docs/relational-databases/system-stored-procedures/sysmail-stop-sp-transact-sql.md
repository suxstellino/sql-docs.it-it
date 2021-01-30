---
description: sysmail_stop_sp (Transact-SQL)
title: sysmail_stop_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_stop_sp_TSQL
- sysmail_stop_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_stop_sp
ms.assetid: 045ee36f-5bf0-4626-b5ee-e84db06ce16f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ba4f429ae813bcc58dd9fbbd24df2ab1a9300c78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181911"
---
# <a name="sysmail_stop_sp-transact-sql"></a>sysmail_stop_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arresta l'esecuzione di Posta elettronica database mediante l'arresto degli oggetti di [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilizzati dal programma esterno.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_stop_sp  
```  
  
## <a name="arguments"></a>Argomenti  
 nessuno  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Questo stored procedure si trova nel database **msdb** .  
  
 Questa stored procedure arresta la coda di Posta elettronica database contenente le richieste dei messaggi in uscita e disabilita [!INCLUDE[ssSB](../../includes/sssb-md.md)] per il programma esterno.  
  
 Se le code vengono arrestate, il programma esterno Posta elettronica database non elabora i messaggi. Questa stored procedure consente di arrestare l'esecuzione di Posta elettronica database per motivi di manutenzione o risoluzione dei problemi.  
  
 Per avviare Posta elettronica database, utilizzare **sysmail_start_sp**. Si noti che **sp_send_dbmail** accetta comunque la posta elettronica quando gli [!INCLUDE[ssSB](../../includes/sssb-md.md)] oggetti vengono arrestati.  
  
> [!NOTE]  
>  Questa stored procedure arresta solo le code di Posta elettronica database e non disattiva il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] nel database. Questa stored procedure non disabilita le stored procedure estese di Posta elettronica database per ridurre la superficie di attacco. Per disabilitare le stored procedure estese, vedere l' [opzione posta elettronica database XPS](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) del sistema **sp_configure** stored procedure.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato l'arresto Posta elettronica database nel database **msdb** . Nell'esempio si presuppone che il programma esterno Posta elettronica database sia stato abilitato.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_stop_sp ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_start_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-start-sp-transact-sql.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
