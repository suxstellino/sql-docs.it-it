---
title: Amministrazione dell'agente di replica | Microsoft Docs
description: Informazioni sulla gestione degli agenti di replica, che eseguono attività per la replica, ad esempio la creazione di copie dello schema e dei dati e la propagazione delle modifiche tra i server.
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Snapshot Agent, administering
- Log Reader Agent, administering
- Queue Reader Agent, administering
- shared agents [SQL Server replication]
- Merge Agent, administering
- Distribution Agent, administering
- agents [SQL Server replication], administering
- replication cleanup jobs [SQL Server]
- administering replication, agents
- replication [SQL Server], administering
- independent agents [SQL Server replication]
ms.assetid: f27186b8-b1b2-4da0-8b2b-91f632c2ab7e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 76c793758fa9886f0f8246c5d6c4aa7ec27fa707
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467332"
---
# <a name="replication-agent-administration"></a>Amministrazione dell'agente di replica
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Gli agenti di replica eseguono numerose attività associate alla replica, tra cui la creazione di copie di schemi e di dati, il rilevamento di aggiornamenti nel server di pubblicazione o nel Sottoscrittore e la distribuzione delle modifiche tra i server. Per impostazione predefinita, gli agenti di replica eseguono passaggi di processo di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent. Gli agenti sono semplici file eseguibili che possono essere chiamati direttamente dalla riga di comando e dagli script batch. Ogni agente di replica supporta un set di parametri run-time utilizzati per controllarne il funzionamento. Tali parametri vengono specificati nei profili degli agenti o dalla riga di comando.  
  
> [!IMPORTANT]  
>  Per impostazione predefinita, il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent è disabilitato durante l'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a meno che non si scelga in modo esplicito di avviarlo automaticamente durante l'installazione.  
  
 I file dell'agente di replica si trovano in [!INCLUDE[ssInstallPathVar](../../../includes/ssinstallpathvar-md.md)]\COM. Nella tabella seguente vengono riportati gli eseguibili di replica disponibili insieme al nome di file corrispondente. Fare clic sul collegamento corrispondente a ogni agente per visualizzarne i parametri di riferimento.  
  
|Eseguibile agente|File Name|  
|----------------------|---------------|  
|[Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)|snapshot.exe|  
|[Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)|distrib.exe|  
|[Agente lettura log repliche](../../../relational-databases/replication/agents/replication-log-reader-agent.md)|logread.exe|  
|[Agente di lettura coda repliche](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)|qrdrsvc.exe|  
|[Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)|replmerg.exe|  
  
 Oltre agli agenti di replica, la replica è caratterizzata da vari processi che eseguono operazioni di manutenzione pianificata e su richiesta.  
  
 **Per eseguire gli agenti e i processi di manutenzione**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] e Monitoraggio replica: [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)  
  
-   Programmazione della replica: [Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
## <a name="agent-profiles"></a>Profili agenti  
 Durante la configurazione della replica viene installato nel server di distribuzione un set di profili agenti. Un profilo agente contiene un set di parametri utilizzati a ogni esecuzione dell'agente. Durante il processo di avvio ogni agente esegue l'accesso al server di distribuzione ed esegue una query dei parametri nel proprio profilo. Per ogni agente viene fornito un profilo predefinito, mentre per l'agente di lettura log, l'agente di distribuzione e l'agente di merge vengono creati profili predefiniti aggiuntivi. Oltre a questi profili, è possibile creare profili specifici in base alle esigenze dell'applicazione. Per altre informazioni, vedere [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 Per informazioni su come specificare i parametri dalla riga di comando, vedere [Concetti di base relativi ai file eseguibili dell'agente di replica](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="monitoring-replication-agents"></a>Monitoraggio degli agenti di replica  
 Monitoraggio replica consente di visualizzare informazioni ed eseguire operazioni associate a ogni agente di replica. Nell'elenco seguente vengono inclusi tutti gli agenti, le relative schede di Monitoraggio replica e un collegamento all'argomento in cui si spiega come accedere a queste schede:  
  
-   Gli agenti seguenti sono associati alle pubblicazioni in Monitoraggio replica:  
  
    -   agente snapshot  
  
    -   Agente di lettura log  
  
    -   Agente di lettura coda  
  
     Accedere a informazioni e attività associate a questi agenti tramite la scheda **Agenti** . Per altre informazioni, vedere [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Gli agenti seguenti sono associati alle sottoscrizioni in Monitoraggio replica:  
  
    -   Agente di distribuzione  
  
    -   Agente di merge  
  
     Accedere a informazioni e attività associate a questi agenti tramite le schede seguenti: **Elenco verifica sottoscrizioni** (disponibile per ogni server di pubblicazione) o **Tutte le sottoscrizioni** (disponibile per ogni pubblicazione). Per altre informazioni, vedere [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
## <a name="independent-and-shared-agents"></a>Agenti indipendenti e condivisi  
 Gli agenti indipendenti elaborano una sola sottoscrizione. Gli agenti condivisi elaborano più sottoscrizioni. Se è necessario sincronizzare più sottoscrizioni utilizzando lo stesso agente condiviso, per impostazione predefinita le sottoscrizioni vengono poste in attesa in una coda e quindi elaborate dall'agente una alla volta. Quando si utilizzano gli agenti indipendenti si ottiene una riduzione della latenza in quanto ciò consente di sincronizzare immediatamente la sottoscrizione ogni volta che è necessario. Per la replica di tipo merge vengono sempre utilizzati agenti indipendenti, mentre per la replica transazionale gli agenti indipendenti vengono utilizzati per impostazione predefinita per le pubblicazioni create con Creazione guidata nuova pubblicazione (nelle precedenti versioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]per la replica transazionale vengono utilizzati per impostazione predefinita gli agenti condivisi).  
  
## <a name="replication-maintenance-jobs"></a>Processi di manutenzione della replica  
 Per eseguire operazioni di manutenzione pianificata e su richiesta vengono utilizzati i processi seguenti.  
  
|Processo di eliminazione|Descrizione|Pianificazione predefinita|  
|------------------|-----------------|----------------------|  
|Eliminazione del contenuto della cronologia dell'agente: Distribuzione|Rimuove la cronologia degli agenti di replica dal database di distribuzione.|Viene eseguito ogni dieci minuti.|  
|Eliminazione del contenuto della distribuzione: Distribuzione|Rimuove le transazioni replicate dal database di distribuzione. |Viene eseguito ogni dieci minuti.|  
|Pulizia dei riferimenti alla sottoscrizione scaduta|Rileva e rimuove le sottoscrizioni scadute dai database di pubblicazione. Nel database di distribuzione, disattiva le sottoscrizioni che non sono state sincronizzate entro il periodo massimo di memorizzazione per la distribuzione.|Viene eseguito ogni giorno alle ore 1.00 del mattino.| 
|Reinizializzazione delle sottoscrizioni con errori di convalida dei dati|Rileva tutte le sottoscrizioni in cui si sono verificati errori di convalida dei dati e le contrassegna per la reinizializzazione. Alla successiva esecuzione dell'agente di merge o dell'agente di distribuzione verrà applicato ai Sottoscrittori un nuovo snapshot.|Nessuna pianificazione predefinita (per impostazione predefinita è disabilitato).|  
|Controllo degli agenti di replica|Rileva gli agenti di replica che non registrano attivamente una cronologia. Scrive nel registro eventi di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows l'eventuale esito negativo di un passaggio del processo.|Viene eseguito ogni dieci minuti.|  
|Aggiornamento del monitoraggio della replica per la distribuzione|Aggiorna le query memorizzate nella cache utilizzate da Monitoraggio replica.|Viene eseguito continuamente.|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio della replica](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
