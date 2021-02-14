---
title: Visualizzare le informazioni ed eseguire attività (Monitoraggio replica)
description: Informazioni su come visualizzare informazioni ed eseguire varie attività usando Monitoraggio replica in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 24eb21c5907e343ec433316fbdceebfc3864cedc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340751"
---
# <a name="view-information-and-perform-tasks-using-replication-monitor"></a>Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
Monitoraggio replica offre schede e opzioni per visualizzare le informazioni ed eseguire diverse attività. Questo articolo descrive le informazioni che possono essere visualizzate e le operazioni che possono essere eseguite usando Monitoraggio replica. 


## <a name="for-a-publication"></a>Per una pubblicazione

### <a name="view-information"></a>Visualizzare informazioni
In Monitoraggio replica sono disponibili le schede seguenti che includono le informazioni sulla pubblicazione selezionata:  
  
-   **Tutte le sottoscrizioni**: questa scheda visualizza informazioni su tutte le sottoscrizioni alla pubblicazione selezionata.  
  
-   **Agenti**: questa scheda visualizza informazioni su tutti gli agenti usati da una pubblicazione:   
    -   Agente snapshot, utilizzato da tutte le pubblicazioni.   
    -   Agente di lettura log, utilizzato da tutte le pubblicazioni transazionali.   
    -   Agente di lettura coda, utilizzato dalle pubblicazioni transazionali contenenti sottoscrizioni ad aggiornamento in coda.  
  
-   **Avvisi**: questa scheda consente di specificare avvisi per gli agenti.  
  
-   **Token di traccia** (solo replica transazionale): questa scheda consente di misurare la latenza, ovvero l'intervallo di tempo che intercorre tra il commit di una transazione nel server di pubblicazione e il commit della transazione corrispondente nel sottoscrittore.  
  
 Per altre informazioni sulle opzioni di ogni scheda, selezionare la scheda nel riquadro a destra e quindi selezionare **?** sulla barra dei menu. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Eseguire attività
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi selezionare una pubblicazione.   
2.  Per visualizzare e modificare le proprietà della pubblicazione, fare clic con il pulsante destro del mouse sulla pubblicazione e quindi selezionare **Proprietà**.    
3.  Per visualizzare informazioni sulle sottoscrizioni, selezionare la scheda **Tutte le sottoscrizioni**, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi selezionare **Proprietà**. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività. 
4.  Per visualizzare informazioni sugli agenti, selezionare la scheda **Agenti**. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività. 
5.  Per visualizzare informazioni su avvisi e soglie degli agenti, selezionare la scheda **Avvisi**. Per altre informazioni, vedere [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
6.  Per visualizzare informazioni sui token di traccia, selezionare la scheda **Token di traccia**. Per ulteriori informazioni sulle modalità di utilizzo dei token di traccia, vedere [Misurazione della latenza e convalida delle connessioni per la replica transazionale](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="for-a-publisher"></a>Per un server di pubblicazione 

### <a name="view-information"></a>Visualizzare informazioni
In Monitoraggio replica sono disponibili le schede seguenti in cui vengono visualizzate informazioni sul server di pubblicazione selezionato:   
-   **Pubblicazioni**: visualizza informazioni su tutte le pubblicazioni del server di pubblicazione selezionato.   
-   **Elenco verifica sottoscrizioni**: visualizza informazioni sulle sottoscrizioni di tutte le pubblicazioni disponibili sul server di pubblicazione selezionato che presentano errori, avvisi o un livello di prestazioni insufficiente. Questa scheda non viene visualizzata per i server di distribuzione che eseguono versioni di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].    
-   **Agenti**: questa scheda visualizza informazioni dettagliate su agenti e processi usati da tutti i tipi di replica. La scheda consente anche di avviare e arrestare ciascun agente e processo. Per visualizzare ulteriori informazioni sulle opzioni disponibili nelle singole schede, fare clic sulla scheda nel riquadro destro e quindi su **?** nella barra dei menu. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Eseguire attività
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro e quindi fare clic su un server di pubblicazione.   
2.  Per visualizzare informazioni su tutte le pubblicazioni, fare clic sulla scheda **Pubblicazioni** .    
3.  Per visualizzare informazioni sulle sottoscrizioni, fare clic sulla scheda **Elenco verifica sottoscrizioni** . In questa scheda, è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività.   
    -   Per visualizzare informazioni dettagliate sull'agente associato a una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**.    
    -   Per visualizzare le proprietà di una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Proprietà**.    
    -   Per sincronizzare una sottoscrizione push, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.   
    -   Per reinizializzare una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Reinizializza sottoscrizione**.   
4.  Per visualizzare informazioni sugli agenti, fare clic sulla scheda **Agenti** . In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività:    
    -   Per visualizzare informazioni dettagliate su un agente, ad esempio messaggi informativi ed eventuali messaggi di errore, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Visualizza dettagli**.  
    -   Per visualizzare informazioni dettagliate sul processo eseguito dall'agente, ad esempio la pianificazione, i dettagli sui passaggi del processo e così via, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Proprietà**.    
    -   Per gestire i profili dell'agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Profilo agente**. Per altre informazioni, vedere [Usare i profili agenti di replica](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).    
    -   Per avviare un agente non in esecuzione, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Avvia agente**.  
    -   Per arrestare l'esecuzione di un agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Arresta agente**.  


## <a name="for-a-subscription"></a>Per una sottoscrizione 

### <a name="view-information"></a>Visualizzare informazioni
  In Monitoraggio replica sono disponibili le schede seguenti, che includono informazioni sulle sottoscrizioni:    
-   **Tutte le sottoscrizioni**: visualizza informazioni su tutte le sottoscrizioni alla pubblicazione selezionata.   
-   **Elenco verifica sottoscrizioni**: visualizza informazioni sulle sottoscrizioni di tutte le pubblicazioni disponibili sul server di pubblicazione selezionato che presentano errori, avvisi o un livello di prestazioni insufficiente. Questa scheda non viene visualizzata per i server di distribuzione che eseguono versioni di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Per ulteriori informazioni sulle opzioni di ogni scheda, fare clic sulla scheda nel riquadro a destra e quindi fare clic su **?** sulla barra dei menu. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Eseguire attività
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.   
2.  Per visualizzare le informazioni sulle sottoscrizioni, fare clic sulla scheda **Tutte le sottoscrizioni** . Per visualizzare solo le sottoscrizioni in uno stato specifico, ad esempio in sincronizzazione, selezionare un'opzione nell'elenco a discesa **Mostra** .    
3.  Per visualizzare e modificare le proprietà della sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Proprietà**. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività. 
  
### <a name="to-view-information-and-perform-tasks-for-subscriptions-in-the-subscription-watch-list-tab"></a>Per visualizzare informazioni ed eseguire attività per le sottoscrizioni nella scheda Elenco verifica sottoscrizioni  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro e quindi fare clic su un server di pubblicazione.    
2.  Per visualizzare informazioni sulle sottoscrizioni, fare clic sulla scheda **Elenco verifica sottoscrizioni** .    
3.  Selezionare il tipo di sottoscrizione da visualizzare nell'elenco a discesa **Mostra sottoscrizioni \<SubscriptionType>** . Per visualizzare solo le sottoscrizioni in uno stato specifico, ad esempio in sincronizzazione, selezionare un'opzione nell'elenco a discesa **Mostra** .    
4.  Per visualizzare e modificare le proprietà della sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Proprietà**. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività. 
  
  
## <a name="for-publication-agents"></a>Per gli agenti di pubblicazione
In Monitoraggio replica è disponibile la scheda **Agenti** , in cui sono presenti informazioni sugli agenti associati alla pubblicazione selezionata. L'agente di distribuzione e l'agente di merge sono associati alle sottoscrizioni. 
  
### <a name="view-information"></a>Visualizzare informazioni
 In questa scheda vengono visualizzate informazioni relative agli agenti seguenti:  
  
-   Agente snapshot, utilizzato da tutte le pubblicazioni.  
-   Agente di lettura log, utilizzato da tutte le pubblicazioni transazionali.  
-   Agente di lettura coda, utilizzato dalle pubblicazioni transazionali abilitate per le sottoscrizioni ad aggiornamento in coda. 
  
 Per visualizzare altre informazioni relative alle opzioni della scheda, fare clic su **?** sulla barra dei menu. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>Per visualizzare informazioni ed eseguire attività relative agli agenti associati a una pubblicazione  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.    
2.  Per visualizzare le informazioni sugli agenti, fare clic sulla scheda **Agenti** . In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività:    
    -   Per visualizzare informazioni dettagliate su un agente, ad esempio messaggi informativi ed eventuali messaggi di errore, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Visualizza dettagli**.  
    -   Per visualizzare informazioni dettagliate sul processo eseguito dall'agente, ad esempio la pianificazione, i dettagli sui passaggi del processo e così via, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Proprietà**.    
    -   Per gestire i profili dell'agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Profilo agente**. Per altre informazioni, vedere [Usare i profili agenti di replica](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).   
    -   Per avviare un agente non in esecuzione, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Avvia agente**.   
    -   Per arrestare l'esecuzione di un agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Arresta agente**.  
  
## <a name="for-subscription-agents"></a>Per gli agenti di sottoscrizione

### <a name="view-information"></a>Visualizzare informazioni
-   **Tutte le sottoscrizioni**: visualizza informazioni su tutte le sottoscrizioni alla pubblicazione selezionata.  
  
-   **Elenco verifica sottoscrizioni**: progettata per visualizzare informazioni sulle sottoscrizioni di tutte le pubblicazioni disponibili sul server di pubblicazione selezionato che presentano errori, avvisi o un livello di prestazioni insufficiente. Questa scheda non viene visualizzata per i server di distribuzione che eseguono versioni di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Per ulteriori informazioni sulle opzioni di ogni scheda, fare clic sulla scheda nel riquadro a destra e quindi fare clic su **?** sulla barra dei menu. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Eseguire attività
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.    
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** per visualizzare le informazioni relative alle sottoscrizioni. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività:   
    -   Per visualizzare informazioni dettagliate sull'agente associato a una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**. Le informazioni dettagliate includono la cronologia dell'agente e i messaggi di errore, statistiche sulle prestazioni nella replica transazionale e statistiche sulla sincronizzazione dei singoli articoli nella replica di tipo merge.  
  
         A seconda del tipo di sottoscrizione dalla finestra dei dettagli vengono avviate schede diverse: per le sottoscrizioni snapshot la scheda **Cronologia server di distribuzione - Sottoscrittore**, per le sottoscrizioni transazionali le schede **Cronologia server di pubblicazione - server di distribuzione**, **Cronologia server di distribuzione - Sottoscrittore** e **Comandi non distribuiti** e per le sottoscrizioni di tipo merge la scheda **Cronologia sincronizzazione**.  
  
    -   Per sincronizzare una sottoscrizione push, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.    
    -   Per reinizializzare una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Reinizializza sottoscrizione**.    
    -   Per convalidare una singola sottoscrizione di tipo merge, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Convalida sottoscrizione**. Per convalidare tutte le sottoscrizioni di una pubblicazione di tipo merge, fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Convalida tutte le sottoscrizioni**. Per convalidare tutte le sottoscrizioni di una pubblicazione transazionale, fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Convalida sottoscrizioni**.    
    -   Per gestire i profili dell'agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Profilo agente**. Per altre informazioni, vedere [Usare i profili agenti di replica](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
### <a name="subscription-watch-list-tab"></a>Scheda Elenco verifica sottoscrizioni 
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro e quindi fare clic su un server di pubblicazione.    
2.  Fare clic sulla scheda **Elenco verifica sottoscrizioni** per visualizzare le informazioni relative alle sottoscrizioni. In questa scheda è inoltre possibile accedere a informazioni più dettagliate ed eseguire altre attività:   
    -   Per visualizzare informazioni dettagliate sull'agente associato a una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**. Le informazioni dettagliate includono la cronologia dell'agente e i messaggi di errore, statistiche sulle prestazioni nella replica transazionale e statistiche sulla sincronizzazione dei singoli articoli nella replica di tipo merge.    
         A seconda del tipo di sottoscrizione dalla finestra dei dettagli vengono avviate schede diverse: per le sottoscrizioni snapshot la scheda **Cronologia server di distribuzione - Sottoscrittore**, per le sottoscrizioni transazionali le schede **Cronologia server di pubblicazione - server di distribuzione**, **Cronologia server di distribuzione - Sottoscrittore** e **Prestazioni** e per le sottoscrizioni di tipo merge la scheda **Cronologia sincronizzazione**.  
  
    -   Per sincronizzare una sottoscrizione push, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.    
    -   Per reinizializzare una sottoscrizione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Reinizializza sottoscrizione**.    
    -   Per convalidare una singola sottoscrizione di tipo merge, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Convalida sottoscrizione**. Per convalidare tutte le sottoscrizioni di una pubblicazione di tipo merge, fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Convalida tutte le sottoscrizioni**. Per convalidare tutte le sottoscrizioni di una pubblicazione transazionale, fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Convalida sottoscrizioni**.    
    -   Per gestire i profili dell'agente, fare clic con il pulsante destro del mouse sull'agente e quindi scegliere **Profilo agente**. Per altre informazioni, vedere [Usare i profili agenti di replica](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  

    


## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Visualizzare e modificare le proprietà delle sottoscrizioni push](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Visualizzare e modificare le proprietà delle sottoscrizioni pull](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
 [Impostare valori di soglia e avvisi in Monitoraggio replica](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [Amministrazione dell'agente di replica](../../../relational-databases/replication/agents/replication-agent-administration.md)    
  
  
