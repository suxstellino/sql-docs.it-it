---
description: MSSQL_ENG020554
title: MSSQL_ENG020554 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG020554 error
ms.assetid: ef1a1b88-b2ab-43e8-99cd-163a973262d6
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1188e7a173b67bbc926e652124b62f1cff2e2222
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205604"
---
# <a name="mssql_eng020554"></a>MSSQL_ENG020554
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|20554|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Nessun messaggio di stato registrato dall'agente di replica negli ultimi %ld minuti. Questa condizione può indicare che l'agente non risponde oppure che l'attività del sistema è elevata. Verificare che i record vengano replicati nella destinazione e che le connessioni al Sottoscrittore, al server di pubblicazione e al server di distribuzione siano ancora attive.|  
  
## <a name="explanation"></a>Spiegazione  
 Il processo **Controllo degli agenti di replica** viene eseguito a intervalli di tempo specificati (per impostazione predefinita ogni 10 minuti) per verificare lo stato di ogni agente di replica. Se un agente non ha registrato alcun messaggio di stato dall'ultima esecuzione del processo di controllo, viene generato l'errore MSSQL_ENG020554. È previsto che l'agente registri almeno messaggi di cronologia anche se non vi è nessun'altra attività di replica in corso. Sebbene l'agente di replica non risponda come previsto, non significa necessariamente che la sua attività si sia arrestata o non sia riuscita. In quest'ultimo caso verrebbe generato l'errore MSSQL_ENG020536.  
  
 L'errore MSSQL_ENG020554 può essere generato dai problemi seguenti:  
  
-   L'agente è occupato.  
  
     Se l'agente è occupato a rispondere al polling del processo di controllo degli agenti, non sarà possibile verificare se è in funzione in modo appropriato. Vi sono vari motivi per cui l'agente di replica potrebbe essere occupato. È possibile che vi siano molti dati in fase di replica oppure problemi di sviluppo o di configurazione delle applicazioni che determinano lunghi tempi di elaborazione dei processi.  
  
-   L'agente non è in grado di accedere a uno dei computer nella topologia.  
  
     Tutti gli agenti hanno un parametro **-LoginTimeOut** (per impostazione predefinita pari a 15 secondi) che determina la durata del tentativo di accesso di un agente a un nodo di replica, ad esempio il tempo di accesso di un agente di merge al server di pubblicazione. Se il valore impostato per **-LoginTimeOut** è superiore all'intervallo in cui viene eseguito il processo di controllo degli agenti di replica, la causa principale dell'errore potrebbe essere un problema di accesso, ovvero l'errore MSSQL_ENG020554 viene generato prima che l'agente riesca a generare un errore più specifico.  
  
## <a name="user-action"></a>Azione dell'utente  
 A seconda della causa dell'errore è necessaria un'azione specifica:  
  
-   Per tutti i casi in cui viene generato questo errore:  
  
     Controllare i dettagli dell'errore in Monitoraggio replica e quindi riavviare l'agente se si è arrestato. Nei dettagli dell'errore potrebbero essere contenute informazioni aggiuntive sui motivi del malfunzionamento dell'agente. Se l'agente è in esecuzione, non arrestarlo né riavviarlo, poiché ciò potrebbe aggravare il problema. Per informazioni sui dettagli di stato e di errore dell'agente di visualizzazione in Monitoraggio replica, vedere [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).    
  
-   Se l'errore viene generato spesso perché l'agente è occupato:  
  
     Potrebbe essere necessario riprogettare l'applicazione in modo da ridurre il tempo di elaborazione.  
  
     È possibile aumentare l'intervallo di controllo dello stato dell'agente tramite la finestra di dialogo **Proprietà processo** . Per informazioni sull'accesso a questa finestra di dialogo per processi di replica, vedere [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Se l'agente non è in grado di accedere a uno dei computer nella topologia:  
  
     È consigliabile impostare il parametro **-LoginTimeOut** su un valore inferiore rispetto all'intervallo di esecuzione del processo di controllo degli agenti di replica. In alcuni casi, il valore di **-LoginTimeOut** è superiore a causa di problemi di rete che determinano il timeout degli accessi. Impostando **-LoginTimeOut** su un valore inferiore, sarà possibile ricevere errori più specifici e quindi risolvere più facilmente i problemi di accesso che potrebbero essere causati da autorizzazioni, problemi di rete o altro. I parametri degli agenti possono essere specificati nei profili agente e dalla riga di comando. Per altre informazioni, vedere:  
  
    -   [Usare i profili agenti di replica](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Visualizzare e modificare i parametri del prompt dei comandi dell'agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Amministrazione dell'agente di replica](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agente di lettura coda repliche](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  
