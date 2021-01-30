---
description: Funzione SQLValidDSN
title: Funzione SQLValidDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLValidDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLValidDSN
helpviewer_keywords:
- SQLValidDSN [ODBC]
ms.assetid: 930d1d89-337a-4429-85a2-84ee10555ac9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62692ed4b5d0ab600ceb36e87e7f796c77120c89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191709"
---
# <a name="sqlvaliddsn-function"></a>Funzione SQLValidDSN
**Conformità**  
 Versione introdotta: ODBC 2,0  
  
 **Summary**  
 **SQLValidDSN** controlla la lunghezza e la validità del nome dell'origine dati prima che il nome venga aggiunto alle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLValidDSN(  
     LPCSTR    lpszDSN);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszDSN*  
 Input Nome dell'origine dati da controllare.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se il nome dell'origine dati è valido. Restituisce FALSE se il nome dell'origine dati non è valido o la chiamata di funzione non è riuscita.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLValidDSN** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. Un *\* pfErrorCode* viene restituito solo se la chiamata di funzione ha esito negativo, non se è stato restituito false perché il nome dell'origine dati non è valido. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLValidDSN** viene chiamato dal [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) di un driver per verificare la lunghezza del nome dell'origine dati e la validità dei singoli caratteri nel nome dell'origine dati. Controlla se la lunghezza del nome è maggiore di SQL_MAX_DSN_LENGTH, come definito in sqlext. h. La lunghezza del nome dell'origine dati viene controllata anche da [SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md). **SQLValidDSN** controlla se nel nome dell'origine dati è incluso uno dei seguenti caratteri non validi:  
  
 [ ] { } ( ) , ; ? * = ! \@ \  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un'origine dati|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (nella DLL di installazione)|  
|Aggiunta, modifica o rimozione di un'origine dati|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Scrittura di un nome di origine dati nelle informazioni di sistema|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
