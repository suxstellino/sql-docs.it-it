---
description: Funzione SQLSetConnectAttrForDbcInfo
title: Funzione SQLSetConnectAttrForDbcInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttrForDbcInfo function [ODBC]
ms.assetid: a28fadb9-b998-472a-b252-709507e92005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ca64e1309c939f4b39b8b9a020a1debb1cc10b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192436"
---
# <a name="sqlsetconnectattrfordbcinfo-function"></a>Funzione SQLSetConnectAttrForDbcInfo
**Conformità**  
 Versione introdotta: ODBC 3,81 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLSetConnectAttrForDbcInfo** è uguale a **SQLSetConnectAttr**, ma imposta l'attributo sul token delle informazioni di connessione anziché sull'handle di connessione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN  SQLSetConnectAttrForDbcInfo(  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                SQLINTEGER            Attribute,  
                SQLPOINTER            ValuePtr,  
                SQLINTEGER            StringLength );  
```  
  
## <a name="arguments"></a>Argomenti  
 *hDbcInfoToken*  
 Input Handle del token.  
  
 *Attributo*  
 Input Attributo da impostare. L'elenco di attributi validi è specifico per i driver e per [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 *ValuePtr*  
 Input Puntatore al valore da associare all' *attributo*. A seconda del valore dell' *attributo*, *ValuePtr* sarà un valore Unsigned Integer a 32 bit o punterà a una stringa di caratteri con terminazione null. Si noti che se l'argomento dell' *attributo* è un valore specifico del driver, il valore in *ValuePtr* può essere un intero con segno.  
  
 *StringLength*  
 Input Se *attribute* è un attributo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di **ValuePtr*. Per i dati di tipo stringa di caratteri, questo argomento deve contenere il numero di byte nella stringa.  
  
 Se *attribute* è un attributo definito da ODBC e *ValuePtr* è un numero intero, *StringLength* viene ignorato.  
  
 Se *attribute* è un attributo definito dal driver, l'applicazione indica la natura dell'attributo a gestione driver impostando l'argomento *StringLength* . *StringLength* può avere i valori seguenti:  
  
-   Se *ValuePtr* è un puntatore a una stringa di caratteri, *StringLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *StringLength*. Questo inserisce un valore negativo in *StringLength*.  
  
-   Se *ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *StringLength* deve essere SQL_IS_POINTER.  
  
-   Se *ValuePtr* contiene un valore a lunghezza fissa, *StringLength* è SQL_IS_INTEGER o SQL_IS_UINTEGER, a seconda delle esigenze.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Uguale a [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), con la differenza che Gestione driver utilizzerà un **HandleType** di SQL_HANDLE_DBC_INFO_TOKEN e un **handle** di *hDbcInfoToken*.  
  
## <a name="remarks"></a>Commenti  
 **SQLSetConnectAttrForDbcInfo** è uguale a **SQLSetConnectAttr**, ma imposta l'attributo sul token delle informazioni di connessione anziché sull'handle di connessione. Se, ad esempio, **SQLSetConnectAttr** non riconosce un attributo, **SQLSetConnectAttrForDbcInfo** deve restituire anche SQL_ERROR per quell'attributo.  
  
 Ogni volta che il driver restituisce SQL_ERROR o SQL_INVALID_HANDLE, il driver deve ignorare questo attributo per calcolare l'ID del pool. Inoltre, gestione driver otterrà le informazioni di diagnostica da *hDbcInfoToken* e restituirà SQL_SUCCESS_WITH_INFO all'applicazione in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) e [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md). Pertanto, un'applicazione può recuperare i dettagli sul motivo per cui non è possibile impostare alcuni attributi.  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
