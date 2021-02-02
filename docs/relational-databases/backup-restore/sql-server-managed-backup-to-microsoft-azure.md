---
title: Backup gestito di SQL Server in Microsoft Azure | Microsoft Docs
description: Backup gestito di SQL Server in Microsoft Azure gestisce e automatizza i backup di SQL Server nel servizio di archiviazione BLOB di Microsoft Azure.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: afa01165-39e0-4efe-ac0e-664edb8599fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: c925c4a8361b54ff563b6a1443e392f0ca392df9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237270"
---
# <a name="sql-server-managed-backup-to-microsoft-azure"></a>Backup gestito di SQL Server in Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] consente di gestire e automatizzare i backup di SQL Server nel servizio di archiviazione BLOB di Microsoft Azure. È possibile consentire a SQL Server di stabilire la pianificazione del backup in base al carico di lavoro della transazione nel database. In alternativa, è possibile usare le opzioni avanzate per definire una pianificazione. Le impostazioni di conservazione specificano per quanto tempo i backup vengono conservati nel servizio di archiviazione BLOB di Azure. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] supporta il ripristino temporizzato per il periodo di memorizzazione specificato.  
  
 A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], sono state modificate le procedure e il comportamento sottostante di [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] . Per altre informazioni, vedere [Migrare le impostazioni di backup gestito di SQL Server 2014 a SQL Server 2016](../../relational-databases/backup-restore/migrate-sql-server-2014-managed-backup-settings-to-sql-server-2016.md).  
  
> [!TIP]  
>  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è consigliato per le istanze di SQL Server in esecuzione in macchine virtuali di Microsoft Azure.  
  
## <a name="benefits"></a>Vantaggi  
 Per l'automatizzazione di backup per più database sono attualmente richiesti lo sviluppo di una strategia di backup, la scrittura di codice personalizzato e la pianificazione di backup. Con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]è possibile creare un piano di backup specificando solo il percorso di memorizzazione e la posizione di archiviazione. Anche se disponibili, le impostazioni avanzate non sono necessarie. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] consente di pianificare, eseguire e gestire i backup.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] a livello di database o a livello di istanza di SQL Server. Se configurato a livello di istanza, il backup dei nuovi database viene eseguito automaticamente. È possibile usare le impostazioni a livello di database per eseguire l'override delle impostazioni predefinite a livello di istanza per singoli casi.  
  
 La crittografia dei backup assicura maggiore sicurezza ed è possibile impostare una pianificazione personalizzata per controllare quando verranno eseguiti i backup. Per altre informazioni sui vantaggi derivanti dall'uso dell'archivio BLOB di Microsoft Azure per i backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
##  <a name="prerequisites"></a><a name="Prereqs"></a> Prerequisiti  
 Archiviazione di Microsoft Azure è usata da [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per l'archiviazione dei file di backup. Sono richiesti i prerequisiti seguenti:  
  
|Prerequisito|Descrizione|  
|------------------|-----------------|  
|**Account Microsoft Azure**|È possibile iniziare a usare Azure con una [versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/) prima di esaminare le [opzioni per l'acquisto](https://azure.microsoft.com/pricing/purchase-options/).|  
|**Account di archiviazione di Azure**|I backup vengono archiviati nel servizio di archiviazione BLOB Azure associato a un account di archiviazione Azure. Per istruzioni dettagliate per creare un account di archiviazione, vedere [Informazioni sugli account di archiviazione Azure](/azure/storage/common/storage-account-create).|  
|**Contenitore BLOB**|I BLOB sono organizzati in contenitori. L'utente specifica il contenitore di destinazione per i file di backup. È possibile creare un contenitore nel [portale di gestione di Azure](https://manage.windowsazure.com/)oppure con il comando di **Azure PowerShell**[New-AzureStorageContainer](/powershell/azure/) .|  
|**Firma di accesso condiviso**|L'accesso al contenitore di destinazione è controllato da una firma di accesso condiviso (SAS, Shared Access Signature). Per una panoramica su SAS, vedere [Firme di accesso condiviso, parte 1: informazioni sul modello di firma di accesso condiviso](/azure/storage/common/storage-sas-overview). È possibile creare un token della firma di accesso condiviso nel codice o con il comando di PowerShell **New-AzureStorageContainerSASToken** . Per un esempio di script di PowerShell che semplifica questo processo, vedere [Simplifying creation of SQL Credentials with Shared Access Signature ( SAS ) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell)(Semplificazione della creazione di credenziali SQL con i token della firma di accesso condiviso in Archiviazione di Azure con Powershell). Il token della firma di accesso condiviso può essere archiviato in **Credenziali SQL** ed essere usato con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|**SQL Server Agent**|Per il corretto funzionamento di [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è necessario che il servizio SQL Server Agent sia in esecuzione. È consigliabile impostare l'opzione per l'avvio automatico.|  
  
## <a name="components"></a>Componenti  
 Transact-SQL è l'interfaccia principale per interagire con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Le stored procedure di sistema vengono utilizzate per l'abilitazione, la configurazione e il monitoraggio di [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Le funzioni di sistema vengono utilizzate per recuperare le informazioni sui file di backup, i valori dei parametri e le impostazioni di configurazione esistenti. Gli eventi estesi vengono utilizzati per esporre errori e avvisi. I meccanismi di avviso sono abilitati tramite i processi di SQL Agent e la gestione basata su criteri di SQL Server. Di seguito sono riportati un elenco di oggetti e una descrizione della relativa funzionalità rispetto a [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Per configurare [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]sono disponibili anche i cmdlet di PowerShell. SQL Server Management Studio supporta il ripristino dei backup creati da [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] tramite l'attività **Ripristina database** .  
  
|Oggetto di sistema|Descrizione|  
|-|-|  
|**MSDB**|Vengono archiviati i metadati e la cronologia di backup di tutti i backup creati da [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)|Abilita [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_advanced &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)|Configura le impostazioni avanzate per [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], ad esempio la crittografia.|  
|[managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|Crea una pianificazione personalizzata per [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_ backup_master_switch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-master-switch-transact-sql.md)|Sospende e riprende [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_set_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql.md)|Attiva e configura il monitoraggio per [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Esempi: abilitazione di eventi estesi, impostazioni della posta elettronica per le notifiche.|  
|[managed_backup.sp_backup_on_demand &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-on-demand-transact-sql.md)|Esegue un backup ad hoc di un database abilitato all'uso di [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] senza interruzione della catena di log.|  
|[managed_backup.fn_backup_db_config &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-backup-db-config-transact-sql.md)|Restituisce i valori di configurazione e di stato di [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] correnti per un database o per tutti i database nell'istanza.|  
|[managed_backup.fn_is_master_switch_on &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-is-master-switch-on-transact-sql.md)|Restituisce lo stato del parametro master.|  
|[managed_backup.sp_get_backup_diagnostics &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql.md)|Restituisce gli eventi registrati da Eventi estesi.|  
|[managed_backup.fn_get_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-parameter-transact-sql.md)|Restituisce i valori correnti delle impostazioni di sistema di backup, ad esempio le impostazioni di monitoraggio e di posta elettronica per gli avvisi.|  
|[managed_backup.fn_available_backups &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)|Recupera i backup disponibili per un determinato database o per tutti i database in un'istanza.|  
|[managed_backup.fn_get_current_xevent_settings &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-current-xevent-settings-transact-sql.md)|Restituisce le impostazioni correnti degli eventi estesi.|  
|[managed_backup.fn_get_health_status &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql.md)|Restituisce i conteggi aggregati di errori registrati dagli eventi estesi per un periodo specificato.|  
  
## <a name="backup-strategy"></a>Strategia di backup  
  
### <a name="backup-scheduling"></a>Pianificazione del backup  
 È possibile specificare una pianificazione di backup personalizzata usando la stored procedure di sistema [managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md). Se non viene specificata una pianificazione personalizzata, il tipo di backup pianificato e la frequenza di esecuzione vengono stabiliti in base al carico di lavoro del database. Le impostazioni del periodo di memorizzazione vengono utilizzate per determinare la durata di conservazione di un file di backup nell'archiviazione e la possibilità di recupero del database fino a una temporizzazione entro il periodo di memorizzazione.  
  
### <a name="backup-file-naming-conventions"></a>Convenzioni di denominazione dei file di backup  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] usa il contenitore specificato dall'utente, che ha quindi il controllo sul nome del contenitore. Per i file di backup, i database non di disponibilità vengono denominati usando la convenzione seguente: il nome viene creato usando i primi 40 caratteri del nome del database, il GUID del database senza '-' e il timestamp. Il carattere di sottolineatura viene inserito tra i segmenti come separatore. Per il backup completo viene utilizzata l'estensione di file **BAK** , mentre per i backup del log viene utilizzata **LOG** . Per i database del gruppo di disponibilità, oltre alla convenzione di denominazione file descritta in precedenza, viene aggiunto il GUID del database del gruppo di disponibilità dopo i 40 caratteri del nome del database. Il valore GUID del database del gruppo di disponibilità è il valore per group_database_id in sys.databases.  
  
### <a name="full-database-backup"></a>Backup completo del database  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pianifica un backup completo del database se si verifica una delle condizioni seguenti:  
  
-   Un database è abilitato da [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per la prima volta o quando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] viene abilitato con le impostazioni predefinite a livello di istanza.  
  
-   L'aumento delle dimensioni del log dopo l'ultimo backup completo del database è uguale o maggiore di 1 GB.  
  
-   È passato l'intervallo di tempo massimo di una settimana dall'ultimo backup completo del database.  
  
-   La catena di log è stata interrotta. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] vengono eseguiti controlli periodici per determinare se la catena di log è intatta confrontando il primo e l'ultimo LSN dei file di backup. Se si verifica un'interruzione nella catena di log per qualsiasi motivo, tramite [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] viene pianificato un backup completo del database. Nella maggior parte dei casi le interruzioni della catena di log sono dovute probabilmente a un comando di backup eseguito tramite Transact-SQL o mediante l'attività di backup in SQL Server Management Studio.  Tra gli altri scenari comuni sono incluse l'eliminazione accidentale dei file di log di backup o la sovrascrittura accidentale dei backup.  
  
### <a name="transaction-log-backup"></a>Backup del log delle transazioni  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pianifica un backup del log se si verifica una delle condizioni seguenti:  
  
-   Non sono disponibili cronologie di backup del log. Questa situazione si verifica in genere quando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] viene abilitato per la prima volta.  
  
-   Lo spazio del log delle transazioni utilizzato è pari ad almeno 5 MB.  
  
-   È stato raggiunto l'intervallo di tempo massimo di 2 ore dall'ultimo backup del log.  
  
-   Ogni volta che il backup del log delle transazioni è in ritardo rispetto a un backup completo del database. L'obiettivo è quello di anticipare la catena di log rispetto al backup completo.  
  
## <a name="retention-period-settings"></a>Impostazioni del periodo di memorizzazione  
 Quando si abilita il backup, è necessario impostare il periodo di conservazione in giorni: il valore minimo è pari a 1 giorno mentre quello massimo a 30 giorni.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] basato sulle impostazioni del periodo di memorizzazione viene valutata la possibilità di eseguire un recupero fino a una data e ora specifiche in un tempo specificato, per determinare quali file di backup mantenere e quali invece eliminare. L'oggetto backup_finish_date del backup viene utilizzato per determinare e far corrispondere il tempo specificato in base alle impostazioni del periodo di memorizzazione.  
  
## <a name="important-considerations"></a>Importanti considerazioni  
 Per un database, se è in esecuzione un processo di backup completo del database esistente, tramite [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] si attende il completamento del processo corrente prima di eseguire un altro backup completo dello stesso database. Analogamente, solo un backup del log delle transazioni può essere in esecuzione in un determinato momento. Tuttavia, un backup completo del database e un backup del log delle transazioni possono essere eseguiti contemporaneamente. Gli errori vengono registrati come eventi estesi.  
  
 Se sono pianificati più di 10 backup completi del database simultanei, viene generato un avviso tramite il canale di debug di eventi estesi. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] viene quindi gestita una coda di priorità per i database rimanenti di cui deve essere eseguito un backup fino alla pianificazione e al completamento di tutti i backup.  

> [!NOTE]
> Il backup gestito di SQL Server non è supportato con i server proxy.
>
  
##  <a name="supportability"></a><a name="support_limits"></a> Facilità di supporto  
 Le seguenti considerazioni e limitazioni del supporto sono specifiche di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]:  
  
-   Il backup di database di sistema **master**, **model** e **msdb** è supportato. Il backup di **tempdb** non è supportato. 
  
-   [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]supporta tutti i modelli di recupero (con registrazione completa, con registrazione minima delle operazioni bulk, con registrazione minima).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] Agent supporta solo backup del log e completi del database. L'automazione di backup di file non è supportata.  
  
-   Il servizio di archiviazione BLOB Microsoft Azure è l'unica opzione di archiviazione di backup supportata. I backup su disco o su nastro non sono supportati.  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] usa la funzionalità di backup su archivio BLOB in blocchi. La dimensione massima di un blob in blocchi è 200 GB. Se si adotta lo striping, la dimensione massima di un singolo backup può arrivare a 12 TB. Se il backup supera questo valore, è consigliabile usare la compressione e verificare la dimensione del file di backup prima di configurare [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. È possibile eseguire tale verifica con un backup in un disco locale o con un backup manuale nel servizio di archiviazione di Microsoft Azure usando l'istruzione Transact-SQL **BACKUP TO URL** . Per altre informazioni, vedere [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] può presentare alcune limitazioni se configurato con altre tecnologie che supportano il backup, la disponibilità elevata o il ripristino di emergenza.  
  
## <a name="see-also"></a>Vedere anche  
- [Abilitare il backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)   
- [Configurare le opzioni avanzate per il backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)   
- [Disabilitare il backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/disable-sql-server-managed-backup-to-microsoft-azure.md)
- [Backup e ripristino di Database di sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
  
