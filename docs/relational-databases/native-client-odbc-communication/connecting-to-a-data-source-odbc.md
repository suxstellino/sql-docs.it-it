---
description: Connessione a un'origine dati (ODBC)
title: Connessione a un'origine dati (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- checking connection states
- ODBC data sources, connections
- data sources [SQL Server Native Client]
- SQLBrowseConnect function
- ODBC applications, connections
- ODBC applications, data sources
- connections [SQL Server Native Client]
- SQLConnect function
- SQLDriveConnect function
- verifying connection states
- SQL Server Native Client ODBC driver, data sources
- SQL Server Native Client ODBC driver, connections
ms.assetid: ae30dd1d-06ae-452b-9618-8fd8cd7ba074
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 97bcd6e94d77f6c4fe76426ac25d6d21c60282b3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756361"
---
# <a name="connecting-to-a-data-source-odbc"></a>Connessione a un'origine dati (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dopo avere allocato handle di ambiente e di connessione e avere impostato tutti gli attributi di connessione, l'applicazione si connette all'origine dati o al driver. È possibile utilizzare tre funzioni per connettersi:  
  
-   **SQLConnect**  
  
-   **SQLDriverConnect**  
  
-   **SQLBrowseConnect**  
  
 Per ulteriori informazioni sull'esecuzione di connessioni a un'origine dati, incluse le varie opzioni della stringa di connessione disponibili, vedere [utilizzo delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
## <a name="sqlconnect"></a>SQLConnect  
 **SQLConnect** è la funzione di connessione più semplice. La funzione accetta tre parametri: un nome di origine dati, un ID utente e una password. Utilizzare **SQLConnect** quando questi tre parametri contengono tutte le informazioni necessarie per connettersi al database. A tale scopo, compilare un elenco di origini dati utilizzando **SQLDataSources**; Richiedi all'utente un'origine dati, un ID utente e una password. e quindi chiamare **SQLConnect**.  
  
 **SQLConnect** presuppone che il nome dell'origine dati, l'ID utente e la password siano sufficienti per connettersi a un'origine dati e che l'origine dati ODBC contenga tutte le altre informazioni necessarie al driver ODBC per stabilire la connessione. A differenza di [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) e [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md), **SQLConnect** non usa una stringa di connessione.  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
 **SQLDriverConnect** viene utilizzato quando sono necessarie più informazioni rispetto al nome dell'origine dati, all'ID utente e alla password. Uno dei parametri di **SQLDriverConnect** è una stringa di connessione contenente informazioni specifiche del driver. È possibile utilizzare **SQLDriverConnect** anziché **SQLConnect** per i motivi seguenti:  
  
-   Per immettere informazioni specifiche del driver in fase di connessione.  
  
-   Per specificare che il driver deve richiedere all'utente le informazioni di connessione.  
  
-   Per connettersi senza utilizzare un'origine dati ODBC.  
  
 La stringa di connessione **SQLDriverConnect** contiene una serie di coppie parola chiave-valore che specificano tutte le informazioni di connessione supportate da un driver ODBC. Ogni driver supporta le parole chiave ODBC standard (DSN, FILEDSN, DRIVER, UID, PWD e SAVEFILE) oltre a parole chiave specifiche del driver per tutte le informazioni di connessione supportate dal driver stesso. **SQLDriverConnect** può essere usato per connettersi senza un'origine dati. Ad esempio, un'applicazione progettata per effettuare una connessione "senza DSN" a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può chiamare **SQLDriverConnect** con una stringa di connessione che definisce l'ID di accesso, la password, la libreria di rete, il nome del server a cui connettersi e il database predefinito da utilizzare.  
  
 Quando si usa **SQLDriverConnect**, sono disponibili due opzioni per richiedere all'utente le informazioni di connessione necessarie:  
  
-   Finestra di dialogo dell'applicazione  
  
     È possibile creare una finestra di dialogo dell'applicazione in cui vengono richieste le informazioni di connessione e quindi viene chiamato **SQLDriverConnect** con un handle di finestra null e *DriverCompletion* impostato su SQL_DRIVER_NOPROMPT. Queste impostazioni impediscono di aprire la finestra di dialogo del driver ODBC. Questo metodo viene utilizzato quando è importante controllare l'interfaccia utente dell'applicazione.  
  
-   Finestra di dialogo del driver  
  
     È possibile codificare l'applicazione per passare un handle di finestra valido a **SQLDriverConnect** e impostare il parametro *DriverCompletion* su SQL_DRIVER_COMPLETE, SQL_DRIVER_PROMPT o SQL_DRIVER_COMPLETE_REQUIRED. Il driver genera quindi una finestra di dialogo per richiedere all'utente le informazioni di connessione. Questo metodo semplifica il codice dell'applicazione.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
 **SQLBrowseConnect**, ad esempio **SQLDriverConnect**, usa una stringa di connessione. Tuttavia, usando **SQLBrowseConnect**, un'applicazione può creare una stringa di connessione completa in modo iterativo con l'origine dati in fase di esecuzione. In questo modo, l'applicazione può eseguire due operazioni:  
  
-   Compilare finestre di dialogo proprie per richiedere tali informazioni, mantenendo in tal modo il controllo sull'interfaccia utente.  
  
-   Esplorare il sistema per individuare origini dati che possano essere utilizzate da un driver specifico, possibilmente in diversi passaggi.  
  
     L'utente, ad esempio, potrebbe esplorare innanzitutto la rete per individuare i server e, dopo averne scelto uno, esplorare il server per individuare i database accessibili dal driver.  
  
 Quando **SQLBrowseConnect** completa una connessione, restituisce una stringa di connessione che può essere usata nelle chiamate successive a **SQLDriverConnect**.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client restituisce sempre SQL_SUCCESS_WITH_INFO in un oggetto **SQLConnect**, **SQLDriverConnect** o **SQLBrowseConnect** riuscito. Quando un'applicazione ODBC chiama **SQLGetDiagRec** dopo avere SQL_SUCCESS_WITH_INFO, può ricevere i messaggi seguenti:  
  
 5701  
 Indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha inserito il contesto dell'utente nel database predefinito specificato nell'origine dati o nel database predefinito specificato per l'ID di accesso utilizzato nella connessione se l'origine dati non dispone di un database predefinito.  
  
 5703  
 Indica la lingua utilizzata nel server.  
  
 Nell'esempio seguente viene riportato il messaggio restituito in una connessione eseguita correttamente dall'amministratore di sistema:  
  
```  
szSqlState = "01000", *pfNativeError = 5701,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed database context to 'pubs'."  
szSqlState = "01000", *pfNativeError = 5703,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed language setting to 'us_english'."  
```  
  
 È possibile ignorare i messaggi 5701 e 5703, in quanto di natura esclusivamente informativa. Non è consigliabile, tuttavia, ignorare un codice restituito di SQL_SUCCESS_WITH_INFO, in quanto è possibile che vengano generati messaggi diversi da 5701 o 5703. Se, ad esempio, un driver si connette a un server che esegue un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con le stored procedure di catalogo obsolete, uno degli errori restituiti tramite **SQLGetDiagRec** dopo un SQL_SUCCESS_WITH_INFO è il seguente:  
  
```  
SqlState:   01000  
pfNative:   0  
szErrorMsg: "[Microsoft][SQL Server Native Client]The ODBC  
            catalog stored procedures installed on server  
            my65server are version 06.50.0193; version 07.00.0205  
            or later is required to ensure proper operation.  
            Please contact your system administrator."  
```  
  
 La funzione di gestione degli errori di un'applicazione per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connessioni deve chiamare **SQLGetDiagRec** fino a quando non restituisce SQL_NO_DATA. Deve quindi agire su tutti i messaggi diversi da quelli con codice *pfNative* 5701 o 5703.  
  
## <a name="see-also"></a>Vedere anche  
 [Comunicazione con SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
