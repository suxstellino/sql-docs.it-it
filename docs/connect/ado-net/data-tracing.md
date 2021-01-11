---
title: Traccia dei dati in SqlClient
description: L’articolo descrive il modo in cui il provider di dati Microsoft SqlClient per SQL Server offre funzionalità di traccia dei dati incorporate.
ms.date: 12/04/2020
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fc8b5a7ca06af3c3e3ea83fcb747f79517e5ef7a
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744435"
---
# <a name="data-tracing-in-sqlclient"></a>Traccia dei dati in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il linguaggio .NET offre la funzionalità di traccia dei dati incorporata che è supportata dal provider di dati Microsoft SqlClient per SQL Server e dai protocolli di rete SQL Server.

L'analisi delle chiamate API di accesso ai dati consente di diagnosticare i seguenti problemi:

- Mancata corrispondenza di schema tra il programma client e il database.

- Database non disponibile o problemi nella libreria di rete.

- Sintassi SQL non corretta definita a livello di codice o generata da un'applicazione.

- Logica di programmazione non corretta.

- Problemi derivanti dall'interazione tra il provider di dati Microsoft SqlClient per SQL Server e i propri componenti.

Per supportare tecnologie di traccia diverse, questa funzionalità è estensibile, pertanto uno sviluppatore può tracciare un problema a qualsiasi livello dello stack dell'applicazione. Il provider di dati Microsoft SqlClient per SQL Server sfrutta le API di analisi e strumentazione generalizzate.

Per altre informazioni sull'impostazione e la configurazione dell'analisi gestita in .NET, vedere [Accesso ai dati di traccia](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

## <a name="access-diagnostic-information-in-the-extended-events-log"></a>Accedere alle informazioni di diagnostica nel registro eventi estesi

Nel provider di dati Microsoft SqlClient per SQL Server, la [Traccia dell’accesso ai dati](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)) rende più semplice correlare gli eventi client con le informazioni diagnostiche, ad esempio gli errori di connessione, dal buffer circolare di connettività del server e le informazioni sulle prestazioni delle applicazioni nel registro eventi estesi. Per ulteriori informazioni sulla lettura del log degli eventi estesi, vedere [View Event Session Data](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110)) (Visualizzare i dati di una sessione di eventi).

Per le operazioni di connessione, il provider di dati Microsoft SqlClient per SQL Server invierà un ID di connessione client. In caso di errore di connessione, è possibile accedere al buffer circolare della connettività ([Risoluzione dei problemi relativi alla connettività in SQL Server 2008 con il buffer circolare della connettività](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)), individuare il campo `ClientConnectionID` e ottenere le informazioni di diagnostica sul problema di connessione. Gli ID di connessione client vengono registrati nel buffer circolare solo se si verifica un errore. In caso di errore di connessione prima dell'invio del pacchetto di preaccesso, l'ID di connessione client non verrà generato. L'ID di connessione client è un GUID a 16 byte. È anche possibile trovare l'ID di connessione client nell'output di destinazione di eventi estesi, se l'azione `client_connection_id` viene aggiunta agli eventi in una sessione di eventi estesi. È possibile abilitare l'analisi di accesso ai dati ed eseguire di nuovo il comando di connessione e osservare il campo `ClientConnectionID` nell'analisi di accesso ai dati, se si necessita di ulteriore assistenza per la diagnostica dei driver del client.

È possibile ottenere l'ID di connessione cliente a livello di codice usando la proprietà `SqlConnection.ClientConnectionID`.

> [!NOTE]
> Il provider di dati Microsoft SqlClient per SQL Server supporta l'ID di processo server a partire dalla versione 2.1.0. Questo ID può essere ottenuto a livello di codice usando la proprietà `SqlConnection.ServerProcessId`.

Gli elementi `ClientConnectionID` e `ServerProcessId` sono disponibili per un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> che stabilisce correttamente una connessione. Se un tentativo di connessione non riesce, `ClientConnectionID` può essere disponibile tramite `SqlException.ToString`.

Il provider di dati Microsoft SqlClient per SQL Server invia anche un ID attività specifico del thread. L'ID attività viene acquisito nelle sessioni di eventi estesi se le sessioni sono avviate con l'opzione TRACK_CAUSALITY abilitata. Per problemi di prestazioni con una connessione attiva, è possibile ottenere l'ID attività dalla traccia di accesso ai dati del client (campo`ActivityID`) e individuare quindi l'ID attività nell'output degli eventi estesi. L'ID attività negli eventi non estesi è un GUID a 16 byte, diverso dal GUID per l'ID connessione client, aggiunto con un numero di sequenza a 4 byte. Il numero di sequenza rappresenta l'ordine di una richiesta all'interno di un thread e indica l'ordine relativo di istruzioni batch e RPC per il thread. `ActivityID` viene attualmente inviato facoltativamente per le istruzioni batch SQL e le richieste RPC quando è abilitata l'analisi di accesso ai dati e il diciottesimo bit della parola di configurazione dell'analisi di accesso ai dati è ON.

Di seguito è riportato un esempio di istruzione SQL che usa Transact-SQL per avviare una sessione di eventi estesi che verrà archiviata in un buffer circolare e registrerà l'ID attività inviato da un client in operazioni RPC e batch.

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
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
