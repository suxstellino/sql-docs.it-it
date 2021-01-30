---
description: MSSQLSERVER_1406
title: MSSQLSERVER_1406 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1406 (Database Engine error)
ms.assetid: 915f97de-9b74-41f8-8bd5-b2d061416718
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c5d7fe3f66ea581305f985e25cc1a2c30c1809e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197021"
---
# <a name="mssqlserver_1406"></a>MSSQLSERVER_1406
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1406|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBM_BADSTATEFORFORCESERVICE|  
|Testo del messaggio|Impossibile forzare il servizio senza rischi. Per ottenere l'accesso, rimuovere il mirroring del database e recuperare il database "%.*ls".|  
  
## <a name="explanation"></a>Spiegazione  
Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] non può forzare il servizio perché lo stato del mirroring non è in grado di garantire che il servizio venga forzato correttamente.  
  
## <a name="user-action"></a>Azione dell'utente  
Rimuovere il mirroring del database. Sarà quindi possibile recuperare il database mirror per potervi accedere.  
  
## <a name="see-also"></a>Vedere anche  
[Utilizzo forzato del servizio in una sessione di mirroring del database &#40;Transact-SQL&#41;](~/database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
[Rimozione del mirroring del database &#40;SQL Server&#41;](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
