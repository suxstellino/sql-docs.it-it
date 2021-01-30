---
description: Informazioni di riferimento sull'interfaccia del provider di servizi (SPI) ODBC
title: Guida di riferimento all'interfaccia del provider di servizi (SPI) ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cdeffb4a-f344-4abe-97f3-be2ede1c8e59
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4cbf20ed3d158fca4e7ae58854bf53778dc9db6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174625"
---
# <a name="odbc-service-provider-interface-spi-reference"></a>Informazioni di riferimento sull'interfaccia del provider di servizi (SPI) ODBC
Tradizionalmente, ODBC definisce un Application Programming Interface (API). Le funzioni nell'API possono essere chiamate dalle applicazioni e devono essere implementate all'interno di gestione driver e del driver.  
  
 Con l'aggiunta della funzionalità di pool di connessioni compatibile con il driver, in ODBC è stata introdotta l'interfaccia del provider di servizi (SPI). Le funzioni nell'SPI vengono utilizzate per la comunicazione tra il driver e la gestione driver. Le funzioni SPI sono implementate dal driver; Gestione driver non espone le funzioni SPI alle applicazioni. Le applicazioni non devono chiamare direttamente queste funzioni.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
 Questa sezione contiene i seguenti argomenti  
  
-   [SQLCleanupConnectionPoolID](../../../odbc/reference/syntax/sqlcleanupconnectionpoolid-function.md)  
  
-   [SQLGetPoolID](../../../odbc/reference/syntax/sqlgetpoolid-function.md)  
  
-   [SQLPoolConnect](../../../odbc/reference/syntax/sqlpoolconnect-function.md)  
  
-   [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)  
  
-   [SQLSetConnectAttrForDbcInfo](../../../odbc/reference/syntax/sqlsetconnectattrfordbcinfo-function.md)  
  
-   [SQLSetConnectInfo](../../../odbc/reference/syntax/sqlsetconnectinfo-function.md)  
  
-   [SQLSetDriverConnectInfo](../../../odbc/reference/syntax/installation-and-configuration-wwi-oltp.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Sviluppo di Connection-Pool consapevolezza in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)   
 [Pool di connessioni di Gestione driver](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md)
