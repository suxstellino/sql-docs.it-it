---
description: MSSQLSERVER_7901
title: MSSQLSERVER_7901 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7901 (Database Engine error)
ms.assetid: 2d0d19b9-947b-4474-9ff8-7e03019ab93d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3723349a65a5e5fafc185ec6a342965992041a80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204584"
---
# <a name="mssqlserver_7901"></a>MSSQLSERVER_7901
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|7901|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC2_DATABASE_IN_EMERGENCY_MODE|  
|Testo del messaggio|L'istruzione di correzione non è stata elaborata. Questo livello di correzione non è supportato quando il database è in modalità di emergenza.|  
  
## <a name="explanation"></a>Spiegazione  
Il database è in modalità di emergenza ed è stato specificato un livello di correzione diverso da REPAIR_ALLOW_DATA_LOSS. Non è possibile implementare correzioni in modalità di emergenza a meno che non venga specificata l'opzione REPAIR_ALLOW_DATA_LOSS.  
  
## <a name="user-action"></a>Azione dell'utente  
Eseguire nuovamente il comando e specificare l'opzione REPAIR_ALLOW_DATA_LOSS.  
  
