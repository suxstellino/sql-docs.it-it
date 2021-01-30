---
description: sysssislog (Transact-SQL)
title: sysssislog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdtslog90_TSQL
- sysdtslog90
dev_langs:
- TSQL
helpviewer_keywords:
- sysssislog system table
ms.assetid: 7fa288a1-81e3-42a0-82f6-8a59019693d0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 58ce8d5a4784f34e83e2b09edda63dd49833a5ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159473"
---
# <a name="sysssislog-transact-sql"></a>sysssislog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni voce di log generata da pacchetti o dalle loro attività o contenitori in fase di esecuzione. Questa tabella viene creata nel database msdb quando si installa [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Se si configura la registrazione nei log in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diverso, viene creata una tabella sysssislog con questo formato nel database specificato.  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] scrive le voci di registrazione in questa tabella **solo** quando i pacchetti utilizzano il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di log.  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**int**|Identificatore univoco della voce del log.|  
|evento|**sysname**|Nome dell'evento che ha generato la voce del log.|  
|computer|**nvarchar**|Computer in cui era in esecuzione il pacchetto al momento della generazione della voce del log.|  
|Operatore|**nvarchar**|Nome utente della persona che ha eseguito il pacchetto che ha generato la voce del log.|  
|source|**nvarchar**|Nome del file eseguibile nel pacchetto che ha generato la voce del log.|  
|sourceid|**uniqueidentifier**|GUID del file eseguibile nel pacchetto che ha generato la voce del log.|  
|executionid|**uniqueidentifier**|GUID dell'istanza di esecuzione del file eseguibile che ha generato la voce del log.|  
|starttime|**datetime**|Ora di inizio dell'esecuzione del pacchetto.|  
|endtime|**datetime**|Ora del completamento del pacchetto.<br /><br /> Questa funzionalità non è implementata. Il valore nella colonna endtime è sempre uguale al valore nella colonna starttime.|  
|datacode|**int**|Valore intero facoltativo che in genere indica il risultato dell'esecuzione del contenitore o dell'attività:|  
|databytes|**image**|Matrice di byte facoltativa che contiene informazioni aggiuntive.|  
|message|**nvarchar**|Descrizione dell'evento e delle informazioni associate all'evento.|  
  
## <a name="see-also"></a>Vedere anche  
 [Registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md)   
  
  
