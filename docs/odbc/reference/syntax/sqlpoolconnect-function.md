---
description: Funzione SQLPoolConnect
title: Funzione SQLPoolConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8317e159a5a438bb22e8077f3a6635a79f9baefd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204553"
---
# <a name="sqlpoolconnect-function"></a>Funzione SQLPoolConnect
**Conformità**  
 Versione introdotta: ODBC 3,8 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLPoolConnect** viene utilizzato per creare una nuova connessione se non è possibile riutilizzare una connessione nel pool.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>Argomenti  
 *hDbc*  
 Input Handle di connessione.  
  
 *hDbcInfoToken*  
 Input Handle del token per la nuova richiesta di connessione dell'applicazione.  
  
 *wszOutConnectString*  
 Output Puntatore a un buffer per la stringa di connessione completata. Al completamento della connessione all'origine dati di destinazione, il buffer contiene la stringa di connessione completata. Le applicazioni devono allocare almeno 1.024 caratteri per questo buffer.  
  
 Se *wszOutConnectString* è null, *cchConnectStringLen* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *wszOutConnectString*.  
  
 *cchConnectStringBuffer*  
 Input Lunghezza del buffer **wszOutConnectString* , in caratteri.  
  
 *cchConnectStringLen*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per restituire in \* *wszOutConnectString*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *cchConnectStringBuffer*, la stringa di connessione completata in \* *wszOutConnectString* viene troncata a *cchConnectStringBuffer* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Simile a [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) per qualsiasi errore di convalida dell'input, con la differenza che Gestione driver utilizzerà un **HandleType** di SQL_HANDLE_DBC_INFO_TOKEN e un **handle** di *hDbcInfoToken*.  
  
## <a name="remarks"></a>Commenti  
 Gestione driver garantisce che l'handle HENV padre di *HDBC* e *hDbcInfoToken* siano uguali.  
  
 A differenza di [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md), non esiste alcun argomento di *DriverCompletion* per richiedere agli utenti di immettere le informazioni di connessione. Una finestra di dialogo di richiesta non è consentita nello scenario di pool.  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Ogni volta che un driver restituisce SQL_ERROR o SQL_INVALID_HANDLE, gestione driver restituisce l'errore all'applicazione (in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Ogni volta che un driver restituisce SQL_SUCCESS_WITH_INFO, gestione driver otterrà le informazioni di diagnostica da *hDbcInfoToken* e restituirà SQL_SUCCESS_WITH_INFO all'applicazione in [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) e [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Quando un'applicazione usa [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md), *wszOutConnectString* sarà un buffer null (gli ultimi tre parametri verranno tutti impostati su NULL, 0, null). In caso contrario, il driver deve restituire la stringa di connessione di output, che verrà restituita alla chiamata della [funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) dell'applicazione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
