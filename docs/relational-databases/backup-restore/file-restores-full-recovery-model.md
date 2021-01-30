---
title: Ripristini di file (modello di recupero con registrazione completa) | Microsoft Docs
description: Un ripristino di file in SQL Server è un'unica sequenza di ripristino che esegue la copia, il rollforward e il recupero di uno o più file di dati senza ripristinare l'intero database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- full recovery model [SQL Server], performing restores
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- file restores [SQL Server], full recovery model
- restoring files [SQL Server], full recovery model
- Transact-SQL restore sequence
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: d2236a2a-4cf1-4c3f-b542-f73f6096e15c
author: cawrites
ms.author: chadam
ms.openlocfilehash: fd5fa04b32d9c00f810b61bbca81963e21bd5697
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076879"
---
# <a name="file-restores-full-recovery-model"></a>Ripristini di file (modello di recupero con registrazione completa)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le informazioni in questo argomento sono rilevanti solo per i database che contengono più file o filegroup e che utilizzano il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk.  
  
 L'obiettivo di un ripristino di file consiste nel ripristinare uno o più file danneggiati senza ripristinare l'intero database. Uno scenario di ripristino di file consiste in un'unica sequenza di ripristino in cui vengono eseguiti la copia, il rollforward e il recupero dei dati appropriati.  
  
 Se il filegroup in fase di ripristino è di lettura/scrittura, è necessario applicare una catena non interrotta di backup del log dopo il ripristino degli ultimi dati o del backup differenziale per portare il filegroup fino ai record di log inclusi nei record di log attivi correnti del file di log. Il punto di recupero si trova in genere, ma non necessariamente, verso la fine del log.  
  
 Se il filegroup in fase di ripristino è di sola lettura, l'applicazione di backup del log in genere non è necessaria e viene ignorata. Se il backup è stato creato dopo l'impostazione del file in modalità di sola lettura, esso verrà ripristinato per ultimo. Il rollforward viene arrestato in corrispondenza del punto di destinazione.  
  
 Gli scenari di ripristino dei file sono i seguenti:  
  
-   Ripristino di file offline  
  
     In un *ripristino di file offline*, i file o i filegroup danneggiati vengono ripristinati mentre il database è offline. Al termine della sequenza di ripristino, il database torna online.  
  
     Tutte le edizioni di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supportano il ripristino di file offline.  
  
-   Ripristino di file online  
  
     In un *ripristino di file offline*, se il database è online al momento del ripristino, rimarrà online durante il ripristino del file. Tuttavia, durante l'operazione di ripristino, ogni filegroup nel quale viene ripristinato un file rimane offline. Al termine del recupero di tutti i file del filegroup offline, viene attivata automaticamente la modalità online per il filegroup.  
  
     Per informazioni sul supporto per il ripristino di pagine e file online, vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). Per altre informazioni sui ripristini in linea, vedere [Ripristino in linea (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md).
  
    > [!TIP]  
    >  Se si desidera attivare la modalità offline per il database per eseguire un ripristino di file, eseguire l'istruzione [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) seguente prima di avviare la sequenza di ripristino: ALTER DATABASE *database_name* SET OFFLINE.  
  
  
##  <a name="restoring-damaged-files-from-file-backups"></a><a name="Overview"></a> Ripristino di file danneggiati da backup di file  
  
1.  Prima di ripristinare uno o più file danneggiati, tentare di creare un [backup della parte finale del log](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
     Se il log è stato danneggiato e non è possibile creare un backup della parte finale del log, è necessario ripristinare l'intero database.  
  
     Per informazioni sul backup di un log delle transazioni, vedere [Backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Per un ripristino di file offline, è sempre necessario creare un backup della parte finale del log prima del ripristino del file. Per un ripristino di file online, è sempre necessario creare il backup del log dopo il ripristino del file per fare in modo che il file recuperato si trovi in uno stato consistente con il resto del database.  
  
2.  Ripristinare ogni file danneggiato dal backup del file più recente.  
  
3.  Ripristinare l'eventuale backup differenziale del file più recente per ogni file ripristinato.  
  
4.  Ripristinare i backup del log delle transazioni in sequenza, iniziando con il backup associato al file ripristinato meno recente e terminando con il backup della parte finale del log creato nel passaggio 1.  
  
     È necessario ripristinare tutti i backup del log delle transazioni creati successivamente ai backup di file per assicurare la consistenza del database. Il rollforward dei backup del log delle transazioni è un'operazione rapida, in quanto vengono applicate soltanto le modifiche valide per i file ripristinati. Il ripristino di singoli file può offrire risultati migliori rispetto al ripristino dell'intero database poiché i file non danneggiati non vengono copiati e non ne viene eseguito il rollforward. Deve comunque essere letta l'intera catena dei backup di log.  
  
5.  Recuperare il database.  

> [!NOTE]  
>  I backup dei file possono essere utilizzati per ripristinare il database a una temporizzazione precedente. A tale scopo, è necessario ripristinare un set completo di backup dei file e quindi ripristinare i backup del log delle transazioni in sequenza, fino a raggiungere un punto nel tempo successivo al backup di file più recente ripristinato. Per altre informazioni sul recupero temporizzato, vedere [Ripristino di un database di SQL Server fino a un punto specifico all'interno di un backup &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
## <a name="transact-sql-restore-sequence-for-an-offline-file-restore-full-recovery-model"></a>Sequenza di ripristino Transact-SQL per il ripristino di file offline (modello di recupero con registrazione completa)  
 Uno scenario di ripristino di file consiste in un'unica sequenza di ripristino in cui vengono eseguiti la copia, il rollforward e il recupero dei dati appropriati.  
  
 In questa sezione vengono illustrate le opzioni [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) essenziali per una sequenza di ripristino di file. La sintassi e i dettagli non rilevanti sono stati omessi.  
  
 Nella sequenza di ripristino dell'esempio seguente viene illustrato un ripristino offline di due file secondari, `A` e `B`mediante WITH NORECOVERY. Successivamente vengono applicati due backup del log con NORECOVERY, seguiti dal backup della parte finale del log, che viene ripristinato con WITH RECOVERY.  
  
> [!NOTE]  
>  La sequenza di ripristino dell'esempio seguente inizia portando il file offline e quindi creando un backup della parte finale del log.  
  
```  
--Take the file offline.  
ALTER DATABASE database_name MODIFY FILE SET OFFLINE;  
-- Back up the currently active transaction log.  
BACKUP LOG database_name  
   TO <tail_log_backup>  
   WITH NORECOVERY;  
GO   
-- Restore the files.  
RESTORE DATABASE database_name FILE=name   
   FROM <file_backup_of_file_A>   
   WITH NORECOVERY;  
RESTORE DATABASE database_name FILE=<name> ......  
   FROM <file_backup_of_file_B>   
   WITH NORECOVERY;  
-- Restore the log backups.  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <tail_log_backup>   
   WITH RECOVERY;  
```  
  
## <a name="examples"></a>Esempi  
  
-   [Esempio: Ripristino online di un file di lettura/scrittura &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Esempio: Ripristino online di un file di sola lettura &#40;Modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
-   [Esempio: Ripristino offline del filegroup primario e di un altro filegroup &#40;Modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per ripristinare file e filegroup**  
  
-   [Ripristino dei file in una nuova posizione &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Ripristino di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.SqlRestore%2A> (SMO)  
  
  
## <a name="see-also"></a>Vedere anche  
 [Backup e ripristino: interoperabilità e coesistenza &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Backup completi del file &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Panoramica del ripristino e del recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Ripristini di database completi &#40;modello di recupero con registrazione minima&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Ripristini a fasi &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
