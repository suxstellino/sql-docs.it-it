---
description: Funzione SQLInstallDriverEx
title: Funzione SQLInstallDriverEx | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverEx
helpviewer_keywords:
- SQLInstallDriverEx function [ODBC]
ms.assetid: 1dd74544-f4e9-46e1-9b5f-c11d84fdab4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ebdfa732ba9c5a4a539009600749563daff4a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189703"
---
# <a name="sqlinstalldriverex-function"></a>Funzione SQLInstallDriverEx
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLInstallDriverEx** aggiunge informazioni sul driver alla voce Odbcinst.ini nelle informazioni sul sistema e incrementa di 1 il *UsageCount* del driver. Tuttavia, se esiste già una versione del driver, ma il valore *UsageCount* per il driver non esiste, il nuovo valore di *UsageCount* viene impostato su 2.  
  
 Questa funzione non copia effettivamente alcun file. È responsabilità del programma chiamante copiare correttamente i file del driver nella directory di destinazione.  
  
 È possibile accedere alle funzionalità di **SQLInstallDriverEx** anche con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLInstallDriverEx(  
     LPCSTR    lpszDriver,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszDriver*  
 Input Descrizione del driver (in genere il nome del DBMS associato) presentata agli utenti anziché al nome del driver fisico. L'argomento *lpszDriver* deve contenere un elenco doppiato con terminazione null di coppie parola chiave/valore che descrivono il driver. Per ulteriori informazioni sulle coppie parola chiave/valore, vedere [sottochiavi di specifica driver](../../../odbc/reference/install/driver-specification-subkeys.md). Per ulteriori informazioni sulla stringa doppia con terminazione null, vedere [funzione ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
 *lpszPathIn*  
 Input Percorso completo della directory di destinazione dell'installazione o un puntatore null. Se *lpszPathIn* è un puntatore null, i driver verranno installati nella directory di sistema.  
  
 *lpszPathOut*  
 Output Percorso della directory di destinazione in cui deve essere installato il driver. Se il driver non è stato installato in precedenza, *lpszPathOut* deve essere uguale a *lpszPathIn*. Se il driver è stato installato in precedenza, *lpszPathOut* è il percorso dell'installazione precedente.  
  
 *cbPathOutMax*  
 Input Lunghezza di *lpszPathOut*.  
  
 *pcbPathOut*  
 Output Numero totale di byte (escluso il carattere di terminazione null) disponibili per restituire in *lpszPathOut*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbPathOutMax*, il percorso di output in *lpszPathOut* viene troncato in *cbPathOutMax* meno il carattere di terminazione null. L'argomento *pcbPathOut* può essere un puntatore null.  
  
 *fRequest*  
 Input Tipo di richiesta. L'argomento *fRequest* deve contenere uno dei valori seguenti:  
  
 ODBC_INSTALL_INQUIRY: informazioni sulla posizione in cui è possibile installare un driver.  
  
 ODBC_INSTALL_COMPLETE: completare la richiesta di installazione.  
  
 *lpdwUsageCount*  
 Output Conteggio di utilizzo del driver dopo la chiamata a questa funzione.  
  
 Le applicazioni non devono impostare il conteggio dell'utilizzo. Il conteggio verrà mantenuto da ODBC.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLInstallDriverEx** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszPathOut* non è sufficientemente grande da contenere il percorso di output. Il buffer contiene il percorso troncato.<br /><br /> L'argomento *cbPathOutMax* è 0 e *fRequest* è stato ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Coppie parola chiave/valore non valide|L'argomento *lpszDriver* contiene un errore di sintassi.|  
|ODBC_ERROR_INVALID_PATH|Percorso di installazione non valido|L'argomento *lpszPathIn* contiene un percorso non valido.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Non è stato possibile caricare la libreria di installazione del driver o del convertitore|Non è stato possibile caricare la libreria di installazione del driver.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Sequenza di parametri non valida|L'argomento *lpszDriver* non contiene un elenco di coppie parola chiave/valore.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Non è stato possibile incrementare o decrementare il numero di utilizzi dei componenti|Il programma di installazione non è riuscito a incrementare il conteggio di utilizzo del driver.|  
  
## <a name="comments"></a>Commenti  
 L'argomento *lpszDriver* è un elenco di attributi sotto forma di coppie parola chiave/valore. Ogni coppia viene terminata con un byte null e l'intero elenco viene terminato con un byte null. Ovvero due byte null contrassegnano la fine dell'elenco. Il formato di questo elenco è il seguente:  
  
 _driver-desc_ **\\** **=**_driver 0DRIVER-dll-filename_ **\\** 0 [installazione installazione **=** _-dll-nomefile_ <b>\\</b> 0]  
  
 [_driver-attr-keyword1_ **=** _value1_ <b>\\</b> 0] [_driver-attr-keyword2_ **=** _value2_ <b>\\</b> 0]... <b>\\</b> 0  
  
 dove \ 0 è un byte null e *driver-attr-* keywordn è qualsiasi parola chiave dell'attributo del driver. Le parole chiave devono essere visualizzate nell'ordine specificato. Si supponga, ad esempio, che un driver per i file di testo formattati disponga di dll di installazione e driver separate e che sia possibile utilizzare file con le estensioni TXT e CSV. L'argomento *lpszDriver* per questo driver potrebbe essere il seguente:  
  
```  
Text\0Driver=TEXT.DLL\0Setup=TXTSETUP.DLL\0FileUsage=1\0  
FileExtns=*.txt,*.csv\0\0  
```  
  
 Si supponga che un driver per SQL Server non disponga di una DLL di installazione separata e che non includa parole chiave degli attributi del driver. L'argomento *lpszDriver* per questo driver potrebbe essere il seguente:  
  
```  
SQL Server\0Driver=SQLSRVR.DLL\0\0  
```  
  
 Dopo che **SQLInstallDriverEx** recupera le informazioni sul driver dall'argomento *lpszDriver* , aggiunge la descrizione del driver alla sezione [ODBC Drivers] della voce Odbcinst.ini nelle informazioni di sistema. Viene quindi creata una sezione intitolata con la descrizione del driver e vengono aggiunti i percorsi completi della DLL del driver e della DLL di installazione. Infine, restituisce il percorso della directory di destinazione dell'installazione ma non copia i file del driver. Il programma chiamante deve effettivamente copiare i file del driver nella directory di destinazione.  
  
 **SQLInstallDriverEx** incrementa il numero di utilizzo del componente per il driver installato di 1. Se esiste già una versione del driver, ma il numero di utilizzo del componente per il driver non esiste, il nuovo valore di conteggio utilizzo componenti viene impostato su 2.  
  
 Il programma di installazione dell'applicazione è responsabile della copia fisica del file del driver e della gestione del numero di utilizzo del file. Se il file del driver non è stato installato in precedenza, il programma di installazione dell'applicazione deve copiare il file nel percorso *lpszPathIn* e creare il conteggio di utilizzo del file. Se il file è stato precedentemente installato, il programma di installazione incrementa semplicemente il conteggio di utilizzo dei file e restituisce il percorso dell'installazione precedente nell'argomento *lpszPathOut* .  
  
> [!NOTE]  
>  Per ulteriori informazioni sui conteggi di utilizzo dei componenti e sui conteggi di utilizzo dei file, vedere [conteggio dell'utilizzo](../../../odbc/reference/install/usage-counting.md).  
  
 Se una versione precedente del file di driver è stata precedentemente installata dall'applicazione, è necessario disinstallare e reinstallare il driver, in modo che il numero di utilizzo del componente driver sia valido. È necessario chiamare prima **SQLConfigDriver** (con un *fRequest* di ODBC_REMOVE_DRIVER), quindi chiamare **SQLRemoveDriver** per decrementare il numero di utilizzo dei componenti. **SQLInstallDriverEx** deve quindi essere chiamato per reinstallare il driver, incrementando il numero di utilizzo dei componenti. Il programma di installazione dell'applicazione deve sostituire il file precedente con il nuovo file. Il conteggio di utilizzo file rimarrà invariato e qualsiasi altra applicazione che utilizzava il file della versione precedente utilizzerà la versione più recente.  
  
> [!NOTE]  
>  Se il driver è stato installato in precedenza e viene chiamato **SQLInstallDriverEx** per installare il driver in una directory diversa, la funzione restituirà true, ma *lpszPathOut* includerà la directory in cui il driver è già installato. Non verrà inclusa la directory immessa nell'argomento *lpszDriver* .  
  
 La lunghezza del percorso in *lpszPathOut* in **SQLInstallDriverEx** consente un processo di installazione in due fasi, in modo che un'applicazione possa determinare quale *cbPathOutMax* deve essere chiamando **SQLInstallDriverEx** con una *fRequest* di modalità ODBC_INSTALL_INQUIRY. Verrà restituito il numero totale di byte disponibili nel buffer *pcbPathOut* . **SQLInstallDriverEx** può quindi essere chiamato con un *fRequest* di ODBC_INSTALL_COMPLETE e l'argomento *cbPathOutMax* impostato sul valore nel buffer *pcbPathOut* , oltre al carattere di terminazione null.  
  
 Se si sceglie di non usare il modello a due fasi per **SQLInstallDriverEx**, è necessario impostare *cbPathOutMax*, che definisce le dimensioni della risorsa di archiviazione per il percorso della directory di destinazione, sul valore _MAX_PATH, come definito in STDLIB. h, per evitare il troncamento.  
  
 Quando *fRequest* è ODBC_INSTALL_COMPLETE, **SQLInstallDriverEx** non consente a *lpszPathOut* di essere null (o *cbPathOutMax* è 0). Se *fRequest* è ODBC_INSTALL_COMPLETE, viene restituito false quando il numero di byte disponibili per restituire è maggiore o uguale a *cbPathOutMax*, con il risultato che si verifica il troncamento.  
  
 Dopo che **SQLInstallDriverEx** è stato chiamato e il programma di installazione dell'applicazione ha copiato il file del driver (se necessario), è necessario che la dll di installazione del driver chiami **SQLConfigDriver** per impostare la configurazione per il driver.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Installazione di Gestione driver|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
