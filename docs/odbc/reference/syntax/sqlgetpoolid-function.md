---
description: Funzione SQLGetPoolID
title: Funzione SQLGetPoolID | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetPoolID function [ODBC]
ms.assetid: 95a8666a-ad68-4d89-bf65-f2cc797f8820
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e7bd06a289eab6499995e40a3c6ad9ea56c747e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180919"
---
# <a name="sqlgetpoolid-function"></a>Funzione SQLGetPoolID
**Conformità**  
 Versione introdotta: ODBC 3,81 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLGetPoolID** recupera l'ID del pool.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN  SQLGetPoolID (  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                POOLID *              pPoolID );  
```  
  
## <a name="arguments"></a>Argomenti  
 *hDbcInfoToken*  
 Input Handle di token che contiene tutte le informazioni di connessione.  
  
 *pPoolID*  
 Output ID del pool, usato per identificare un set di connessioni che possono essere usate in modo intercambiabile (eventualmente richiedendo una reimpostazione aggiuntiva).  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetPoolID** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, gestione driver utilizzerà un **HandleType** di SQL_HANDLE_DBC_INFO_TOKEN e un **handle** di *hDbcInfoToken*.  
  
## <a name="remarks"></a>Commenti  
 **SQLGetPoolID** viene usato per ottenere l'ID del pool in base a un set di informazioni di connessione (da **SQLSetConnectAttrForDbcInfo**, **SQLSetDriverConnectInfo** e **SQLSetConnectInfo**). Questo ID del pool viene usato per identificare un set di connessioni che possono essere usate in modo interscambiabile (probabilmente richiedendo una reimpostazione aggiuntiva). L'ID del pool verrà usato per identificare il pool di connessioni per il gruppo di connessioni.  
  
 Ogni volta che un driver restituisce SQL_ERROR o SQL_INVALID_HANDLE, gestione driver restituisce l'errore all'applicazione (in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Ogni volta che un driver restituisce SQL_SUCCESS_WITH_INFO, gestione driver otterrà le informazioni di diagnostica da *hDbcInfoToken* e restituirà SQL_SUCCESS_WITH_INFO all'applicazione in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) e [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
