---
description: Recupero di dati dei risultati
title: Recupero dei dati dei risultati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], fetching
- SQLBindCol function
- result sets [ODBC], fetching
- fetching [ODBC]
- ODBC data types, fetching
- SQLFetch function
- SQL Server Native Client ODBC driver, data types
- SQLGetData function
ms.assetid: b289c7fb-5017-4d7e-a2d3-19401e9fc4cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f151fefdb895259720904b9eda81719be1703bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752781"
---
# <a name="fetching-result-data"></a>Recupero di dati dei risultati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Un'applicazione ODBC dispone di tre opzioni per il recupero dei dati dei risultati.  
  
 La prima opzione è basata su [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md). Prima di recuperare il set di risultati, l'applicazione utilizza **SQLBindCol** per associare ogni colonna del set di risultati a una variabile di programma. Dopo aver associato le colonne, il driver trasferisce i dati della riga corrente alle variabili associato alle colonne del set di risultati ogni volta che l'applicazione chiama **SQLFetch** o [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Il driver gestisce le conversioni di dati se la colonna del set di risultati e la variabile di programma hanno tipi di dati diversi. Se l'applicazione dispone di SQL_ATTR_ROW_ARRAY_SIZE impostato su un valore maggiore di 1, può associare le colonne dei risultati a matrici di variabili, che verranno tutte riempite a ogni chiamata a **SQLFetchScroll**.  
  
 La seconda opzione è basata su [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md). L'applicazione non utilizza **SQLBindCol** per associare le colonne del set di risultati alle variabili di programma. Dopo ogni chiamata a **SQLFetch**, l'applicazione chiama **SQLGetData** una volta per ogni colonna del set di risultati. **SQLGetData** indica al driver di trasferire i dati da una colonna del set di risultati specifica a una variabile di programma specifica e specifica i tipi di dati della colonna e della variabile. In questo modo il driver può convertire dati se la colonna dei risultati e la variabile di programma hanno tipi di dati diversi. Le colonne di tipo **Text**, **ntext** e **Image** sono in genere troppo grandi per adattarsi a una variabile di programma, ma possono comunque essere recuperate tramite **SQLGetData**. Se i dati di tipo **Text**, **ntext** o **Image** nella colonna risultato sono maggiori della variabile di programma, **SQLGetData** restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01004 (dati stringa, troncati a destra). Le chiamate successive a **SQLGetData** restituiscono blocchi successivi dei dati di **testo** o di **immagine** . Quando viene raggiunta la fine dei dati, **SQLGetData** restituisce SQL_SUCCESS. Ciascun recupero restituisce un set di righe se SQL_ATTR_ROW_ARRAY_SIZE è maggiore di 1. Prima di utilizzare **SQLGetData**, è necessario innanzitutto utilizzare **SQLSetPos** per specificare una riga specifica all'interno del set di righe come riga corrente.  
  
 La terza opzione prevede l'uso di una combinazione di **SQLBindCol** e **SQLGetData**. Un'applicazione potrebbe, ad esempio, associare le prime dieci colonne di un set di risultati e quindi, a ogni recupero, chiamare **SQLGetData** tre volte per recuperare i dati da tre colonne non associate. Questa operazione viene in genere utilizzata quando un set di risultati contiene una o più colonne di **testo** o di **immagine** .  
  
 A seconda delle opzioni del cursore impostate per il set di risultati, un'applicazione può utilizzare anche le opzioni di scorrimento di **SQLFetchScroll** per scorrere intorno al set di risultati.  
  
 Un utilizzo eccessivo di **SQLBindCol** per associare una colonna del set di risultati a una variabile di programma è costoso perché **SQLBindCol** comporta l'allocazione della memoria da un driver ODBC. Quando si associa una colonna risultato a una variabile, tale binding rimane attivo fino a quando non si chiama [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) per liberare l'handle di istruzione oppure si chiama [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) con *fOption* impostato su SQL_UNBIND. Le associazioni non vengono annullate automaticamente al termine dell'istruzione.  
  
 Questa logica consente di gestire in modo efficace l'esecuzione della stessa istruzione SELECT più volte con parametri diversi. Poiché il set di risultati mantiene la stessa struttura, è possibile associare il set di risultati una sola volta, elaborare tutte le istruzioni SELECT, quindi chiamare **SQLFreeStmt** con *fOption* impostato su SQL_UNBIND dopo l'ultima esecuzione. Non chiamare **SQLBindCol** per associare le colonne in un set di risultati senza prima chiamare **SQLFreeStmt** con *fOption* impostato su SQL_UNBIND per liberare le associazioni precedenti.  
  
 Quando si usa **SQLBindCol**, è possibile eseguire l'associazione per riga o per colonna. L'associazione per riga è leggermente più veloce dell'associazione per colonna.  
  
 È possibile utilizzare **SQLGetData** per recuperare i dati in base a una colonna, anziché associare le colonne del set di risultati utilizzando **SQLBindCol**. Se un set di risultati contiene solo poche righe, l'utilizzo di **SQLGetData** invece di **SQLBindCol** è più veloce; in caso contrario, **SQLBindCol** offre le migliori prestazioni. Se non si inseriscono sempre i dati nello stesso set di variabili, è consigliabile utilizzare **SQLGetData** anziché riassociarlo costantemente. È possibile utilizzare **SQLGetData** solo nelle colonne presenti nell'elenco di selezione dopo che tutte le colonne sono associate a **SQLBindCol**. La colonna deve essere visualizzata anche dopo tutte le colonne in cui è già stato utilizzato **SQLGetData**.  
  
 Le funzioni ODBC che gestiscono lo stato di trasferimento dei dati all'interno o all'esterno delle variabili di programma, ad esempio **SQLGetData**, **SQLBindCol** e [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), supportano la conversione implicita dei tipi di dati. Se, ad esempio, un'applicazione associa una colonna integer a una variabile di programma stringa di caratteri, il driver converte automaticamente i dati da integer in carattere prima di inserirli nella variabile di programma.  
  
 La conversione dei dati nelle applicazioni deve essere ridotta al minimo. A meno che la conversione dei dati non sia necessaria per l'elaborazione eseguita dall'applicazione, è preferibile che le applicazioni non associno colonne e parametri a variabili di programma dello stesso tipo di dati. Se i dati devono essere convertiti da un tipo a un altro, tuttavia, è più efficiente fare in modo che il driver esegua la conversione anziché lasciare che la conversione venga eseguita nell'applicazione. Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client trasferisce in genere i dati direttamente dai buffer di rete alle variabili dell'applicazione. La richiesta al driver di convertire i dati forza il driver a eseguire il buffer dei dati e a utilizzare cicli della CPU per la conversione dei dati.  
  
 Le variabili di programma devono essere sufficientemente grandi da contenere i dati trasferiti da una colonna, ad eccezione dei dati di tipo **Text**, **ntext** e **Image** . Se un'applicazione tenta di recuperare dati del set di risultati e di inserirli in una variabile di dimensioni troppo piccole per contenerli, il driver genera un avviso. Ciò forza il driver ad allocare memoria per il messaggio e il driver e l'applicazione devono entrambi impiegare cicli della CPU per l'elaborazione del messaggio e per la gestione degli errori. L'applicazione deve allocare una variabile sufficientemente grande per contenere i dati recuperati oppure utilizzare la funzione SUBSTRING nell'elenco di selezione per ridurre le dimensioni della colonna nel set di risultati.  
  
 Quando si utilizza SQL_C_DEFAULT per specificare il tipo della variabile C, è necessario procedere con cautela. SQL_C_DEFAULT specifica che il tipo della variabile C corrisponde al tipo di dati SQL della colonna o del parametro. Se SQL_C_DEFAULT viene specificato per una colonna **ntext**, **nchar** o **nvarchar** , i dati Unicode vengono restituiti all'applicazione. Ciò può provocare diversi problemi se l'applicazione non è stata codificata per la gestione di dati Unicode. È possibile che si verifichino gli stessi tipi di problemi con il tipo di dati **uniqueidentifier** (SQL_GUID).  
  
 i dati di tipo **Text**, **ntext** e **Image** sono in genere troppo grandi per essere contenuti in una singola variabile di programma e vengono in genere elaborati con **SQLGetData** anziché con **SQLBindCol**. Quando si utilizzano i cursori server, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client è ottimizzato per non trasmettere i dati per le colonne di tipo **Text**, **ntext** o **Image** non associato al momento del recupero della riga. I dati di tipo **Text**, **ntext** o **Image** non vengono effettivamente recuperati dal server fino a quando l'applicazione non rilascia **SQLGetData** per la colonna.  
  
 Questa ottimizzazione può essere applicata alle applicazioni in modo che non vengano visualizzati dati di tipo **Text**, **ntext** o **Image** quando un utente scorre verso l'alto e verso il basso di un cursore. Dopo che l'utente ha selezionato una riga, l'applicazione può chiamare **SQLGetData** per recuperare i dati di tipo **Text**, **ntext** o **Image** . In questo modo viene salvata la trasmissione dei dati di tipo **Text**, **ntext** o **Image** per tutte le righe che l'utente non seleziona e che può salvare la trasmissione di quantità molto elevate di dati.  
  
## <a name="see-also"></a>Vedere anche  
 [Elaborazione dei risultati &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
