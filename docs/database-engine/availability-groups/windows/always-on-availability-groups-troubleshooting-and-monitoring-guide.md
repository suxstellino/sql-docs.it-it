---
title: Guida al monitoraggio e alla risoluzione dei problemi dei gruppi di disponibilità
description: Indice dei contenuti per iniziare rapidamente a monitorare i gruppi di disponibilità Always On e a risolverne alcuni problemi comuni.
ms.custom: seo-lt-2019
ms.date: 05/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
ms.assetid: 8d6d9954-ff6b-4e58-882e-eff0174f0d07
author: rothja
ms.author: jroth
ms.openlocfilehash: 5594a39b6ab4f8c9c6bd2edaadbe64e99c983229
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643694"
---
# <a name="monitor-and-troubleshoot-availability-groups"></a>Monitoraggio e risoluzione dei problemi dei gruppi di disponibilità
 Questa guida consente di iniziare rapidamente a monitorare i gruppi di disponibilità Always On e a risolvere alcuni problemi più comuni dei gruppi di disponibilità. Riporta contenuto originale nonché una pagina di destinazione con informazioni utili pubblicate anche altrove. Sebbene in questa guida non sia possibile trattare in modo approfondito tutti i problemi che possono verificarsi nell'ambito dei gruppi di disponibilità, l'utente troverà un'indicazione della direzione da seguire per l'analisi della causa principale e la risoluzione dei problemi. 
 
 Poiché i gruppi di disponibilità sono una tecnologia integrata, molti problemi riscontrati potrebbero essere sintomi di altri problemi nel sistema di database. Alcuni problemi sono causati dalle impostazioni del gruppo di disponibilità, ad esempio la sospensione di un database di disponibilità. Altri problemi possono riguardare altri aspetti di SQL Server, ad esempio le impostazioni di SQL Server, le distribuzioni dei file di database e i problemi di prestazioni del sistema non legati alla disponibilità. Inoltre, possono esistere altri problemi al di fuori di SQL Server legati, ad esempio all'I/O di rete, a TCP/IP, ad Active Directory e a WSFC. I problemi che emergono in un gruppo di disponibilità, in una replica o in un database richiedono spesso l'analisi di varie tecnologie in cui cercare la causa principale.  
  
  
##  <a name="troubleshooting-scenarios"></a><a name="BKMK_SCENARIOS"></a> Scenari di risoluzione dei problemi  
 Nella tabella seguente sono riportati collegamenti agli scenari di risoluzione dei problemi comuni per i gruppi di disponibilità. Questi sono suddivisi in categorie in base al tipo di scenario, ad esempio configurazione, connettività client, failover e prestazioni.  
  
|Scenario|Tipo di scenario|Descrizione|  
|--------------|-------------------|-----------------|  
|[Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità Always On &#40;SQL Server&#41;](troubleshoot-always-on-availability-groups-configuration-sql-server.md)|Configurazione|Vengono fornite informazioni per la risoluzione dei problemi tipici relativi alla configurazione delle istanze del server per i gruppi di disponibilità. Tra i problemi di configurazione tipici sono inclusi la disabilitazione dei gruppi di disponibilità, la configurazione errata degli account, l'endpoint del mirroring del database inesistente, l'endpoint inaccessibile (errore di SQL Server 1418), l'accesso alla rete inesistente e l'esito negativo di un comando di creazione di join del database (errore di SQL Server 35250).|  
|[Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)|Configurazione|Un'operazione di aggiunta file ha causato la sospensione del database secondario e l'attivazione dello stato NON IN SINCRONIZZAZIONE.|  
|[Impossibile connettersi al listener del gruppo di disponibilità in un ambiente con più subnet](https://support.microsoft.com/kb/2792139/en-us)|Connettività client|Dopo aver configurato il listener del gruppo di disponibilità non si riesce a effettuare il ping del listener o a connettersi a esso da un'applicazione.|  
|[Risoluzione dei problemi dei failover automatici non riusciti](https://support.microsoft.com/kb/2833707)|Failover|Un failover automatico non è stato completato correttamente.|  
|[Risoluzione dei problemi: Il gruppo di disponibilità ha superato la soglia RTO](troubleshoot-availability-group-exceeded-rto.md)|Prestazioni|Dopo un failover automatico o un failover manuale pianificato senza perdita di dati, il tempo di failover supera l'obiettivo RTO. Oppure, quando si stima il tempo di failover di una replica secondaria con commit asincrono (ad esempio un partner di failover automatico), si scopre che supera l'obiettivo RTO.|  
|[Risoluzione dei problemi: Il gruppo di disponibilità ha superato la soglia RPO](troubleshoot-availability-group-exceeded-rpo.md)|Prestazioni|Dopo aver eseguito un failover manuale forzato, la perdita di dati è maggiore dell'obiettivo RPO. In alternativa, quando si calcola la potenziale perdita di dati di una replica secondaria con commit asincrono, si scopre che supera l'obiettivo RPO.|  
|[Risoluzione dei problemi: Le modifiche nella replica primaria non vengono riflesse nella replica secondaria](troubleshoot-primary-changes-not-reflected-on-secondary.md)|Prestazioni|L'applicazione client completa con esito positivo un aggiornamento per la replica primaria, ma l'esecuzione di query sulla replica secondaria rivela che qui la modifica non è stata eseguita.|  
|[Risoluzione dei problemi: Tipo di attesa HADR_SYNC_COMMIT elevato con i con gruppi di disponibilità Always On](/archive/blogs/sql_server_team/troubleshooting-high-hadr_sync_commit-wait-type-with-always-on-availability-groups)|Prestazioni|Se HADR_SYNC_COMMIT richiede dei tempi insolitamente lunghi, esiste un problema di prestazioni nel flusso di spostamento dei dati o nella protezione avanzata del log di replica secondaria.|  

##  <a name="useful-tools-for-troubleshooting"></a><a name="BKMK_TOOLS"></a> Strumenti utili per la risoluzione dei problemi  
 In fase di configurazione o di esecuzione dei gruppi di disponibilità, vari strumenti possono aiutare a diagnosticare diversi tipi di problemi. Nella tabella seguente vengono forniti collegamenti a informazioni utili su tali strumenti.  
  
|Strumento|Descrizione|  
|----------|-----------------|  
|[Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](use-the-always-on-dashboard-sql-server-management-studio.md)|Visibilità immediata dell'integrità del gruppo di disponibilità in un'interfaccia intuitiva.|  
|[Criteri Always On](always-on-policies.md)|Usati dalla dashboard Always On.|  
|[Log degli errori di SQL Server &#40;gruppi di disponibilità Always On&#41;](sql-server-error-log-always-on-availability-groups.md)|Registra gli eventi di transizione dello stato di gruppi di disponibilità, repliche e database, lo stato di altri componenti Always On e gli errori Always On.|  
|[CLUSTER.LOG &#40;gruppi di disponibilità Always On&#41;](cluster-log-always-on-availability-groups.md)|Registra gli eventi cluster, comprese le transizioni di stato della risorsa del gruppo di disponibilità, nonché gli eventi e gli errori dalla DLL risorse SQL Server.|  
|[Log di diagnostica dell'integrità Always On](always-on-health-diagnostics-log.md)|Registra la diagnostica dell'integrità di SQL Server segnalata nel cluster WSFC (DLL risorse SQL Server) da [sp_server_diagnostics &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md).|  
|[DMV e viste del catalogo di sistema &#40;gruppi di disponibilità Always On&#41;](dynamic-management-views-and-system-catalog-views-always-on-availability-groups.md)|Riporta informazioni sui gruppi di disponibilità, ad esempio configurazione, stato di integrità e metriche delle prestazioni.|  
|[Eventi estesi Always On](always-on-extended-events.md)|Fornisce dati diagnostici dettagliati sui gruppi di disponibilità utili per l'analisi causa principale.|  
|[Tipi di attesa Always On](always-on-wait-types.md)|Fornisce statistiche relative alle attese specifiche ai gruppi di disponibilità e utili per ottimizzare le prestazioni.|  
|Contatori delle prestazioni Always On|Consentono di monitorare le attività dei gruppi di disponibilità, sono riportati in Monitor di sistema e sono utili per ottimizzare le prestazioni. Per altre informazioni, vedere [Replica di disponibilità di SQL Server](~/relational-databases/performance-monitor/sql-server-availability-replica.md) e [Replica di database di SQL Server](~/relational-databases/performance-monitor/sql-server-database-replica.md).|  
|[Buffer circolari Always On](always-on-ring-buffers.md)|Registrano gli avvisi all'interno del sistema SQL Server per la diagnostica interna e possono essere usati per eseguire il debug di problemi relativi ai gruppi di disponibilità.|  
  
##  <a name="monitoring-availability-groups"></a><a name="BKMK_MONITOR"></a> Monitoraggio dei gruppi di disponibilità  
 Il momento ideale per risolvere i problemi di un gruppo di disponibilità è prima che il problema richieda un failover automatico o manuale. Questa attività può essere eseguita monitorando le metriche delle prestazioni del gruppo di disponibilità e configurando l'invio di avvisi quando le repliche di disponibilità hanno prestazioni che non rientrano nei limiti del contratto di servizio (SLA). Se una replica secondaria asincrona ha problemi di prestazioni che determinano l'aumento del tempo di failover stimato, ad esempio, non è consigliabile attendere un failover automatico e scoprire che il tempo di failover supera l'obiettivo del tempo di ripristino.  
  
 Poiché i gruppi di disponibilità sono una soluzione di elevata disponibilità e di ripristino di emergenza, le metriche delle prestazioni più importanti da monitorare sono il tempo di failover stimato, che influenza l'obiettivo tempo di ripristino (RTO), e la potenziale perdita di dati in caso di emergenza, che interessa l'obiettivo punto di ripristino (RPO). Queste metriche possono essere raccolte dai dati che SQL Server espone in qualsiasi momento, pertanto è possibile ricevere avvisi di un problema delle funzionalità di ripristino di emergenza a disponibilità elevata (HADR) nel sistema prima che si verifichino veri eventi di errore. Pertanto, è importante acquisire familiarità con il processo di sincronizzazione dei dati dei gruppi di disponibilità e di raccolta delle relative metriche.  
  
 La tabella riportata di seguito fa riferimento ad argomenti che aiutano a monitorare l'integrità della soluzione dei gruppi di disponibilità.  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Monitorare le prestazioni per i gruppi di disponibilità Always On](monitor-performance-for-always-on-availability-groups.md)|Descrive il processo di sincronizzazione dei dati per i gruppi di disponibilità, i controlli di flusso e le metriche utili per il monitoraggio di un gruppo di disponibilità. Viene illustrato anche come raccogliere le metriche per gli obiettivi RTO e RPO.|  
|[Monitoraggio dei gruppi di disponibilità &#40;SQL Server&#41;](monitoring-of-availability-groups-sql-server.md)|Fornisce informazioni sugli strumenti per eseguire il monitoraggio di un gruppo di disponibilità.|  
|[The Always On Health Model Part 1: Health Model Architecture (Modello di integrità Always On, parte 1: Architettura del modello di integrità)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)|Fornisce una panoramica del modello di integrità Always On.|  
|[The Always On health model, part 2: Extending the health model (Modello di integrità Always On, parte 2: Estensione del modello di integrità)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)|Illustra come personalizzare il modello di integrità Always On e il dashboard Always On per visualizzare informazioni aggiuntive.|  
|[Monitoring Always On health with PowerShell - Part 1: Basic cmdlet overview (Monitoraggio dell'integrità AlwaysOn con PowerShell, parte 1: Panoramica sui cmdlet di base)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)|Fornisce una panoramica sui cmdlet PowerShell Always On che possono essere utilizzati per monitorare l'integrità di un gruppo di disponibilità.|  
|[Monitoring Always On health with PowerShell, part 2: Advanced cmdlet usage (Monitoraggio dell'integrità Always On con PowerShell, parte 2: Utilizzo avanzato dei cmdlet)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)|Fornisce informazioni sull'utilizzo avanzato dei cmdlet PowerShell Always On per monitorare l'integrità di un gruppo di disponibilità.|  
|[Monitoring Always On health with PowerShell, part 3: A simple monitoring application (Monitoraggio dell'integrità Always On con PowerShell, parte 3: Applicazione di monitoraggio semplice)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)|Illustra come monitorare automaticamente un gruppo di disponibilità con un'applicazione.|  
|[Monitoring Always On health with PowerShell, part 4: Integration with SQL Server Agent (Monitoraggio dell'integrità Always On con PowerShell, parte 4: Integrazione con SQL Server Agent)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)|Fornisce informazioni su come integrare il monitoraggio dei gruppi di disponibilità con SQL Server Agent e configurare la notifica delle entità appropriate in caso di problemi.|  

## <a name="next-steps"></a>Passaggi successivi  
 [SQL Server Always On Team Blog (Blog di SQL Server Always On)](/archive/blogs/sqlalwayson/)   
 [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
