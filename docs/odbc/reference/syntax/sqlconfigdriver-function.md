---
description: Funzione SQLConfigDriver
title: Funzione SQLConfigDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDriver
helpviewer_keywords:
- SQLConfigDriver function [ODBC]
ms.assetid: 4f681961-ac9f-4d88-b065-5258ba112642
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abe28e8d870216cf090e57a93e0c75766db08231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165332"
---
# <a name="sqlconfigdriver-function"></a>Funzione SQLConfigDriver
**Conformità**  
 Versione introdotta: ODBC 2,5  
  
 **Summary**  
 **SQLConfigDriver** carica la dll di installazione del driver appropriata e chiama la funzione **ConfigDriver** .  
  
 È possibile accedere alle funzionalità di **SQLConfigDriver** anche con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLConfigDriver(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszArgs,  
     LPSTR    lpszMsg,  
     WORD     cbMsgMax,  
     WORD *   pcbMsgOut);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hwndParent*  
 Input Handle della finestra padre. Se l'handle è null, nella funzione non verrà visualizzata alcuna finestra di dialogo.  
  
 *fRequest*  
 Input Tipo di richiesta. *fRequest* deve contenere uno dei valori seguenti:  
  
 ODBC_CONFIG_DRIVER: modifica il timeout del pool di connessioni utilizzato dal driver.  
  
 ODBC_INSTALL_DRIVER: installa un nuovo driver.  
  
 ODBC_REMOVE_DRIVER: rimuove un driver esistente.  
  
 Questa opzione può anche essere specifica del driver, nel qual caso il *fRequest* per la prima opzione deve iniziare da ODBC_CONFIG_DRIVER_MAX + 1. Il valore di *fRequest* per qualsiasi opzione aggiuntiva deve iniziare anche da un valore maggiore di ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Input Nome del driver registrato nelle informazioni di sistema.  
  
 *lpszArgs*  
 Input Stringa con terminazione null che contiene gli argomenti per un *fRequest* specifico del driver.  
  
 *lpszMsg*  
 Output Stringa con terminazione null che contiene un messaggio di output generato dall'installazione del driver.  
  
 *cbMsgMax*  
 Input Lunghezza di *lpszMsg.*  
  
 *pcbMsgOut*  
 Output Numero totale di byte disponibili da restituire in *lpszMsg*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbMsgMax*, il messaggio di output in *lpszMsg* viene troncato in *cbMsgMax* meno il carattere di terminazione null. L'argomento *pcbMsgOut* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLConfigDriver** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszMsg* non è valido.|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|L'argomento *hwndParent* non è valido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> L'argomento *fRequest* era un'opzione specifica del driver che era minore o uguale a ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszDriver* non è valido. Non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Coppie parola chiave/valore non valide|L'argomento *lpszArgs* contiene un errore di sintassi.|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|Il programma di installazione non è riuscito a eseguire l'operazione richiesta dall'argomento *fRequest* . La chiamata a **ConfigDriver** non è riuscita.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Non è stato possibile caricare la libreria di installazione del driver o del convertitore|Non è stato possibile caricare la libreria di installazione del driver.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLConfigDriver** consente a un'applicazione di chiamare la routine **ConfigDriver** di un driver senza che sia necessario conoscerne il nome e caricare la dll di installazione specifica del driver. Un programma di installazione chiama questa funzione dopo che è stata installata la DLL di installazione del driver. Il programma chiamante deve tenere presente che questa funzione potrebbe non essere disponibile per tutti i driver. In tal caso, il programma chiamante deve continuare senza errori.  
  
## <a name="driver-specific-options"></a>Opzioni di Driver-Specific  
 Un'applicazione può richiedere funzionalità specifiche del driver esposte dal driver tramite l'argomento *fRequest* . Il valore di *fRequest* per la prima opzione sarà ODBC_CONFIG_DRIVER_MAX + 1 e le opzioni aggiuntive verranno incrementate di 1 da tale valore. Tutti gli argomenti richiesti dal driver per la funzione devono essere specificati in una stringa con terminazione null passata nell'argomento *lpszArgs* . I driver che forniscono tali funzionalità devono gestire una tabella di opzioni specifiche del driver. Le opzioni devono essere completamente documentate nella documentazione del driver. I writer di applicazioni che utilizzano opzioni specifiche del driver devono tenere presente che questo utilizzo renderà l'applicazione meno interoperativa.  
  
## <a name="setting-connection-pooling-timeout"></a>Impostazione del timeout del pool di connessioni  
 È possibile impostare le proprietà di timeout del pool di connessioni quando si imposta la configurazione del driver. **SQLConfigDriver** viene chiamato con un *fRequest* di ODBC_CONFIG_DRIVER e *lpszArgs* impostato su **CPTimeout**. **CPTimeout** determina il periodo di tempo durante il quale una connessione può rimanere nel pool di connessioni senza essere utilizzata. Alla scadenza del timeout, la connessione viene chiusa e rimossa dal pool. Il timeout predefinito è di 60 secondi.  
  
 Quando viene chiamato **SQLConfigDriver** con *fRequest* impostato su ODBC_INSTALL_DRIVER o ODBC_REMOVE_DRIVER, gestione driver carica la dll di installazione del driver appropriata e chiama la funzione **ConfigDriver** . Quando **SQLConfigDriver** viene chiamato con un *fRequest* di ODBC_CONFIG_DRIVER, tutte le operazioni di elaborazione vengono eseguite nel programma di installazione ODBC, in modo che non sia necessario caricare la dll di installazione del driver.  
  
## <a name="messages"></a>Messaggi  
 Una routine di installazione dei driver può inviare un messaggio di testo a un'applicazione come stringhe con terminazione null nel buffer di *lpszMsg* . Il messaggio verrà troncato in *cbMsgMax* meno il carattere di terminazione null dalla funzione **ConfigDriver** se è maggiore o uguale a *cbMsgMax* caratteri.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un driver|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)(nella DLL di installazione)|  
|Rimozione dell'origine dati predefinita|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|
