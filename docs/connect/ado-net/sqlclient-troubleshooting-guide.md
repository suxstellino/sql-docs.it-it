---
title: Guida alla risoluzione dei problemi di SqlClient
description: Pagina in cui sono riportate le soluzioni ai problemi più comuni.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 725d1522f98e721f581a7d4b7614181fd2632962
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464804"
---
# <a name="sqlclient-troubleshooting-guide"></a>Guida alla risoluzione dei problemi di SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>Eccezioni durante la connessione a SQL Server

Esistono diversi motivi per cui non è possibile stabilire la connessione. Di seguito sono riportati alcuni suggerimenti che possono essere usati come guida per analizzare e risolvere molti dei problemi.

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>Impossibile caricare la libreria SNI (Server Name Indication) nativa

#### <a name="issues-in-net-framework-applications"></a>Problemi delle applicazioni .NET Framework

L'analisi dello stack ha rilevato quanto segue:

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI è la libreria nativa di C++ da cui SqlClient dipende per varie operazioni di rete durante l'esecuzione in Windows. Nelle applicazioni .NET Framework compilate con l'SDK del progetto MSBuild, le DLL native non vengono gestite con i comandi di ripristino. Quindi un file ".targets" viene incluso nel pacchetto NuGet "Microsoft.Data.SqlClient.SNI" che definisce le operazioni di copia necessarie.

Se esiste una dipendenza diretta dalla libreria "Microsoft.Data.SqlClient", viene fatto automaticamente riferimento al file ".targets" incluso. Negli scenari in cui esiste un riferimento transitivo (indiretto), è necessario fare riferimento al file ".targets" manualmente per garantire che le operazioni di copia possano essere eseguite quando necessario.

**Soluzione consigliata:** Verificare che si faccia riferimento al file ".targets" nel file ".csproj" dell'applicazione per garantire che vengano eseguite le operazioni di copia.

Queste destinazioni coprono solo le destinazioni note e comunemente usate da Microsoft. Se in uno strumento o un'applicazione esterni vengono definite destinazioni personalizzate per la copia dei file binari, i gestori degli strumenti devono definire nuove destinazioni per assicurarsi che le DLL SNI native vengano copiate insieme ai file binari di Microsoft.Data.SqlClient.dll e siano disponibili quando si eseguono le applicazioni client.

#### <a name="issues-in-net-core-applications"></a>Problemi nelle applicazioni .NET Core

L'analisi dello stack ha rilevato quanto segue:

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> Questo errore può verificarsi solo nelle applicazioni Windows. Se si verifica in un ambiente UNIX, è necessario verificare che l'applicazione sia compilata in modo da usare correttamente come destinazione un runtime UNIX e non per Windows.

SNI è la libreria nativa di C++ da cui SqlClient dipende per varie operazioni di rete durante l'esecuzione in Windows. Microsoft.Data.SqlClient non gestisce il caricamento e lo scaricamento di questa libreria in .NET Core.

**Soluzione consigliata:** Verificare che le autorizzazioni di esecuzione vengano concesse nel file system in cui le librerie di runtime native vengono caricate nel processo .NET Core. Se il problema persiste, è possibile registrare un problema nel repository [dotnet/runtime](https://github.com/dotnet/runtime) per ricevere ulteriore assistenza.

#### <a name="native-sni-pdb-not-found-errors"></a>Errori SNI nativo (PDB non trovato)

L'analisi dello stack ha rilevato quanto segue:

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

**Soluzione consigliata:** Assicurarsi che l'applicazione client faccia riferimento alla versione minima [v 2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0) del pacchetto Microsoft.Data.SqlClient. Quando si usa EF Core, aggiungere direttamente un riferimento a questa versione pacchetto di Microsoft.Data.SqlClient per eseguire l'override della dipendenza.

### <a name="hostname-resolution-errors"></a>Errori di risoluzione del nome host

L'analisi dello stack ha rilevato quanto segue:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>Motivi possibili

- Il protocollo TCP/Named Pipe non è abilitato in SQL Server

  **Soluzione consigliata:** Abilitare il protocollo TCP/Named Pipe nell'istanza di SQL Server dalla console di Gestione configurazione SQL Server.

- Nome host sconosciuto

  **Soluzione consigliata:** Verificare che il nome host venga risolto nell'indirizzo IP del server dal client in cui viene avviata la connessione.


### <a name="login-phase-errors"></a>Errori della fase di accesso

L'analisi dello stack ha rilevato quanto segue:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>Possibili motivi e soluzioni

- SQL Server non supporta TLS 1.2

  Questo errore si verifica in genere in ambienti client come i contenitori di immagini Docker, i client UNIX o i client Windows in cui TLS 1.2 è il protocollo TLS minimo supportato.

  **Soluzione consigliata:** Installare gli aggiornamenti più recenti nelle versioni supportate di SQL Server <sup>1</sup> e verificare che il protocollo TLS 1.2 sia abilitato nel server.

  _<sup>1</sup> Consultare il [ciclo di vita del supporto per il driver SqlClient](sqlclient-driver-support-lifecycle.md) per l'elenco delle versioni di SQL Server supportate con versioni diverse di Microsoft.Data.SqlClient._

  **Soluzione non sicura:** Configurare le impostazioni TLS/SSL nell'ambiente client/immagine Docker per la connessione con TLS 1.0.

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > Quando si effettua la connessione a Microsoft.Data.SqlClient v 2.0 e successive da un ambiente Windows/Linux con TLS 1.0 o TLS 1.1, viene generato un avviso di sicurezza se l'istanza di SQL Server di destinazione e il client non riescono a negoziare un minimo di TLS versione 1.2 quando si stabilisce la connessione: `Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- Crittografia applicata da SQL Server

  Se il server di destinazione è un'istanza di Azure SQL o un'istanza locale di SQL Server con la proprietà "Forza crittografia" attivata, verrà stabilita una connessione crittografata, per cui il client deve stabilire una relazione di trust con il server.

  **Soluzione consigliata:** Per correggere questo problema sono disponibili due opzioni:

    1. Installare il certificato TLS/SSL dell'istanza di SQL Server di destinazione nell'ambiente client. Verrà convalidato se è necessaria la crittografia.
    2. Impostare la proprietà "Trust Server Certificate = true" nella stringa di connessione.

  **Soluzione non sicura:** Disabilitare l'impostazione "Forza crittografia" in SQL Server.

- I certificati TLS/SSL non sono firmati con SHA-256 o versione successiva.

  **Soluzione consigliata:** Generare un nuovo certificato TLS/SSL per il server il cui hash è firmato con almeno l'algoritmo hash SHA-256.

- Pacchetti di crittografia strettamente limitati in Linux con .NET 5 +

  In .NET 5 è stata introdotta una modifica sostanziale per i client Linux, in cui viene usato per impostazione predefinita un elenco rigorosamente limitato di pacchetti di crittografia consentiti. Potrebbe essere necessario espandere l'elenco predefinito dei pacchetti di crittografia per accettare i client legacy (o per contattare i server legacy) specificando un `CipherSuitePolicy` valore o modificando il file di configurazione _openssl_ .
  
  Per un'azione consigliata, vedere la pagina relativa ai pacchetti [di crittografia TLS predefiniti per .NET in Linux ](/dotnet/core/compatibility/cryptography/5.0/default-cipher-suites-for-tls-on-linux) .

### <a name="connection-pool-exhaustion-errors"></a>Errori di esaurimento del pool di connessione

L'analisi dello stack ha rilevato quanto segue:

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>Possibili motivi e soluzioni

L'applicazione client sta aprendo un numero di connessioni superiore a quello che il pool di connessioni può mantenere attivo in un determinato momento.

**Soluzione consigliata:** Configurare la proprietà di connessione "Dimensioni massime pool" su un valore più alto e chiudere al più presto le connessioni non in uso.

## <a name="contact-support"></a>Contattare il supporto tecnico

Se questa guida non risolve i problemi di connettività, è possibile visualizzare i problemi esistenti nel repository [dotnet/sqlclient](https://github.com/dotnet/SqlClient) e aprire un nuovo problema, se necessario.
