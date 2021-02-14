---
title: Configurare avvisi per le metriche delle prestazioni di mirroring del database
description: 'Linee guida per la configurazione di avvisi e valori soglia degli avvisi per le metriche delle prestazioni usate dal mirroring del database. '
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- thresholds [SQL Server]
- database mirroring [SQL Server], managing in SQL Server Management Studio
- alerts [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
- warnings [database mirroring]
ms.assetid: 8cdd1515-0bd7-4f8c-a7fc-a33b575e20f6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b7b014ffdd1377dcda2bfd0c72e4b91b6116a71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352178"
---
# <a name="use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server"></a>Utilizzare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento sono incluse informazioni sugli eventi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per cui è possibile configurare e gestire valori soglia degli avvisi per il mirroring del database. È possibile usare Monitoraggio mirroring del database o le stored procedure **sp_dbmmonitorchangealert**, **sp_dbmmonitorhelpalert** e **sp_dbmmonitordropalert** . Nell'argomento sono inoltre incluse informazioni relative alle configurazione degli avvisi per gli eventi di mirroring del database.  
  
 Dopo aver stabilito il monitoraggio per un database con mirroring, un amministratore di sistema può configurare soglie di avviso su alcune metriche chiave delle prestazioni. Un amministratore può inoltre configurare avvisi su questi e altri eventi di mirroring del database.  
  
  
##  <a name="performance-metrics-and-warning-thresholds"></a><a name="PerfMetricsAndWarningThresholds"></a> Misurazioni delle prestazioni e delle soglie di avviso  
 Nella tabella seguente vengono elencate le misurazioni delle prestazioni per cui è possibile configurare avvisi, vengono descritte le soglie di avviso corrispondenti ed elencate le etichette di Monitoraggio mirroring del database corrispondenti.  
  
|Misurazione delle prestazioni|Valore soglia avvisi|Etichetta di Monitoraggio mirroring del database|  
|------------------------|-----------------------|--------------------------------------|  
|Log non inviato|Specifica la quantità di log non inviati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di KB ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|**Avvisa se il log non inviato supera la soglia**|  
|Log non ripristinato|Specifica la quantità di log non ripristinati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server mirror. Questo avviso consente di misurare il tempo di failover. Il *tempo di failover* corrisponde essenzialmente al tempo necessario al server mirror precedente per eseguire il rollforward di tutti i log rimanenti nella propria coda di rollforward, più un breve tempo aggiuntivo.<br /><br /> Nota: per un failover automatico, il tempo necessario al sistema per rilevare l'errore è indipendente dal tempo di failover.<br /><br /> Per altre informazioni, vedere [Stimare l'interruzione del servizio durante il cambio di ruolo &#40;mirroring del database&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|**Avvisa se il log non ripristinato supera la soglia**|  
|Transazione non inviata meno recente|Specifica la quantità di transazioni, espressa in minuti, che può accumularsi nella coda di invio prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di tempo ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|**Avvisa se il tempo di memorizzazione della transazione non inviata meno recente è superiore alla soglia**|  
|Overhead commit mirror|Specifica il ritardo medio per transazione, espresso in millisecondi, che è consentito prima che venga generato un avviso nell'istanza del server principale. Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward. Questo valore è rilevante solo nella modalità a sicurezza elevata.|**Avvisa se l'overhead di commit del mirror supera la soglia**|  
  
 Per qualsiasi di queste misurazioni delle prestazioni, un amministratore di sistema può specificare una soglia su un database con mirroring. Per ulteriori informazioni, vedere [Impostazione e gestione delle soglie di avviso](#SetUpManageWarningThresholds), più avanti in questo argomento.  
  
##  <a name="setting-up-and-managing-warning-thresholds"></a><a name="SetUpManageWarningThresholds"></a> Impostazione e gestione delle soglie di avviso  
 Un amministratore di sistema può configurare uno o più soglie di avviso per le misurazioni chiave delle prestazioni di mirroring. È consigliabile impostare una soglia per un determinato avviso su entrambi i partner per assicurare che l'avviso persista in caso di failover del database. La soglia appropriata per ogni partner dipende dalle capacità in termini di prestazioni del sistema di tale partner.  
  
 È possibile configurare e gestire le soglie di avviso utilizzando uno degli elementi seguenti:  
  
-   Monitoraggio mirroring del database  
  
     In Monitoraggio mirroring del database l'amministratore può visualizzare contemporaneamente la configurazione corrente degli avvisi per un database selezionato nelle istanze del server mirror e del server principale selezionando la pagina a schede **Avvisi** . Da questa pagina, l'amministratore può aprire la finestra di dialogo **Imposta soglie di avviso** per abilitare e configurare uno o soglie di avviso.  
  
     Per un'introduzione all'interfaccia di Monitoraggio mirroring del database, vedere [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Per informazioni sull'avvio del Monitoraggio mirroring del database, vedere [Avviare Monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Stored procedure di sistema  
  
     Il set seguente di stored procedure di sistema consente a un amministratore di impostare e gestire le soglie di avviso su database con mirroring di un partner alla volta.  
  
    |Procedura|Descrizione|  
    |---------------|-----------------|  
    |[sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)|Aggiunge o modifica la soglia di avviso per una misurazione delle prestazioni di mirroring specificata.|  
    |[sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)|Restituisce informazioni sulle soglie di avviso su una o tutte le misurazioni delle prestazioni di monitoraggio del mirroring del database.|  
    |[sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)|Elimina l'avviso per una misurazione delle prestazioni specificata.|  
  
## <a name="performance-threshold-events-sent-to-the-windows-event-log"></a>Eventi di soglia delle prestazioni inviati al registro eventi di Windows  
 Se viene definito un valore soglia avviso per una misurazione delle prestazioni, quando viene aggiornata la tabella di stato viene valutato il valore più recente rispetto al valore soglia. Se viene raggiunto il valore soglia, la procedura di aggiornamento, **sp_dbmmonitorupdate**, genera un evento informativo, un *evento di soglia delle prestazioni*, per la metrica e scrive l'evento nel registro eventi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Nella tabella seguente sono elencati gli ID degli eventi di soglia delle prestazioni.  
  
|Misurazione delle prestazioni|ID evento|  
|------------------------|--------------|  
|Log non inviato|32042|  
|Log non ripristinato|32043|  
|Transazione non inviata meno recente|32040|  
|Overhead commit mirror|32044|  
  
> [!NOTE]  
>  Un amministratore può definire avvisi su uno o più di questi eventi. Per ulteriori informazioni, vedere [Utilizzo di avvisi per un database con mirroring](#UseAlerts), più avanti in questo  
>   
>  argomento.  
  
##  <a name="using-alerts-for-a-mirrored-database"></a><a name="UseAlerts"></a> Utilizzo di avvisi per un database con mirroring  
 Una parte importante del monitoraggio di un database con mirroring consiste nella configurazione di avvisi sugli eventi significativi di mirroring del database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera i tipi seguenti di eventi del mirroring del database:  
  
-   Eventi di soglia delle prestazioni  
  
     Per ulteriori informazioni, vedere "Eventi di soglia delle prestazioni inviati al registro eventi di Windows" più indietro in questo argomento.  
  
-   Eventi di modifica dello stato  
  
     Si tratta di eventi WMI che vengono generati quando si verificano modifiche nello stato interno di una sessione di mirroring del database.  
  
    > [!NOTE]  
    >  Per altre informazioni, vedere [Concetti relativi al provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 Un amministratore di sistema può configurare avvisi su questi eventi utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent o altre applicazioni quali [!INCLUDE[msCoName](../../includes/msconame-md.md)] Operations Manager.  
  
 Quando si definiscono avvisi su eventi di mirroring del database, è consigliabile definire valori soglia avvisi su entrambe le istanze dei server partner. Nel server principale o nel server mirror vengono generati singoli eventi, ma ognuno dei partner può eseguire in qualsiasi momento uno o l'altro dei due ruoli. Per assicurarsi che un avviso continui a funzionare dopo un failover, l'avviso deve essere definito su entrambi i partner.  
  
> [!IMPORTANT]  
>  Per tutte le sessioni di mirroring, è consigliabile configurare il database per l'invio di un avviso per qualsiasi evento di modifica di stato. A meno che non sia prevista una modifica dello stato in seguito a una modifica manuale della configurazione, si è verificato un evento che potrebbe compromettere i dati. Per proteggere i dati, identificare e correggere la causa della modifica imprevista dello stato.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per creare un avviso utilizzando SQL Server Management Studio**  
  
-   [Creazione di un avviso utilizzando un numero di errore](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Creare un avviso per evento WMI](../../ssms/agent/create-a-wmi-event-alert.md)  
  
 **Per monitorare il mirroring del database**  
  
-   [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
