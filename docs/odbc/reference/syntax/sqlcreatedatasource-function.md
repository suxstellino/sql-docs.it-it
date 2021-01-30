---
description: Funzione SQLCreateDataSource
title: Funzione SQLCreateDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCreateDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCreateDataSource
helpviewer_keywords:
- SQLCreateDataSource function [ODBC]
ms.assetid: 76ee851a-dca9-40cc-8e9e-eb3f74e560ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: feca3e6c135e4dba441a20b1fa458f990b437fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206110"
---
# <a name="sqlcreatedatasource-function"></a>Funzione SQLCreateDataSource
**Conformità**  
 Versione introdotta: ODBC 2,0  
  
 **Summary**  
 **SQLCreateDataSource** Visualizza una finestra di dialogo con cui l'utente può aggiungere un'origine dati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLCreateDataSource(  
     HWND    hwnd,  
     LPSTR   lpszDS);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HWND*  
 Input Handle della finestra padre.  
  
 *lpszDS*  
 Input Nome dell'origine dati. *lpszDS* può essere un puntatore null o una stringa vuota.  
  
## <a name="returns"></a>Restituisce  
 **SQLCreateDataSource** restituisce true se l'origine dati viene creata. In caso contrario, restituisce FALSE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLCreateDataSource** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_HWND|Handle di finestra non valido|Argomento *HWND* non valido o null.|  
|ODBC_ERROR_INVALID_DSN|DSN non valido|L'argomento *lpszDS* contiene una stringa non valida per un DSN.|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|La chiamata a **ConfigDSN** con l'opzione ODBC_ADD_DSN non è riuscita.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Non è stato possibile caricare la libreria di installazione del driver o del convertitore|Non è stato possibile caricare la libreria di installazione del driver.|  
|ODBC_ERROR_USER_CANCELED|Operazione annullata dall'utente|Creazione di una nuova origine dati annullata dall'utente.|  
|ODBC_ERROR_CREATE_DSN_FAILED|Non è stato possibile creare il DSN richiesto|Impossibile connettersi al database. la chiamata a **SQLDriverConnect** per un DSN su file non ha restituito una connessione riuscita.<br /><br /> Non è stato possibile scrivere nel file.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 Se *HWND* è null, **SQLCreateDataSource** restituisce false. In caso contrario, viene visualizzata la finestra di dialogo **Crea nuova origine dati** con una pagina della procedura guidata per la scelta del tipo di origine dati da impostare, come illustrato nella figura seguente.  
  
 ![Finestra di dialogo Crea nuova origine dati: selezione del tipo](../../../odbc/reference/syntax/media/ch23a.gif "CH23A")  
  
 L'opzione predefinita è **origine dati file**. Quando si sceglie un'origine dati e si fa clic su **Avanti** , viene visualizzata la pagina della procedura guidata seguente che contiene un elenco dei driver installati.  
  
 ![Finestra di dialogo Crea nuova origine dati: selezione del driver](../../../odbc/reference/syntax/media/ch23b.gif "CH23B")  
  
 Se si fa clic su **Annulla** , la finestra di dialogo scompare e **SQLCreateDataSource** restituisce false con il codice di errore ODBC_ERROR_USER_CANCELED. Se è stata selezionata l'opzione **origine dati utente** o **origine dati di sistema** , il pulsante **Avanzate** non è disponibile.  
  
 Quando si fa clic sul pulsante **Avanti** , si verifica una delle condizioni seguenti, a seconda del tipo di origine dati selezionato:  
  
-   Se è stata selezionata l'opzione **origine dati file** , viene visualizzata una pagina della procedura guidata che consente all'utente di immettere un nome di file.  
  
-   Se è stata selezionata un'origine **dati utente** o un' **origine dati di sistema** , viene visualizzata una pagina della procedura guidata che Visualizza il tipo di origine dati e il driver per la revisione e, quando si fa clic su **fine** , viene impostata l'origine dati.  
  
 Se si fa clic su **Avanzate** nella pagina Creazione guidata nuova origine dati, viene visualizzata una pagina della procedura guidata che consente all'utente di immettere informazioni specifiche del driver. Nella casella di testo della finestra di dialogo digitare il driver e le parole chiave separate da Returns, come illustrato nella figura seguente.  
  
 ![Finestra di dialogo Impostazioni avanzate creazione DNS su file](../../../odbc/reference/syntax/media/ch23c.gif "CH23C")  
  
 È possibile trovare altre parole chiave specifiche del driver con la descrizione di **SQLDriverConnect**. Sono consentiti tutti i **DSN** eccetto.  
  
 Il valore predefinito per l'opzione **Verifica connessione** è true. Questa impostazione predefinita si applica se questa pagina della procedura guidata è attivata o meno. Se si fa clic su **OK** , la stringa specificata nella casella di testo e il valore **verifica questa** opzione di connessione vengono memorizzati nella cache. Se si fa clic sul pulsante **Chiudi** o su **Annulla** , le informazioni specifiche del driver appena immesse andranno perse perché la stringa specificata nella casella di testo e il valore di verifica dell'opzione di **connessione** non vengono memorizzati nella cache.  
  
 Se è stata selezionata l'opzione **origine dati file** nella prima pagina della procedura guidata, dopo che è stato selezionato un driver ed è stato immesso il valore della parola chiave nella pagina avanzate della procedura guidata, all'utente viene richiesto di immettere un nome file. Fare clic su **Sfoglia** per cercare un nome di file, nel qual caso la directory predefinita nella casella **Sfoglia** viene specificata da una combinazione del percorso specificato da CommonFileDir in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion e "ODBC\DataSources". (Se CommonFileDir era "C:\Programmi C:\Programmi\File", la directory predefinita sarebbe "C:\Programmi\File Comuni\odbc\data Sources").  
  
 Quando viene immesso un nome file e viene fatto clic su **Avanti** , viene verificata la validità del nome file immesso rispetto alle regole di denominazione dei file standard del sistema operativo. Se il nome file non è valido, una finestra di messaggio di errore informa l'utente che è stato immesso un nome di file non valido. Dopo che l'utente ha riconosciuto la finestra di messaggio, lo stato attivo viene restituito alla pagina della procedura guidata in cui viene immesso il nome del file. Se il nome del file è valido, per la revisione viene visualizzata una pagina della procedura guidata che mostra le coppie parola chiave/valore selezionate, come illustrato nella figura seguente.  
  
 ![Finestra di dialogo Crea nuova origine dati: verifica](../../../odbc/reference/syntax/media/ch23d.gif "CH23D")  
  
 Se si fa clic su **fine** e si seleziona **origine dati file** come tipo di origine dati e se l' **opzione Verifica connessione** è true, **SQLDriverConnect** viene chiamato con le parole chiave **SaveFile** e **driver** . L'argomento *DriverCompletion* è impostato su SQL_DRIVER_COMPLETE. Il nome del file per la parola chiave **SaveFile** è il nome immesso o scelto e il nome del driver per la parola chiave **driver** è il nome scelto. Se nella pagina avanzate della procedura guidata è stata specificata una stringa di connessione specifica del driver, tale stringa verrà aggiunta dopo la parola chiave **driver** .  
  
 Se **SQLDriverConnect** restituisce SQL_SUCCESS, gestione driver ha creato il DSN del file. **SQLCreateDataSource** restituisce true. Se **SQLDriverConnect** non restituisce SQL_SUCCESS, viene visualizzata una finestra di messaggio di avviso che indica che non è stato possibile eseguire una connessione all'origine dati. È ancora possibile creare un DSN con le informazioni di connessione minime. Questa finestra di messaggio consente all'utente di annullare o continuare con la creazione del DSN del file.  
  
 Se l'utente sceglie di continuare a creare il DSN, questo processo continua come se l'opzione **Verifica connessione** fosse impostata su false. Se l'utente sceglie di annullare, viene restituito FALSE per **SQLCreateDataSource** con il codice di errore ODBC_ERROR_CREATE_DSN_FAILED.  
  
 Se l' **origine dati del file** è stata selezionata come tipo di origine dati e l'opzione **Verifica connessione** è false, viene creato un DSN file con la parola chiave **driver** e la stringa di connessione specificata dall'utente (se presente) dalla pagina avanzate della procedura guidata. Se la creazione del file ha avuto esito positivo, viene restituito TRUE per **SQLCreateDataSource**. Se la creazione del file non è riuscita, una finestra di messaggio di errore informa l'utente che l'errore è stato restituito dal sistema operativo. Viene restituito FALSE per **SQLCreateDataSource** con il codice di errore ODBC_ERROR_CREATE_DSN_FAILED. Per ulteriori informazioni sulle origini dati dei file, vedere [connessione tramite origini dati file](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Se è stata selezionata l'opzione **utente** o **origine dati di sistema** come tipo di origine dati, **ConfigDSN** nella libreria di installazione del driver viene chiamato con la ODBC_ADD_DSN *fRequest*. Per ulteriori informazioni, vedere [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Gestione delle origini dati|[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|
