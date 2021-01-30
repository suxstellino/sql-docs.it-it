---
description: MSSQLSERVER_2516
title: MSSQLSERVER_2516 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2516 (Database Engine error)
ms.assetid: 79ed14b5-f53c-40c6-8334-8a083f732207
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d61eb6974d446f15b0658cef56a8f2d09922f67f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208450"
---
# <a name="mssqlserver_2516"></a>MSSQLSERVER_2516
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2516|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_REPAIR_DIFF_MAP_INVALIDATED|  
|Testo del messaggio|La correzione ha invalidato la mappa di bit differenziale per il database NAME. La catena di backup differenziale è stata interrotta. Prima di eseguire un backup differenziale è necessario eseguire un backup completo del database.|  
  
## <a name="explanation"></a>Spiegazione  
Questo messaggio informativo viene restituito quando si corregge l'errore MSSQLEngine_2515.  
  
## <a name="user-action"></a>Azione dell'utente  
Eseguire un backup completo del database.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione di un backup completo del database &#40;SQL Server&#41;](~/relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
