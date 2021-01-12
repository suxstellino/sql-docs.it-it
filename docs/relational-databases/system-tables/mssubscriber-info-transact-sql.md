---
description: MSsubscriber_info (Transact-SQL)
title: MSsubscriber_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscriber_info_TSQL
- MSsubscriber_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriber_info system table
ms.assetid: 5ca22f41-6020-4f72-8110-e69baf3447cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 05ea16f0f6687a82a04ab04039edf3223bb8ef70
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099667"
---
# <a name="mssubscriber_info-transact-sql"></a>MSsubscriber_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSsubscriber_info** contiene una riga per ogni coppia server di pubblicazione/Sottoscrittore di cui è in corso il push delle sottoscrizioni dal server di distribuzione locale. Questa tabella è archiviata nel database di distribuzione.  
  
 **Nota** Questa tabella di sistema è stata deprecata e viene mantenuta per supportare le versioni precedenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="definition"></a>Definizione  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**type**|**tinyint**|Tipo di Sottoscrittore:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottoscrittore 0.<br /><br /> **1** = origine dati ODBC.|  
|**accesso**|**sysname**|Account di accesso per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Viene archiviata in forma crittografata se il Sottoscrittore viene aggiunto con la modalità di autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**nvarchar (524)**|Password utilizzata per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Viene archiviata in forma crittografata se il Sottoscrittore viene aggiunto con la modalità di autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**description**|**nvarchar(255)**|Descrizione del Sottoscrittore.|  
|**security_mode**|**int**|Modalità di sicurezza implementata:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticazione di Windows.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
