---
description: Funzione SQLDataSourceToDriver
title: Funzione SQLDataSourceToDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSourceToDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSourceToDriver
helpviewer_keywords:
- SQLDataSourceToDriver function [ODBC]
ms.assetid: 0d87fcac-30a0-4303-ad8f-a5b53f4b428d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 20e4a4cf601990f014655f3c067e9304cc5f4388
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206091"
---
# <a name="sqldatasourcetodriver-function"></a>Funzione SQLDataSourceToDriver
**SQLDataSourceToDriver** supportstranslations per i driver ODBC. Questa funzione non viene chiamata dalle applicazioni abilitate per ODBC. le applicazioni richiedono la conversione tramite **SQLSetConnectAttr**. Il driver associato al *connectionHandle* specificato in **SQLSETCONNECTATTR** chiama la DLL specificata per eseguire le traduzioni di tutti i dati che scorrono dall'origine dati al driver. È possibile specificare una DLL di traduzione predefinita nel file di inizializzazione ODBC.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLDataSourceToDriver(  
     UDWORD     fOption,  
     SWORD      fSqlType,  
     PTR        rgbValueIn,  
     SDWORD     cbValueIn,  
     PTR        rgbValueOut,  
     SDWORD     cbValueOutMax,  
     SDWORD *   pcbValueOut,  
     UCHAR *    szErrorMsg,  
     SWORD      cbErrorMsgMax,  
     SWORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Argomenti  
 *fOption*  
 Input Valore dell'opzione.  
  
 *fSqlType*  
 Input Tipo di dati SQL. Questo argomento indica al driver come convertire *rgbValueIn* in un modulo accettabile per l'applicazione. Per un elenco dei tipi di dati SQL validi, vedere la sezione tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati.  
  
 *rgbValueIn*  
 Input Valore da tradurre.  
  
 *cbValueIn*  
 Input Lunghezza di *rgbValueIn*.  
  
 *rgbValueOut*  
 Output Risultato della conversione.  
  
> [!NOTE]  
>  La DLL di conversione non è null. terminare questo valore.  
  
 *cbValueOutMax*  
 Input Lunghezza di *rgbValueOut*.  
  
 *pcbValueOut*  
 Output Il numero totale di byte, escluso il byte di terminazione null, disponibile per restituire in *rgbValueOut*.  
  
 Per i dati di tipo carattere o binario, se è maggiore o uguale a *cbValueOutMax*, i dati in *rgbValueOut* vengono troncati a *cbValueOutMax* byte.  
  
 Per tutti gli altri tipi di dati, il valore di *cbValueOutMax* viene ignorato e la dll di traduzione presuppone che la dimensione di *rgbValueOut* sia la dimensione del tipo di dati C predefinito del tipo di dati SQL specificato con *fSqlType*.  
  
 L'argomento *pcbValueOut* può essere un puntatore null.  
  
 *szErrorMsg*  
 Output Puntatore alla risorsa di archiviazione per un messaggio di errore. Si tratta di una stringa vuota a meno che la traduzione non abbia esito negativo.  
  
 *cbErrorMsgMax*  
 Input Lunghezza di *szErrorMsg*.  
  
 *pcbErrorMsg*  
 Output Puntatore al numero totale di byte, escluso il byte di terminazione null, disponibile per restituire in *szErrorMsg*. Se è maggiore o uguale a *cbErrorMsg*, i dati in *szErrorMsg* vengono troncati a *cbErrorMsgMax* meno il carattere di terminazione null. L'argomento *pcbErrorMsg* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 TRUE se la conversione ha avuto esito positivo, FALSE se la conversione non è riuscita.  
  
## <a name="comments"></a>Commenti  
 Il driver chiama **SQLDataSourceToDriver** per tradurre ALLDATA (dati del set di risultati, nomi di tabella, conteggi di righe, messaggi di errore e così via) passando dall'origine dati al driver. La DLL di traduzione potrebbe non tradurre alcuni dati, a seconda del tipo di dati e dello scopo della DLL di traduzione. ad esempio, una DLL che converte i dati di tipo carattere da una tabella codici a un'altra ignora tutti i dati numerici e binari.  
  
 Il valore di *fOption* è impostato sul valore di *parametro vParam* specificato chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_TRANSLATE_OPTION. Si tratta di un valore a 32 bit che ha un significato specifico per una determinata DLL di traduzione. Ad esempio, è possibile specificare una determinata conversione del set di caratteri.  
  
 Se viene specificato lo stesso buffer per *rgbValueIn* e *rgbValueOut*, la conversione dei dati nel buffer verrà eseguita sul posto.  
  
 Anche se *cbValueIn*, *cbValueOutMax* e *pcbValueOut* sono di tipo SDWORD, **SQLDataSourceToDriver** non supporta necessariamente puntatori enormi.  
  
 Se **SQLDataSourceToDriver** restituisce false, il troncamento dei dati potrebbe essersi verificato durante la conversione. Se *pcbValueOut* (il numero di byte disponibili per restituire nel buffer di output) è maggiore di *cbValueOutMax* (lunghezza del buffer di output), si è verificato un troncamento. Il driver deve determinare se il troncamento è accettabile. Se non si è verificato il troncamento, **SQLDataSourceToDriver** ha restituito false a causa di un altro errore. In entrambi i casi, in *szErrorMsg* viene restituito un messaggio di errore specifico.  
  
 Per ulteriori informazioni sulla conversione dei dati, vedere [dll di traduzione](../../../odbc/reference/develop-app/translation-dlls.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Conversione dei dati inviati all'origine dati|[SQLDriverToDataSource](../../../odbc/reference/syntax/sqldrivertodatasource-function.md)|  
|Restituzione dell'impostazione di un attributo di connessione|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Impostazione di un attributo di connessione|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
