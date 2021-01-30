---
description: Funzione SQLInstallTranslatorEx
title: Funzione SQLInstallTranslatorEx | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslatorEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslatorEx
helpviewer_keywords:
- SQLInstallTranslatorEx function [ODBC]
ms.assetid: a0630602-53c1-4db0-98ce-70d160aedf8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e055f3f3565c6240134bf43b4b4f8951059e2725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189644"
---
# <a name="sqlinstalltranslatorex-function"></a>Funzione SQLInstallTranslatorEx
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLInstallTranslatorEx** aggiunge informazioni su un convertitore alla sezione Odbcinst.ini delle informazioni di sistema (HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI chiave del registro di sistema \ODBC Translator).  
  
 È possibile accedere alle funzionalità di **SQLInstallTranslatorEx** anche con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLInstallTranslatorEx(  
     LPCSTR    lpszTranslator,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszTranslator*  
 Input Questo deve contenere un elenco con terminazione doppia per coppie parola chiave/valore che descrivono il traduttore. Per ulteriori informazioni sulla sintassi delle coppie parola chiave/valore, vedere la pagina relativa alle [sottochiavi di specifica del traduttore](../../../odbc/reference/install/translator-specification-subkeys.md).  
  
 Le parole chiave **Translator** e **Setup** devono essere incluse nella stringa *lpszTranslator* . La DLL di traduzione viene elencata con la parola chiave **Translator** e la dll di installazione del convertitore viene elencata con la parola chiave **Setup** . Ogni coppia viene terminata con un byte NULL e l'intero elenco viene terminato con un byte NULL. Ovvero due byte NULL contrassegnano la fine dell'elenco. Il formato di *lpszTranslator* è il seguente:  
  
 \0Translator =*Translator-dll-filename*\ 0 [Setup =*Setup-dll-filename*\ 0] \ 0  
  
 *lpszPathIn*  
 Input Percorso completo della posizione in cui deve essere installato il convertitore o puntatore null. Se *lpszPath* è un puntatore null, i convertitori verranno installati nella directory di sistema.  
  
 *lpszPathOut*  
 Output Percorso della directory di destinazione in cui deve essere installato il traduttore. Se il convertitore non è mai stato installato, *lpszPathOut* è uguale a *lpszPathIn*. Se è presente un'installazione precedente del traduttore, *lpszPathOut* è il percorso dell'installazione precedente.  
  
 *cbPathOutMax*  
 Input Lunghezza di *lpszPathOut.*  
  
 *pcbPathOut*  
 Output Numero totale di byte disponibili da restituire in *lpszPathOut*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbPathOutMax*, il percorso di output in *lpszPathOut* viene troncato in *pcbPathOutMax* meno il carattere di terminazione null. L'argomento *pcbPathOut* può essere un puntatore null.  
  
 *fRequest*  
 Input Tipo di richiesta. *fRequest* deve contenere uno dei valori seguenti:  
  
 ODBC_INSTALL_INQUIRY: informazioni sulla posizione in cui è possibile installare un convertitore.  
  
 ODBC_INSTALL_COMPLETE: completare la richiesta di installazione.  
  
 *lpdwUsageCount*  
 Output Conteggio di utilizzo del traduttore dopo la chiamata a questa funzione.  
  
 Le applicazioni non devono impostare il conteggio dell'utilizzo. Il conteggio verrà mantenuto da ODBC.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLInstallTranslatorEx** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszPathOut* non è sufficientemente grande da contenere il percorso di output. Il buffer contiene il percorso troncato.<br /><br /> L'argomento *cbPathOutMax* è 0 e l'argomento *fRequest* è stato ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *fRequest* non è uno dei seguenti:<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Coppie parola chiave/valore non valide|L'argomento *lpszTranslator* contiene un errore di sintassi.|  
|ODBC_ERROR_INVALID_PATH|Percorso di installazione non valido|L'argomento *lpszPathIn* contiene un percorso non valido.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Sequenza di parametri non valida|L'argomento *lpszTranslator* non contiene un elenco di coppie parola chiave/valore.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Non è stato possibile incrementare o decrementare il numero di utilizzo del componente del registro|Il programma di installazione non è riuscito a incrementare il conteggio di utilizzo del traduttore.|  
  
## <a name="comments"></a>Commenti  
 **SQLInstallTranslatorEx** fornisce un meccanismo per installare solo il traduttore. Questa funzione non copia effettivamente alcun file. Il programma chiamante è responsabile della copia dei file di conversione.  
  
 **SQLInstallTranslatorEx** incrementa di 1 il conteggio di utilizzo del componente per il traduttore installato. Se una versione del traduttore esiste già, ma il conteggio di utilizzo del componente per il traduttore non esiste, il nuovo valore di conteggio utilizzo componenti viene impostato su 2.  
  
 Il programma di installazione dell'applicazione è responsabile della copia fisica del file di traduzione e della gestione del numero di utilizzo del file. Se il file di conversione non è stato installato in precedenza, il programma di installazione dell'applicazione deve copiare il file o i file e creare il conteggio di utilizzo di file o file. Se il file è stato precedentemente installato, il programma di installazione incrementa semplicemente il numero di utilizzo del file.  
  
 Se una versione precedente del traduttore è stata precedentemente installata dall'applicazione, è necessario disinstallare e reinstallare il convertitore, in modo che il conteggio di utilizzo del componente di conversione sia valido. **SQLRemoveTranslator** deve essere chiamato per decrementare il numero di utilizzo del componente, quindi è necessario chiamare **SQLInstallTranslatorEx** per incrementare il conteggio di utilizzo dei componenti. Il programma di installazione dell'applicazione deve sostituire il file o i file obsoleti con il nuovo file. Il conteggio di utilizzo file rimarrà invariato e altre applicazioni che utilizzano il file della versione precedente utilizzeranno ora la versione più recente.  
  
 La lunghezza del percorso in *lpszPathOut* in **SQLInstallTranslatorEx** consente un processo di installazione in due fasi, in modo che un'applicazione possa determinare quale *cbPathOutMax* deve essere chiamando **SQLInstallTranslatorEx** con una *fRequest* di modalità ODBC_INSTALL_INQUIRY. Verrà restituito il numero totale di byte disponibili nel buffer *pcbPathOut* . **SQLInstallTranslatorEx** può quindi essere chiamato con un *fRequest* di ODBC_INSTALL_COMPLETE e l'argomento *cbPathOutMax* impostato sul valore nel buffer *pcbPathOut* , oltre al carattere di terminazione null.  
  
 Se si sceglie di non usare il modello a due fasi per **SQLInstallTranslatorEx**, è necessario impostare *cbPathOutMax*, che definisce le dimensioni della risorsa di archiviazione per il percorso della directory di destinazione, sul valore _MAX_PATH, come definito in STDLIB. h, per evitare il troncamento.  
  
 Quando *fRequest* è ODBC_INSTALL_COMPLETE, **SQLInstallTranslatorEx** non consente a *lpszPathOut* di essere null (o *cbPathOutMax* è 0). Se *fRequest* è ODBC_INSTALL_COMPLETE, viene restituito false quando il numero di byte disponibili per restituire è maggiore o uguale a *cbPathOutMax*, con il risultato che si verifica il troncamento.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione di un'opzione di conversione predefinita|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Selezione di traduttori|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Rimozione di traduttori|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
