---
description: Funzione SQLConfigDataSource
title: Funzione SQLConfigDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDataSource
helpviewer_keywords:
- SQLConfigDataSource function [ODBC]
ms.assetid: f8d6e342-c010-434e-b1cd-f5371fb50a14
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ddcfac8f1f211884aa3f46db3f56a2762778ed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165341"
---
# <a name="sqlconfigdatasource-function"></a>Funzione SQLConfigDataSource
**Conformità**  
 Versione introdotta: ODBC 1,0  
  
 **Summary**  
 **SQLConfigDataSource** aggiunge, modifica o Elimina le origini dati.  
  
 È possibile accedere alle funzionalità di **SQLConfigDataSource** anche con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLConfigDataSource(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hwndParent*  
 Input Handle della finestra padre. Se l'handle è null, nella funzione non verrà visualizzata alcuna finestra di dialogo.  
  
 *fRequest*  
 Input Tipo di richiesta. L'argomento *fRequest* deve contenere uno dei valori seguenti:  
  
 ODBC_ADD_DSN: aggiungere una nuova origine dati utente.  
  
 ODBC_CONFIG_DSN: configurare (modificare) un'origine dati utente esistente.  
  
 ODBC_REMOVE_DSN: rimuovere un'origine dati utente esistente.  
  
 ODBC_ADD_SYS_DSN: aggiungere una nuova origine dati di sistema.  
  
 ODBC_CONFIG_SYS_DSN: modificare un'origine dati di sistema esistente.  
  
 ODBC_REMOVE_SYS_DSN: rimuovere un'origine dati di sistema esistente.  
  
 ODBC_REMOVE_DEFAULT_DSN: rimuovere la sezione specifica dell'origine dati predefinita dalle informazioni di sistema. Rimuove anche la sezione relativa alla specifica del driver predefinita dalla voce Odbcinst.ini nelle informazioni sul sistema. Questo *fRequest* esegue la stessa funzione della funzione **SQLRemoveDefaultDataSource** deprecata. Quando si specifica questa opzione, tutti gli altri parametri nella chiamata a **SQLConfigDataSource** devono essere null; Se non sono NULL, verranno ignorati.  
  
 *lpszDriver*  
 Input Descrizione del driver (in genere il nome del DBMS associato) presentata agli utenti anziché al nome del driver fisico.  
  
 *lpszAttributes*  
 Input Elenco di attributi che termina doppiamente null sotto forma di coppie parola chiave/valore. Per ulteriori informazioni, vedere [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo. Se nelle informazioni di sistema non è presente alcuna voce quando viene chiamata questa funzione, la funzione restituisce FALSE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLConfigDataSource** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|L'argomento *hwndParent* non è valido o è null.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN ODBC_ADD_SYS_DSN ODBC_CONFIG_SYS_DSN ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DEFAULT_DSN|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszDriver* non è valido. Non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Coppie parola chiave/valore non valide|L'argomento *lpszAttributes* contiene un errore di sintassi.|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|Il programma di installazione non è riuscito a eseguire l'operazione richiesta dall'argomento *fRequest* . La chiamata a **ConfigDSN** non è riuscita.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Non è stato possibile caricare la libreria di installazione del driver o del convertitore|Non è stato possibile caricare la libreria di installazione del driver.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLConfigDataSource** usa il valore di *lpszDriver* per leggere il percorso completo della DLL di installazione del driver dalle informazioni di sistema. Carica la DLL e chiama **ConfigDSN** con gli stessi argomenti passati.  
  
 **SQLConfigDataSource** restituisce false se non è in grado di trovare o caricare la dll di installazione o se l'utente annulla la finestra di dialogo. In caso contrario, restituisce lo stato ricevuto da **ConfigDSN**.  
  
 **SQLConfigDataSource** esegue il mapping del DSN di sistema *FRequest* al DSN utente *fRequest* s (ODBC_ADD_SYS_DSN ODBC_ADD_DSN, ODBC_CONFIG_SYS_DSN ODBC_CONFIG_DSN e ODBC_REMOVE_SYS_DSN a ODBC_REMOVE_DSN). Per distinguere i DSN utente e di sistema, **SQLConfigDataSource** imposta la modalità di configurazione del programma di installazione in base alla tabella seguente. Prima di restituire, **SQLConfigDataSource** Reimposta la modalità di configurazione su BOTHDSN. **ConfigDSN** (implementato dai driver) deve chiamare **SQLWriteDSNToIni** e **SQLWritePrivateProfileString** per supportare un DSN di sistema. Per altre informazioni, vedere [funzione ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
|*fRequest*|Modalità di configurazione|  
|----------------|------------------------|  
|ODBC_ADD_DSN|USERDSN_ONLY|  
|ODBC_CONFIG_DSN|USERDSN_ONLY|  
|ODBC_REMOVE_DSN|USERDSN_ONLY|  
|ODBC_ADD_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_CONFIG_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_REMOVE_SYS_DSN|SYSTEMDSN_ONLY|  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un'origine dati|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (nella DLL di installazione)|  
|Rimozione di un nome di origine dati dalle informazioni di sistema|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Aggiunta di un nome di origine dati alle informazioni sul sistema|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
