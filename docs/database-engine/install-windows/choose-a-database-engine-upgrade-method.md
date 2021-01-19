---
title: Scegliere un metodo di aggiornamento del motore di database | Microsoft Docs
description: Questo articolo descrive i percorsi di aggiornamento per il motore di database in SQL Server, incluso l'aggiornamento sul posto, la migrazione a una nuova installazione e l'aggiornamento in sequenza.
ms.custom: ''
ms.date: 07/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5e57a427-2e88-4ef6-b142-4ccad97bcecc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4acdd7f1d8b6a39af4054f30381665d1504bcb71
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171833"
---
# <a name="choose-a-database-engine-upgrade-method"></a>Scegliere un metodo di aggiornamento del motore di database
[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Sono disponibili diversi approcci da considerare quando si intende eseguire l'aggiornamento di [!INCLUDE[ssDE](../../includes/ssde-md.md)] da una versione precedente di SQL Server per ridurre al minimo i tempi di inattività e il rischio. È possibile eseguire un aggiornamento sul posto, la migrazione a una nuova installazione o un aggiornamento in sequenza. Il diagramma seguente consentirà di scegliere tra questi approcci. Ogni approccio del diagramma viene anche illustrato in basso. Per assistenza nei punti decisionali all'interno del diagramma, consultare anche [pianificare e testare il Database Engine Upgrade Plan](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 ![Albero delle decisioni per il metodo di aggiornamento del motore di database](../../database-engine/install-windows/media/database-engine-upgrade-method-decision-tree.png "Albero delle decisioni per il metodo di aggiornamento del motore di database")  
  
 **Scaricare**  
  
-   Per scaricare [!INCLUDE[SSnoversion](../../includes/ssnoversion-md.md)], passare a  **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server)** .  
  
-   Se si ha un account di Azure,  fare clic **[qui](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=overview)** per creare rapidamente una macchina virtuale in cui è già installato [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Developer Edition.  
  
> [!NOTE]  
>  Nell'ambito del piano di aggiornamento, è anche possibile considerare l'aggiornamento del database SQL di Azure o la virtualizzazione dell'ambiente SQL Server. Questi articoli non rientrano nell'ambito di questo articolo, ma di seguito sono riportati alcuni collegamenti:
>   - [Panoramica di SQL Server in macchine virtuali di Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/#overview)
>   - [Database SQL di Azure](https://azure.microsoft.com/services/sql-database/) 
>   - [Selezione di un'opzione di SQL Server in Azure](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview).  
  
## <a name="upgrade-in-place"></a>Aggiornamento sul posto  
 Con questo approccio, il programma di installazione di SQL Server aggiorna l'installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistente sostituendo i bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistenti con i bit nuovi di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] e quindi aggiorna tutti i database utente e di sistema.  Il metodo di aggiornamento sul posto è più semplice, implica tempo di inattività, richiede più tempo per il fallback, se necessario, e non è supportato in tutti gli scenari. Per altre informazioni sugli scenari di aggiornamento sul posto supportati e non supportati, vedere [Aggiornamenti di versione ed edizione supportati](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md).  
  
 Questo approccio viene spesso usato negli scenari seguenti:  
  
-   Un ambiente di sviluppo senza una configurazione a disponibilità elevata.  
  
-   Un ambiente di produzione non mission-critical in grado di tollerare i tempi di inattività e in esecuzione su hardware e software recenti. La quantità di tempo di inattività dipende dalle dimensioni del database e dalla velocità del sottosistema di I/O. L'aggiornamento di SQL Server 2014 quando le tabelle ottimizzate per la memoria sono in uso comporta maggiore tempo. Per altre informazioni, vedere [pianificare e testare il Database Engine Upgrade Plan](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  

> [!NOTE]
> Per un esempio del modo in cui Microsoft ha usato questa strategia, vedere il post di blog seguente: [To upgrade or not to upgrade](https://azure.microsoft.com/blog/azure-sql-server-to-upgrade-or-not-to-upgrade-that-is-the-question/) (Aggiornare o non aggiornare). 


> [!WARNING]  
>  Quando si esegue il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestata e riavviata quando si eseguono i controlli di pre-aggiornamento.  
  
> [!CAUTION]  
>  Quando si aggiorna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'istanza precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene sovrascritta e non sarà più disponibile nel computer. Prima dell'aggiornamento, eseguire il backup dei database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e degli altri oggetti associati all'istanza precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Il diagramma seguente illustra una panoramica generale dei passaggi necessari per un aggiornamento sul posto di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 ![Aggiornamento sul posto non a disponibilità elevata del motore di database](../../database-engine/install-windows/media/database-engine-upgrade-non-ha-in-place-upgrade.png "Aggiornamento sul posto non a disponibilità elevata del motore di database")  
  
 Per i passaggi dettagliati, vedere [Eseguire l'aggiornamento di SQL Server usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
## <a name="migrate-to-a-new-installation"></a>Migrazione verso una nuova installazione  
 Con questo approccio è possibile mantenere l'ambiente corrente mentre si crea un nuovo ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , spesso su hardware nuovo e con una nuova versione del sistema operativo. Dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel nuovo ambiente, è necessario eseguire una procedura per preparare il nuovo ambiente alla migrazione dei database utente esistenti dall'ambiente esistente al nuovo ambiente, riducendo al minimo i tempi di inattività. Questa procedura comprende la migrazione degli elementi seguenti:  
  
-   **Oggetti di sistema:** Alcune applicazioni dipendono da informazioni, entità e/o oggetti esterni all'ambito di un database in modalità a utente singolo. Un'applicazione include in genere dipendenze nei database master e msdb, nonché nel database utente. Qualsiasi elemento archiviato all'esterno di un database utente necessario per il corretto funzionamento di tale database deve essere reso disponibile nell'istanza del server di destinazione. Ad esempio, gli account di accesso per un'applicazione vengono archiviati come metadati nel database master e devono essere ricreati nel server di destinazione. Se il piano di manutenzione di un'applicazione o un database dipende da processi di SQL Server Agent i cui metadati sono archiviati nel database msdb, è necessario ricreare tali processi nell'istanza del server di destinazione. Analogamente, i metadati per un trigger a livello di server vengono archiviati nel database master.  
 
   Quando il database per un'applicazione viene spostato in un'altra istanza del server, è necessario creare di nuovo tutti i metadati delle entità e degli oggetti dipendenti nei database master e msdb dell'istanza del server di destinazione. Ad esempio, se un'applicazione del database utilizza trigger a livello di server, non è sufficiente collegare o ripristinare il database nel nuovo sistema. Il database non funzionerà come previsto a meno che non si ricreino manualmente i metadati per tali trigger nel database master. Per informazioni dettagliate, vedere [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
-   **Pacchetti di Integration Services archiviati in MSDB:** se i pacchetti vengono archiviati nel database MSDB, è necessario inserire i pacchetti nello script usando [dtutil Utility](../../integration-services/dtutil-utility.md) o ridistribuendoli sul nuovo server. Prima di usare i pacchetti nel nuovo server, è necessario aggiornarli a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
-   **Chiavi di crittografia Reporting Services:** Un aspetto importante della configurazione del server di report riguarda la creazione di una copia di backup della chiave simmetrica utilizzata per crittografare le informazioni riservate. La copia di backup della chiave è obbligatoria per molte operazioni di routine e consente di riutilizzare un database del server di report esistente in una nuova installazione. Per altre informazioni, vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md) ed [Eseguire l'aggiornamento e la migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
  
 Quando il nuovo   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ambiente dispone degli stessi oggetti di sistema dell'ambiente esistente, procedere quindi alla migrazione dei database utente dal sistema esistente verso l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo da ridurre al minimo i tempi di inattività del sistema esistente. Eseguire la migrazione del database tramite backup e ripristino oppure tramite un nuovo puntamento dei numeri di unità logica (LUN) se si dispone di un ambiente SAN. Nel diagramma seguente vengono descritte le procedure per entrambi i metodi.  
  
> [!CAUTION]  
>  La quantità di tempo di inattività dipende dalle dimensioni del database e dalla velocità del sottosistema di I/O. L'aggiornamento di SQL Server 2014 quando le tabelle ottimizzate per la memoria sono in uso comporta maggiore tempo. Per altre informazioni, vedere [pianificare e testare il Database Engine Upgrade Plan](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 Dopo la migrazione dei database utente, puntare i nuovi utenti verso la nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando uno dei diversi metodi, ad esempio rinominando il server, usando una voce DNS, modificando le stringhe di connessione.  Il nuovo approccio di installazione riduce rischi e tempi di inattività rispetto all'aggiornamento sul posto e agevola gli aggiornamenti hardware e del sistema operativo in combinazione con l'aggiornamento a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Se si ha già una soluzione a disponibilità elevata o altri numerosi ambienti dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Aggiornamenti in sequenza](#rolling-upgrade). Se non si ha una soluzione a disponibilità elevata, è possibile configurare temporaneamente il [mirroring del database](../database-mirroring/setting-up-database-mirroring-sql-server.md) per ridurre maggiormente i tempi di inattività e semplificare l'aggiornamento oppure configurare un [gruppo di disponibilità Always On](../availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md) come soluzione a disponibilità elevata permanente.  
  
 Ad esempio, è possibile usare questo approccio per aggiornare gli elementi seguenti:  
  
-   Un'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su un sistema operativo non supportato.    
-   Un'installazione x86 di SQL Server dal momento che [!INCLUDE[ss2016](../../includes/sssql16-md.md)] e versioni successive non supportano le installazioni x86.   
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su un nuovo hardware e/o su una nuova versione del sistema operativo.    
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in combinazione con il consolidamento dei server.   
-   SQL Server 2005 come [!INCLUDE[ss2016](../../includes/sssql16-md.md)] e versioni successive non supportano l'aggiornamento sul posto di SQL Server 2005. Per altre informazioni, vedere [Aggiornamento da SQL Server 2005](../../sql-server/end-of-support/sql-server-end-of-life-overview.md).

  
I passaggi necessari per un nuovo aggiornamento di installazione variano leggermente a seconda che si usi l'archiviazione associata o l'archiviazione SAN.  
  
-   **Ambiente di archiviazione associata:** se si dispone di un ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che usa l'archiviazione associata, il diagramma seguente e i collegamenti all'interno del diagramma guidano nella procedura necessaria per un nuovo aggiornamento dell'installazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
     ![Nuovo metodo di aggiornamento dell'installazione mediante backup e ripristino per l'archiviazione associata](../../database-engine/install-windows/media/new-installation-upgrade-method-using-backup-and-restore-for-attached-storage.png "Nuovo metodo di aggiornamento dell'installazione mediante backup e ripristino per l'archiviazione associata")  
  
-   **Ambiente di archiviazione SAN:**  se si dispone di un ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che usa l'archiviazione SAN, il diagramma seguente e i collegamenti all'interno del diagramma guidano nella procedura necessaria per un nuovo aggiornamento dell'installazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
     ![Nuovo metodo di aggiornamento dell'installazione mediante collegamento e scollegamento per l'archiviazione SAN](../../database-engine/install-windows/media/new-installation-upgrade-method-using-detach-and-attach-for-san-storage.png "Nuovo metodo di aggiornamento dell'installazione mediante collegamento e scollegamento per l'archiviazione SAN")  
  
## <a name="rolling-upgrade"></a>Aggiornamenti in sequenza  
 È necessario un aggiornamento in sequenza in ambienti di soluzioni SQL Server che includono più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da aggiornare in un determinato ordine al fine di ottimizzare i tempi di attività, ridurre i rischi e mantenere la funzionalità. Un aggiornamento in sequenza è essenzialmente l'aggiornamento di più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un ordine specifico, eseguendo un aggiornamento sul posto in ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistente oppure eseguendo un nuovo aggiornamento dell'installazione per facilitare l'aggiornamento di hardware e/o sistema operativo come parte del progetto di aggiornamento. Esistono diversi scenari in cui è necessario usare il metodo di aggiornamento in sequenza. Questi scenari sono documentati negli articoli seguenti:  
  
-   Gruppi di disponibilità Always On: per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Aggiornamento delle istanze di replica dei gruppi di disponibilità Always On](../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).    
-   Istanze con clustering di failover: per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Eseguire l'aggiornamento di un'istanza del cluster di failover di SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)    
-   Istanze con mirroring: per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Aggiornamento di istanze con mirroring](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).    
-   Istanze con log shipping: per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Aggiornamento del log shipping a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md).    
-   Ambiente di replica: Per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Aggiornare database replicati](../../database-engine/install-windows/upgrade-replicated-databases.md).  
-   Ambiente SQL Server Reporting Services con scalabilità orizzontale: per i passaggi dettagliati dell'esecuzione di un aggiornamento in sequenza in questo ambiente, vedere [Eseguire l'aggiornamento e la migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
## <a name="next-steps"></a>Passaggi successivi
 [Pianificare e testare il piano di aggiornamento del motore di Database](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)   
 [Completare l'aggiornamento al motore di database](../../database-engine/install-windows/complete-the-database-engine-upgrade.md)