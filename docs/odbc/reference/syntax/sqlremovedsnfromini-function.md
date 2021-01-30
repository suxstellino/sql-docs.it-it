---
description: Funzione SQLRemoveDSNFromIni
title: Funzione SQLRemoveDSNFromIni | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDSNFromIni
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDSNFromIni
helpviewer_keywords:
- SQLRemoveDSNFromIni function [ODBC]
ms.assetid: bb2e8273-7b61-4113-bfc8-f7ccc607c811
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 26bbcf3bf68dda172220323cb7d9b0dc39e3178d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192485"
---
# <a name="sqlremovedsnfromini-function"></a>Funzione SQLRemoveDSNFromIni
**Conformità**  
 Versione introdotta: ODBC 1,0  
  
 **Summary**  
 **SQLRemoveDSNFromIni** rimuove un'origine dati dalle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLRemoveDSNFromIni(  
     LPCSTR   lpszDSN);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszDSN*  
 Input Nome dell'origine dati da rimuovere.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se l'origine dati viene rimossa o se l'origine dati non è presente nel file di Odbc.ini. Restituisce FALSE se non è possibile rimuovere l'origine dati.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLRemoveDSNFromIni** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_DSN|DSN non valido|L'argomento *lpszDSN* non è valido.|  
|ODBC_ERROR_REQUEST_FAILED|Richiesta non riuscita|Il programma di installazione non è riuscito a rimuovere le informazioni DSN dal registro di sistema.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLRemoveDSNFromIni** rimuove il nome dell'origine dati dalla sezione [origini dati ODBC] delle informazioni sul sistema. Consente inoltre di rimuovere la sezione specifica dell'origine dati dalle informazioni di sistema.  
  
 Questa funzione deve essere chiamata solo da una libreria di installazione driver.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un'origine dati|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)|  
|Aggiunta, modifica o rimozione di un'origine dati|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Rimozione dell'origine dati predefinita|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Aggiunta di un nome di origine dati alle informazioni sul sistema|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
