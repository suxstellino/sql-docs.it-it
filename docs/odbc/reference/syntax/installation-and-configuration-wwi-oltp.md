---
description: Funzione SQLSetDriverConnectInfo
title: Funzione SQLSetDriverConnectInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDriverConnectInfo function [ODBC]
ms.assetid: bfd4dfc2-fbca-4ef3-81e5-2706f2389256
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0a21b8fa3e620f4a6884311c10c93c36518849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174685"
---
# <a name="sqlsetdriverconnectinfo-function"></a>Funzione SQLSetDriverConnectInfo
**Conformità**  
 Versione introdotta: ODBC 3,81 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLSetDriverConnectInfo** viene usato per impostare la stringa di connessione nel token delle informazioni di connessione per la chiamata **SQLDriverConnect** di un'applicazione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN SQLSetDriverConnectInfo(  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              InConnectionString,  
                SQLSMALLINT          StringLength1 );  
```  
  
## <a name="arguments"></a>Argomenti  
 *TokenHandle*  
 Input Handle del token.  
  
 *InConnectionString*  
 Input Una stringa di connessione completa (vedere la sintassi in "comments" in [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)), una stringa di connessione parziale o una stringa vuota.  
  
 *StringLength1*  
 Input Lunghezza di **InConnectionString*, in caratteri se la stringa è Unicode, o byte se la stringa è ANSI o DBCS.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Uguale a [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) correlato a un errore di convalida dell'input, con la differenza che Gestione driver utilizzerà un **HandleType** di SQL_HANDLE_DBC_INFO_TOKEN e un **handle** di *hDbcInfoToken*.  
  
## <a name="remarks"></a>Commenti  
 Ogni volta che un driver restituisce SQL_ERROR o SQL_INVALID_HANDLE, gestione driver restituisce l'errore all'applicazione (in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Ogni volta che un driver restituisce SQL_SUCCESS_WITH_INFO, gestione driver otterrà le informazioni di diagnostica da *hDbcInfoToken* e restituirà SQL_SUCCESS_WITH_INFO all'applicazione in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) e [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
