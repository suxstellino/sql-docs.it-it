---
description: Viene illustrato come utilizzare le istruzioni e i set di risultati in JDBC e come utilizzare l'oggetto appropriato per il processo.
title: Uso delle istruzioni e dei set di risultati
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cc917534-f5f8-4844-87c8-597c48b4e06d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52a99005c733ab436c1ea30f02a60ac491d77c0d
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673955"
---
# <a name="working-with-statements-and-result-sets"></a>Uso delle istruzioni e dei set di risultati

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Quando si usa [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] e gli oggetti Statement e ResultSet offerti, sono disponibili diverse tecniche per migliorare le prestazioni e l'affidabilità delle applicazioni.

## <a name="use-the-appropriate-statement-object"></a>Uso dell'oggetto istruzione appropriato

Quando si usa uno degli oggetti Statement del driver JDBC, ad esempio l'oggetto [SQLServerStatement](reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md) o [SQLServerCallableStatement](reference/sqlservercallablestatement-class.md), verificare che l'oggetto sia appropriato per il processo che si sta eseguendo.

- In assenza di parametri OUT non è necessario usare l'oggetto SQLServerCallableStatement. Usare invece l'oggetto SQLServerStatement o SQLServerPreparedStatement.  
- Se non si intende eseguire l'istruzione più di una volta o in assenza di parametri IN o OUT, non è necessario usare l'oggetto SQLServerCallableStatement o SQLServerPreparedStatement. Usare invece l'oggetto SQLServerStatement.

## <a name="use-the-appropriate-concurrency-for-resultset-objects"></a>Uso della concorrenza appropriata per gli oggetti ResultSet

Non richiedere la concorrenza aggiornabile quando si creano le istruzioni che producono i set di risultati, a meno che non si intenda aggiornare effettivamente i risultati. Il modello predefinito di cursore forward only di sola lettura è il più veloce per la lettura dei set di risultati di piccole dimensioni.

## <a name="limit-the-size-of-your-result-sets"></a>Limitazione delle dimensioni dei set di risultati

È consigliabile valutare l'opportunità di usare il metodo [setMaxRows](reference/setmaxrows-method-sqlserverstatement.md) (o la sintassi SET ROWCOUNT o SELECT TOP N SQL) per limitare il numero di righe restituite da set di risultati che potrebbero risultare di grandi dimensioni. Se si opera con set di risultati di grandi dimensioni, valutare l'opportunità di utilizzare un buffer adattivo per le risposte impostando la proprietà della stringa di connessione responseBuffering=adaptive, che corrisponde alla modalità predefinita. Questo approccio consente all'applicazione di elaborare set di risultati di grandi dimensioni senza richiedere i cursori sul lato server e di ridurre al minimo l'utilizzo della memoria dell'applicazione. Per altre informazioni, vedere [Uso del buffer adattivo](using-adaptive-buffering.md).

## <a name="use-the-appropriate-fetch-size"></a>Uso della dimensione di recupero appropriata

Per i cursori server di sola lettura, il compromesso è un round trip al server rispetto alla quantità di memoria utilizzata nel driver. Per i cursori server aggiornabili, la dimensione del recupero influenza anche la sensibilità del set di risultati alle modifiche e alla concorrenza sul server. Gli aggiornamenti alle righe nel buffer di recupero corrente non sono visibili fino all'esecuzione esplicita del metodo [refreshRow](reference/refreshrow-method-sqlserverresultset.md) o fino a quando il cursore non lascia il buffer di recupero. I buffer di recupero di grandi dimensioni offriranno prestazioni migliori (meno round trip al server), ma sono meno sensibili alle modifiche e riducono la concorrenza sul server se si utilizza CONCUR_SS_SCROLL_LOCKS (1009). Per ottenere il livello massimo di sensibilità alle modifiche, utilizzare la dimensione di recupero 1. Questa impostazione, tuttavia, comporterà un round trip al server per ogni riga recuperata.

## <a name="use-streams-for-large-in-parameters"></a>Uso dei flussi per parametri IN di grandi dimensioni

Utilizzare i flussi o i tipi di dati BLOB e CLOB che vengono materializzati in modo incrementale per gestire l'aggiornamento dei valori delle colonne di grandi dimensioni o l'invio di parametri IN di grandi dimensioni. Il driver JDBC "suddivide" questi tipi nel server in più round trip, consentendo di impostare e aggiornare i valori maggiori di quelli che rientrano nella memoria.

## <a name="see-also"></a>Vedere anche

[Uso del driver JDBC per il miglioramento di prestazioni e affidabilità](improving-performance-and-reliability-with-the-jdbc-driver.md)
