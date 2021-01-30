---
description: Funzione ConfigDriver
title: Funzione ConfigDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDriver
helpviewer_keywords:
- ConfigDriver [ODBC]
ms.assetid: 9473f48f-bcae-4784-89c1-7839bad4ed13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5abe589b84eff738aeaf41139082781254fb0488
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174741"
---
# <a name="configdriver-function"></a>Funzione ConfigDriver
**Conformità**  
 Versione introdotta: ODBC 2,5  
  
 **Summary**  
 **ConfigDriver** consente a un programma di installazione di eseguire le funzioni di installazione e disinstallazione senza richiedere al programma di chiamare **ConfigDSN**. Questa funzione eseguirà funzioni specifiche del driver, ad esempio la creazione di informazioni di sistema specifiche del driver e l'esecuzione di conversioni DSN durante l'installazione, nonché la pulizia delle modifiche delle informazioni di sistema durante la disinstallazione. Questa funzione è esposta dalla DLL di installazione del driver o da una DLL di installazione separata.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL ConfigDriver(  
      HWND    hwndParent,  
      WORD    fRequest,  
      LPCSTR  lpszDriver,  
      LPCSTR  lpszArgs,  
      LPSTR   lpszMsg,  
      WORD    cbMsgMax,  
      WORD *  pcbMsgOut);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hwndParent*  
 Input Handle della finestra padre. Se l'handle è null, nella funzione non verrà visualizzata alcuna finestra di dialogo.  
  
 *fRequest*  
 Input Tipo di richiesta. L'argomento *fRequest* deve contenere uno dei valori seguenti:  
  
 ODBC_INSTALL_DRIVER: installare un nuovo driver.  
  
 ODBC_REMOVE_DRIVER: rimuovere un driver.  
  
 Questa opzione può anche essere specifica del driver, nel qual caso l'argomento *fRequest* per la prima opzione deve iniziare da ODBC_CONFIG_DRIVER_MAX + 1. L'argomento *fRequest* per qualsiasi opzione aggiuntiva deve iniziare anche da un valore maggiore di ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Input Nome del driver registrato nella chiave Odbcinst.ini delle informazioni sul sistema.  
  
 *lpszArgs*  
 Input Stringa con terminazione null che contiene gli argomenti per un *fRequest* specifico del driver.  
  
 *lpszMsg*  
 Output Stringa con terminazione null contenente un messaggio di output generato dall'installazione del driver.  
  
 *cbMsgMax*  
 Input Lunghezza di *lpszMsg*.  
  
 *pcbMsgOut*  
 Output Numero totale di byte disponibili da restituire in *lpszMsg*.  
  
 Se il numero di byte disponibili per restituire è maggiore o uguale a *cbMsgMax*, il messaggio di output in *lpszMsg* viene troncato in *cbMsgMax* meno il carattere di terminazione null. L'argomento *pcbMsgOut* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **ConfigDriver** restituisce false, un valore *\* pfErrorCode* associato viene inserito nel buffer di errore del programma di installazione mediante una chiamata a **SQLPostInstallerError** e può essere ottenuto chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|L'argomento *hwndParent* non è valido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> L'opzione specifica del driver era minore o uguale a ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszDriver* non è valido. Non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|Non è stato possibile eseguire l'operazione richiesta dall'argomento *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Errore specifico del driver o del convertitore|Errore specifico del driver per il quale non è stato definito alcun errore del programma di installazione ODBC. L'argomento *SzError* in una chiamata alla funzione **SQLPostInstallerError** deve contenere il messaggio di errore specifico del driver.|  
  
## <a name="comments"></a>Commenti  
  
### <a name="driver-specific-options"></a>Opzioni di Driver-Specific  
 Un'applicazione può richiedere funzionalità specifiche del driver esposte dal driver tramite l'argomento *fRequest* . Il valore di *fRequest* per la prima opzione sarà ODBC_CONFIG_DRIVER_MAX più 1 e le opzioni aggiuntive verranno incrementate di 1 da tale valore. Tutti gli argomenti richiesti dal driver per la funzione devono essere specificati in una stringa con terminazione null passata nell'argomento *lpszArgs* . I driver che forniscono tali funzionalità devono gestire una tabella di opzioni specifiche del driver. Le opzioni devono essere completamente documentate nella documentazione del driver. I writer di applicazioni che utilizzano opzioni specifiche del driver devono tenere presente che l'applicazione renderà meno interoperabile.  
  
### <a name="messages"></a>Messaggi  
 Una routine di installazione dei driver può inviare un messaggio di testo a un'applicazione come stringa con terminazione null nel buffer di *lpszMsg* . Il messaggio verrà troncato in *cbMsgMax* meno il carattere di terminazione null dalla funzione **ConfigDriver** se è maggiore o uguale a *cbMsgMax* caratteri.
