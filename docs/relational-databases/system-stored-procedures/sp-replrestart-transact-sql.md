---
description: sp_replrestart (Transact-SQL)
title: sp_replrestart (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replrestart_TSQL
- sp_replrestart
helpviewer_keywords:
- sp_replrestart
ms.assetid: 111b3dbf-92f8-4670-b156-1468c63e4fc1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2cdcac7e3193922ece51ac76f8a879a1142e7a0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211659"
---
# <a name="sp_replrestart-transact-sql"></a>sp_replrestart (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Questa stored procedure viene utilizzata dalla replica transazionale durante il backup e il ripristino, in modo che i dati replicati nel server di distribuzione vengano sincronizzati con i dati nel server di pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  **sp_replrestart** è un stored procedure di replica interno e deve essere utilizzato solo per il ripristino di un database pubblicato in una topologia di replica transazionale, come indicato nell'argomento [strategie per il backup e il ripristino della replica snapshot e transazionale](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replrestart  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replrestart** viene utilizzata quando il valore del numero di sequenza del file di log (LSN) più elevato nel server di distribuzione non corrisponde al valore LSN massimo nel server di pubblicazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_replrestart**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
