---
description: MSSQLSERVER_9001
title: MSSQLSERVER_9001 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9001 (Database Engine error)
ms.assetid: a54de936-90c6-4845-aa96-29d32f154601
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d444024bbe25bdb6d49bc02b3fa82596dd6e8a6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162003"
---
# <a name="mssqlserver_9001"></a>MSSQLSERVER_9001
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|9001|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LOG_NOT_AVAIL|  
|Testo del messaggio|Il log per il database '%.*ls' non è disponibile. Controllare il registro eventi per i messaggi di errore correlati. Risolvere eventuali errori e riavviare il database.|  
  
## <a name="explanation"></a>Spiegazione  
Il log del database è stato messo offline. In genere questa situazione implica un errore irreversibile per cui è necessario riavviare il database.  
  
## <a name="user-action"></a>Azione dell'utente  
Diagnosticare altri errori e riavviare l'istanza di SQL Server qualora il riavvio non sia già stato eseguito automaticamente.  
  
