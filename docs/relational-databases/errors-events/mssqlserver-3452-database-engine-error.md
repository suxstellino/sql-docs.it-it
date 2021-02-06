---
description: MSSQLSERVER_3452
title: MSSQLSERVER_3452 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3452 (Database Engine error)
ms.assetid: bf838f02-7186-4b33-b01e-361b0c02de1f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1fe941e7ffaa594daa11e11e72140b4ace07cd33
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623795"
---
# <a name="mssqlserver_3452"></a>MSSQLSERVER_3452
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3452|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|REC_CHECKIDENTITY|  
|Testo del messaggio|Durante il recupero del database '%.*ls' (%d) è stata rilevata una possibile inconsistenza dei valori Identity nella tabella con ID %d. Eseguire DBCC CHECKIDENT ('%.\*ls').|  
  
## <a name="explanation"></a>Spiegazione  

Durante un aggiornamento o durante l'applicazione di un aggiornamento del servizio, il processo di recupero dei valori Identity nel database ha rilevato un'incoerenza nei metadati.  
  
## <a name="user-action"></a>Azione dell'utente

Eseguire DBCC CHECKIDENT.  
  
