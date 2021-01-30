---
description: sp_replflush (Transact-SQL)
title: sp_replflush (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replflush
- sp_replflush_TSQL
helpviewer_keywords:
- sp_replflush
ms.assetid: 20809f5f-941d-427f-8f0c-de7a6c487584
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45a015f0b7b2448c0cf19c147247e1dd09b565b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211896"
---
# <a name="sp_replflush-transact-sql"></a>sp_replflush (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Scarica la cache degli articoli. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Non dovrebbe risultare necessario eseguire questa procedura manualmente. **sp_replflush** deve essere utilizzato solo per la risoluzione dei problemi di replica come indicato da un professionista del supporto di replica esperto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replflush  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replflush** viene utilizzata nella replica transazionale.  
  
 Le definizioni degli articoli vengono archiviate nella cache per migliorare il grado di efficienza. **sp_replflush** viene utilizzata da altre stored procedure di replica ogni volta che la definizione di un articolo viene modificata o eliminata.  
  
 Una sola connessione client può disporre dell'accesso a un determinato database tramite un agente di lettura log. Se un client dispone di accesso in lettura log a un database, l'esecuzione di **sp_replflush** causa il rilascio dell'accesso da parte del client. Gli altri client possono quindi eseguire l'analisi del log delle transazioni utilizzando **sp_replcmds** o **sp_replshowcmds**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_replflush**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_repltrans &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
