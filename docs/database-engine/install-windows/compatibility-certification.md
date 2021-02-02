---
title: Certificazione di compatibilità | Microsoft Docs
description: La certificazione di compatibilità elimina i rischi di compatibilità delle applicazioni, consentendo di aggiornare un database di SQL Server in locale e nel cloud.
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
- Databases [SQL Server], upgrading
- Database Engine [SQL Server], upgrading
- compatibility [SQL Server], certification
- compatibility level [SQL Server], upgrades
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433856
author: pmasl
ms.author: pelopes
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0231dbe4d6be585892dc3d97b911e4206f182ef4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236686"
---
# <a name="compatibility-certification"></a>Certificazione di compatibilità

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La certificazione di compatibilità consente alle aziende di aggiornare e modernizzare un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in locale, nel cloud e sul perimetro, eliminando i rischi di compatibilità delle applicazioni. 

Lo stesso [!INCLUDE[ssde_md](../../includes/ssde_md.md)] è alla base sia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che di [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] (incluso Istanza gestita). Questo [!INCLUDE[ssde_md](../../includes/ssde_md.md)] condiviso significa che un database utente può essere spostato senza problemi tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale e [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], mentre il codice dell'applicazione eseguito nel database come [!INCLUDE[tsql](../../includes/tsql-md.md)] continua a funzionare come nel sistema di origine.

Per ogni nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il livello di compatibilità predefinito è impostato in base alla versione del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Tuttavia, il livello di compatibilità delle versioni precedenti viene mantenuto per garantire la compatibilità continua delle applicazioni esistenti. Questa matrice di compatibilità è visibile [qui](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats).
Pertanto, un'applicazione certificata per interagire con una determinata versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **viene in effetti certificata per funzionare con il livello di compatibilità predefinito di tale versione**.

Ad esempio, il livello di compatibilità del database 130 è l'impostazione predefinita in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Dato che i livelli di compatibilità impongono comportamenti specifici funzionali e di ottimizzazione delle query per [!INCLUDE[tsql](../../includes/tsql-md.md)], **un database certificato per funzionare in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] viene implicitamente certificato per il livello di compatibilità del database 130**. Questo database può funzionare così com'è in una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ad esempio [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]) e [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], a condizione che venga mantenuto il livello di compatibilità del database 130. 

Si tratta di un principio fondamentale per il modello di operazione di integrazione continua [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] viene costantemente migliorato e aggiornato in Azure, ma poiché i database esistenti mantengono il livello di compatibilità corrente, continuano a funzionare come previsto anche dopo gli aggiornamenti del [!INCLUDE[ssde_md](../../includes/ssde_md.md)] sottostante. 

Questo è anche il modo in cui SharePoint Server 2016 e SharePoint Server 2019 vengono certificati per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], consentendo di distribuire qualsiasi [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] che possa usare i livelli di compatibilità del database supportati per queste versioni di SharePoint Server. Per altre informazioni, vedere [Requisiti hardware e software per SharePoint Server 2016](/sharepoint/install/hardware-and-software-requirements#minimum-requirements-for-a-database-server-in-a-farm) e [Requisiti hardware e software per SharePoint Server 2019](/sharepoint/install/hardware-and-software-requirements-2019#minimum-requirements-for-a-database-server-in-a-farm).

## <a name="managing-upgrade-risk-with-compatibility-certification"></a>Gestione dei rischi di aggiornamento con la certificazione di compatibilità
L'uso della certificazione di compatibilità è un approccio prezioso per la modernizzazione del database. Grazie alla certificazione basata sul livello di compatibilità, gli sviluppatori definiscono i requisiti tecnici per il supporto di un'applicazione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], ma separano il ciclo di vita dell'applicazione dal ciclo di vita della piattaforma di database. Questo consente alle aziende di mantenere aggiornato il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in base ai requisiti specifici per i criteri del ciclo di vita, nonché di sfruttare i nuovi miglioramenti per scalabilità e prestazioni che non sono dipendenti dal codice e le applicazioni che si connettono possono **mantenere lo stato funzionale** tra un aggiornamento e l'altro.

La possibilità di influire negativamente sulle funzionalità e sulle prestazioni rappresenta il principale fattore di rischio per qualsiasi aggiornamento. La certificazione di compatibilità offre una certa tranquillità per la gestione di questi rischi di aggiornamento:

-  Per quanto riguarda il comportamento di [!INCLUDE[tsql](../../includes/tsql-md.md)], qualsiasi modifica significa che la correttezza di un'applicazione deve essere ricertificata. L'impostazione del [livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) garantisce tuttavia la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo per il database specificato e non per l'intero server. Mantenere il livello di compatibilità del database così com'è garantisce che le query dell'applicazione esistenti continuino a esibire lo stesso comportamento prima e dopo un aggiornamento del [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Per altre informazioni sul comportamento e sui livelli di compatibilità di [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Uso dei livelli di compatibilità per la compatibilità con le versioni precedenti](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

-  Per quanto riguarda le prestazioni, poiché vengono introdotti miglioramenti per Query Optimizer con ogni versione, è possibile che si verifichino differenze del piano di query tra versioni diverse del [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Le differenze del piano di query nell'ambito di un aggiornamento in genere rappresentano un rischio se è possibile che alcune modifiche possano essere dannose per una query o un carico di lavoro specifico. A sua volta, questo rischio è una motivazione per la ricertificazione, che può ritardare gli aggiornamenti e creare complicazioni per ciclo di vita e supporto. 
   La mitigazione dei rischi di aggiornamento è il motivo per cui i miglioramenti di Query Optimizer vengono vincolati al livello di compatibilità predefinito di una nuova versione (in altre parole, il più alto livello di compatibilità per qualsiasi nuova versione). La certificazione di compatibilità include la **protezione della forma del piano di query**: se si mantiene invariato un livello di compatibilità del database subito dopo un aggiornamento di [!INCLUDE[ssde_md](../../includes/ssde_md.md)], nella nuova versione si userà lo stesso modello di ottimizzazione delle query adottato prima dell'aggiornamento e la forma del piano di query rimane invariata. 
   Per altre informazioni, vedere la sezione [Che cos'è la forma del piano di query](#queryplan_shape) di questo articolo.
   
Per altre informazioni sui livelli di compatibilità, vedere [Uso dei livelli di compatibilità per la compatibilità con le versioni precedenti](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).
   
> [!IMPORTANT]
> Per un'applicazione esistente già certificata per un determinato livello di compatibilità, aggiornare il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e **mantenere** il livello di compatibilità del database precedente. Non è necessario ricertificare un'applicazione in questo scenario. Per altre informazioni, vedere [Livelli di compatibilità e aggiornamenti del motore di database](#compatibility-levels-and-database-engine-upgrades) di seguito in questo articolo.
>
> Per i nuovi progetti di sviluppo o quando un'applicazione esistente richiede l'uso di nuove funzionalità come l'[elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md) oltre a nuovi elementi [!INCLUDE[tsql](../../includes/tsql-md.md)], pianificare l'aggiornamento del livello di compatibilità del database a quello più recente disponibile in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e certificare di nuovo l'applicazione per l'uso di questo livello di compatibilità. Per altre informazioni sull'aggiornamento del livello di compatibilità del database, vedere [Procedure consigliate per aggiornare il livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level).
   
### <a name="why-query-plan-shape"></a><a name="queryplan_shape"></a> Che cos'è la forma del piano di query?      
La forma del piano di query si riferisce alla rappresentazione visiva dei diversi operatori che costituiscono un piano di query. Sono inclusi operatori quali seeks, scans, joins e sorts, nonché le connessioni fra tali operatori, che indicano il flusso di dati e l'ordine delle operazioni da eseguire per produrre il set di risultati desiderato. La forma del piano di query è determinata da Query Optimizer.

Per mantenere prevedibili le prestazioni delle query durante un aggiornamento, è fondamentale garantire che venga usata la stessa forma del piano di query. Questo obiettivo può essere raggiunto evitando di modificare il livello di compatibilità del database subito dopo un aggiornamento, anche se nel [!INCLUDE[ssde_md](../../includes/ssde_md.md)] sottostante sono presenti versioni diverse. Se nell'ecosistema di esecuzione delle query non sono state apportate altre modifiche, ad esempio modifiche significative nelle risorse disponibili o nella distribuzione dei dati sottostanti, le prestazioni delle query rimangono invariate. 

Mantenere invariata la forma di un piano di query, tuttavia, non è l'unico fattore che può incidere sulle prestazioni dopo un aggiornamento. Se si sposta il database in un [!INCLUDE[ssde_md](../../includes/ssde_md.md)] più recente e si apportano modifiche ambientali, è possibile che vengano introdotti fattori con un impatto immediato sulle prestazioni di una query, anche se la forma del piano di query non cambia tra le varie versioni. Le modifiche ambientali possono essere costituite, ad esempio, dalla disponibilità in [!INCLUDE[ssde_md](../../includes/ssde_md.md)] di una quantità maggiore o minore di risorse di memoria e CPU, da modifiche alle opzioni di configurazione del server o del database o da modifiche alla distribuzione dei dati che influiscono sulle modalità di creazione di un piano di query. Ecco perché è importante capire che il mantenimento del livello di compatibilità del database protegge effettivamente da modifiche alla **forma** del piano di query, ma non offre alcuna protezione da altri aspetti ambientali che influiscono sulle prestazioni delle query, tra cui modifiche avviate dall'utente.

Per altre informazioni, vedere [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements).
   
## <a name="compatibility-certification-benefits"></a>Vantaggi della certificazione di compatibilità
Esistono diversi vantaggi immediati per la certificazione del database come approccio basato sulla compatibilità anziché come approccio con versione denominata:

-  **Separare la certificazione delle applicazioni dalla piattaforma**. Considerata la condivisione del [!INCLUDE[ssde_md](../../includes/ssde_md.md)], per le applicazioni che richiedono solo l'esecuzione di query [!INCLUDE[tsql](../../includes/tsql-md.md)] non è necessario gestire processi di certificazione distinti per Azure e in locale.
-  **Riduzione dei rischi di aggiornamento** perché durante la modernizzazione della piattaforma di database, i cicli di aggiornamento del livello dell'applicazione e del livello piattaforma di database possono essere separati per minori disservizi e una migliore gestione delle modifiche.
-  **Aggiornamento senza modifiche del codice**. L'aggiornamento a una nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] può essere eseguito senza modifiche al codice mantenendo lo stesso livello di compatibilità del sistema di origine e non è necessario ripetere la certificazione fino a quando l'applicazione deve sfruttare miglioramenti disponibili solo con livelli di compatibilità del database più alti.
- **Migliorare la gestibilità e la scalabilità** senza che sia necessario apportare modifiche all'applicazione, usando miglioramenti non vincolati al livello di compatibilità del database. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questi miglioramenti includono ad esempio: 
  - Miglioramenti del monitoraggio e della risoluzione dei problemi, con le nuove [Viste a gestione dinamica (DMV) di sistema](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md), gli [Eventi estesi](../../relational-databases/extended-events/extended-events.md) e l'[Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md). 
  - Maggiore scalabilità, ad esempio con [Architettura soft-NUMA automatica](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa), [Ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-concepts.md) o [Metadati tempdb ottimizzati per la memoria](../../relational-databases/in-memory-database.md#memory-optimized-tempdb-metadata).

I nuovi database sono ancora impostati sul livello di compatibilità predefinito della versione del [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Tuttavia, quando un database viene spostato da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], il database mantiene il livello di compatibilità esistente. 

> [!IMPORTANT]
> Prima di trasferire un database a una nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], verificare che il livello di compatibilità del database sia ancora supportato. La matrice di supporto del livello di compatibilità del database può essere visualizzata [qui](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments). 
>
> Se in fase di aggiornamento di un database viene usato un livello di compatibilità inferiore a quello consentito (ad esempio, il livello 90 predefinito in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]), il database viene automaticamente impostato al livello più basso di compatibilità consentito (100).
>
> Per determinare il livello di compatibilità corrente, eseguire una query sulla colonna **compatibility_level** di [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

## <a name="compatibility-levels-and-database-engine-upgrades"></a>Livelli di compatibilità e aggiornamenti del motore di database
Per aggiornare il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] alla versione più recente, mantenendo il livello di compatibilità del database che esisteva prima dell'aggiornamento e il relativo stato di supporto, è consigliabile eseguire la **convalida della superficie di attacco funzionale statica** del codice dell'applicazione **nel database** (oggetti di programmabilità come stored procedure, funzioni, trigger e altri) e **nell'applicazione** (mediante una traccia del carico di lavoro che acquisisce il codice dinamico inviato dall'applicazione).

Questa operazione può essere eseguita facilmente usando lo strumento [Microsoft Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA). L'assenza di errori nell'output dello strumento DMA, relativi a funzionalità mancanti o non compatibili, protegge l'applicazione da regressioni funzionali nella nuova versione di destinazione. Se sono necessarie modifiche per garantire che il database funzioni nella nuova versione, DMA consentirà di individuare le modifiche necessarie e le soluzioni alternative disponibili. Per altre informazioni, vedere [Panoramica di Data Migration Assistant](../../dma/dma-overview.md).   

> [!TIP]
> Questa convalida funzionale è particolarmente importante quando si sposta un database da una versione legacy, ad esempio [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] o [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], in una nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], perché è possibile che il codice dell'applicazione usi istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non più disponibili e non protette dal livello di compatibilità del database. Tuttavia, quando si passa da una versione più recente (ad esempio [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], non vi sono istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sospese di cui preoccuparsi. Per altre informazioni sulle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sospese, vedere [Uso dei livelli di compatibilità per la compatibilità con le versioni precedenti](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

> [!NOTE]
> DMA supporta il livello di compatibilità del database 100 e superiore. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] come versione di origine è escluso.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di eseguire una quantità minima di test per convalidare l'esito positivo di un aggiornamento mantenendo il livello di compatibilità del database precedente. Determinare questa quantità minima di test in base all'applicazione e allo scenario specifici.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] offre protezione della forma del piano di query quando:
>
> - La nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (destinazione) viene eseguita su hardware simile a quello in cui veniva eseguita la versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (origine).
> - Viene usato lo stesso [livello di compatibilità del database supportato](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats) nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione e nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di origine.
> - Lo **stesso** database e lo stesso carico di lavoro vengono usati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione e nell'origine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
>
> Eventuali regressioni della forma del piano di query (rispetto all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di origine) che si dovessero verificare nelle condizioni sopra riportate verranno risolte. A questo scopo, contattare il supporto tecnico Microsoft.
  
## <a name="see-also"></a>Vedere anche 
[Livello di compatibilità di ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)       
[Visualizzare o modificare il livello di compatibilità di un database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)       
[Procedure consigliate per aggiornare il livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level)