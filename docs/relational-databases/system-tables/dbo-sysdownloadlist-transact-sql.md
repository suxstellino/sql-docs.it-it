---
description: dbo.sysdownloadlist (Transact-SQL)
title: dbo.sysdownload (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysdownloadlist
- sysdownloadlist_TSQL
- dbo.sysdownloadlist_TSQL
- sysdownloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sysdownloadlist system table
ms.assetid: 71087a4c-e829-488e-aa7d-a9476e2b4779
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8670a2b3ca9e8033b3097b06fe96de040a4fb7c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158372"
---
# <a name="dbosysdownloadlist-transact-sql"></a>dbo.sysdownloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Include la coda delle istruzioni di download per tutti i server di destinazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Colonna Identity che indica la sequenza di inserimento naturale delle righe.|  
|**source_server**|**sysname**|Nome del server di origine.|  
|**operation_code**|**tinyint**|Codice operativo per il processo:<br /><br /> **1** = ins (inserimento)<br /><br /> **2** = UPD (aggiornamento)<br /><br /> **3** = CANC (Delete)<br /><br /> **4** = avvio<br /><br /> **5** = arresta|  
|**object_type**|**tinyint**|Codice del tipo di oggetto.|  
|**object_id** <sup>1</sup>|**uniqueidentifier**|Numero di identificazione dell'oggetto.|  
|**target_server**|**sysname**|Nome del server di destinazione.|  
|**error_message**|**nvarchar(1024)**|Messaggio di errore restituito se nel server di destinazione viene rilevato un errore durante l'elaborazione di una determinata riga|  
|**date_posted**|**datetime**|Giorno e ora in cui il processo è stato inviato al server di destinazione.|  
|**date_downloaded**|**datetime**|Giorno e ora in cui è stato eseguito l'ultimo download del processo.|  
|**Stato**|**tinyint**|Stato del processo:<br /><br /> **0** = non ancora scaricato<br /><br /> **1** = scaricato correttamente|  
|**deleted_object_name**|**sysname**|Nome dell'oggetto eliminato.|  
  
 <sup>1</sup> la colonna **object_id** può essere un valore pari a **-1**, che corrisponde al valore all se la colonna **operation_code** è un valore Delete.  
  
  
