---
description: Funzione SQLGetTranslator
title: Funzione SQLGetTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTranslator
helpviewer_keywords:
- SQLGetTranslator function [ODBC]
ms.assetid: 33879db3-5ef9-4585-9be5-69376157e017
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ec960312c652c625a6fb27388bd9c21a09f3206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189729"
---
# <a name="sqlgettranslator-function"></a>Funzione SQLGetTranslator
**Conformità**  
 Versione introdotta: ODBC 2,0  
  
 **Summary**  
 **SQLGetTranslator** Visualizza una finestra di dialogo da cui un utente può selezionare un convertitore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLGetTranslator(  
     HWND      hwndParent,  
     LPSTR     lpszName,  
     WORD      cbNameMax,  
     WORD *    pcbNameOut,  
     LPSTR     lpszPath,  
     WORD      cbPathMax,  
     WORD *    pcbPathOut,  
     DWORD *   pvOption);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hwndParent*  
 Input Handle della finestra padre.  
  
 *lpszName*  
 [Input/output] Nome del traduttore dalle informazioni di sistema.  
  
 *cbNameMax*  
 Input Lunghezza massima del buffer *lpszName* .  
  
 *pcbNameOut*  
 [Input/output] Numero totale di byte (escluso il byte di terminazione null) passato o restituito in *lpszName*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbNameMax*, il nome del traduttore in *lpszName* viene troncato in *cbNameMax* meno il carattere di terminazione null. L'argomento *pcbNameOut* può essere un puntatore null.  
  
 *lpszPath*  
 Output Percorso completo della DLL di traduzione.  
  
 *cbPathMax*  
 Input Lunghezza massima del buffer *lpszPath* .  
  
 *pcbPathOut*  
 Output Numero totale di byte (escluso il byte di terminazione null) restituito in *lpszPath*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbPathMax*, il percorso della dll della traduzione in *lpszPath* viene troncato in *cbPathMax* meno il carattere di terminazione null. L'argomento *pcbPathOut* può essere un puntatore null.  
  
 *pvOption*  
 [Output] opzione di conversione a 32 bit.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo o se l'utente annulla la finestra di dialogo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetTranslator** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *cbNameMax* o *cbPathMax* è minore o uguale a 0.|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|L'argomento *hwndParent* non è valido o è null.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszName* non è valido. Non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Non è stato possibile caricare la libreria di installazione del driver o del convertitore|Non è stato possibile caricare la libreria di conversione.|  
|ODBC_ERROR_INVALID_OPTION|Opzione di transazione non valida|L'argomento *pvOption* contiene un valore non valido.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 Se *hwndParent* è null o *lpszName*, *lpszPath* o *pvOption* è un puntatore null, **SQLGetTranslator** restituisce false. In caso contrario, viene visualizzato l'elenco dei convertitori installati nella finestra di dialogo seguente.  
  
 ![Finestra di dialogo Selezione convertitore](../../../odbc/reference/syntax/media/ch23j.gif "CH23J")  
  
 Se *lpszName* contiene un nome di traduttore valido, viene selezionato. In caso contrario, \<No Translator> viene selezionato.  
  
 Se l'utente sceglie \<No Translator> , il contenuto di *lpszName*, *lpszPath* e *pvOption* non viene toccato. **SQLGetTranslator** imposta *pcbNameOut* e *PCBPATHOUT* su 0 e restituisce true.  
  
 Se l'utente sceglie un convertitore, **SQLGetTranslator** chiama **ConfigTranslator** nella DLL di installazione del traduttore. Se **ConfigTranslator** restituisce false, **SQLGetTranslator** torna alla relativa finestra di dialogo. Se **ConfigTranslator** restituisce true, **SQLGetTranslator** restituisce true, insieme al nome del traduttore, al percorso e all'opzione di conversione selezionati.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Configurazione di un convertitore|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Recupero di un attributo Translation|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Impostazione di un attributo Translation|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
