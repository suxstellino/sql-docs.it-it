---
description: Funzione SQLInstallerError
title: Funzione SQLInstallerError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallerError
helpviewer_keywords:
- SQLInstallerError [ODBC]
ms.assetid: e6474b79-4d55-458f-81ce-abfafe357f83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dcafadc902504ddfd32fa8b7369780bd5c296a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189679"
---
# <a name="sqlinstallererror-function"></a>Funzione SQLInstallerError
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLInstallerError** restituisce informazioni sugli errori o sullo stato per le funzioni del programma di installazione ODBC.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
RETCODE SQLInstallerError(  
     WORD      iError,  
     DWORD *   pfErrorCode,  
     LPSTR     lpszErrorMsg,  
     WORD      cbErrorMsgMax,  
     WORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Argomenti  
 *IErrore*  
 Input Numero di record di errore. I numeri validi sono compresi tra 1 e 8.  
  
 *pfErrorCode*  
 Output Codice di errore del programma di installazione. Per ulteriori informazioni, vedere "Commenti".  
  
 *lpszErrorMsg*  
 Output Puntatore alla risorsa di archiviazione per il testo del messaggio di errore.  
  
 *cbErrorMsgMax*  
 Input Lunghezza massima del buffer *szErrorMsg* . Deve essere minore o uguale a SQL_MAX_MESSAGE_LENGTH meno il carattere di terminazione null.  
  
 *cbErrorMsgMax*  
 Input Lunghezza massima del buffer *szErrorMsg* . Deve essere minore o uguale a SQL_MAX_MESSAGE_LENGTH meno il carattere di terminazione null.  
  
 *pcbErrorMsg*  
 Output Puntatore al numero totale di byte, escluso il carattere di terminazione null, disponibile per restituire in *lpszErrorMsg*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbErrorMsgMax*, il testo del messaggio di errore in *lpszErrorMsg* viene troncato in *cbErrorMsgMax* meno i byte del carattere di terminazione null. L'argomento *pcbErrorMsg* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA o SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnostica  
 **SQLInstallerError** non pubblica i valori di errore per se stesso. **SQLInstallerError** restituisce SQL_NO_DATA quando non è in grado di recuperare le informazioni sull'errore, nel qual caso *pfErrorCode* non è definito. Se **SQLInstallerError** non è in grado di accedere ai valori di errore per qualsiasi motivo che in genere restituisca SQL_ERROR, **SQLInstallerError** restituisce SQL_ERROR ma non pubblica alcun valore di errore. Se non si conosce la lunghezza della stringa di avviso (*lpszErrorMsg*), è possibile impostare *lpszErrorMsg* su null e chiamare **SQLInstallerError**. **SQLInstallerError** restituirà quindi la lunghezza della stringa di avviso in *cbErrorMsgMax*. Se il buffer per il messaggio di errore è troppo breve, **SQLInstallerError** restituisce SQL_SUCCESS_WITH_INFO e restituisce il valore *PfErrorCode* corretto per **SQLInstallerError**.  
  
 Per determinare se si è verificato un troncamento nel messaggio di errore, un'applicazione può confrontare il valore nell'argomento *cbErrorMsgMax* con la lunghezza effettiva del testo del messaggio scritto nell'argomento *pcbErrorMsg* . Se il troncamento si verifica, è necessario allocare la lunghezza del buffer corretta per *lpszErrorMsg* e **SQLInstallerError** deve essere chiamato di nuovo con il record *IErrore* corrispondente.  
  
## <a name="comments"></a>Commenti  
 Un'applicazione chiama **SQLInstallerError** quando una precedente chiamata alla funzione ODBC Installer restituisce false. Il programma di installazione ODBC e le funzioni di installazione del driver o del convertitore inviano zero o più errori solo quando la funzione non riesce (restituisce FALSE); Pertanto, un'applicazione chiama **SQLInstallerError** solo dopo la mancata riuscita di una funzione del programma di installazione ODBC.  
  
 La coda di errori del programma di installazione ODBC viene scaricata ogni volta che viene chiamata una nuova funzione del programma di installazione. Pertanto, un'applicazione non può prevedere di recuperare gli errori per le funzioni diverse dall'ultima chiamata di funzione del programma di installazione.  
  
 Per recuperare più errori per una chiamata di funzione, un'applicazione chiama **SQLInstallerError** più volte.  
  
 Quando non sono disponibili informazioni aggiuntive, **SQLInstallerError** restituisce SQL_NO_DATA, l'argomento *pfErrorCode* non è definito, l'argomento *pcbErrorMsg* è uguale a 0 e l'argomento *lpszErrorMsg* contiene un singolo carattere di terminazione null (a meno che l'argomento *cbErrorMsgMax* sia uguale a 0).
