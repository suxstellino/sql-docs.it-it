---
description: Funzione SQLSetConnectInfo
title: Funzione SQLSetConnectInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectInfo function [ODBC]
ms.assetid: 0782a1c3-c5d1-499b-a8ba-134162db9990
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1930d16605c1b2a02db08560549019afa656ca52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192431"
---
# <a name="sqlsetconnectinfo-function"></a>Funzione SQLSetConnectInfo
**Conformità**  
 Versione introdotta: ODBC 3,81 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLSetConnectInfo** viene usato per impostare l'origine dati, l'ID utente e la password nel token di informazioni di connessione per la chiamata [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) di un'applicazione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN  SQLSetConnectInfo(  
                SQLHDBC_INFO_TOKEN   TokenHandle,  
                WCHAR *              ServerName,  
                SQLSMALLINT          NameLength1,  
                WCHAR *              UserName,  
                SQLSMALLINT          NameLength2,  
                WCHAR *              Authentication,  
                SQLSMALLINT          NameLength3 );  
```  
  
## <a name="arguments"></a>Argomenti  
 *TokenHandle*  
 Input Handle del token.  
  
 *ServerName*  
 Input Nome dell'origine dati. I dati potrebbero trovarsi nello stesso computer del programma o in un altro computer in una rete. Per informazioni sul modo in cui un'applicazione sceglie un'origine dati, vedere scelta di un' [origine dati o](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)di un driver.  
  
 *NameLength1*  
 Input Lunghezza di **ServerName* in caratteri.  
  
 *UserName*  
 Input Identificatore utente.  
  
 *NameLength2*  
 Input Lunghezza di **username* in caratteri.  
  
 *autenticazione*  
 Input Stringa di autenticazione (in genere la password).  
  
 *NameLength3*  
 Input Lunghezza di **autenticazione* in caratteri.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Uguale a [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) per gli errori di convalida dell'input, con la differenza che Gestione driver utilizzerà un **HandleType** di SQL_HANDLE_DBC_INFO_TOKEN e un **handle** di *hDbcInfoToken*.  
  
## <a name="remarks"></a>Commenti  
 Ogni volta che un driver restituisce SQL_ERROR o SQL_INVALID_HANDLE, gestione driver restituisce l'errore all'applicazione (in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Ogni volta che un driver restituisce SQL_SUCCESS_WITH_INFO, gestione driver otterrà le informazioni di diagnostica da *hDbcInfoToken* e restituirà SQL_SUCCESS_WITH_INFO all'applicazione in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) e [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
