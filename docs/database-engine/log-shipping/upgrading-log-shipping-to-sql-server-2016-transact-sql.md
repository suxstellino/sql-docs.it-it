---
title: Aggiornare il log shipping a SQL Server 2016 e versioni successive
description: Informazioni sull'ordine appropriato per preservare la soluzione di ripristino di emergenza per il log shipping durante l'aggiornamento a SQL Server 2016 e versioni successive da una versione precedente.
ms.custom: seo-lt-2019
ms.date: 02/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], upgrading
ms.assetid: b1289cc3-f5be-40bb-8801-0e3eed40336e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b3e98cc2f5ce76093f3710f6f49e1b186219c04
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643845"
---
# <a name="upgrading-log-shipping-to-sql-server-2016-transact-sql"></a>Aggiornamento del log shipping a SQL Server 2016 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Durante l'aggiornamento da una configurazione per il log shipping di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una nuova versione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], un nuovo Service Pack di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un aggiornamento cumulativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'aggiornamento dei server di log shipping nell'ordine appropriato consentirà di preservare la soluzione di ripristino di emergenza per il log shipping.  
  
> [!NOTE]  
>  La[compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md) è stata introdotta in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]. In una configurazione per il log shipping aggiornata viene usata l'opzione di configurazione a livello di server **backup compression default** per controllare se la compressione dei backup viene usata per i file di backup del log delle transazioni. Il comportamento della compressione dei backup relativa ai backup del log può essere specificato per ogni configurazione per il log shipping. Per altre informazioni, vedere [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
 **Contenuto dell'argomento:**  
  
-   [Prerequisiti](#Prerequisites)  
  
-   [Protezione dei dati prima dell'aggiornamento](#ProtectData)  
  
-   [Aggiornamento dell'istanza del server di monitoraggio](#UpgradeMonitor)  
  
-   [Aggiornamento delle istanze del server secondario](#UpgradeSecondaries)  
  
-   [Aggiornamento dell'istanza primaria](#UpgradePrimary)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 Prima di iniziare, esaminare le informazioni seguenti:  
  
-   [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md): verificare che sia possibile eseguire l'aggiornamento a SQL Server 2016 dalla versione del sistema operativo Windows e di SQL Server. Ad esempio, non è possibile eseguire l'aggiornamento diretto da un'istanza di SQL Server 2005 a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
-   [Scegliere un Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): selezionare il metodo di aggiornamento appropriato e i passaggi in base alla verifica degli aggiornamenti di versione ed edizione supportati e anche in base agli altri componenti installati nell'ambiente interessato per aggiornare i componenti di ordine corretto.  
  
-   [Pianificare e testare il piano di aggiornamento del motore di database](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): esaminare le note sulla versione, i problemi di aggiornamento noti e l'elenco di controllo pre-aggiornamento e sviluppare e testare il piano di aggiornamento.  
  
-   [Hardware and Software Requirements for Installing SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md): esaminare i requisiti software per l'installazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Se è necessario software aggiuntivo, installarlo in ogni nodo prima di iniziare il processo di aggiornamento per ridurre al minimo eventuali tempi di inattività.  
  
##  <a name="protect-your-data-before-the-upgrade"></a><a name="ProtectData"></a> Protezione dei dati prima dell'aggiornamento  
 Prima di eseguire un aggiornamento del log shipping, è consigliabile proteggere i dati.  
  
 **Per proteggere i dati**  
  
1.  Eseguire un backup completo di ciascun database primario.  
  
     Per altre informazioni, vedere [Creare un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  Eseguire il comando [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) in ciascun database primario.  
  
> [!IMPORTANT]  
>  Assicurarsi di disporre di spazio sufficiente nel server primario per contenere le copie di backup del log per tutto il tempo previsto per l'aggiornamento dei server secondari.  Se è previsto il failover su un server secondario, la stessa considerazione è valida per il server secondario, ovvero il nuovo primario.  
  
##  <a name="upgrading-the-optional-monitor-server-instance"></a><a name="UpgradeMonitor"></a> Aggiornamento dell'istanza del server di monitoraggio (facoltativo)  
 L'istanza del server di monitoraggio, se presente, può essere aggiornata in qualsiasi momento. Non è tuttavia è necessario aggiornare il server di monitoraggio facoltativo quando si aggiornano i server primario e secondari.  
  
 Durante l'aggiornamento del server di monitoraggio, la configurazione per il log shipping continua a funzionare, ma lo stato relativo non viene registrato nelle tabelle sul monitor e non verrà attivato alcun avviso configurato. Dopo l'aggiornamento, è possibile eseguire la stored procedure di sistema [sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md) per aggiornare le informazioni contenute nelle tabelle di monitoraggio.   Per altre informazioni su un server di monitoraggio, vedere [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
##  <a name="upgrading-the-secondary-server-instances"></a><a name="UpgradeSecondaries"></a> Aggiornamento delle istanze del server secondario  
 Il processo di aggiornamento prevede prima di tutto l'aggiornamento delle istanze del server secondario di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] prima di aggiornare l'istanza del server primario. È sempre necessario aggiornare prima le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] secondarie. Il log shipping non viene interrotto durante tutto il processo di aggiornamento perché le istanze del server secondario aggiornate continuano a ripristinare i backup del log dall'istanza del server primario di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se l'istanza del server primario viene aggiornata prima dell'istanza del server secondario, il log shipping non funzionerà perché un backup creato in una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può essere ripristinato in una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile aggiornare le istanze secondarie contemporaneamente o in sequenza, ma tutte le istanze secondarie devono essere aggiornate prima dell'istanza primaria, per evitare un errore di log shipping.  
  
 Durante l'aggiornamento di un'istanza del server secondario, i processi di copia e ripristino del log shipping non vengono eseguiti. Questo significa che i backup del log delle transazioni non ripristinati si accumulano nel server primario ed è necessario avere a disposizione spazio sufficiente per conservare questi backup non ripristinati. La quantità di accumulo dipende dalla frequenza di esecuzione dei backup pianificati nell'istanza del server primario e dalla sequenza di aggiornamento delle istanze secondarie. Se è stato configurato un server di monitoraggio separato, inoltre, è possibile che vengano generati avvisi che indicano che i ripristini non sono stati eseguiti per un tempo maggiore rispetto all'intervallo configurato.  
  
 Al termine dell'aggiornamento delle istanze del server secondario, i processi dell'agente di log shipping riprendono e continuano con la copia e il ripristino dei backup del log dall'istanza del server primario alle istanze del server secondario. La quantità di tempo necessaria affinché le istanze de server secondario aggiornino il database secondario varia a seconda del tempo impiegato per aggiornare le istanze del server secondario e della frequenza dei backup nel server primario.  
  
> [!NOTE]  
>  Durante l'aggiornamento del server, il database secondario non viene aggiornato a un database di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Verrà aggiornato solo se viene portato online avviando un failover del database di distribuzione dei log. In teoria, questa condizione potrebbe persistere all'infinito. La quantità di tempo per aggiornare i metadati del database quando viene avviato un failover è limitata.  
  
> [!IMPORTANT]  
>  L'opzione RESTORE WITH STANDBY non è supportata per i database che richiedono aggiornamenti. Se un database secondario aggiornato è stato configurato tramite RESTORE WITH STANDBY, non sarà più possibile ripristinare i log delle transazioni dopo l'aggiornamento. Per riprendere il log shipping sul database secondario, sarà necessario configurarlo nuovamente sul server di standby. Per altre informazioni sull'opzione STANDBY, vedere [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="upgrading-the-primary-server-instance"></a><a name="UpgradePrimary"></a> Aggiornamento dell'istanza del server primario  
 Dato che il log shipping è principalmente una soluzione di ripristino di emergenza, lo scenario più semplice e più comune consiste nell'aggiornare l'istanza primaria sul posto e il database è semplicemente non disponibile durante l'aggiornamento. Dopo che il server è stato aggiornato, viene attivata automaticamente la modalità online per il database determinandone pertanto l'aggiornamento. Dopo che il database è stato aggiornato, viene ripresa l'esecuzione dei processi per il log shipping.  
  
> [!NOTE]  
>  Il log shipping supporta anche l'opzione per [Eseguire il failover in un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) e facoltativamente per la [modifica dei ruoli tra i server primario e secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md). Dato però che il log shipping viene ormai configurato molto raramente come soluzione a disponibilità elevata dato che le opzioni più recenti sono molto più efficaci, il failover non consente in genere di ridurre al massimo i tempi di inattività, in quanto gli oggetti di database di sistema non verranno sincronizzati e fare in modo che i client possano individuare facilmente un server secondario alzato di livello e connettersi a tale server può essere veramente complicato.  
  
## <a name="see-also"></a>Vedere anche  
 [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installare SQL Server 2016 dal prompt dei comandi](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)   
 [Monitorare il log shipping &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)   
 [Tabelle e stored procedure relative al log shipping](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
