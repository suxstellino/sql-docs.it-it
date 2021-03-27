---
title: Mirroring del database in SQL Server
description: Informazioni sul mirroring del database e su come considerare gli eventi di failover nell'applicazione ADO.NET durante lo sviluppo.
ms.date: 09/30/2019
dev_langs:
- csharp
ms.assetid: 89befaff-bb46-4290-8382-e67cdb0e3de9
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: afa10c2c657ba2f67d6a3521768a364e89f7268b
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633230"
---
# <a name="database-mirroring-in-sql-server"></a>Mirroring del database in SQL Server

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Il mirroring del database in SQL Server consente di mantenere una copia, o mirror, di un database di SQL Server in un server di standby. Il mirroring garantisce che siano sempre presenti due copie separate dei dati, garantendo disponibilità elevata e ridondanza dei dati completa. Il provider SqlClient Microsoft per SQL Server fornisce supporto implicito per il mirroring del database. Lo sviluppatore non deve eseguire alcuna operazione una volta che il client è stato configurato per un database SQL Server. Inoltre, l' <xref:Microsoft.Data.SqlClient.SqlConnection> oggetto supporta una modalità di connessione esplicita che consente di fornire il nome di un server partner di failover in <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> .

Questa sequenza di eventi semplificata si verifica per un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> che ha come destinazione un database configurato per il mirroring:

1. L'applicazione client esegue la connessione al database principale e il server restituisce il nome del server partner, memorizzato nella cache del client.
2. In caso di errore del server contenente il database principale o di interruzione della connettività, lo stato della connessione e della transazione andrà perso. L'applicazione client tenta di ristabilire una connessione al database principale e non riesce.
3. L'applicazione client tenta quindi di stabilire in modo trasparente una connessione al database mirror nel server partner. Se l'esito è positivo, la connessione viene reindirizzata al database mirror che diventa quindi il nuovo database principale.

## <a name="specifying-the-failover-partner-in-the-connection-string"></a>Definizione del partner di failover nella stringa di connessione

Se si fornisce il nome di un server partner di failover nella stringa di connessione e il database principale non è disponibile quando l'applicazione client si connette, il client tenterà in modo trasparente la connessione al partner di failover.

```csharp
";Failover Partner=PartnerServerName"
```

Se si omette il nome del server partner di failover e il database principale non è disponibile quando l'applicazione client si connette per la prima volta, <xref:Microsoft.Data.SqlClient.SqlException> si verifica un errore.

Quando un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> viene aperto correttamente, il server restituisce il nome del partner di failover, che sostituisce tutti i valori specificati nella stringa di connessione.

> [!NOTE]
> Per gli scenari di mirroring del database, nella stringa di connessione è necessario specificare in modo esplicito il nome del catalogo o del database iniziale. Se il client riceve informazioni di failover su una connessione che non presenta un catalogo o un database iniziale specificato in modo esplicito, tali informazioni non vengono memorizzate nella cache e l'applicazione non tenta di eseguire il failover se si verifica un errore nel server principale. Se una stringa di connessione ha un valore per il partner di failover, ma nessun valore per il catalogo o il database iniziale, viene generata l'eccezione `InvalidArgumentException`.

## <a name="retrieving-the-current-server-name"></a>Recupero del nome del server corrente

Quando si verifica un failover, è possibile recuperare il nome del server a cui è connessa la connessione corrente utilizzando la <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> proprietà di un <xref:Microsoft.Data.SqlClient.SqlConnection> oggetto. Il frammento di codice seguente recupera il nome del server attivo, supponendo che la variabile di connessione faccia riferimento a un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> aperto.

Quando si verifica un evento di failover e la connessione passa al server mirror, la proprietà **DataSource** viene aggiornata per riflettere il nome del mirror.

```csharp
string activeServer = connection.DataSource;
```

## <a name="sqlclient-mirroring-behavior"></a>Comportamento di mirroring di SqlClient

Il client tenta sempre di connettersi al server principale. Se non riesce, prova a connettersi al partner di failover. Se il database mirror è stato già passato al ruolo principale nel server partner, la connessione riesce e il nuovo mapping ruolo principale-mirror viene inviato al client e memorizzato nella cache per la durata dell'oggetto <xref:System.AppDomain> chiamante. Non è archiviato nell'archivio permanente e non è disponibile per le connessioni future in un **AppDomain** o processo diverso. Tuttavia, è disponibile per le connessioni successive nello stesso **AppDomain**. Un altro **AppDomain** o processo in esecuzione nello stesso computer o in un computer diverso ha sempre il proprio pool di connessioni e tali connessioni non vengono reimpostate. In questo caso, se il database primario diventa inattivo, ogni processo o **AppDomain** ha esito negativo una volta e il pool viene cancellato automaticamente.

> [!NOTE]
> Il supporto del mirroring nel server viene configurato per ogni database. Se vengono eseguite operazioni di manipolazione dei dati su altri database non inclusi nel set principale/mirror, usando nomi multipart o modificando il database corrente, le modifiche apportate a questi altri database non vengono propagate in caso di errore. Quando si modificano i dati in un database senza mirroring, non viene generato alcun errore. Lo sviluppatore deve valutare il possibile impatto di queste operazioni.

## <a name="next-steps"></a>Passaggi successivi

### <a name="database-mirroring-resources"></a>Risorse per il mirroring del database

Per la documentazione concettuale e per informazioni sulla configurazione, la distribuzione e l'amministrazione del mirroring, vedere le risorse seguenti nella documentazione di SQL Server.

|Risorsa|Descrizione|
|--------------|-----------------|
|[Mirroring del database](../../../database-engine/database-mirroring/database-mirroring-sql-server.md)|Viene descritto come impostare e configurare il mirroring in SQL Server.|
