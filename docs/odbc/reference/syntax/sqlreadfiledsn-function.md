---
description: Funzione SQLReadFileDSN
title: Funzione SQLReadFileDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLReadFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLReadFileDSN
helpviewer_keywords:
- SQLReadFileDSN function [ODBC]
ms.assetid: ead464aa-cdc3-47dd-a0c0-997711205d31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7a04747258f612a86746642ae11fb06e9b742662
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192540"
---
# <a name="sqlreadfiledsn-function"></a>Funzione SQLReadFileDSN
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLReadFileDSN** legge le informazioni dal DSN di un file.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLReadFileDSN(  
     LPCSTR   lpszFileName,  
     LPCSTR   lpszAppName,  
     LPCSTR   lpszKeyName,  
     LPSTR    lpszString,  
     WORD     cbString,  
     WORD *   pcbString);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszFileName*  
 Input Puntatore al buffer di dati contenente il nome del file con estensione DSN. Un'estensione DSN viene aggiunta a tutti i nomi di file che non dispongono già di un'estensione DSN. Il valore di *\* lpszFileName* deve essere una stringa con terminazione null.  
  
 *lpszAppName*  
 Input Puntatore al buffer di dati contenente il nome dell'applicazione. Si tratta di "ODBC" per la sezione ODBC. Il valore di *\* lpszAppName* deve essere una stringa con terminazione null.  
  
 *lpszKeyName*  
 Input Puntatore al buffer di dati contenente il nome della chiave da leggere. Vedere "Commenti" per le parole chiave riservate. Il valore di *\* lpszAppName* deve essere una stringa con terminazione null.  
  
 *lpszString*  
 Output Puntatore al buffer di dati contenente la stringa associata alla chiave da leggere.  
  
 Se *\* lpszFileName* è un nome di file con estensione DSN valido ma l'argomento *lpszAppName* è un puntatore null e l'argomento *lpszKeyName* è un puntatore null, *\* lpszString* contiene un elenco di applicazioni valide. Se *\* lpszFileName* è un nome di file. DSN valido e *\* lpszAppName* è un nome di applicazione valido, ma l'argomento *lpszKeyName* è un puntatore null, *\* lpszString* contiene un elenco di parole chiave riservate valide nella sezione appropriata del file DSN, delimitato da punti e virgola. Se *\* lpszFileName* è un nome di file con estensione DSN valido, ma *\* lpszAppName* è un puntatore null e l'argomento *lpszKeyName* è un puntatore null, *\* lpszString* contiene un elenco delle sezioni del file DSN, delimitate da punti e virgola.  
  
 *cbString*  
 Input Lunghezza del buffer *\* lpszString* .  
  
 *pcbString*  
 Output Numero totale di byte disponibili da restituire in *\* lpszString*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbString*, la stringa di output in *\* lpszString* viene troncata a *cbString* meno il carattere di terminazione null. L'argomento *pcbString* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLReadFileDSN** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszString* è null.<br /><br /> L'argomento *cbString* è minore o uguale a 0.|  
|ODBC_ERROR_INVALID_PATH|Percorso di installazione non valido|Il percorso del nome file specificato nell'argomento *lpszFileName* non è valido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *lpszAppName* è null, mentre l'argomento *lpszKeyName* è valido.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
|ODBC_ERROR_OUTPUT_STRING_TRUNCATED|Stringa di output troncata|La stringa restituita in *\* lpszString* è stata troncata perché il valore in *cbString* era minore o uguale al valore di *\* pcbString*.|  
|ODBC_ERROR_REQUEST_FAILED|Richiesta non riuscita|La parola chiave non esisteva nel DSN del file.|  
  
## <a name="comments"></a>Commenti  
 ODBC riserva il nome della sezione [ODBC] in cui archiviare le informazioni di connessione. Le parole chiave riservate per questa sezione sono le stesse riservate per una stringa di connessione in **SQLDriverConnect**. Per ulteriori informazioni, vedere la descrizione della funzione [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) .  
  
 Le applicazioni possono utilizzare queste parole chiave riservate per leggere le informazioni contenute in un DSN di file. Se un'applicazione desidera trovare la stringa di connessione senza DSN associata al DSN di un file, può chiamare **SQLReadFileDSN** per qualsiasi parola chiave della stringa di connessione riservata nella sezione [ODBC]. La stringa di connessione completa passata in una connessione senza DSN è una combinazione di tutte le parole chiave (riservate e specifiche del driver) nella sezione [ODBC].  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Scrittura di informazioni in un DSN di file|[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|
