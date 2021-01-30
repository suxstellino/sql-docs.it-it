---
description: Funzione ConfigDSN
title: Funzione ConfigDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDSN
helpviewer_keywords:
- ConfigDSN [ODBC]
ms.assetid: 01ced74e-c575-4a25-83f5-bd7d918123f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42e85b0c77ccc71946836e3103953fc85206b6e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174730"
---
# <a name="configdsn-function"></a>Funzione ConfigDSN
**Conformità**  
 Versione introdotta: ODBC 1,0  
  
 **Summary**  
 **ConfigDSN** aggiunge, modifica o Elimina le origini dati dalle informazioni di sistema. Potrebbe richiedere informazioni di connessione all'utente. Può trovarsi nella DLL del driver o in una DLL di installazione separata.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL ConfigDSN(  
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
  
 ODBC_ADD_DSN: aggiungere una nuova origine dati.  
  
 ODBC_CONFIG_DSN: configurare (modificare) un'origine dati esistente.  
  
 ODBC_REMOVE_DSN: rimuovere un'origine dati esistente.  
  
 *lpszDriver*  
 Input Descrizione del driver (in genere il nome del DBMS associato) presentata agli utenti anziché al nome del driver fisico.  
  
 *lpszAttributes*  
 Input Elenco di attributi che termina doppiamente null sotto forma di coppie parola chiave/valore. Per ulteriori informazioni, vedere "Commenti".  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **ConfigDSN** restituisce false, un valore *\* pfErrorCode* associato viene inserito nel buffer di errore del programma di installazione mediante una chiamata a **SQLPostInstallerError** e può essere ottenuto chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|L'argomento *hwndParent* non è valido.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Coppie parola chiave/valore non valide|L'argomento *lpszAttributes* contiene un errore di sintassi.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszDriver* non è valido. Non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|Non è stato possibile eseguire l'operazione richiesta dall'argomento *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Errore specifico del driver o del convertitore|Errore specifico del driver per il quale non è stato definito alcun errore del programma di installazione ODBC. L'argomento *SzError* in una chiamata alla funzione **SQLPostInstallerError** deve contenere il messaggio di errore specifico del driver.|  
  
## <a name="comments"></a>Commenti  
 **ConfigDSN** riceve le informazioni di connessione dalla dll del programma di installazione come un elenco di attributi sotto forma di coppie parola chiave/valore. Ogni coppia viene terminata con un byte null e l'intero elenco viene terminato con un byte null. Ovvero due byte null contrassegnano la fine dell'elenco. Gli spazi non sono consentiti intorno al segno di uguale nella coppia parola chiave/valore. **ConfigDSN** può accettare parole chiave che non sono parole chiave valide per **SQLBrowseConnect** e **SQLDriverConnect**. **ConfigDSN** non supporta necessariamente tutte le parole chiave valide per **SQLBrowseConnect** e **SQLDriverConnect**. (**ConfigDSN** non accetta la parola chiave **driver** ). Le parole chiave usate dalla funzione **ConfigDSN** devono supportare tutte le opzioni necessarie per ricreare l'origine dati usando la funzionalità di installazione automatica del programma di installazione. Quando gli utilizzi dei valori **ConfigDSN** e i valori della stringa di connessione sono uguali, è necessario utilizzare le stesse parole chiave.  
  
 Come in **SQLBrowseConnect** e **SQLDriverConnect**, le parole chiave e i relativi valori non devono contenere **[] {} (),;? \* =! @** characters e il valore della parola chiave **DSN** non possono essere costituiti solo da spazi vuoti. Grazie alla grammatica del registro di sistema, le parole chiave e i nomi delle origini dati non possono contenere il carattere barra rovesciata ( \\ ).  
  
 **ConfigDSN** deve chiamare **SQLValidDSN** per verificare la lunghezza del nome dell'origine dati e per verificare che nel nome non siano inclusi caratteri non validi. Se il nome dell'origine dati supera SQL_MAX_DSN_LENGTH o include caratteri non validi, **SQLValidDSN** restituisce un errore e **ConfigDSN** restituisce un errore. La lunghezza del nome dell'origine dati viene controllata anche da **SQLWriteDSNToIni**.  
  
 Per configurare, ad esempio, un'origine dati che richiede un ID utente, una password e un nome di database, un'applicazione di installazione potrebbe passare le seguenti coppie parola chiave-valore:  
  
```  
DSN=Personnel Data\0UID=Smith\0PWD=Sesame\0DATABASE=Personnel\0\0  
```  
  
 Per ulteriori informazioni su queste parole chiave, vedere [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) e la documentazione di ogni driver.  
  
 Per visualizzare una finestra di dialogo, *hwndParent* non deve essere null.  
  
## <a name="adding-a-data-source"></a>Aggiunta di un'origine dati  
 Se il nome di un'origine dati viene passato a **ConfigDSN** in *lpszAttributes*, **ConfigDSN** verifica che il nome sia valido. Se il nome dell'origine dati corrisponde a un nome di origine dati esistente e *hwndParent* è null, **ConfigDSN** sovrascrive il nome esistente. Se corrisponde a un nome esistente e *hwndParent* non è null, **ConfigDSN** richiede all'utente di sovrascrivere il nome esistente.  
  
 Se *lpszAttributes* contiene informazioni sufficienti per connettersi a un'origine dati, **ConfigDSN** può aggiungere l'origine dati o visualizzare una finestra di dialogo in cui l'utente può modificare le informazioni di connessione. Se *lpszAttributes* non contiene informazioni sufficienti per connettersi a un'origine dati, **ConfigDSN** deve determinare le informazioni necessarie; Se *hwndParent* non è null, viene visualizzata una finestra di dialogo per recuperare le informazioni dall'utente.  
  
 Se **ConfigDSN** Visualizza una finestra di dialogo, deve visualizzare tutte le informazioni di connessione passate in *lpszAttributes*. In particolare, se è stato passato un nome di origine dati, **ConfigDSN** Visualizza tale nome ma non consente all'utente di modificarlo. **ConfigDSN** può fornire valori predefiniti per le informazioni di connessione non passate in *lpszAttributes*.  
  
 Se **ConfigDSN** non è in grado di ottenere informazioni di connessione complete per un'origine dati, restituisce false.  
  
 Se **ConfigDSN** è in grado di ottenere informazioni complete sulla connessione per un'origine dati, chiama **SQLWriteDSNToIni** nella dll del programma di installazione per aggiungere la nuova specifica dell'origine dati al file di Odbc.ini (o al registro di sistema). **SQLWriteDSNToIni** aggiunge il nome dell'origine dati alla sezione [origini dati ODBC], crea la sezione relativa alla specifica dell'origine dati e aggiunge la parola chiave **driver** con la descrizione del driver come valore. **ConfigDSN** chiama **SQLWritePrivateProfileString** nella dll del programma di installazione per aggiungere eventuali parole chiave e valori aggiuntivi usati dal driver.  
  
## <a name="modifying-a-data-source"></a>Modifica di un'origine dati  
 Per modificare un'origine dati, è necessario passare un nome dell'origine dati a **ConfigDSN** in *lpszAttributes*. **ConfigDSN** verifica che il nome dell'origine dati si trovi nel file Odbc.ini (o nel registro di sistema).  
  
 Se *hwndParent* è null, **ConfigDSN** usa le informazioni in *lpszAttributes* per modificare le informazioni nel file di Odbc.ini (o nel registro di sistema). Se *hwndParent* non è null, **ConfigDSN** Visualizza una finestra di dialogo usando le informazioni in *lpszAttributes*; per informazioni non in *lpszAttributes*, vengono usate le informazioni delle informazioni di sistema. L'utente può modificare le informazioni prima che **ConfigDSN** le memorizzi nelle informazioni di sistema.  
  
 Se il nome dell'origine dati è stato modificato, **ConfigDSN** prima chiama **SQLRemoveDSNFromIni** nella dll del programma di installazione per rimuovere la specifica dell'origine dati esistente dal file Odbc.ini (o dal registro di sistema). Segue quindi i passaggi della sezione precedente per aggiungere la nuova specifica dell'origine dati. Se il nome dell'origine dati non è stato modificato, **ConfigDSN** chiama **SQLWritePrivateProfileString** nella dll del programma di installazione per apportare altre modifiche. **ConfigDSN** non può eliminare o modificare il valore della parola chiave del **driver** .  
  
## <a name="deleting-a-data-source"></a>Eliminazione di un'origine dati  
 Per eliminare un'origine dati, è necessario passare un nome dell'origine dati a **ConfigDSN** in *lpszAttributes*. **ConfigDSN** verifica che il nome dell'origine dati si trovi nel file Odbc.ini (o nel registro di sistema). Chiama quindi **SQLRemoveDSNFromIni** nella dll del programma di installazione per rimuovere l'origine dati.  
  
## <a name="note"></a>Nota
 Se si scrive una versione Unicode di questa routine, è necessario chiamarla **ConfigDSNW**, con argomenti LPCWSTR anziché LPCSTR.
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un'origine dati|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Ottenere un valore dal file Odbc.ini o dal registro di sistema|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|  
|Rimozione dell'origine dati predefinita|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Rimozione di un nome di origine dati da Odbc.ini (o registro di sistema)|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Aggiunta di un nome dell'origine dati a Odbc.ini (o registro di sistema)|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|  
|Scrittura di un valore nel file di Odbc.ini o nel registro di sistema|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
