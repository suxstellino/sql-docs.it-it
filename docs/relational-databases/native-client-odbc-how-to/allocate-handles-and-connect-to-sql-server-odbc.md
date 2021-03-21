---
description: Allocare handle e connettersi a SQL Server (ODBC)
title: Allocare handle e connettersi a SQL Server (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- handles [ODBC]
- handles [ODBC], connection
- handles [ODBC], about handles
ms.assetid: 6172cd52-9c9a-467d-992f-def07f3f3bb1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c981ed247a40c42d8ef8737d1ae891296194dad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753281"
---
# <a name="allocate-handles-and-connect-to-sql-server-odbc"></a>Allocare handle e connettersi a SQL Server (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-allocate-handles-and-connect-to-sql-server"></a>Per allocare handle e connettersi a SQL Server  
  
1.  Includere i file di intestazione ODBC Sql.h, Sqlext.h, Sqltypes.h.  
  
2.  Includere il file di intestazione specifico del driver [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Odbcss.h.  
  
3.  Chiamare [SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) con un **HandleType** di SQL_HANDLE_ENV per inizializzare ODBC e allocare un handle di ambiente.  
  
4.  Chiamare [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) con **Attribute** impostato su SQL_ATTR_ODBC_VERSION e **ValuePtr** impostato su SQL_OV_ODBC3 per indicare che l'applicazione userà le chiamate di funzione ODBC 3. x-format.  
  
5.  Facoltativamente, chiamare [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) per impostare altre opzioni di ambiente o chiamare [SQLGetEnvAttr](../../odbc/reference/syntax/sqlgetenvattr-function.md) per ottenere le opzioni di ambiente.  
  
6.  Chiamare [SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) con un **HandleType** di SQL_HANDLE_DBC per allocare un handle di connessione.  
  
7.  Facoltativamente, chiamare [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) per impostare le opzioni di connessione oppure chiamare [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) per ottenere le opzioni di connessione.  
  
8.  Chiamare SQLConnect per utilizzare un'origine dati esistente a cui connettersi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Oppure  
  
     Chiamare [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) per usare una stringa di connessione per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Il formato minimo di una stringa di connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] completa può essere uno dei seguenti:  
  
    ```  
    DSN=dsn_name;Trusted_connection=yes;  
    DRIVER={SQL Server Native Client 10.0};SERVER=server;Trusted_connection=yes;  
    ```  
  
     Se la stringa di connessione non è completa, **SQLDriverConnect** può richiedere le informazioni necessarie. Questa operazione è controllata dal valore specificato per il parametro *DriverCompletion* .  
  
     \- - oppure -  
  
     Chiamare [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) più volte in modo iterativo per compilare la stringa di connessione e connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
9. Facoltativamente, chiamare [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) per ottenere gli attributi e il comportamento del driver per l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origine dati.  
  
10. Allocare e utilizzare le istruzioni.  
  
11. Chiamare Disconnect per disconnettersi da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e rendere disponibile l'handle di connessione per una nuova connessione.  
  
12. Chiamare [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) con un **HandleType** di SQL_HANDLE_DBC per liberare l'handle di connessione.  
  
13. Chiamare **SQLFreeHandle** con un **HandleType** di SQL_HANDLE_ENV per liberare l'handle di ambiente.  
  
> [!IMPORTANT]  
>  Se possibile, usare l'autenticazione di Windows. Se non è disponibile, agli utenti verrà richiesto di immettere le credenziali in fase di esecuzione. Evitare di archiviare le credenziali in un file. Se è necessario rendere persistenti le credenziali, è consigliabile crittografarle usando l'[API di crittografia Win32](/windows/win32/seccrypto/cryptography-reference).  
  
## <a name="example"></a>Esempio  
 In questo esempio viene illustrata una chiamata a **SQLDriverConnect** per connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza richiedere un'origine dati ODBC esistente. Passando una stringa di connessione incompleta a **SQLDriverConnect**, il driver ODBC chiede all'utente di immettere le informazioni mancanti.  
  
```  
#define MAXBUFLEN   255  
  
SQLHENV      henv = SQL_NULL_HENV;  
SQLHDBC      hdbc1 = SQL_NULL_HDBC;  
SQLHSTMT      hstmt1 = SQL_NULL_HSTMT;  
  
SQLCHAR      ConnStrIn[MAXBUFLEN] =  
         "DRIVER={SQL Server Native Client 10.0};SERVER=MyServer";  
  
SQLCHAR      ConnStrOut[MAXBUFLEN];  
SQLSMALLINT   cbConnStrOut = 0;  
  
// Make connection without data source. Ask that driver   
// prompt if insufficient information. Driver returns  
// SQL_ERROR and application prompts user  
// for missing information. Window handle not needed for  
// SQL_DRIVER_NOPROMPT.  
retcode = SQLDriverConnect(hdbc1,      // Connection handle  
                  NULL,         // Window handle  
                  ConnStrIn,      // Input connect string  
                  SQL_NTS,         // Null-terminated string  
                  ConnStrOut,      // Address of output buffer  
                  MAXBUFLEN,      // Size of output buffer  
                  &cbConnStrOut,   // Address of output length  
                  SQL_DRIVER_PROMPT);  
```  
  
