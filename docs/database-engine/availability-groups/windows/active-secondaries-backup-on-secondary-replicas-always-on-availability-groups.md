---
title: Ripartire i backup in una replica secondaria di un gruppo di disponibilità
description: Informazioni sui diversi tipi di backup supportati quando si effettua la ripartizione dei backup in una replica secondaria di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 82afe51b-71d1-4d5b-b20a-b57afc002405
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4cefec3a8ff6ed45c00b09a339d06c3d022bdc1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339287"
---
# <a name="offload-supported-backups-to-secondary-replicas-of-an-availability-group"></a>Ripartire i backup supportati nelle repliche secondarie di un gruppo di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Le funzionalità secondarie attive di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] includono il supporto per l'esecuzione di backup nelle repliche secondarie. Le operazioni di backup possono richiedere considerevoli risorse a livello di I/O e di CPU (con compressione dei backup). La ripartizione dei backup su una replica secondaria sincronizzata o in sincronizzazione consente di utilizzare le risorse sull'istanza del server che ospita la replica primaria per i carichi di lavoro di livello 1.  

> [!NOTE]  
>  Le istruzioni RESTORE non sono consentite nei database primari o secondari di un gruppo di disponibilità.  
  
 
##  <a name="backup-types-supported-on-secondary-replicas"></a><a name="SupportedBuTypes"></a> Tipi di backup supportati nelle repliche secondarie  
  
-   **BACKUP DATABASE** supporta solo i backup completi di sola copia di database, file o filegroup quando viene eseguito nelle repliche secondarie. I backup di sola copia non influenzano la catena di log e non cancellano la mappa di bit differenziale.  
  
-   I backup differenziali non sono supportati nelle repliche secondarie.

-   I backup simultanei, ad esempio il backup del log delle transazioni nella replica primaria durante l'esecuzione di un backup completo del database nella replica secondaria, non sono attualmente supportati. 
  
-   **BACKUP LOG** supporta solo i backup di log regolari (l'opzione COPY_ONLY non è supportata per i backup di log in repliche secondarie).  
  
     È garantita una catena di log coerente tra i backup di log eseguiti nelle repliche (primarie e secondarie), indipendentemente dalla relativa modalità di disponibilità (commit sincrono o asincrono).  
  
-   Per eseguire il backup di un database secondario, è necessario che una replica secondaria riesca a comunicare con la replica primaria e sia nello stato **SYNCHRONIZED** o **SYNCHRONIZING**.  

In un gruppo di disponibilità distribuito è possibile eseguire i backup sulle repliche secondarie nello stesso gruppo di disponibilità della replica primaria attiva oppure sulla replica primaria di qualsiasi gruppo di disponibilità secondario. Non è possibile eseguire i backup su una replica secondaria in un gruppo di disponibilità secondario perché le repliche secondarie comunicano solo con la replica primaria nel proprio gruppo di disponibilità. Solo le repliche che comunicano direttamente con la replica primaria globale possono eseguire le operazioni di backup.

##  <a name="configuring-where-backup-jobs-run"></a><a name="WhereBuJobsRun"></a> Configurazione del percorso di esecuzione dei processi di backup  
 L'esecuzione di backup su una replica secondaria per ripartire il carico di lavoro di backup dal server di produzione primario comporta notevoli vantaggi, tuttavia rende più complessa la selezione dei percorsi di esecuzione dei processi di backup. Per risolvere questo problema, configurare dove eseguire i processi di backup come segue:  
  
1.  Configurare il gruppo di disponibilità per specificare le repliche di disponibilità per cui si desidera venga eseguito il backup. Per altre informazioni, vedere i parametri *AUTOMATED_BACKUP_PREFERENCE* e *BACKUP_PRIORITY* in [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md) o [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
2.  Creare processi di backup controllati da script per ogni database di disponibilità in ogni istanza del server che ospita una replica di disponibilità che è un candidato per l'esecuzione del backup. Per altre informazioni, vedere la sezione "Completamento: Dopo avere configurato il backup su repliche secondarie" di [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare il backup delle repliche secondarie**  
  
-   [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 **Per determinare se la replica corrente è la replica di backup preferita**  
  
-   [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Per creare un processo di backup**  
  
-   [Utilizzare la Creazione guidata piano di manutenzione](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
-   [Implementazione di processi](../../../ssms/agent/implement-jobs.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Backup di sola copia &#40;SQL Server&#41;](../../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
