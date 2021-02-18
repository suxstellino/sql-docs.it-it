---
title: Applicazione sqllogship
description: L'applicazione sqllogship esegue un'operazione di backup, copia o ripristino e le attività di pulizia per una configurazione per il log shipping in un database di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqllogship
ms.assetid: 8ae70041-f3d9-46e4-8fa8-31088572a9f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 349b8f5bd757e623b79b043d14ce9de91c34e942
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341981"
---
# <a name="sqllogship-application"></a>Applicazione sqllogship
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L'applicazione **sqllogship** esegue un'operazione di backup, copia o ripristino e le attività di pulizia associate per una configurazione per il log shipping. L'operazione viene eseguita su una specifica istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per un database specifico.  
  
 ![Icona di collegamento a un argomento](../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") Per le convenzioni di sintassi, vedere [Guida di riferimento alle utilità del prompt dei comandi &#40;Motore di database&#41;](../tools/command-prompt-utility-reference-database-engine.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqllogship -server instance_name { -backup primary_id | -copy secondary_id | -restore secondary_id } [ -verboselevel level ] [ -logintimeout timeout_value ] [ -querytimeout timeout_value ]  
```  
  
## <a name="arguments"></a>Argomenti  
 **-server** _instance_name_  
 Specifica l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in cui verrà eseguita l'operazione. L'istanza del server da specificare dipende dall'operazione di distribuzione dei log specificata. Nel caso dell'operazione **-backup**, *instance_name* deve essere il nome del server primario in una configurazione per il log shipping. Nel caso di un'operazione **-copy** o **-restore**, *instance_name* deve essere il nome del server secondario in una configurazione per il log shipping.  
  
 **-backup** _primary_id_  
 Esegue un'operazione di backup per il database primario corrispondente all'ID primario specificato tramite *primary_id*. Per ottenere questo ID, selezionarlo dalla tabella di sistema [log_shipping_primary_databases](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md) oppure usare la stored procedure [sp_help_log_shipping_primary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md) .  
  
 L'operazione di backup crea il backup del log nella directory di backup. L'applicazione **sqllogship** elimina quindi tutti i file di backup meno recenti in base al periodo di conservazione dei file. La cronologia per l'operazione di backup viene poi registrata dall'applicazione nel server primario e nel server di monitoraggio. L'applicazione esegue infine [sp_cleanup_log_shipping_history](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)che elimina le informazioni sulla cronologia meno recenti in base al periodo di conservazione.  
  
 **-copy** _secondary_id_  
 Esegue un'operazione di copia per copiare i backup dal server secondario specificato per il database o i database secondari, corrispondenti all'ID secondario specificato tramite *secondary_id*. Per ottenere questo ID, selezionarlo dalla tabella di sistema [log_shipping_secondary](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md) oppure usare la stored procedure [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .  
  
 L'operazione copia i file di backup dalla directory di backup alla directory di destinazione. L'applicazione **sqllogship** registra quindi la cronologia per l'operazione di copia nel server secondario e nel server di monitoraggio.  
  
 **-restore** _secondary_id_  
 Esegue un'operazione di ripristino nel server secondario specificato per il database o i database secondari, corrispondenti all'ID secondario specificato tramite *secondary_id*. Per ottenere questo ID è possibile usare la stored procedure **sp_help_log_shipping_secondary_database** .  
  
 Tutti i file di backup nella directory di destinazione creati dopo il punto di ripristino più recente vengono ripristinati nel database o nei database secondari. L'applicazione **sqllogship** elimina quindi tutti i file di backup meno recenti in base al periodo di conservazione dei file. La cronologia per l'operazione di ripristino viene poi registrata dall'applicazione nel server secondario e nel server di monitoraggio. L'applicazione esegue infine **sp_cleanup_log_shipping_history** che elimina le informazioni sulla cronologia meno recenti in base al periodo di conservazione.  
  
 **-verboselevel** _level_  
 Specifica il livello di messaggi aggiunti alla cronologia di log shipping. *level* può essere uno dei valori interi seguenti:  
  
|Level|Descrizione|  
|-----------|-----------------|  
|0|L'output non include messaggi di traccia e di debug.|  
|1|L'output include messaggi di gestione degli errori.|  
|2|L'output include messaggi di avviso e di gestione degli errori.|  
|**3**|L'output include messaggi informativi, di avviso e di gestione degli errori. Si tratta del valore predefinito.|  
|4|L'output include tutti i messaggi di debug e di traccia.|  
  
 **-logintimeout** _timeout_value_  
 Specifica la quantità di tempo assegnata per un tentativo di accesso all'istanza del server prima del timeout. Il valore predefinito è 15 secondi. *timeout_value* is **int** _._  
  
 **-querytimeout** _timeout_value_  
 Specifica la quantità di tempo assegnata per l'avvio dell'operazione specificata prima del timeout del tentativo. Il valore predefinito non prevede un periodo di timeout. *timeout_value* is **int** _._  
  
## <a name="remarks"></a>Osservazioni  
 È consigliabile utilizzare i processi di backup, copia e ripristino per eseguire le operazioni corrispondenti, quando possibile. Per avviare questi processi da un'operazione batch o un'altra applicazione, chiamare la stored procedure [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) .  
  
 La cronologia di log shipping creata da **sqllogship** è intercalata dalla cronologia creata dai processi di backup, copia e ripristino del log shipping. Se si prevede di usare ripetutamente **sqllogship** per eseguire operazioni di backup, copia o ripristino per una configurazione per il log shipping, prendere in considerazione di disabilitare il processo o i processi per il log shipping corrispondenti. Per altre informazioni, vedere [Disable or Enable a Job](../ssms/agent/disable-or-enable-a-job.md).  
  
 L'applicazione **sqllogship** , SqlLogShip.exe, è installata nella directory x:\Program Files\Microsoft SQL Server\130\Tools\Binn.  
  
## <a name="permissions"></a>Autorizzazioni  
 **sqllogship** usa l'autenticazione di Windows. L'account con autenticazione di Windows utilizzato per l'esecuzione del comando deve disporre delle autorizzazioni di accesso alle directory di Windows e delle autorizzazioni per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Il requisito dipende dal fatto che il comando **sqllogship** specifichi l'opzione **-backup**, **-copy** oppure **-restore** .  
  
|Opzione|Accesso alla directory|Autorizzazioni|  
|------------|----------------------|-----------------|  
|**-backup**|È richiesto l'accesso in lettura/scrittura alla directory di backup.|Sono richieste le stesse autorizzazioni necessarie per l'istruzione BACKUP. Per altre informazioni, vedere [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md).|  
|**-copy**|È richiesto l'accesso in lettura alla directory di backup e l'accesso in scrittura alla directory di copia.|Sono richieste le stesse autorizzazioni necessarie per la stored procedure [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .|  
|**-restore**|È richiesto l'accesso in lettura/scrittura alla directory di copia.|Sono richieste le stesse autorizzazioni necessarie per l'istruzione RESTORE. Per altre informazioni, vedere [RESTORE &#40;Transact-SQL&#41;](../t-sql/statements/restore-statements-transact-sql.md).|  
  
> [!NOTE]  
>  Per trovare i percorsi delle directory di backup e di copia, è possibile eseguire la stored procedure **sp_help_log_shipping_secondary_database** o visualizzare la tabella **log_shipping_secondary** in **msdb**. I percorsi della directory di backup e della directory di destinazione sono indicati rispettivamente nelle colonne **backup_source_directory** e **backup_destination_directory** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)  
  
  
