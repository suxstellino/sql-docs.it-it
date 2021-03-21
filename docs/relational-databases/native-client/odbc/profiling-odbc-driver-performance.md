---
description: Profiling delle prestazioni del driver ODBC
title: Profilatura delle prestazioni del driver ODBC | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- profiling ODBC driver performance data [SQL Server Native Client]
- performance [ODBC]
- application statistics [ODBC]
- time statistics [ODBC]
- ODBC, performance data
- SQL Server Native Client ODBC driver, profiling performance data
- SQLPERF data structure
- statistical information [ODBC]
ms.assetid: 8f44e194-d556-4119-a759-4c9dec7ecead
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e2771a14544aed31304779c66fc089769b5b7a2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740721"
---
# <a name="profiling-odbc-driver-performance"></a>Profiling delle prestazioni del driver ODBC
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il driver ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client può eseguire il profiling di due tipi di dati sulle prestazioni:  
  
-   Query con esecuzione prolungata.  
  
     Il driver può scrivere in un file di log tutte le query che non ottengono una risposta dal server entro un periodo di tempo specificato. I programmatori di applicazioni o gli amministratori di database possono quindi eseguire la ricerca di ogni istruzione SQL registrata per determinare il modo in cui poterne migliorare le prestazioni.  
  
-   Dati sulle prestazioni del driver.  
  
     Il driver può registrare le statistiche sulle prestazioni e scriverle in un file o renderle disponibili in un'applicazione tramite una struttura dei dati specifica del driver denominata SQLPERF. Il file che contiene le statistiche sulle prestazioni è un file delimitato da tabulazioni che può essere analizzato facilmente con qualsiasi foglio di calcolo che supporti i file delimitati da tabulazioni, ad esempio Microsoft Excel.  
  
 È possibile attivare entrambi i tipi di profiling nel seguente modo:  
  
-   Connettendosi a un'origine dati che specifica la registrazione.  
  
-   Chiamata di [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) per impostare attributi specifici del driver che controllano la profilatura.  
  
 Ogni processo dell'applicazione ottiene la propria copia del driver ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client mentre il profiling è globale per la combinazione di copia del driver e processo dell'applicazione. Quando il profiling viene attivato in un'applicazione, vengono registrate le informazioni per tutte le connessioni attive nel driver a partire da tale applicazione. Vengono anche incluse le connessioni che non hanno specificamente richiesto il profiling.  
  
 Il log di profiling (log di dati sulle prestazioni o log di query con esecuzione prolungata) aperto dal driver viene chiuso solo dopo che il driver viene scaricato da Gestione driver ODBC, quando un'applicazione libera tutti gli handle di ambiente aperti nel driver. Se l'applicazione apre un nuovo handle di ambiente, viene caricata una nuova copia del driver. Se l'applicazione si connette quindi a un'origine dati che specifica lo stesso file di log o imposta gli attributi specifici del driver per eseguire la registrazione nello stesso file, il driver sovrascrive il log precedente.  
  
 Se un'applicazione avvia il profiling in un file di log e una seconda applicazione tenta di avviare il profiling nello stesso file di log, la seconda applicazione non potrà eseguire la registrazione dei dati del profiling. Se la seconda applicazione avvia il profiling dopo che la prima applicazione ha scaricato il driver, la seconda applicazione sovrascriverà il file di log della prima applicazione.  
  
 Se un'applicazione si connette a un'origine dati con la profilatura abilitata, il driver restituisce SQL_ERROR se l'applicazione chiama **SQLSetConnectOption** per avviare la registrazione. Una chiamata a **SQLGetDiagRec** restituisce quindi quanto segue:  
  
```  
SQLState: 01000, pfNative = 0  
ErrorMsg: [Microsoft][SQL Server Native Client]  
   An error has occurred during the attempt to access  
   the log file, logging disabled.  
```  
  
 Il driver arresta la raccolta dei dati sulle prestazioni quando viene chiuso un handle di ambiente. Se un'applicazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dispone di più connessioni, ciascuna con il proprio handle di ambiente, il driver arresterà la raccolta dei dati sulle prestazioni quando vengono chiusi tutti gli handle di ambiente associati.  
  
 I dati sulle prestazioni del driver possono essere archiviati nella struttura dei dati SQLPERF o registrati in un file delimitato da tabulazioni. I dati includono le seguenti categorie di statistiche:  
  
-   Profilo dell'applicazione  
  
-   Connessioni  
  
-   Rete  
  
-   Tempo  
  
 Nella tabella seguente le descrizioni dei campi per la struttura dei dati SQLPERF vengono applicate anche alle statistiche registrate nel file di log delle prestazioni.  
  
### <a name="application-profile-statistics"></a>Statistiche sul profilo dell'applicazione  
  
|Campo SQLPERF|Descrizione|  
|-------------------|-----------------|  
|TimerResolution|Risoluzione minima del tempo di clock del server in millisecondi. Viene generalmente riportato come 0 (zero) e deve essere considerato solo se il numero riportato è maggiore. Se la risoluzione minima del tempo di clock del server è maggiore dell'intervallo probabile per alcune delle statistiche basate sul timer, tali statistiche possono essere state alterate.|  
|SQLidu|Numero di istruzioni INSERT, DELETE o UPDATE dopo SQL_PERF_START.|  
|SQLiduRows|Numero di istruzioni INSERT, DELETE o UPDATE dopo SQL_PERF_START.|  
|SQLSelects|Numero di istruzioni SELECT elaborate dopo SQL_PERF_START.|  
|SQLSelectRows|Numero di righe selezionate dopo SQL_PERF_START.|  
|Transazioni|Numero di transazioni utente dopo SQL_PERF_START, inclusi i rollback. Quando un'applicazione ODBC è in esecuzione con SQL_AUTOCOMMIT_ON, ogni comando viene considerato una transazione.|  
|SQLPrepares|Numero di chiamate di [funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md) dopo SQL_PERF_START.|  
|ExecDirects|Numero di chiamate a **SQLExecDirect** dopo SQL_PERF_START.|  
|SQLExecutes|Numero di chiamate **SQLExecute** dopo SQL_PERF_START.|  
|CursorOpens|Numero di volte in cui il driver ha aperto un cursore server dopo SQL_PERF_START.|  
|CursorSize|Numero di righe nei set di risultati aperti dai cursori dopo SQL_PERF_START.|  
|CursorUsed|Numero di righe effettivamente recuperate tramite il driver dai cursori dopo SQL_PERF_START.|  
|PercentCursorUsed|È uguale a CursorUsed/CursorSize. Se ad esempio un'applicazione determina l'apertura di un cursore server da parte del driver per eseguire "SELECT COUNT(*) FROM Authors", nel set di risultati saranno presenti 23 righe per l'istruzione SELECT. Se l'applicazione recupera solo tre di queste righe, CursorUsed/CursorSize sarà 3/23 e quindi PercentCursorUsed sarà 13.043478.|  
|AvgFetchTime|È uguale a SQLFetchTime/SQLFetchCount.|  
|AvgCursorSize|È uguale a CursorSize/CursorOpens.|  
|AvgCursorUsed|È uguale a CursorUsed/CursorOpens.|  
|SQLFetchTime|Tempo cumulativo impiegato per completare le operazioni di recupero sui cursori server.|  
|SQLFetchCount|Numero di operazioni di recupero eseguite sui cursori server dopo SQL_PERF_START.|  
|CurrentStmtCount|Numero di handle di istruzione attualmente aperti in tutte le connessioni aperte nel driver.|  
|MaxOpenStmt|Numero massimo di handle di istruzione aperti contemporaneamente dopo SQL_PERF_START.|  
|SumOpenStmt|Numero di handle di istruzione aperti dopo SQL_PERF_START.|  
|**Statistiche sulla connessione:**||  
|CurrentConnectionCount|Numero corrente di handle di connessione attivi aperti dall'applicazione nel server.|  
|MaxConnectionsOpened|Numero massimo di handle di connessione aperti contemporaneamente dopo SQL_PERF_START.|  
|SumConnectionsOpened|Somma del numero di handle di connessione aperti dopo SQL_PERF_START.|  
|SumConnectionTime|Somma del periodo di tempo in cui tutte le connessioni sono state aperte dopo SQL_PERF_START. Se ad esempio un'applicazione ha aperto 10 connessioni e ha mantenuto ogni connessione per 5 secondi, SumConnectionTime sarà uguale a 50 secondi.|  
|AvgTimeOpened|È uguale a SumConnectionsOpened/SumConnectionTime.|  
|**Statistiche di rete:**||  
|ServerRndTrips|Numero di volte in cui il driver ha inviato comandi al server e ottenuto una risposta.|  
|BuffersSent|Numero di pacchetti di flussi TDS (Tabular Data Stream) inviati a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dal driver dopo SQL_PERF_START. I comandi di grandi dimensioni possono accettare più buffer, pertanto se un comando di grandi dimensioni viene inviato al server e riempie sei pacchetti, ServerRndTrips viene incrementato di uno e BuffersSent viene incrementato di sei.|  
|BuffersRec|Numero di pacchetti TDS ricevuti dal driver da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dopo l'avvio dell'applicazione mediante il driver.|  
|BytesSent|Numero di byte di dati inviati a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nei pacchetti TDS dopo l'avvio dell'applicazione mediante il driver.|  
|BytesRec|Numero di byte di dati nei pacchetti TDS ricevuti dal driver da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dopo l'avvio dell'applicazione mediante il driver.|  
  
### <a name="time-statistics"></a>Statistiche temporali  
  
|Campo SQLPERF|Descrizione|  
|-------------------|-----------------|  
|msExecutionTime|Tempo cumulativo impiegato dal driver per l'elaborazione dopo SQL_PERF_START, incluso il tempo di attesa per le risposte dal server.|  
|msNetworkServerTime|Tempo cumulativo di attesa del driver per le risposte dal server.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Procedure per la profilatura delle prestazioni del driver ODBC &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/profiling-odbc-driver-performance-odbc.md)  
  
