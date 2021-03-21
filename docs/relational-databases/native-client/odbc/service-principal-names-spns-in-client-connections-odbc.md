---
title: Nomi dell'entità servizio (SPN) nel client ODBC
description: Informazioni sugli attributi e le funzioni ODBC che supportano i nomi dell'entità servizio (SPN) nelle applicazioni client.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 1d60cb30-4c46-49b2-89ab-701e77a330a2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e7309f75d9e1a92adf831b0405d8e93f3eb73f1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754091"
---
# <a name="service-principal-names-spns-in-client-connections-odbc"></a>Nomi SPN (Service Principal Name) nelle connessioni client (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono descritti gli attributi e le funzioni ODBC che supportano i nomi SPN (Service Principal Name, nome dell'entità servizio) nelle applicazioni client. Per ulteriori informazioni sui nomi SPN nelle applicazioni client, vedere [nome dell'entità servizio &#40;spn&#41; supporto nelle connessioni client](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md) e [ottenere l'autenticazione Kerberos reciproca](../../../relational-databases/native-client-odbc-how-to/get-mutual-kerberos-authentication.md).  
  
## <a name="connection-string-keywords"></a>Parole chiave per le stringhe di connessione  
 Le parole chiave per le stringhe di connessione seguenti consentono la specifica di un nome SPN nelle applicazioni client.  
  
|Parola chiave|Valore|  
|-------------|-----------|  
|**ServerSPN**|Nome SPN del server. Il valore predefinito è una stringa vuota. In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client tale stringa causa l'utilizzo del nome SPN predefinito generato dal driver.|  
|**FailoverPartnerSPN**|Nome SPN del partner di failover. Il valore predefinito è una stringa vuota. In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client tale stringa causa l'utilizzo del nome SPN predefinito generato dal driver.|  
  
## <a name="connection-attributes"></a>Attributi di connessione  
 Gli attributi di connessione seguenti consentono la specifica di un nome SPN e di una query per il metodo di autenticazione nelle applicazioni client.  
  
|Nome|Type|Uso|  
|----------|----------|-----------|  
|SQL_COPT_SS_SERVER_SPN<br /><br /> SQL_COPT_SS_FAILOVER_PARTNER_SPN|SQLTCHAR, lettura/scrittura|Specifica il nome SPN per il server. Il valore predefinito è una stringa vuota. In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client tale stringa causa l'utilizzo del nome SPN predefinito generato dal driver.<br /><br /> Su questo attributo è possibile eseguire una query solo in seguito alla relativa impostazione a livello di codice o all'apertura di una connessione. Se si tenta di eseguire una query su questo attributo in una connessione non aperta e con l'attributo non impostato a livello di codice, viene restituito SQL_ERROR e viene registrato un record di diagnostica con SQLState 08003 e il messaggio "Connessione non aperta".<br /><br /> Se si tenta di impostare l'attributo quando una connessione è aperta, viene restituito SQL_ERROR e viene registrato un record di diagnostica con SQLState HY011 e il messaggio "Operazione correntemente non valida".|  
|SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD|SQLTCHAR, sola lettura|Restituisce il metodo di autenticazione utilizzato per la connessione. Il valore restituito all'applicazione è il valore che Windows restituisce a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. I valori possibili sono:<br /><br /> "NTLM", restituito quando una connessione viene aperta utilizzando l'autenticazione NTLM.<br /><br /> "Kerberos", restituito quando una connessione viene aperta utilizzando l'autenticazione Kerberos.<br /><br /> <br /><br /> Questo attributo può essere letto solo per una connessione aperta in cui è stata utilizzata l'autenticazione di Windows. Se si tenta di leggerlo prima che venga aperta una connessione, viene restituito SQL_ERROR e viene registrato un errore con SQLState 08003 e il messaggio "Connessione non aperta".<br /><br /> Se si esegue una query su questo attributo in una connessione in cui non è stata utilizzata l'autenticazione di Windows, viene restituito SQL_ERROR e viene registrato un errore con SQLState HY092 e il messaggio "Identificatore di opzione o di attributo non valido (SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD è disponibile solo per connessioni trusted)".<br /><br /> Se non è possibile determinare il metodo di autenticazione, viene restituito SQL_ERROR e viene registrato un errore con SQLState HY000 e il messaggio "Errore generale".|  
|SQL_COPT_SS_MUTUALLY_AUTHENTICATED|SQLSMALLINT, sola lettura|Restituisce SQL_TRUE se nella connessione è stata eseguita un'autenticazione reciproca del server. In caso contrario, restituisce SQL_FALSE.<br /><br /> Questo attributo può essere letto solo per una connessione aperta. Se si tenta di leggerlo prima che venga aperta una connessione, viene restituito SQL_ERROR e viene registrato un errore con SQLState 08003 e il messaggio "Connessione non aperta".<br /><br /> Se viene eseguita una query sull'attributo per una connessione in cui non è stata utilizzata l'autenticazione di Windows, viene restituito SQL_FALSE.|  
  
## <a name="odbc-function-support-for-specifying-spns"></a>Supporto delle funzioni ODBC per la specifica dei nomi SPN  
 Le funzioni ODBC seguenti supportano le applicazioni client e i nomi SPN:  
  
-   [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
-   [SQLConnect](../../../relational-databases/native-client-odbc-api/sqlconnect.md)  
  
-   [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)  
  
-   [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)  
  
-   [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
