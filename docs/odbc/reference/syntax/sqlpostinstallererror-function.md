---
description: Funzione SQLPostInstallerError
title: Funzione SQLPostInstallerError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPostInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPostInstallerError
helpviewer_keywords:
- SQLPostInstallerError function [ODBC]
ms.assetid: 4c60d827-b2d2-4f27-b220-daa9e1fcdd8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 72f3059e53e68cce0fc40286ae7c02d0e7eb38bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204514"
---
# <a name="sqlpostinstallererror-function"></a>Funzione SQLPostInstallerError
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLPostInstallerError** fornisce un meccanismo per la libreria di installazione di un driver o di un convertitore per segnalare gli errori per le funzioni **ConfigDriver**, **ConfigDSN** e **ConfigTranslator** alla coda degli errori del programma di installazione. Le applicazioni non usano questa API; usano **SQLInstallerError** per recuperare l'errore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
RETCODE SQLPostInstallerError(  
     DWORD    fErrorCode,  
     LPSTR    szErrorMsg);  
```  
  
## <a name="arguments"></a>Argomenti  
 *fErrorCode*  
 Input Codice di errore del programma di installazione.  
  
 *szErrorMsg*  
 Input Testo del messaggio di errore.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS o SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnostica  
 **SQLPostInstallerError** non pubblica i valori di errore per se stesso. Se l'errore è stato inserito correttamente nella coda degli errori del programma di installazione (recuperabile con **SQLInstallerError**), viene restituito SQL_SUCCESS. Se il valore nell'argomento *dwErrorCode* non è uno dei codici di errore del programma di installazione specificato, verrà restituito SQL_ERROR.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un driver|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)|  
|Aggiunta, modifica o rimozione di origini dati|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)|  
|Impostazione di un'opzione di traduzione|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|
