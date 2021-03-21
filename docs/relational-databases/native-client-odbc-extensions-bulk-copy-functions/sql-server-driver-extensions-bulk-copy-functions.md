---
description: Estensioni del driver SQL Server - Funzioni di copia bulk
title: Funzioni di copia bulk | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], functions
- ODBC, bulk copy operations
- functions [ODBC]
ms.assetid: 6526b892-1d58-4f55-8335-f09887f6ea02
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 533446dd9409ad6716bd1a6cf1ca6fcdc37d8ee9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752521"
---
# <a name="sql-server-driver-extensions---bulk-copy-functions"></a>Estensioni del driver SQL Server - Funzioni di copia bulk
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC (Open Database Connectivity) è un'API Microsoft Win32 utilizzata dalle applicazioni per l'accesso ai dati delle origini dati ODBC. Nella guida di riferimento al driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client non sono documentate tutte le chiamate alle funzioni di ODBC. Vengono infatti prese in esame solo le funzioni che, se utilizzate con il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, presentano comportamenti o parametri specifici del driver.  
  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client è conforme alla specifica ODBC 3.51. Per un riferimento completo di ODBC 3,51, scaricare Microsoft Data Access Components SDK da [Data Access and Storage Developer Center](https://go.microsoft.com/fwlink?linkid=4173)oppure visualizzare la documentazione online di [ODBC Programmer ' s Reference](../../odbc/reference/odbc-programmer-s-reference.md) .  
 
 L'estensione API della copia bulk specifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client consente alle applicazioni client di aggiungere o di estrarre rapidamente righe di dati in una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Quando si utilizza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, è possibile fare riferimento alle funzioni di copia bulk (BCP) in SQLNCLI11.LIB e SQLNCLI.H.  
  
 Un'applicazione in cui vengono utilizzate le chiamate alla funzione API BCP dovrebbe essere collegata alla libreria (con estensione lib) fornita con il driver (con estensione dll) utilizzato dall'applicazione. Un'applicazione BCP non dovrebbe essere collegata a più di una libreria del driver.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md)  
  
-   [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)  
  
-   [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)  
  
-   [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)  
  
-   [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md)  
  
-   [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)  
  
-   [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)  
  
-   [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md)  
  
-   [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md)  
  
-   [bcp_getcolfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-getcolfmt.md)  
  
-   [bcp_gettypename](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-gettypename.md)  
  
-   [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)  
  
-   [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)  
  
-   [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md)  
  
-   [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)  
  
-   [bcp_setbulkmode](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setbulkmode.md)  
  
-   [bcp_setcolfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setcolfmt.md)  
  
-   [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni del driver SQL Server]()   
 [Esecuzione di operazioni di copia bulk &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
