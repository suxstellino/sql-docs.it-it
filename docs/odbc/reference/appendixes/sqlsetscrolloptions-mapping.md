---
description: Mapping di SQLSetScrollOptions
title: Mapping di SQLSetScrollOptions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetScrollOptions
ms.assetid: a0fa4510-8891-4a61-a867-b2555bc35f05
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 105443a2b33a1903f1f22affef1c6ca70ee2cfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202601"
---
# <a name="sqlsetscrolloptions-mapping"></a>Mapping di SQLSetScrollOptions
Quando un'applicazione chiama **SQLSetScrollOptions** tramite un driver ODBC *3. x* e il driver non supporta **SQLSetScrollOptions**, la chiamata a  
  
```  
SQLSetScrollOptions(StatementHandle, Concurrency, KeysetSize, RowsetSize)  
```  
  
 il risultato è il seguente:  
  
-   Una chiamata a  
  
    ```  
    SQLGetInfo(ConnectionHandle, InfoType, InfoValuePtr, BufferLength, StringLengthPtr)  
    ```  
  
     con l'argomento *InfoType* impostato su uno dei valori nella tabella seguente, a seconda del valore dell'argomento *KeysetSize* in **SQLSetScrollOptions**.  
  
    |*Argomento KeysetSize*|*Argomento InfoType*|  
    |---------------------------|-------------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_STATIC|SQL_STATIC_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_DYNAMIC|SQL_DYNAMIC_CURSOR_ATTRIBUTES2|  
    |Un valore maggiore dell'argomento *RowsetSize*|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
  
     Se il valore dell'argomento *KeysetSize* non è elencato nella tabella precedente, la chiamata a **SQLSetScrollOptions** restituisce SQLSTATE S1107 (valore di riga non compreso nell'intervallo) e non viene eseguito nessuno dei passaggi seguenti.  
  
     Gestione driver verifica quindi se il bit appropriato è impostato nel valore **InfoValuePtr* restituito dalla chiamata a **SQLGetInfo**, in base al valore dell'argomento di *concorrenza* in **SQLSetScrollOptions**.  
  
    |Argomento di *concorrenza*|Impostazione *InfoType*|  
    |----------------------------|------------------------|  
    |SQL_CONCUR_READ_ONLY|SQL_CA2_READ_ONLY_CONCURRENCY|  
    |SQL_CONCUR_LOCK|SQL_CA2_LOCK_CONCURRENCY|  
    |SQL_CONCUR_ROWVER|SQL_CA2_ROWVER_CONCURRENCY|  
    |SQL_CONCUR_VALUES|SQL_CA2_VALUES_CONCURRENCY|  
  
     Se l'argomento della *concorrenza* non è uno dei valori nella tabella precedente, la chiamata a **SQLSetScrollOptions** restituisce SQLSTATE S1108 (opzione di concorrenza non compresa nell'intervallo) e non viene eseguito nessuno dei seguenti passaggi. Se il bit appropriato (come indicato nella tabella precedente) non è impostato in **InfoValuePtr* su uno dei valori corrispondenti all'argomento della *concorrenza* , la chiamata a **SQLSetScrollOptions** restituisce SQLSTATE S1C00 (driver non idoneo) e non viene eseguito nessuno dei passaggi seguenti.  
  
-   Una chiamata a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CURSOR_TYPE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* impostato su uno dei valori nella tabella seguente, in base al valore dell'argomento *KeysetSize* in **SQLSetScrollOptions**.  
  
    |Argomento *KeysetSize*|*\*ValuePtr*|  
    |---------------------------|------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_CURSOR_FORWARD_ONLY|  
    |SQL_SCROLL_STATIC|SQL_CURSOR_STATIC|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_CURSOR_KEYSET_DRIVEN|  
    |SQL_SCROLL_DYNAMIC|SQL_CURSOR_DYNAMIC|  
    |Un valore maggiore dell'argomento *RowsetSize*|SQL_CURSOR_KEYSET_DRIVEN|  
  
-   Una chiamata a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CONCURRENCY, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* impostato sull'argomento della *concorrenza* in **SQLSetScrollOptions**.  
  
-   Se l'argomento *KeysetSize* nella chiamata a **SQLSetScrollOptions** è positivo, una chiamata a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_KEYSET_SIZE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* impostato sull'argomento *KeysetSize* in **SQLSetScrollOptions**.  
  
-   Una chiamata a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ROWSET_SIZE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* impostato sull'argomento *RowsetSize* in **SQLSetScrollOptions**.  
  
    > [!NOTE]  
    >  Quando Gestione driver esegue il mapping di **SQLSetScrollOptions** per un'applicazione che utilizza un driver ODBC *3. x* che non **supporta SQLSetScrollOptions**, gestione driver imposta l'opzione dell'istruzione SQL_ROWSET_SIZE, non l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE, sull'argomento *RowsetSize* in **SQLSetScrollOption**. Di conseguenza, **SQLSetScrollOptions** non può essere utilizzato da un'applicazione durante il recupero di più righe mediante una chiamata a **SQLFetch** o **SQLFetchScroll**. Può essere utilizzato solo quando si recuperano più righe tramite una chiamata a **SQLExtendedFetch**.
