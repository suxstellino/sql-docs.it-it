---
title: Opzioni di fine del supporto
description: Informazioni sulle diverse opzioni disponibili per i prodotti SQL Server che hanno raggiunto la fine del supporto, ad esempio SQL Server 2005, SQL Server 2008 e SQL Server 2008 R2.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352899"
---
# <a name="sql-server-end-of-support-options"></a>Opzioni di fine del supporto di SQL Server 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo illustra le opzioni disponibili per i prodotti SQL Server che hanno raggiunto la fine del supporto.

## <a name="understanding-the-sql-server-lifecycle"></a>Informazioni sul ciclo di vita di SQL Server

Ogni versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha un supporto di almeno 10 anni che include cinque anni di supporto Mainstream e cinque anni di supporto esteso:
-  Il **supporto Mainstream** include aggiornamenti di funzionalità, prestazioni, scalabilità e sicurezza. 
-  Il **supporto esteso** include solo gli aggiornamenti della sicurezza. 

La **fine del supporto** (a volte chiamata anche scadenza) indica che un prodotto ha raggiunto la fine del ciclo di vita e che la manutenzione e il supporto non sono più disponibili per il prodotto. Per altre informazioni sul ciclo di vita Microsoft, vedere [Criteri relativi al ciclo di vita Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy).



## <a name="options"></a>Opzioni

Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha raggiunto la fine del supporto, è possibile scegliere di:
- Eseguire l'aggiornamento a una versione corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Acquistare una [sottoscrizione per gli aggiornamenti della sicurezza estesa](https://www.microsoft.com/cloud-platform/extended-security-updates). 
- Eseguire la migrazione del carico di lavoro a una macchina virtuale di Azure senza modifiche per ottenere gli [aggiornamenti della sicurezza estesa gratuiti](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support).
- Eseguire la migrazione del carico di lavoro a un [servizio di database SQL di Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas). 

Per altre informazioni, istruzioni e strumenti per pianificare e automatizzare l'aggiornamento o la migrazione, vedere [Fine del supporto per SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005) e [Fine del supporto per SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008).  

![Opzioni di fine del supporto](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

Questo articolo descrive i vantaggi e le considerazioni per ogni approccio, oltre a risorse aggiuntive che possono risultare utili per il processo decisionale.

## <a name="upgrade-sql-server"></a>Eseguire l'aggiornamento di SQL Server

Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] raggiunge la fine del supporto, è possibile scegliere di eseguire l'aggiornamento a una versione più recente supportata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ciò garantisce la coerenza ambientale, consente di usare il set di funzionalità più recente e di adottare il ciclo di vita del supporto della nuova versione.

### <a name="benefits"></a>Vantaggi
- **Tecnologia più recente**: le nuove versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introducono innovazioni che includono prestazioni, scalabilità, funzionalità a disponibilità elevata e sicurezza migliorata. 
- **Controllo**: si ha un maggiore controllo sulle funzionalità e sulla scalabilità perché vengono gestiti sia l'hardware che il software.
- **Ambiente familiare**: se si esegue l'aggiornamento da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questo è l'ambiente più simile.
- **Ampia applicabilità**: si applica alle applicazioni di database di qualsiasi tipo, inclusi sistemi OLTP e data warehousing.
- **Rischio basso per le applicazioni di database**: mantenendo la compatibilità del database allo stesso livello del sistema legacy, le applicazioni di database esistenti sono protette da modifiche funzionali e di prestazioni che possono avere effetti negativi. Un'applicazione deve essere completamente ricertificata solo quando è necessario usare le funzionalità gestite da un'impostazione di compatibilità del database più recente. Per altre informazioni, vedere [Certificazione della compatibilità](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considerazioni

- **Costo**: questo approccio richiede l'investimento iniziale più grande e la gestione più continuativa. È necessario acquistare, eseguire la manutenzione e gestire il proprio hardware e il software.
- **Tempo di inattività**: potrebbero verificarsi tempi di inattività a seconda della strategia di aggiornamento. Esiste anche un rischio intrinseco di problemi durante un processo di aggiornamento sul posto.
- **Complessità**: in Windows Server 2008 o [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] sarà necessario aggiornare anche il sistema operativo poiché le versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero non essere supportate nelle versioni di Windows. Poiché esiste un ulteriore rischio durante il processo di aggiornamento del sistema operativo, una migrazione affiancata può risultare l'approccio più prudente, sebbene più costoso. Gli aggiornamenti sul posto del sistema operativo non sono supportati nelle istanze del cluster di failover per Windows Server 2008 o [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)]. 

  > [!NOTE]
  > Gli aggiornamenti in sequenza del sistema operativo del cluster sono disponibili a partire da Windows Server 2016.

### <a name="resources"></a>Risorse

[Supporto di installazione](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[Aggiornare SQL Server usando l'Installazione guidata](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Novità della versione:
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

Requisiti hardware:
- [SQL Server 2017 e versioni precedenti](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

Aggiornamenti di versione ed edizione supportati:
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

Strumenti:
-  [Database Experimentation Assistant](../../dea/database-experimentation-assistant-overview.md) consente di valutare la versione di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per un carico di lavoro specifico. 
-  [Data Migration Assistant](../../dma/dma-overview.md) consente di rilevare problemi di compatibilità che possono influire sulle funzionalità del database nella nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
-  [Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) consente di ottimizzare i carichi di lavoro che potrebbero comportare effetti negativi durante l'aggiornamento della compatibilità del database.

L'immagine seguente offre un esempio di innovazione nelle diverse versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel corso degli anni: 

![25 anni di innovazione di SQL Server](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>Estendere il supporto 

Se non si è pronti a eseguire l'aggiornamento e a passare al cloud, è possibile acquistare una sottoscrizione degli aggiornamenti della sicurezza estesa per ricevere gli aggiornamenti della sicurezza **critici** per un massimo di tre anni successivi alla data di fine del supporto.  

### <a name="benefits"></a>Vantaggi 

- **Supporto dell'applicazione**: questa è l'opzione migliore se l'applicazione richiede una nuova certificazione per una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si tratta di un problema comune per le applicazioni che non usano la [certificazione di compatibilità](../../database-engine/install-windows/compatibility-certification.md). 
- **Infrastruttura coerente**: non è necessario modificare l'infrastruttura in alcun modo. 
- **Supporto tecnico**: se si ha aderito al programma Software Assurance o a un altro piano di supporto, è possibile continuare a ricevere supporto tecnico da [!INCLUDE[msCoName](../../includes/msconame-md.md)] per il prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per cui è terminato il supporto. Questo è l'unico modo per ottenere supporto per [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. 
- **Time**: questa opzione è disponibile per tre anni e offre tempo aggiuntivo per certificare le applicazioni. 

### <a name="considerations"></a>Considerazioni 

- **Disponibilità limitata**: questa opzione è disponibile solo per i clienti che hanno aderito al programma Software Assurance o con licenze di sottoscrizione. 
- **Costo**: questa opzione può rivelarsi costosa poiché gli aggiornamenti della sicurezza estesa rappresentano circa il 75% del costo annuale della licenza locale.
- **Durata limitata**: poiché questa opzione è disponibile solo per tre anni, è comunque necessario eseguire l'aggiornamento o la migrazione alla fine del periodo di tre anni per garantire sicurezza e conformità.
- **Nessuna correzione dei bug**: se si verifica un bug non di sicurezza nel prodotto, [!INCLUDE[msCoName](../../includes/msconame-md.md)] non rilascerà alcuna correzione. 
- **Supporto limitato**: gli aggiornamenti della sicurezza estesa non includono nuove funzionalità, miglioramenti funzionali o correzioni richieste dai clienti. Le correzioni di sicurezza sono limitate a quelle classificate come critiche da [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/).

### <a name="resources"></a>Risorse

[Panoramica sugli aggiornamenti della sicurezza estesa](sql-server-extended-security-updates.md)       
[Domande frequenti dettagliate sugli aggiornamenti della sicurezza estesa](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Estendere il supporto gratuitamente tramite la migrazione senza modifiche a una macchina virtuale di Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Macchina virtuale di Azure

Un'altra opzione consiste nell'eseguire la migrazione del carico di lavoro a una [macchina virtuale di Azure che esegue SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview). È possibile eseguire la migrazione del sistema senza modifiche e mantenere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la fine del supporto oppure eseguire l'aggiornamento a una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si tratta dell'opzione migliore per le migrazioni e le applicazioni che richiedono l'accesso a livello di sistema operativo. Le macchine virtuali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono pronte per il trasferimento in modalità lift-and-shift per le applicazioni esistenti che richiedono una migrazione rapida al cloud con modifiche minime o senza modifiche. 

### <a name="benefits"></a>Vantaggi

- **Aggiornamenti della sicurezza estesa gratuiti**: se si sceglie di mantenere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza modifiche usando [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], è possibile ottenere aggiornamenti della sicurezza estesa gratuiti per i tre anni successivi alla data di fine del supporto, anche senza aderire al programma Software Assurance. 
- **Risparmio**: si risparmiano i costi dell'hardware e del software del server, pagando solo per l'utilizzo orario. 
- **Lift-and-shift**: è possibile trasferire in modalità lift-and-shift [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'infrastruttura dell'applicazione nel cloud con modifiche minime o senza modifiche. 
- **Ambiente ospitato**: si otterranno i vantaggi di un ambiente ospitato, ad esempio l'offload dell'hardware e la manutenzione del software. 
- **Automazione**: in [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] e versioni successive si otterranno i vantaggi dell'applicazione automatica delle patch e dei backup automatici. 
- **Controllo del sistema operativo**: si ha il controllo dell'ambiente del sistema operativo, ma con il set di funzionalità già noto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Distribuzione rapida**: è possibile eseguire rapidamente la distribuzione da una libreria di immagini della macchina virtuale. 
- **Mobilità delle licenze**: è possibile trasferire la propria licenza, riducendo i costi operativi. 
- **Disponibilità elevata**: non solo è possibile ottenere i vantaggi della disponibilità delle macchine virtuali predefinita da parte dell'infrastruttura di Azure che offre una disponibilità del 99,99%, ma è anche possibile usare le opzioni a disponibilità elevata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio le istanze del cluster di failover e i gruppi di disponibilità Always On. 
- **Rischio basso per le applicazioni di database**: mantenendo la compatibilità del database allo stesso livello dei database legacy, le applicazioni di database esistenti sono protette da modifiche funzionali e di prestazioni che possono avere effetti negativi. Un'applicazione deve essere completamente ricertificata solo quando è necessario usare le funzionalità gestite da un'impostazione di compatibilità del database più recente. Per altre informazioni, vedere [Certificazione della compatibilità](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considerazioni

- **Gestibilità**: è necessario gestire sia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che il software del sistema operativo. 
- **Rete**: è necessario configurare la macchina virtuale per l'integrazione con l'infrastruttura di rete e di Active Directory, con un ulteriore livello di complessità. 
- **FCI di archiviazione condivisa**: le macchine virtuali di Azure supportano solo le istanze del cluster di failover che usano Spazi di archiviazione diretta o condivisioni file Premium e non supportano un'istanza del cluster di failover che usa l'archiviazione condivisa. Di conseguenza, le macchine virtuali di Azure supportano solo le istanze del cluster di failover quando si usa Windows Server 2012 o versioni successive.
- **Tempo di inattività di scalabilità**: si verifica un tempo di inattività durante la modifica delle risorse della CPU e di archiviazione. 
- **Limitazione delle dimensioni**: sebbene l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possa supportare il numero di database necessario, il totale cumulativo di tutti i database per una singola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è 256 TB, anziché 524 PB come per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. 

### <a name="resources"></a>Risorse

[Panoramica delle VM di SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)       
[Scelta di un'opzione SQL di Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Eseguire la migrazione di SQL Server a una macchina virtuale di Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)       
[Aggiornamenti della sicurezza estesa gratuiti per la migrazione ad Azure senza modifiche](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Panoramica sugli aggiornamenti della sicurezza estesa](sql-server-extended-security-updates.md)       
[Domande frequenti dettagliate sugli aggiornamenti della sicurezza estesa](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Applicazione automatica delle patch nelle macchine virtuali SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)       
[Backup automatico nelle macchine virtuali SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)       
[Disponibilità elevata delle macchine virtuali SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)       
[Domande frequenti sulle macchine virtuali SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq)       

## <a name="azure-sql-database-single-database"></a>Database singolo di database SQL di Azure

Se si vuole eseguire l'offload della manutenzione, ridurre i costi ed eliminare la necessità di eseguire l'aggiornamento in futuro, è possibile spostare il carico di lavoro nel [database singolo del database SQL di Azure](/azure/sql-database/sql-database-single-database). Questa opzione è ideale per le applicazioni cloud moderne in cui si vogliono usare le funzionalità stabili di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] più recenti e che hanno vincoli di tempo per lo sviluppo e il marketing. 

### <a name="benefits"></a>Vantaggi

- **Costo**: il database singolo può essere economicamente conveniente poiché viene eseguito l'offload di hardware, software e manutenzione ed è possibile pagare per l'utilizzo al secondo o all'ora. 
- **Flessibilità**: il database singolo è particolarmente adatto per le applicazioni progettate per il cloud quando la produttività degli sviluppatori e tempi di commercializzazione rapidi per le soluzioni sono cruciali o per le applicazioni che richiedono l'accesso esterno.  
- **Funzionalità comuni**: sono disponibili le funzionalità di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] più comunemente usate, ma non le tante disponibili per Istanza gestita di SQL.  
- **Distribuzione rapida**: è possibile distribuire rapidamente un database singolo. 
- **Scalabilità**: è possibile aumentare o ridurre le risorse in modo rapido e semplice in base alle esigenze aziendali, offrendo ulteriori vantaggi in termini di costi. 
- **Disponibilità**: il costo del servizio include archiviazione e disponibilità elevata, con una disponibilità garantita del 99,995%.  
- **Automazione**: l'applicazione di patch e i backup vengono eseguiti automaticamente, consentendo di ridurre i tempi di manutenzione.  
- **Intelligent Insights**: è possibile ottenere informazioni dettagliate sulle prestazioni del database con Intelligence e analisi predefinite.  
- **Senza versione**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è senza versione, ovvero viene sempre usata la versione più recente e non è necessario preoccuparsi di aggiornamenti o tempi di inattività. Inoltre, la versione usata è la più recente e aggiornata poiché le ultime funzionalità stabili vengono rilasciate prima nel cloud.
- **Rischio basso per le applicazioni di database**: mantenendo la compatibilità del database allo stesso livello del database locale, le applicazioni esistenti sono protette da modifiche funzionali e di prestazioni che possono avere effetti negativi. Un'applicazione deve essere completamente ricertificata solo quando è necessario usare le funzionalità gestite da un'impostazione di compatibilità del database più recente. Per altre informazioni, vedere [Certificazione della compatibilità](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considerazioni

- **Opzioni di migrazione limitate**:  è possibile eseguire solo la migrazione di un database singolo alla volta, anziché di un'intera istanza.   
- **Limitazione delle funzionalità**:  sebbene siano disponibili le funzionalità del database SQL di Azure usate più di frequente, il set di funzionalità per un database singolo non è completo come quello di Istanza gestita di SQL di Azure o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Differenze di Transact-SQL**:  esistono alcune differenze di [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) tra database singolo e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. 
- **Limitazioni delle dimensioni**:  un database singolo ha dimensioni massime del database pari a 100 TB, anziché pari a 524 PB come per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Tempi di manutenzione**: non vengono garantiti tempi di manutenzione esatti, sebbene siano quasi trasparenti. 

### <a name="resources"></a>Risorse

[Panoramica del database SQL di Azure](/azure/sql-database/sql-database-technical-overview)       
[Scelta di un'opzione SQL di Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Confronto tra le funzionalità del database SQL](/azure/sql-database/sql-database-features)       
[Eseguire la migrazione di SQL Server a un database singolo](/azure/sql-database/sql-database-single-database-migrate)       
[Processo di migrazione più ampio](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       
[Differenze di T-SQL del database singolo](/azure/sql-database/sql-database-transact-sql-information)       
Limiti delle risorse [vCore](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) e [DTU](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)       
[Intelligent Insights](/azure/sql-database/sql-database-intelligent-insights)       

Strumenti:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Servizio Migrazione del database di Azure](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>Istanza gestita di SQL

Se si vuole usufruire dei vantaggi offerti dall'offload della manutenzione e dei costi, ma il set di funzionalità di un database singolo di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] risulta troppo limitato, è possibile passare a [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance). Un'istanza gestita è molto simile a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale, senza doversi preoccupare di problemi hardware o dell'applicazione di patch. Un'istanza gestita è una raccolta di database di sistema e utente con un set di risorse condiviso pronto per il trasferimento in modalità lift-and-shift e che può essere usato per la maggior parte delle migrazioni nel cloud. Questa opzione è ideale per le applicazioni nuove o le applicazioni locali esistenti in cui si vogliono usare le funzionalità di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] stabili più recenti e di cui viene eseguita la migrazione al cloud con modifiche minime. 

### <a name="benefits"></a>Vantaggi

- **Costo**: è possibile ridurre i costi tramite l'offload della manutenzione del software e dell'hardware.  
- **Lift-and-shift**: è possibile trasferire in modalità lift-and-shift l'intera istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale in un'istanza gestita, inclusi tutti i database senza apportare modifiche al database o con modifiche minime. 
- **Funzionalità**: il set di funzionalità di un'istanza gestita corrisponde a quello di un'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e include ad esempio le query tra database, la pubblicazione e la distribuzione della replica transazionale, la pianificazione dei processi SQL e il supporto CLR. 
- **Scalabilità**: tutti i database all'interno di un'istanza gestita condividono le risorse ed è possibile aumentare o ridurre le risorse in qualsiasi momento senza tempi di inattività.   
- **Automazione**: l'applicazione di patch e i backup vengono eseguiti automaticamente, consentendo di ridurre i tempi di manutenzione.  
- **Disponibilità**: il costo del servizio include archiviazione e disponibilità elevata, con una disponibilità garantita del 99,99%.  
- **Intelligent Insights**: è possibile ottenere informazioni dettagliate sulle prestazioni dei database con Intelligence e analisi predefinite.  
- **Senza versione**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è senza versione, ovvero viene sempre usata la versione più recente e non è necessario preoccuparsi di aggiornamenti o tempi di inattività. Inoltre, la versione usata è la più recente e aggiornata poiché le ultime funzionalità stabili vengono rilasciate prima nel cloud.
- **Rischio basso per le applicazioni di database**: mantenendo la compatibilità del database allo stesso livello dei database locali, le applicazioni di database esistenti sono protette da modifiche funzionali e di prestazioni che possono avere effetti negativi. Un'applicazione deve essere completamente ricertificata solo quando è necessario usare le funzionalità gestite da un'impostazione di compatibilità del database più recente. Per altre informazioni, vedere [Certificazione della compatibilità](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considerazioni

- **Costo**: l'opzione dell'istanza gestita può risultare più costosa rispetto a quella del database singolo.  
- **Differenze di Transact-SQL**: esistono alcune differenze di [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) tra database singolo e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale.  
- **Distribuzione**:  la distribuzione di un'istanza gestita può richiedere più tempo rispetto a un database singolo.  
- **Limitazione delle funzionalità**: sebbene un'istanza gestita condivida la maggior parte delle funzionalità con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alcune funzionalità non sono supportate. 
- **Limitazione delle dimensioni**: le dimensioni di archiviazione combinate per tutti i database all'interno di un'istanza gestita sono limitate a 8 TB, anziché a 524 PB come per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale.  
- **Rete**: i requisiti di rete per un'istanza gestita aggiungono un ulteriore livello di complessità all'infrastruttura e richiedono un gateway VPN o Azure ExpressRoute.
- **Tempi di manutenzione**: non vengono garantiti tempi di manutenzione esatti, sebbene siano quasi trasparenti. 

### <a name="resources"></a>Risorse

[Panoramica di Istanza gestita di SQL](/azure/sql-database/sql-database-managed-instance)       
[Scelta di un'opzione SQL di Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Confronto tra le funzionalità del database SQL](/azure/sql-database/sql-database-features)       
[Eseguire la migrazione di SQL Server in Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance-migrate)       
[Processo di migrazione più ampio](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       

Strumenti:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Servizio Migrazione del database di Azure](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>Opzioni non SQL

Per determinati tipi di applicazioni, è anche possibile prendere in considerazione una soluzione non relazionale o NoSQL, ad esempio Azure Cosmos DB o l'archiviazione tabelle di Azure.

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Prendere in considerazione Azure Cosmos DB per applicazioni moderne, scalabili, mobili e Web che usano i dati JSON e richiedono una combinazione affidabile di funzionalità di query e di elaborazione dei dati transazionali. Per altre informazioni, vedere [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Per informazioni sull'importazione di dati, vedere [Importare dati in Cosmos DB](/azure/cosmos-db/import-data/).

Azure Cosmos DB offre i vantaggi seguenti:
- I documenti vengono indicizzati ed è possibile eseguire query usando la sintassi SQL con cui si ha già familiarità.
- Il database è senza schema.
- È possibile aggiungere proprietà ai documenti senza dover ricompilare gli indici.
- Il supporto per JSON e JavaScript viene fornito direttamente all'interno del motore di database.
- Il supporto nativo viene fornito per i dati geospaziali e l'integrazione con altri servizi di Azure, inclusi Ricerca di Azure, HDInsight e Data Factory.
- Si ottiene un'archiviazione a bassa latenza e ad alte prestazioni con livelli di velocità effettiva riservati.

### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

Prendere in considerazione l'archiviazione tabelle di Azure per archiviare petabyte di dati semistrutturati in una soluzione economicamente conveniente. Per altre informazioni, vedere [Archivio tabelle](https://azure.microsoft.com/services/storage/tables/).

L'archiviazione tabelle di Azure offre i vantaggi seguenti:
- È possibile sviluppare le app e lo schema della tabella senza disconnettere i dati.
- È possibile usare la scalabilità verticale senza partizionamento orizzontale del set di dati.
- Si ottiene un'archiviazione con ridondanza geografica che replica i dati in più regioni.

## <a name="lifecycle-dates"></a>Date del ciclo di vita

La tabella seguente offre un'approssimazione delle date del ciclo di vita per i prodotti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per dettagli e accuratezza maggiori, vedere la pagina [Criteri relativi al ciclo di vita Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy). 

| **Versione**     | **Anno di pubblicazione** | **Anno di fine del supporto Mainstream** | **Anno di fine del supporto esteso** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> Se è presente una discrepanza tra questa tabella e la pagina del ciclo di vita di [!INCLUDE[msCoName](../../includes/msconame-md.md)], il ciclo di vita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] sostituisce questa tabella creata come riferimento approssimativo.  

## <a name="next-steps"></a>Passaggi successivi  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[SQL Server 2005 end of support](https://www.microsoft.com/sql-server/sql-server-2005)   
[Fine del supporto di SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[Panoramica sugli aggiornamenti della sicurezza estesa](sql-server-extended-security-updates.md)   
[Aggiornamenti della sicurezza estesa gratuiti per la migrazione ad Azure senza modifiche](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)   
[Panoramica delle VM di SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)   
[Panoramica del database SQL di Azure](/azure/sql-database/sql-database-technical-overview)