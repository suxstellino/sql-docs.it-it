---
description: Aggiornamenti posizionati (ODBC)
title: Aggiornamenti posizionati (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- SQLSetPos function
- SQLSetCursorName function
- ODBC applications, cursors
- cursors [ODBC], positioned updates
- positioned updates [ODBC]
- ODBC cursors, positioned updates
ms.assetid: ff404e02-630f-474d-b5d4-06442b756991
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b75d3ca4e493804325680954375bf3ae2850ca8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756271"
---
# <a name="positioned-updates-odbc"></a>Aggiornamenti posizionati (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In ODBC sono supportati due metodi per eseguire gli aggiornamenti posizionati in un cursore:  
  
-   **SQLSetPos**  
  
-   Clausola WHERE CURRENT OF  
  
 L'approccio più comune consiste nell'usare **SQLSetPos**. che dispone delle opzioni seguenti.  
  
 SQL_POSITION  
 Posiziona il cursore in una riga specifica del set di righe corrente.  
  
 SQL_REFRESH  
 Aggiorna le variabili di programma associate alle colonne del set di risultati in base ai valori della riga su cui è posizionato il cursore.  
  
 SQL_UPDATE  
 Aggiorna la riga corrente del cursore con i valori archiviati nelle variabili di programma associate alle colonne del set di risultati.  
  
 SQL_DELETE  
 Elimina la riga corrente dal cursore.  
  
 **SQLSetPos** può essere utilizzato con qualsiasi set di risultati di istruzioni quando gli attributi del cursore dell'handle di istruzione sono impostati per utilizzare i cursori server. Le colonne del set di risultati devono essere associate a variabili di programma. Non appena l'applicazione ha recuperato una riga, chiama **SQLSetPos**(SQL_POSTION) per posizionare il cursore sulla riga. È possibile quindi chiamare SQLSetPos (SQL_DELETE) per eliminare la riga corrente o spostare i nuovi valori dei dati nelle variabili di programma associate e chiamare SQLSetPos (SQL_UPDATE) per aggiornare la riga corrente.  
  
 Le applicazioni possono aggiornare o eliminare qualsiasi riga del set di righe con **SQLSetPos**. La chiamata di **SQLSetPos** è una pratica alternativa alla costruzione e all'esecuzione di un'istruzione SQL. **SQLSetPos** opera sul set di righe corrente e può essere utilizzato solo dopo una chiamata a [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md).  
  
 Le dimensioni del set di righe vengono impostate da una chiamata a [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) con un argomento attribute di SQL_ATTR_ROW_ARRAY_SIZE. **SQLSetPos** utilizza una nuova dimensione del set di righe, ma solo dopo una chiamata a **SQLFetch** o **SQLFetchScroll**. Se, ad esempio, le dimensioni del set di righe vengono modificate, viene chiamato **SQLSetPos** e viene chiamato **SQLFetch** o **SQLFetchScroll** . La chiamata a **SQLSetPos** usa le dimensioni del set di righe precedenti, ma **SQLFetch** o **SQLFetchScroll** usa le nuove dimensioni del set di righe.  
  
 La prima riga nel set di righe è il numero di riga 1. L'argomento RowNumber in **SQLSetPos** deve identificare una riga nel set di righe; ovvero, il valore deve essere compreso tra 1 e il numero di righe recuperate più di recente. Questo valore potrebbe essere inferiore alle dimensioni del set di righe. Se RowNumber è 0, l'operazione viene applicata a ogni riga nel set di righe.  
  
 L'operazione di eliminazione di **SQLSetPos** consente all'origine dati di eliminare una o più righe selezionate di una tabella. Per eliminare righe con **SQLSetPos**, l'applicazione chiama **SQLSetPos** con Operation set su SQL_DELETE e RowNumber impostato sul numero della riga da eliminare. Se RowNumber è 0, tutte le righe nel set di righe vengono eliminate.  
  
 Quando **SQLSetPos** restituisce, la riga eliminata è la riga corrente e il relativo stato è SQL_ROW_DELETED. La riga non può essere usata in altre operazioni posizionate, ad esempio le chiamate a [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) o **SQLSetPos**.  
  
 Quando si eliminano tutte le righe del set di righe (RowNumber è uguale a 0), l'applicazione può impedire al driver di eliminare determinate righe usando la matrice dell'operazione di riga come per l'operazione di aggiornamento di **SQLSetPos**.  
  
 Ogni riga eliminata deve essere una riga che esiste nel set di risultati. Se dopo il recupero i buffer dell'applicazione risultano pieni e se è stata conservata una matrice di stato della riga, i valori in ognuna di queste posizioni delle righe non devono essere SQL_ROW_DELETED, SQL_ROW_ERROR o SQL_ROW_NOROW.  
  
 Gli aggiornamenti posizionati possono inoltre essere eseguiti utilizzando la clausola WHERE CURRENT OF nelle istruzioni UPDATE, DELETE e INSERT. LADDOVE CURRENT OF richiede un nome di cursore che verrà generato da ODBC quando viene chiamata la funzione [SQLGetCursorName](../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) o che è possibile specificare chiamando **SQLSetCursorName**. Di seguito sono riportati i passaggi generali utilizzati per eseguire un aggiornamento di WHERE CURRENT OF in un'applicazione ODBC:  
  
-   Chiamare **SQLSetCursorName** per stabilire un nome di cursore per l'handle dell'istruzione.  
  
-   Compilare un'istruzione SELECT con una clausola FOR UPDATE OF ed eseguirla.  
  
-   Chiamare **SQLFetchScroll** per recuperare un set di righe o **SQLFetch** per recuperare una riga.  
  
-   Chiamare **SQLSetPos** (SQL_POSITION) per posizionare il cursore sulla riga.  
  
-   Compilare ed eseguire un'istruzione UPDATE con una clausola WHERE CURRENT OF utilizzando il nome del cursore impostato con **SQLSetCursorName**.  
  
 In alternativa, è possibile chiamare **SQLGetCursorName** dopo l'esecuzione dell'istruzione SELECT anziché chiamare **SQLSetCursorName** prima di eseguire l'istruzione SELECT. **SQLGetCursorName** restituisce un nome di cursore predefinito assegnato da ODBC se non si imposta un nome di cursore utilizzando **SQLSetCursorName**.  
  
 **SQLSetPos** è preferibile rispetto a where current of quando si utilizzano i cursori server. Se si utilizza un cursore statico aggiornabile con la libreria di cursore ODBC, la libreria di cursori implementa gli aggiornamenti di WHERE CURRENT OF aggiungendo una clausola WHERE con i valori chiave per la tabella sottostante. Se le chiavi nella tabella non sono univoche, possono verificarsi aggiornamenti non desiderati.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di cursori &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
