---
title: Risultati dell'elaborazione (ODBC) | Microsoft Docs
description: Sono disponibili informazioni sull'elaborazione dei dati restituiti da SQL Server quando un'applicazione ODBC invia un'istruzione SQL.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC], about result sets
- SQLRowCount function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- COMPUTE clause
- result sets [ODBC]
- COMPUTE BY clause
ms.assetid: 61a8db19-6571-47dd-84e8-fcc97cb60b45
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3c5b8bad410418a95a9ba180ab3921eb02e0644
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752771"
---
# <a name="processing-results-odbc"></a>Risultati dell'elaborazione (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dopo l'invio di un'istruzione SQL da parte di un'applicazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce eventuali dati risultanti come uno o più set di risultati. Un set di risultati è un set di righe e colonne che corrispondono ai criteri della query. Le istruzioni SELECT, le funzioni di catalogo e alcune stored procedure producono un set di risultati reso disponibile a un'applicazione in formato tabulare. Se l'istruzione SQL eseguita è una stored procedure, un batch contenente più comandi o un'istruzione SELECT contenente parole chiave, il numero di set di risultati da elaborare sarà maggiore.  
  
 Anche le funzioni di catalogo ODBC possono recuperare dati. [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md) , ad esempio, consente di recuperare dati sulle colonne dell'origine dati. Questi set di risultati possono contenere zero o più righe.  
  
 Le altre istruzioni SQL, ad esempio GRANT o REVOKE, non restituiscono set di risultati. Per queste istruzioni, il codice restituito da **SQLExecute** o **SQLExecDirect** è in genere l'unica indicazione che l'istruzione ha avuto esito positivo.  
  
 Ogni istruzione INSERT, UPDATE e DELETE restituisce un set di risultati contenente solo il numero di righe modificate. Questo conteggio viene reso disponibile quando l'applicazione chiama [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md). ODBC 3. le applicazioni *x* devono chiamare **SQLRowCount** per recuperare il set di risultati o [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) per annullarlo. Quando un'applicazione esegue un batch o stored procedure contenente più istruzioni INSERT, UPDATE o DELETE, il set di risultati di ogni istruzione di modifica deve essere elaborato utilizzando **SQLRowCount** o annullato utilizzando **SQLMoreResults**. Questi conteggi possono essere annullati includendo un'istruzione SET NOCOUNT ON nel batch o nella stored procedure.  
  
 Transact-SQL include l'istruzione SET NOCOUNT. Se l'opzione NOCOUNT è impostata su on, SQL Server non restituisce i conteggi delle righe interessate da un'istruzione e **SQLRowCount** restituisce 0. La [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione del driver ODBC di Native Client introduce un'opzione [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) specifica del driver, SQL_SOPT_SS_NOCOUNT_STATUS, per segnalare se l'opzione NOCOUNT è impostata su on o off. Quando **SQLRowCount** restituisce 0, l'applicazione deve testare SQL_SOPT_SS_NOCOUNT_STATUS. Se viene restituito SQL_NC_ON, il valore 0 di **SQLRowCount** indica solo che SQL Server non ha restituito un conteggio di righe. Se viene restituito SQL_NC_OFF, significa che NOCOUNT è disattivato e il valore 0 di **SQLRowCount** indica che l'istruzione non ha influire sulle righe. Quando SQL_SOPT_SS_NOCOUNT_STATUS viene SQL_NC_OFF, le applicazioni non dovrebbero visualizzare il valore di **SQLRowCount** . Le stored procedure o i batch di grandi dimensioni possono contenere più istruzioni SET NOCOUNT, pertanto i programmatori non possono presupporre che SQL_SOPT_SS_NOCOUNT_STATUS rimanga costante. L'opzione deve essere testata ogni volta che **SQLRowCount** restituisce 0.  
  
 Diverse altre istruzioni Transact-SQL restituiscono nei messaggi dati anziché set di risultati. Quando il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client riceve questi messaggi, restituisce SQL_SUCCESS_WITH_INFO per informare l'applicazione che i messaggi informativi sono disponibili. L'applicazione può quindi chiamare **SQLGetDiagRec** per recuperare questi messaggi. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che funzionano in questo modo sono:  
  
-   DBCC  
  
-   SET SHOWPLAN (disponibile con versioni precedenti di SQL Server)  
  
-   SET STATISTICS  
  
-   PRINT  
  
-   RAISERROR  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client restituisce SQL_ERROR in un RAISERROR con gravità 11 o superiore. Se la gravità di RAISERROR è 19 o superiore, viene anche interrotta la connessione.  
  
 Per elaborare i set di risultati da un'istruzione SQL, l'applicazione:  
  
-   Determina le caratteristiche del set di risultati.  
  
-   Associa le colonne a variabili di programma.  
  
-   Recupera un valore singolo, un'intera riga di valori o più righe di valori.  
  
-   Verifica se sono presenti altri set di risultati e, in caso affermativo, esegue il ciclo all'indietro per determinare le caratteristiche del nuovo set di risultati.  
  
 Il processo che consente di recuperare le righe dall'origine dati e di restituirle alle applicazioni viene denominato recupero.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Determinazione delle caratteristiche di un set di risultati &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/determining-the-characteristics-of-a-result-set-odbc.md)  
  
-   [Assegnazione di archiviazione](../../relational-databases/native-client-odbc-results/assigning-storage.md)  
  
-   [Recupero di dati dei risultati](../../relational-databases/native-client-odbc-results/fetching-result-data.md)  
  
-   [Mapping dei tipi di dati &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-results/mapping-data-types-odbc.md)  
  
-   [Utilizzo del tipo di dati](../../relational-databases/native-client-odbc-results/data-type-usage.md)  
  
-   [Conversione automatica dei dati di tipo carattere](../../relational-databases/native-client-odbc-results/autotranslation-of-character-data.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Procedure per l'elaborazione dei risultati &#40;ODBC&#41;](../native-client-odbc-how-to/processing-results-process-results.md)  
  
