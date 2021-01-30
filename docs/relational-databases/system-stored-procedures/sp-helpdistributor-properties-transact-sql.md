---
description: sp_helpdistributor_properties (Transact-SQL)
title: sp_helpdistributor_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributor_properties_TSQL
- sp_helpdistributor_properties
helpviewer_keywords:
- sp_helpdistributor_properties
ms.assetid: ee267724-3244-49eb-84c9-f38dbefdd639
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7229533780107de72f09163745dcfa5fdc858332
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197630"
---
# <a name="sp_helpdistributor_properties-transact-sql"></a>sp_helpdistributor_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce le proprietà del server di distribuzione. La stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdistributor_properties   
```  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**heartbeat_interval**|**int**|Periodo massimo in minuti durante il quale un agente può non registrare alcun messaggio sullo stato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpdistributor_properties** viene utilizzato con tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione e gli utenti nell'elenco di accesso alla pubblicazione per una pubblicazione che utilizza questo server di distribuzione possono eseguire **sp_helpdistributor_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_changedistributor_property &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)  
  
  
