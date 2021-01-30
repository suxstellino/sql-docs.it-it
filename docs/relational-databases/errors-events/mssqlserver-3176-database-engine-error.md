---
description: MSSQLSERVER_3176
title: MSSQLSERVER_3176 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3176 (Database Engine error)
ms.assetid: 4be24c64-2d52-4cb4-b4d7-36efbe4555b6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6bc3304a75bdea90515e5e33ab98f24a95d068a3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194172"
---
# <a name="mssqlserver_3176"></a>MSSQLSERVER_3176
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID evento|3176|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LDDB_FILE_CLAIM|  
|Testo del messaggio|Il file '%ls' è richiesto da '%ls'(%d) e '%ls'(%d). Per rilocare uno o più file è possibile utilizzare la clausola WITH MOVE.|  
  
## <a name="explanation"></a>Spiegazione  
Viene tentato l'utilizzo di un file per più scopi.  
  
### <a name="possible-causes"></a>Possibili cause  
Il nome file viene già utilizzato da un altro database.  
  
## <a name="user-action"></a>Azione dell'utente  
Ripristinare i file di database in un altro percorso. In un'istruzione RESTORE, utilizzare una clausola WITH MOVE per spostare ogni file. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] modificare i percorsi dei file nella griglia **Ripristina file di database come** nella pagina Opzioni della finestra di dialogo **Ripristina database**.  
  
## <a name="see-also"></a>Vedere anche  
[Ripristinare un database in una nuova posizione &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
[Ripristino dei file in una nuova posizione &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
  
