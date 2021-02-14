---
title: 'Mirroring del database: Prerequisiti, restrizioni e raccomandazioni'
description: Informazioni sui prerequisiti e le restrizioni e consigli per la configurazione del mirroring del database con SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- partners [SQL Server]
- database mirroring [SQL Server], prerequisites
- database mirroring [SQL Server], recommendations
- database mirroring [SQL Server], restrictions
- database mirroring [SQL Server], planning
- database mirroring [SQL Server], about database mirroring
ms.assetid: fdcf2251-9895-44c6-b81e-768fef32e732
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 95dd833cfe2fa558bc03b2aa68b29ac5b13d3272
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343283"
---
# <a name="prerequisites-restrictions-and-recommendations-for-database-mirroring"></a>Prerequisiti restrizioni e indicazioni per il mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 In questo argomento vengono descritti i prerequisiti e le indicazioni per l'impostazione del mirroring del database. Per un'introduzione al mirroring del database, vedere [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
  
##  <a name="support-for-database-mirroring"></a><a name="DbmSupport"></a> Supporto per il mirroring del database  
 Per informazioni sul supporto del mirroring del database in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vedere [Edizioni e funzionalità supportate in SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).
  
 Si noti che il mirroring del database funziona con qualsiasi livello di compatibilità del database supportato. Per informazioni sui livelli di compatibilità supportati, vedere [Livello di compatibilità di ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Affinché venga stabilita una sessione di mirroring, i partner e il server di controllo, se presente, devono essere in esecuzione nella stessa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   I due partner, cioè il server principale e il server mirror, devono essere in esecuzione nella stessa edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il server di controllo, se presente, può essere eseguito in qualsiasi edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che supporta il mirroring del database.  
  
    > [!NOTE]  
    >  È possibile aggiornare istanze del server che operano come partner in una sessione di mirroring a una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Aggiornamento di istanze con mirroring](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   Il database deve utilizzare il modello di recupero con registrazione completa. I modelli di recupero con registrazione minima e con registrazione minima delle operazioni bulk non supportano il mirroring del database. Di conseguenza, le operazioni bulk vengono sempre registrate completamente per un database con mirroring. Per informazioni sui modelli di recupero, vedere [Modelli di recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
-   Verificare che nel server mirror sia disponibile spazio su disco sufficiente per il database mirror.  
  
    > [!NOTE]  
    >  Per informazioni sull'utilizzo del mirroring del database in un database replicato, vedere [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
-   Durante la creazione del database mirror nel server mirror, verificare che il backup del database principale venga ripristinato specificando lo stesso nome di database con l'opzione WITH NORECOVERY. È inoltre necessario applicare tutti i backup del log creati dopo l'esecuzione di tale backup, sempre tramite WITH NORECOVERY.  
  
    > [!IMPORTANT]  
    >  Se il mirroring del database è stato arrestato, prima di poterlo riavviare è necessario che eventuali backup di log successivi eseguiti sul database principale vengano applicati al database mirror.  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> Restrizioni  
  
-   Il mirroring può essere eseguito solo dei database utente. Non è possibile eseguire il mirroring dei database **master**, **msdb**, **tempdb** o **model** .  
  
-   Un database con mirroring non può essere rinominato durante una sessione di mirroring del database.  
  
-   Il mirroring del database non supporta FILESTREAM. Non è possibile creare un filegroup FILESTREAM nel server principale, né configurare il mirroring per un database in cui sono contenuti filegroup FILESTREAM.  
  
-   Il mirroring del database non è supportato né nelle transazioni tra database né nelle transazioni distribuite. Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
  
##  <a name="recommendations-for-configuring-partner-servers"></a><a name="RecommendationsForPartners"></a> Indicazioni per la configurazione dei server partner  
  
-   È consigliabile che i partner siano eseguiti in sistemi simili, in grado di gestire carichi di lavoro identici.  
  
    > [!NOTE]  
    >  Se si intende utilizzare la modalità a protezione elevata con failover automatico, il carico normale su ogni partner di failover dovrebbe utilizzare meno del 50% della CPU. Se il carico di lavoro determina un overload della CPU, un partner di failover potrebbe non essere in grado di eseguire il ping delle altre istanze del server nella sessione di mirroring, determinando un failover non necessario. Se non è possibile mantenere l'utilizzo della CPU al di sotto del 50%, è consigliabile utilizzare la modalità a protezione elevata senza failover automatico o la modalità a prestazioni elevate.  
  
-   Se possibile, è consigliabile che il percorso del database mirror, inclusa la lettera di unità, sia identico a quello del database principale. È necessario includere l'opzione MOVE nell’istruzione RESTORE se i layout dei file sono diversi, ad esempio se il database principale è sull'unità 'F:' ma il sistema di mirror non ha un'unità F:.  
  
    > [!IMPORTANT]  
    >  Se durante la creazione del database mirror i file del database vengono spostati, potrebbe risultare impossibile aggiungere successivamente file al database senza sospendere il mirroring.  
  
-   È consigliabile che tutte le istanze del server in una sessione di mirroring utilizzino la stessa tabella codici master e le stesse regole di confronto. Eventuali differenze possono determinare un problema durante l'impostazione del mirroring.  
  
-   Facoltativamente, valutare i tempi necessari per il failover su un database, in modo da verificare se la configurazione del sistema consente di ottenere le prestazioni desiderate. Per altre informazioni, vedere [Stimare l'interruzione del servizio durante il cambio di ruolo &#40;mirroring del database&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).  
  
-   Per ottimizzare le prestazioni, utilizzare per il mirroring una scheda di rete dedicata (scheda di interfaccia di rete).  
  
-   Non vengono date indicazioni sull'affidabilità di una rete WAN (Wide Area Network) per il mirroring del database in modalità a sicurezza elevata. Se si decide di utilizzare la modalità a protezione elevata in una rete WAN, fare attenzione in caso di aggiunta di un server di controllo del mirroring alla sessione, in quanto potrebbero verificarsi failover automatici indesiderati. Per altre informazioni, vedere [Indicazioni per la distribuzione del mirroring del database](#RecommendationsForDeploying)di seguito in questo argomento.  
  
  
##  <a name="recommendations-for-deploying-database-mirroring"></a><a name="RecommendationsForDeploying"></a> Indicazioni per la distribuzione del mirroring del database  
 L'operazione asincrona consente di ottenere prestazioni ottimali per il mirroring del database. Le prestazioni di una sessione di mirroring che utilizza l'operazione asincrona possono essere rallentate quando il relativo carico di lavoro genera notevoli quantità di dati del log delle transazioni.  
  
 Negli ambienti di prova è consigliabile analizzare tutte le modalità operative per valutare le prestazioni del mirroring del database. Tuttavia, prima di distribuire il mirroring in un ambiente di produzione, accertarsi di aver compreso il funzionamento della rete in condizioni normali nel mondo reale.  
  
 La modalità a protezione elevata con failover automatico è progettata per una rete con elevati livelli di servizio con una connessione dedicata o una configurazione di rete piuttosto semplice in grado di ridurre al minimo le cause dei possibili errori di rete. Questo tipo di ambiente di rete a qualità elevata è necessario per la modalità a protezione elevata con failover automatico ed è consigliato per le sessioni di mirroring del database. La modalità a prestazioni elevate e a protezione elevata senza failover automatico sono tuttavia molto meno influenzate dall'affidabilità della rete.  
  
 Per la distribuzione negli ambienti di produzione è pertanto consigliabile attenersi alle linee guida seguenti:  
  
1.  Iniziare l'esecuzione nella modalità asincrona a prestazioni elevate. Questa modalità è la meno dipendente dall'ambiente di rete e offre la migliore configurazione per l'analisi del mirroring. È consigliabile eseguire il sistema in modo asincrono finché non si è certi che la larghezza di banda supporti il mirroring e non si conoscono in modo approfondito l'impostazione del mirroring e le prestazioni della modalità asincrona nell'ambiente utilizzato. Per altre informazioni, vedere [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
    > [!IMPORTANT]  
    >  Nel corso dell'intero test, è consigliabile monitorare le sessioni per individuare gli errori di rete che causano l'esito negativo del mirroring del database. Per ulteriori informazioni sulle possibili cause di errore, vedere [Possible Failures During Database Mirroring](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md). Per informazioni su come monitorare il mirroring del database, vedere [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md).  
  
2.  Se l'operazione asincrona soddisfa le specifiche esigenze aziendali, è possibile utilizzarla per aumentare il livello di protezione dei dati. Durante il test del funzionamento del mirroring sincrono nell'ambiente utilizzato, è consigliabile iniziare dalla modalità a protezione elevata senza failover automatico. Lo scopo principale di questo test è valutare l'influenza dell'operazione sincrona sulle prestazioni del database. Per altre informazioni, vedere [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
3.  Prima di abilitare il failover automatico, verificare che la modalità a protezione elevata senza failover automatico soddisfi le esigenze dell'azienda e che gli errori di rete non danneggino il sistema. Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).  
  
  
## <a name="see-also"></a>Vedere anche  
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Risolvere i problemi relativi alla configurazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
