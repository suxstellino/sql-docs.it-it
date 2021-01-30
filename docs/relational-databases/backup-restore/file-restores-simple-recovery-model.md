---
title: Ripristini di file (modello di recupero con registrazione minima) | Microsoft Docs
description: In SQL Server un ripristino di file si applica a uno o più file danneggiati senza ripristinare l'intero database.
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- simple recovery model [SQL Server]
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- Transact-SQL restore sequence
- restoring files [SQL Server], simple recovery model
- file restores [SQL Server], simple recovery model
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: b6d07386-7c6f-4cc6-be32-93289adbd3d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e2df8edb5b31ff2ffcd45eba9b5d9cf234c528d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076889"
---
# <a name="file-restores-simple-recovery-model"></a>Ripristini di file (modello di recupero con registrazione minima)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le informazioni contenute in questo argomento sono rilevanti solo per i database che utilizzano il modello di recupero con registrazione minima e includono almeno un filegroup secondario di sola lettura.  
  
 L'obiettivo di un ripristino di file consiste nel ripristinare uno o più file danneggiati senza ripristinare l'intero database. In base al modello di recupero con registrazione minima, i backup di file sono supportati solo per i file di sola lettura. Il filegroup primario e i filegroup secondari di lettura/scrittura vengono sempre ripristinati insieme attraverso il ripristino di un backup del database o di un backup parziale.  
  
 Gli scenari di ripristino dei file sono i seguenti:  
  
-   Ripristino di file offline  
  
     In un *ripristino di file offline*, i file o i filegroup danneggiati vengono ripristinati mentre il database è offline. Al termine della sequenza di ripristino, il database torna online.  
  
     Tutte le edizioni di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supportano il ripristino di file offline.  
  
-   Ripristino di file online  
  
     In un *ripristino di file offline*, se il database è online al momento del ripristino, rimarrà online durante il ripristino del file. Tuttavia, durante l'operazione di ripristino, ogni filegroup nel quale viene ripristinato un file rimane offline. Al termine del recupero di tutti i file del filegroup offline, viene attivata automaticamente la modalità online per il filegroup.  
  
     Per informazioni sul supporto per il ripristino di pagine e file in linea, vedere [Caratteristiche e attività del motore di database](../../sql-server/what-s-new-in-sql-server-ver15.md). Per altre informazioni sui ripristini online, vedere [Ripristino in linea &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
    > [!TIP]  
    >  Se si desidera attivare la modalità offline per il database per eseguire un ripristino di file, eseguire l'istruzione [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) seguente prima di avviare la sequenza di ripristino: ALTER DATABASE *database_name* SET OFFLINE.  
  
 **Contenuto dell'argomento**  
  
-   [Panoramica del ripristino di file e filegroup nel modello di recupero con registrazione minima](#Overview)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="overview-of-file-and-filegroup-restore-under-the-simple-recovery-model"></a><a name="Overview"></a> Panoramica del ripristino di file e filegroup nel modello di recupero con registrazione minima  
 Uno scenario di ripristino di file consiste in un'unica sequenza di ripristino che consente di eseguire la copia, il rollforward e il recupero dei dati appropriati come descritto di seguito:  
  
1.  Ripristinare ogni file danneggiato dal backup di file più recente.  
  
2.  Ripristinare il backup differenziale di file più recente per ogni file ripristinato e recuperare il database.  
  
### <a name="transact-sql-steps-for-file-restore-sequence-simple-recovery-model"></a>Passaggi di Transact-SQL per la sequenza di ripristino di file (modello di recupero con registrazione minima)  
 Questa sezione descrive le opzioni [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) essenziali di [!INCLUDE[tsql](../../includes/tsql-md.md)] per una semplice sequenza di ripristino di file. La sintassi e i dettagli non rilevanti sono stati omessi.  
  
 La sequenza di ripristino contiene solo due istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] . La prima istruzione esegue il ripristino di un file secondario, il file `A`, che viene ripristinato utilizzando WITH NORECOVERY. La seconda operazione ripristina altri due file, i file `B` e `C` , che vengono ripristinati utilizzando WITH RECOVERY da un diverso dispositivo di backup:  
  
1.  RESTORE DATABASE *database* FILE **=** _nome_file_A_  
  
     FROM *backup_file_A*  
  
     WITH NORECOVERY **;**  
  
2.  RESTORE DATABASE *database* FILE **=** _nome_file_B_ **,** _nome_file_C_  
  
     FROM *backup_dei_file_B_e_C*  
  
     WITH RECOVERY **;**  
  
### <a name="examples"></a>Esempi  
  
-   [Esempio: Ripristino online di un file di sola lettura &#40;Modello di recupero con registrazione minima&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Esempio: Ripristino offline del filegroup primario e di un altro filegroup &#40;Modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per ripristinare file e filegroup**  
  
-   [Ripristinare file e filegroup sovrascrivendo file esistenti &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Ripristino di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Ripristino di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Metodo Restore.SqlRestore (server) (SMO)](/dotnet/api/microsoft.sqlserver.management.smo.restore.sqlrestore)   
  
## <a name="see-also"></a>Vedere anche  
 [Backup e ripristino: interoperabilità e coesistenza &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Backup completi del file &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Panoramica del ripristino e del recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Ripristini di database completi &#40;modello di recupero con registrazione minima&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Ripristini a fasi &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
