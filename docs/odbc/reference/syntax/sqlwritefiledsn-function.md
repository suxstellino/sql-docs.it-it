---
description: Funzione SQLWriteFileDSN
title: Funzione SQLWriteFileDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteFileDSN
helpviewer_keywords:
- SQLWriteFileDSN [ODBC]
ms.assetid: 9e18f56f-1061-416b-83d4-ffeec42ab5a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f778c2669a55e3f63c1946d2b4323ae64242c533
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191670"
---
# <a name="sqlwritefiledsn-function"></a>Funzione SQLWriteFileDSN
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLWriteFileDSN** scrive le informazioni in un DSN di file.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLWriteFileDSN(  
     LPCSTR     lpszFileName,  
     LPCSTR     lpszAppName,  
     LPCSTR     lpszKeyName,  
     LPCSTR     lpszString);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszFileName*  
 Input Puntatore al nome del DSN del file. Viene aggiunta un'estensione DSN a tutti i nomi di file che non dispongono già di un'estensione DSN.  
  
 *lpszAppName*  
 Input Puntatore al nome dell'applicazione. Si tratta di "ODBC" per la sezione ODBC.  
  
 *lpszKeyName*  
 Input Puntatore al nome della chiave da leggere. Vedere "Commenti" per le parole chiave riservate.  
  
 *lpszString*  
 Output Punta alla stringa associata alla chiave da scrivere. La lunghezza massima della stringa a cui punta questo argomento è 32.767 byte.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLWriteFileDSN** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_PATH|Percorso di installazione non valido|Il percorso del nome file specificato nell'argomento *lpszFileName* non è valido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo di richiesta non valido|L'argomento *lpszAppName*, *lpszKeyName* o *lpszString* è null.|  
  
## <a name="comments"></a>Commenti  
 ODBC riserva il nome della sezione [ODBC] in cui archiviare le informazioni di connessione. Le parole chiave riservate per questa sezione sono le stesse riservate per una stringa di connessione in **SQLDriverConnect**. Per ulteriori informazioni, vedere la descrizione della funzione [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) .  
  
 Le applicazioni possono utilizzare queste parole chiave riservate per scrivere informazioni direttamente in un DSN di file. Se un'applicazione vuole creare o modificare la stringa di connessione senza DSN associata al DSN di un file, può chiamare **SQLWriteFileDSN** per qualsiasi parola chiave della stringa di connessione riservata nella sezione [ODBC].  
  
 Se l'argomento *lpszString* è un puntatore null, la parola chiave a cui fa riferimento l'argomento *lpszKeyName* verrà eliminata dal file con estensione DSN. Se gli argomenti *lpszString* e *lpszKeyName* sono entrambi puntatori null, la sezione a cui punta l'argomento *lpszAppName* verrà eliminata dal file con estensione DSN.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Lettura di informazioni da file DSN|[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|
