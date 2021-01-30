---
description: MSSQL_ENG018752
title: MSSQL_ENG018752 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018752 error
ms.assetid: 405b2655-acb4-4e15-bcc6-b8f86bb22b37
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 00d3e098ca0b3686a96be5741d2800973d632147
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205615"
---
# <a name="mssql_eng018752"></a>MSSQL_ENG018752
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|18752|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|A un database può connettersi un solo agente di lettura log o una sola procedura correlata ai log (sp_repldone, sp_replcmds e sp_replshowcmds) alla volta. Se è stata eseguita una procedura correlata ai log, eliminare la connessione utilizzata per eseguire la procedura oppure eseguire sp_replflush tramite tale connessione prima di avviare l'agente di lettura log o di eseguire un'altra procedura relativa ai log.|  
  
## <a name="explanation"></a>Spiegazione  
 È in corso da parte di più connessioni il tentativo di eseguire una delle procedure seguenti: **sp_repldone**, **sp_replcmds** o **sp_replshowcmds**. Le stored procedure [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md) e [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) vengono usate dall'agente di lettura log per trovare e aggiornare le informazioni sulle transazioni replicate in un database pubblicato. La stored procedure [sp_replshowcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md) viene usata per la risoluzione di alcuni tipi di problemi relativi alla replica transazionale.  
  
 Questo errore viene generato nelle circostanze seguenti:  
  
-   Se l'agente di lettura log di un database pubblicato è in esecuzione e un secondo agente di lettura log tenta l'esecuzione sullo stesso database, per il secondo agente viene generato l'errore, che appare nella cronologia dell'agente.  
  
     In una situazione in cui compaiono più agenti, è possibile che uno di loro sia il risultato di un processo orfano.  
  
-   Se l'agente di lettura log di un database pubblicato viene avviato e un utente esegue **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** sullo stesso database, viene generato l'errore nell'applicazione in cui è stata eseguita la stored procedure (ad esempio **sqlcmd**).  
  
-   Se l'agente di lettura log di un database pubblicato viene avviato e un utente esegue **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** e non chiude la connessione su cui è stata eseguita la procedura, quando l'agente di lettura log tenta di connettersi al database viene generato l'errore.  
  
## <a name="user-action"></a>Azione dell'utente  
 I passaggi seguenti possono contribuire alla risoluzione del problema. Se uno dei passaggi consente l'avvio senza errori dell'agente di lettura log, non è necessario completare i passaggi rimanenti.  
  
-   Verificare nella cronologia dell'agente di lettura log la presenza di eventuali altri errori che potrebbero contribuire a questo errore. Per informazioni sui dettagli di stato e di errore dell'agente di visualizzazione in Monitoraggio replica, vedere [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Verificare nell'output di [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) la presenza di numeri di identificazione di processo (SPID) connessi al database pubblicato. Chiudere le connessioni che potrebbero aver eseguito **sp_repldone**, **sp_replcmds** o **sp_replshowcmds**.  
  
-   Riavviare l'agente di lettura log. Per altre informazioni, vedere [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
-   Riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent (metterlo offline oppure online in un cluster) sul server di distribuzione. Se vi è una possibilità che un processo pianificato abbia eseguito **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** da altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , riavviare l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent anche per queste istanze. Per altre informazioni, vedere [Avviare, arrestare o sospendere il servizio SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md).  
  
-   Eseguire [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) nel server di pubblicazione sul database di pubblicazione e quindi riavviare l'agente di lettura log.  
  
-   Se l'errore continua a verificarsi, aumentare il livello di dettaglio per la registrazione delle operazioni dell'agente e specificare un file di output per il log. A seconda del contesto dell'errore, in questo modo si potrebbero ottenere ulteriori informazioni sui passaggi che conducono all'errore e/o messaggi di errore aggiuntivi.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Agente lettura log repliche](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
