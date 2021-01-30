---
description: Mapping di SQLSetStmtOption
title: Mapping di SQLSetStmtOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLSetStmtOption
- SQLSetStmtOption function [ODBC], mapping
ms.assetid: 6a9921aa-8a53-4668-9b13-87164062f1e5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8907de6d6ac80737ba0bb47ca3c6954a482c857f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202576"
---
# <a name="sqlsetstmtoption-mapping"></a>Mapping di SQLSetStmtOption
Quando un'applicazione chiama **SQLSetStmtOption** tramite un driver ODBC *3. x* , la chiamata a  
  
```  
SQLSetStmtOption(StatementHandle, fOption, vParam)  
```  
  
 il risultato è il seguente:  
  
-   Se *fOption* indica un attributo di istruzione definito da ODBC che è una stringa, gestione driver chiama  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, SQL_NTS)  
    ```  
  
-   Se *fOption* indica un attributo di istruzione definito da ODBC che restituisce un valore integer a 32 bit, gestione driver chiama  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, 0)  
    ```  
  
-   Se *fOption* indica un attributo di istruzione definito dal driver, gestione driver chiama  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, BufferLength)  
    ```  
  
 Nei tre casi precedenti, l'argomento **statementHandle** è impostato sul valore in *HSTMT*, l'argomento *attribute* è impostato sul valore in *fOption* e l'argomento *ValuePtr* è impostato sul valore come *parametro vParam*.  
  
 Poiché Gestione driver non sa se l'attributo di istruzione definito dal driver necessita di un valore integer stringa o a 32 bit, deve passare un valore valido per l'argomento *StringLength* di **SQLSetStmtAttr**. Se il driver ha definito una semantica speciale per gli attributi di istruzione definiti dal driver e deve essere chiamata usando **SQLSetStmtOption**, deve supportare **SQLSetStmtOption**.  
  
 Se un'applicazione chiama **SQLSetStmtOption** per impostare un'opzione di istruzione specifica del driver in un driver ODBC *3. x* e l'opzione è stata definita in una versione ODBC *2. x* del driver, è necessario definire una nuova costante manifesto per l'opzione nel driver ODBC *3. x* . Se la costante del manifesto precedente viene utilizzata nella chiamata a **SQLSetStmtOption**, gestione driver chiamerà **SQLSetStmtAttr** con l'argomento *StringLength* impostato su 0.  
  
 Quando un'applicazione chiama **SQLSetStmtAttr** per impostare SQL_ATTR_USE_BOOKMARKS SQL_UB_ON in un driver ODBC *3. x* , l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è impostato su SQL_UB_FIXED. SQL_UB_ON è la stessa costante SQL_UB_FIXED. Gestione driver passa SQL_UB_FIXED al driver. SQL_UB_FIXED è stato deprecato in ODBC *3. x*, ma un driver ODBC *3. x* deve implementarlo per funzionare con le applicazioni ODBC *2. x* che utilizzano segnalibri a lunghezza fissa.  
  
 Per un driver ODBC *3. x* , gestione driver non verifica più se l' *opzione* è compresa tra SQL_STMT_OPT_MIN e SQL_STMT_OPT_MAX oppure è maggiore di SQL_CONNECT_OPT_DRVR_START.
