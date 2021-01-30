---
description: Funzione SQLDriverToDataSource
title: Funzione SQLDriverToDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverToDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverToDataSource
helpviewer_keywords:
- SQLDriverToDataSource function [ODBC]
ms.assetid: 0de28eb5-8aa9-43e4-a87f-7dbcafe800dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ea0697dcedcf27992e771382b47a4b6713b1535
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211911"
---
# <a name="sqldrivertodatasource-function"></a>Funzione SQLDriverToDataSource
**SQLDriverToDataSource** supporta le traduzioni per i driver ODBC. Questa funzione non viene chiamata dalle applicazioni abilitate per ODBC. le applicazioni richiedono la conversione tramite **SQLSetConnectAttr**. Il driver associato al *connectionHandle* specificato in **SQLSETCONNECTATTR** chiama la DLL specificata per eseguire traduzioni di tutti i flussi di dati dal driver all'origine dati. È possibile specificare una DLL di traduzione predefinita nel file di inizializzazione ODBC.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLDriverToDataSource(  
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
 Input Tipo di dati SQL ODBC. Questo argomento indica al driver come convertire *rgbValueIn* in un form accettabile dall'origine dati. Per un elenco dei tipi di dati SQL validi, vedere [tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md).  
  
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
 Il driver chiama **SQLDriverToDataSource** per tradurre tutti i dati (istruzioni SQL, parametri e così via) passando dal driver all'origine dati. La DLL di traduzione potrebbe non tradurre alcuni dati, a seconda del tipo di dati e dello scopo della DLL di traduzione. Ad esempio, una DLL che converte i dati di tipo carattere da una tabella codici a un'altra ignora tutti i dati numerici e binari.  
  
 Il valore di *fOption* è impostato sul valore di *parametro vParam* specificato chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_TRANSLATE_OPTION. Si tratta di un valore a 32 bit che ha un significato specifico per una determinata DLL di traduzione. Ad esempio, è possibile specificare una determinata conversione del set di caratteri.  
  
 Se viene specificato lo stesso buffer per *rgbValueIn* e *rgbValueOut*, la conversione dei dati nel buffer verrà eseguita sul posto.  
  
 Anche se *cbValueIn*, *cbValueOutMax* e *pcbValueOut* sono di tipo SDWORD, **SQLDriverToDataSource** non supporta necessariamente puntatori enormi.  
  
 Se **SQLDriverToDataSource** restituisce false, il troncamento dei dati potrebbe essersi verificato durante la conversione. Se *pcbValueOut* (il numero di byte disponibili per restituire nel buffer di output) è maggiore di *cbValueOutMax* (lunghezza del buffer di output), si è verificato un troncamento. Il driver deve determinare se il troncamento è accettabile o meno. Se non si è verificato il troncamento, **SQLDriverToDataSource** ha restituito false a causa di un altro errore. In entrambi i casi, in *szErrorMsg* viene restituito un messaggio di errore specifico.  
  
 Per ulteriori informazioni sulla conversione dei dati, vedere [dll di traduzione](../../../odbc/reference/develop-app/translation-dlls.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Conversione dei dati restituiti dall'origine dati|[SQLDataSourceToDriver](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|  
|Restituzione dell'impostazione di un attributo di connessione|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Impostazione di un attributo di connessione|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
