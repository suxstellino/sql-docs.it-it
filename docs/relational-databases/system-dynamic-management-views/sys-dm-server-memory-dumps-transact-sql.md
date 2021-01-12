---
description: sys.dm_server_memory_dumps (Transact-SQL)
title: sys.dm_server_memory_dumps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_server_memory_dumps_TSQL
- dm_server_memory_dumps_TSQL
- dm_server_memory_dumps
- sys.dm_server_memory_dumps
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_memory_dumps dynamic management view
ms.assetid: 41782719-f54d-4e11-941a-c050c7576e23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 29ae5e940daffd0487d5d3e4514b5b624dd1320e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096498"
---
# <a name="sysdm_server_memory_dumps-transact-sql"></a>sys.dm_server_memory_dumps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni file di dump della memoria generato dal [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Utilizzare la DMV per risolvere potenziali problemi.  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**filename**|**nvarchar(256)**|Percorso e nome del file di dump della memoria. Non può essere null.|  
|**creation_time**|**datetimeoffset(7)**|Data e ora di creazione del file. Non può essere null.|  
|**size_in_bytes**|**bigint**|Dimensioni (in byte) del file. Ammette i valori Null.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Il dump può essere di tipo minidump, all-thread o completo. I file hanno estensione .mdmp.  
  
## <a name="security"></a>Sicurezza  
 I file di dump possono contenere informazioni riservate. Per proteggere tali informazioni, è possibile utilizzare un elenco di controllo di accesso per limitare l'accesso ai file oppure copiare i file in una cartella con accesso limitato. Ad esempio, prima di inviare i file di debug ai servizi di supporto tecnico Microsoft, è consigliato rimuovere qualsiasi informazione sensibile o riservata.  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE.  
  
  
