---
description: Funzione SQLGetInstalledDrivers
title: Funzione SQLGetInstalledDrivers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetInstalledDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInstalledDrivers
helpviewer_keywords:
- SQLGetInstalledDrivers function [ODBC]
ms.assetid: a1983a2e-0edf-422e-bd1b-ec5db40a34bc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f770254874b09158632ebaee071964c00265a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180953"
---
# <a name="sqlgetinstalleddrivers-function"></a>Funzione SQLGetInstalledDrivers
**Conformità**  
 Versione introdotta: ODBC 1,0  
  
 **Summary**  
 **SQLGetInstalledDrivers** legge la sezione [driver ODBC] delle informazioni di sistema e restituisce un elenco di descrizioni dei driver installati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLGetInstalledDrivers(  
     LPSTR   lpszBuf,  
     WORD    cbBufMax,  
     WORD *  pcbBufOut);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszBuf*  
 Output Elenco di descrizioni dei driver installati. Per informazioni sulla struttura dell'elenco, vedere "Commenti".  
  
 *cbBufMax*  
 Input Lunghezza di *lpszBuf*.  
  
 *pcbBufOut*  
 Output Numero totale di byte (escluso il byte di terminazione null) restituito in *lpszBuf*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbBufMax*, l'elenco di descrizioni dei driver in *lpszBuf* viene troncato a *cbBufMax* meno il carattere di terminazione null. L'argomento *pcbBufOut* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetInstalledDrivers** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszBuf* è null o non valido oppure l'argomento *cbBufMax* è minore o uguale a 0.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Componente non trovato nel registro di sistema|Il programma di installazione non è riuscito a trovare la sezione [driver ODBC] nel registro di sistema.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 Ogni descrizione del driver viene terminata con un byte null e l'intero elenco viene terminato con un byte null. Ovvero due byte null contrassegnano la fine dell'elenco. Se il buffer allocato non è sufficientemente grande da mantenere l'intero elenco, l'elenco viene troncato senza errori. Se viene passato un puntatore null come *lpszBuf*, viene restituito un errore.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione di attributi e descrizioni dei driver|[SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|
