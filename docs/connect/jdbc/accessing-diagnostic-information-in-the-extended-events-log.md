---
title: Accesso alle informazioni di diagnostica nel log degli eventi estesi
description: Informazioni su come accedere agli eventi estesi nel server correlati a eventi da Microsoft JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a79e9468-2257-4536-91f1-73b008c376c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b42d175fcc9a3cf10563b647756e8619d45b5dd3
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633256"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accesso alle informazioni di diagnostica nel log degli eventi estesi

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In la [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] traccia ([operazione del driver di traccia](tracing-driver-operation.md)) semplifica la correlazione degli eventi client con le informazioni di diagnostica. È possibile tracciare elementi come gli errori di connessione dal buffer circolare della connettività del server e le informazioni sulle prestazioni dell'applicazione nel log degli eventi estesi. Per informazioni sulla lettura del registro eventi esteso, vedere [Eventi estesi](../../relational-databases/extended-events/extended-events.md).

## <a name="details"></a>Dettagli

Per operazioni di connessione, tramite [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] verrà inviato un ID connessione client. Se la connessione non riesce, è possibile accedere al buffer circolare della connettività e trovare il campo **ClientConnectionID** per ottenere informazioni di diagnostica sull'errore. Per ulteriori informazioni sul buffer circolare, vedere la pagina relativa alla [risoluzione dei problemi di connettività in SQL Server 2008 con il buffer circolare della connettività](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer). Gli ID di connessione client vengono registrati nel buffer circolare solo se si verifica un errore. Se una connessione non riesce prima di inviare il pacchetto di preaccesso, non verrà generato un ID di connessione client.

L'ID di connessione client è un GUID a 16 byte. Se l'azione **client_connection_id** viene aggiunta agli eventi in una sessione di eventi estesi, l'ID connessione client sarà nell'output di destinazione degli eventi estesi. Per altre funzionalità di diagnostica dei driver client, è possibile abilitare la traccia ed eseguire di nuovo il comando di connessione per visualizzare il campo **ClientConnectionID** nella traccia.

È possibile ottenere l'ID di connessione client a livello di codice usando l'[interfaccia ISQLServerConnection](reference/isqlserverconnection-interface.md). L'ID connessione sarà inoltre disponibile in tutte le eccezioni correlate alla connessione.

Quando si verifica un errore di connessione, con l'ID connessione client nelle informazioni relative alla traccia Built-In Diagnostics (BID) del server e nel buffer circolare è possibile semplificare la correlazione delle connessioni client alle connessioni nel server. Per altre informazioni sulle tracce BID nel server, vedere la pagina relativa alla [traccia di accesso ai dati](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Si noti che nell'articolo sulla traccia di accesso ai dati sono inoltre contenute informazioni su una traccia di accesso ai dati che non si applica a [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Vedere [Creazione di tracce](tracing-driver-operation.md) per informazioni sull'esecuzione di una traccia di accesso ai dati usando [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

Il driver JDBC invia anche un ID attività specifico del thread. Se la sessione viene avviata con l'opzione TRACK_CAUSAILITY abilitata, l'ID attività viene acquisito nella sessione eventi estesi. Per problemi di prestazioni con una connessione attiva, è possibile ottenere l'ID attività dalla traccia del client (campo ActivityID) e quindi individuare l'ID attività nell'output degli eventi estesi.

L'ID attività negli eventi estesi è un GUID a 16 byte, non lo stesso del GUID per l'ID connessione client, aggiunto con un numero di sequenza a 4 byte. Il numero di sequenza rappresenta l'ordine di una richiesta all'interno di un thread. ActivityId viene inviato per le istruzioni batch SQL e per le richieste RPC. Per abilitare l'invio di ActivityId al server, specificare la coppia chiave-valore seguente nel file logging. Properties:

```java
com.microsoft.sqlserver.jdbc.traceactivity = on
```

Qualsiasi valore diverso da `on` (con distinzione tra maiuscole e minuscole) disabiliterà l'invio di ActivityId.

Per altre informazioni, vedere [Creazione di tracce](tracing-driver-operation.md). Questo flag di traccia viene utilizzato con i logger degli oggetti JDBC corrispondenti per stabilire se tracciare e inviare ActivityId nel driver JDBC. Oltre ad aggiornare il file logging. Properties, abilitare il logger com. Microsoft. SqlServer. JDBC a una posizione più fine o superiore. Per inviare ActivityId al server per le richieste eseguite da una determinata classe, abilitare il logger di classe corrispondente in modo più preciso o migliore. Se ad esempio la classe è SQLServerStatement, abilitare il logger com.microsoft.sqlserver.jdbc.SQLServerStatement.

Nell'esempio seguente viene usato [!INCLUDE[tsql](../../includes/tsql-md.md)] per avviare una sessione di eventi estesi archiviata in un buffer circolare e viene registrato l'ID attività inviato da un client nelle operazioni RPC e batch:

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>Vedere anche

[Diagnosi dei problemi relativi al driver JDBC](diagnosing-problems-with-the-jdbc-driver.md)
