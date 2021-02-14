---
title: Pianificare e testare il piano di aggiornamento del motore di database | Microsoft Docs
description: Questo articolo descrive la pianificazione prima di iniziare l'aggiornamento di SQL Server. Include un elenco di controllo della pianificazione e lo sviluppo e il test di un piano di aggiornamento.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 19c5b725-7400-4881-af8f-fd232ca28234
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7a6311f79aa87d93ee980cf4d0c8d00352b14687
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353049"
---
# <a name="plan-and-test-the-database-engine-upgrade-plan"></a>Pianificare e testare il piano di aggiornamento del motore di database

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
 Per eseguire correttamente l'aggiornamento di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , indipendentemente dall'approccio, è opportuna un'accurata pianificazione.  
  
## <a name="release-notes-and-known-upgrade-issues"></a>Note sulla versione e problemi di aggiornamento noti  
 Prima di aggiornare il [!INCLUDE[ssDE](../../includes/ssde-md.md)], vedere:

- [Note sulla versione di SQL Server 2019](../../sql-server/sql-server-version-15-release-notes.md)
- [Note sulla versione di SQL Server 2017](../../sql-server/sql-server-2017-release-notes.md) 
- [Note sulla versione di SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md)
- Articolo [Compatibilità con le versioni precedenti del motore di database di SQL Server](../discontinued-database-engine-functionality-in-sql-server.md).  
  
## <a name="pre-upgrade-planning-checklist"></a>Elenco di controllo per la pianificazione pre-aggiornamento  
 Prima di aggiornare il [!INCLUDE[ssDE](../../includes/ssde-md.md)], esaminare il seguente elenco di controllo e gli articoli correlati. Questi articoli si applicano a tutti gli aggiornamenti, indipendentemente dal metodo di aggiornamento e consentono di determinare il metodo di aggiornamento più appropriato: aggiornamento in sequenza, aggiornamento con nuova installazione o aggiornamento sul posto. Ad esempio, potrebbe non essere possibile eseguire un aggiornamento in sequenza o sul posto se si esegue l'aggiornamento del sistema operativo, l'aggiornamento da SQL Server 2005 o l'aggiornamento da una versione a 32 bit di SQL Server. Per l'albero delle decisioni, vedere [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Requisiti hardware e software:** vedere i requisiti hardware e software per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi requisiti sono disponibili in: [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Nel ciclo di pianificazione dell'aggiornamento è opportuno considerare l'aggiornamento dell'hardware (un hardware più recente è più veloce e può consentire di ridurre le licenze grazie a un minor numero di processori o al consolidamento di database e server) e l'aggiornamento del sistema operativo. Questi tipi di modifiche hardware e software influiscono sul tipo di metodo di aggiornamento.  
  
-   **Ambiente corrente:** eseguire ricerche nell'ambiente corrente per comprendere i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in uso e i client connessi all'ambiente.  
  
    -   **Provider client:** anche se l'aggiornamento non richiede di aggiornare il provider dei singoli client, è possibile scegliere di eseguire questa operazione. Se si esegue l'aggiornamento da [!INCLUDE[sql14](../../includes/sssql14-md.md)] o versione precedente, le funzionalità [!INCLUDE[sql15](../../includes/sssql16-md.md)] seguenti richiedono un provider aggiornato per ogni client o per offrire funzionalità aggiuntive:  
  
       -   [Always Encrypted &#40;Motore di database&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
  
       -   [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
       -   [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
       -   Aggiornamento della sicurezza TLS  

   >[!NOTE]
   >L'elenco precedente si applica anche a [!INCLUDE[sscurrent](../../includes/sscurrent-md.md)].
  
-   **Componenti di terze parti:** determinare la compatibilità dei componenti di terze parti, ad esempio il backup integrato.  
  
-   **Ambiente di destinazione:** verificare che l'ambiente di destinazione soddisfi i requisiti hardware e software e che supporti i requisiti del sistema originale. Ad esempio, l'aggiornamento potrebbe comportare il consolidamento di più istanze di SQL Server in un'unica e nuova istanza di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] o la virtualizzazione dell'ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su un cloud privato o pubblico.  
  
-   **Edizione:** determinare la versione appropriata di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] e i percorsi di aggiornamento appropriati per l'aggiornamento. Per informazioni dettagliate, vedere [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md). Prima di eseguire l'aggiornamento da un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un'altra, verificare che le funzionalità attualmente in uso siano supportate nell'edizione a cui si desidera eseguire l'aggiornamento.  
  
    > [!NOTE]  
    >  Quando si esegue l'aggiornamento a [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition, scegliere tra Enterprise Edition: licenze basate su core ed Enterprise Edition. Queste due edizioni differiscono solo per le modalità di gestione delle licenze. Per altre informazioni, vedere [Compute Capacity Limits by Edition of SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
-   **Compatibilità con le versioni precedenti:** vedere l'articolo sulla compatibilità con le versioni precedenti del motore di database di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] per esaminare i cambiamenti nel comportamento tra [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] e la versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dalla quale si sta eseguendo l'aggiornamento. Vedere [SQL Server Database Engine Backward Compatibility](../discontinued-database-engine-functionality-in-sql-server.md).  
  
-   **Data Migration Assistant:** eseguire Data Migration Assistant per un supporto nell'analisi dei problemi che potrebbero bloccare il processo di aggiornamento o richiedere modifiche degli script esistenti o delle applicazioni a seguito di una modifica importante.
    È possibile scaricare Data Migration Assistant [qui](https://aka.ms/get-dma).  
  
-   **Controllo configurazione sistema:** eseguire il Controllo configurazione sistema (SCC, System Configuration Checker) di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] per determinare se il programma di installazione di SQL Server rileva problemi di blocco prima di pianificare l'aggiornamento. Per altre informazioni, vedere [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   **Aggiornamento di tabelle ottimizzate per la memoria:** quando si aggiorna un'istanza del database SQL Server 2014 che contiene le tabelle ottimizzate per la memoria a SQL Server 2016, il processo di aggiornamento richiede più tempo per convertire le tabelle ottimizzate per la memoria nel nuovo formato su disco (e il database rimane offline durante questa procedura).   La quantità di tempo dipende dalle dimensioni delle tabelle ottimizzate per la memoria e dalla velocità del sottosistema di I/O. L'aggiornamento richiede tre dimensioni di operazioni di dati per gli aggiornamenti sul posto e con nuova installazione (il passaggio 1 non è obbligatorio per gli aggiornamenti in sequenza, ma i passaggi 2 e 3 sono obbligatori):  
  
    1.  Eseguire il ripristino del database usando il formato su disco precedente (e caricando tutti i dati nelle tabelle ottimizzate per la memoria all'interno della memoria del disco)  
  
    2.  Serializzare i dati su disco nel nuovo formato su disco  
  
    3.  Eseguire il ripristino del database usando il nuovo formato (e caricando tutti i dati nelle tabelle ottimizzate per la memoria all'interno della memoria del disco)  
  
     Inoltre, l'insufficienza di spazio su disco durante questo processo impedisce l'esecuzione del ripristino. Assicurarsi che sia disponibile spazio sufficiente su disco per l'archiviazione del database esistente e ulteriore spazio aggiuntivo corrispondente alla dimensione corrente dei contenitori del filegroup MEMORY_OPTIMIZED_DATA del database per eseguire un aggiornamento sul posto o connettere un database SQL Server 2014 a un'istanza di SQL Server 2016. Usare la query seguente per determinare lo spazio su disco necessario per il filegroup MEMORY_OPTIMIZED_DATA e la quantità di spazio su disco necessaria per l'esecuzione dell'aggiornamento:  
  
    ```  
    select cast(sum(size) as float)*8/1024/1024 'size in GB'   
    from sys.database_files  
    where data_space_id in (select data_space_id from sys.filegroups where type=N'FX')  
    ```  
  
## <a name="develop-and-test-the-upgrade-plan"></a>Sviluppare e testare il piano di aggiornamento  
 L'approccio migliore consiste nel considerare l'aggiornamento come qualsiasi progetto IT. Organizzare un team dedicato all'aggiornamento che disponga di amministrazione del database, rete, estrazione, trasformazione e caricamento (ETL) e altre competenze necessarie per l'aggiornamento. Il team deve:  
  
-   **Scegliere il metodo di aggiornamento:** vedere [Scegliere un metodo di aggiornamento del motore di database](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Sviluppare un piano di ripristino dello stato precedente:** l'esecuzione di questo piano consente di ripristinare l'ambiente originale se è necessario eseguire il rollback.  
  
-   **Determinare i criteri di accettazione:** verificare che l'aggiornamento sia riuscito prima di trasferire gli utenti nell'ambiente aggiornato.  
  
-   **Testare il piano di aggiornamento:** per testare le prestazioni con il carico di lavoro effettivo, usare l'utilità Riesecuzione distribuita di Microsoft SQL Server. L'utilità può usare più computer per rieseguire dati di traccia, simulando un carico di lavoro di importanza critica. Eseguendo una riesecuzione in un server di prova prima e dopo un aggiornamento di SQL Server, è possibile rilevare le differenze di prestazioni e individuare eventuali incompatibilità dell'applicazione con l'aggiornamento. Per altre informazioni, vedere [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md) e [Opzioni della riga di comando dello strumento di amministrazione &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md).  
  
## <a name="next-steps"></a>Passaggi successivi  
[Aggiornare il motore di database](../../database-engine/install-windows/upgrade-database-engine.md) 
  
## <a name="additional-resources"></a>Risorse aggiuntive 
[Guida alla migrazione dei database](https://aka.ms/datamigration)