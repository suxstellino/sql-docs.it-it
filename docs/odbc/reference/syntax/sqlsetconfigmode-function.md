---
description: Funzione SQLSetConfigMode
title: Funzione SQLSetConfigMode | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConfigMode
helpviewer_keywords:
- SQLSetConfigMode function [ODBC]
ms.assetid: 09eb88ea-b6f6-4eca-b19d-0951cebc6c0a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a136a476b56f108bef77395f0ee4b13ae23b10a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192458"
---
# <a name="sqlsetconfigmode-function"></a>Funzione SQLSetConfigMode
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLSetConfigMode** imposta la modalità di configurazione che indica il punto in cui la voce Odbc.ini elencando i valori DSN è presente nelle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLSetConfigMode(  
     UWORD     wConfigMode);  
```  
  
## <a name="arguments"></a>Argomenti  
 *wConfigMode*  
 Input Modalità di configurazione del programma di installazione (vedere "Commenti"). Il valore in *wConfigMode* può essere:  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetConfigMode** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Sequenza di parametri non valida|L'argomento *wConfigMode* non contiene ODBC_USER_DSN, ODBC_SYSTEM_DSN o ODBC_BOTH_DSN.|  
  
## <a name="comments"></a>Commenti  
 Questa funzione viene utilizzata per impostare il punto in cui la voce Odbc.ini elencando i valori DSN è presente nelle informazioni di sistema. Se *wConfigMode* è ODBC_USER_DSN, il DSN è un DSN utente e la funzione legge dalla voce Odbc.ini in HKEY_CURRENT_USER. Se è ODBC_SYSTEM_DSN, il DSN è un DSN di sistema e la funzione legge dalla voce Odbc.ini in HKEY_LOCAL_MACHINE. Se è ODBC_BOTH_DSN, viene eseguito un tentativo di HKEY_CURRENT_USER. in caso contrario, viene utilizzato HKEY_LOCAL_MACHINE.  
  
 Questa funzione non influisce su **SQLCreateDataSource** e **SQLDriverConnect**. È necessario impostare la modalità di configurazione quando un driver legge dal registro di sistema chiamando **SQLGetPrivateProfileString** o scrive nel registro di sistema chiamando **SQLWritePrivateProfileString**. Le chiamate a **SQLGetPrivateProfileString** e **SQLWritePrivateProfileString** usano la modalità di configurazione per individuare la parte del registro su cui operare.  
  
> [!CAUTION]  
>  **SQLSetConfigMode** deve essere chiamato solo quando necessario; Se la modalità non è impostata correttamente, il programma di installazione ODBC potrebbe non funzionare correttamente.  
  
 **SQLSetConfigMode** esegue una modifica diretta del registro di sistema della modalità di configurazione. Questa operazione è diversa dal processo di modifica della modalità di configurazione mediante una chiamata a **SQLConfigDataSource**. Una chiamata a **SQLConfigDataSource** imposta la modalità di configurazione per distinguere i DSN utente e di sistema quando si modifica un DSN. Prima di restituire, **SQLConfigDataSource** Reimposta la modalità di configurazione su BOTHDSN.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Creazione di un'origine dati|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Recupero della modalità di configurazione|[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|
