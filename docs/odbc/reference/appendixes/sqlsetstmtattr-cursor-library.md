---
description: SQLSetStmtAttr (libreria di cursori)
title: SQLSetStmtAttr (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6018a733-c2c8-4047-92ec-92cf85031767
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a395052df9eb5623e63463e3e79722769583721
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202583"
---
# <a name="sqlsetstmtattr-cursor-library"></a>SQLSetStmtAttr (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLSetStmtAttr** nella libreria di cursori. Per informazioni generali su **SQLSetStmtAttr**, vedere [funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
 La libreria di cursori supporta gli attributi di istruzione seguenti con **SQLSetStmtAttr**:  

:::row:::
    :::column:::
        SQL_ATTR_CONCURRENCY  
        SQL_ATTR_CURSOR_TYPE  
        SQL_ATTR_FETCH_BOOKMARK_PTR  
        SQL_ATTR_PARAM_BIND_OFFSET_PTR  
        SQL_ATTR_PARAM_BIND_TYPE  
    :::column-end:::
    :::column:::
        SQL_ATTR_ROW_BIND_OFFSET_PTR  
        SQL_ATTR_ROW_BIND_TYPE  
        SQL_ATTR_ROWSET_ARRAY_SIZE  
        SQL_ATTR_SIMULATE_CURSOR  
        SQL_ATTR_USE_BOOKMARKS  
    :::column-end:::
:::row-end:::

 La libreria di cursori supporta solo i valori SQL_CURSOR_FORWARD_ONLY e SQL_CURSOR_STATIC dell'attributo SQL_ATTR_CURSOR_TYPE Statement.  
  
 Per i cursori di sola trasmissione, la libreria di cursori supporta il SQL_CONCUR_READ_ONLY valore dell'attributo SQL_ATTR_CONCURRENCY Statement. Per i cursori statici, la libreria di cursori supporta i valori SQL_CONCUR_READ_ONLY e SQL_CONCUR_VALUES dell'attributo SQL_ATTR_CONCURRENCY Statement.  
  
 La libreria di cursori supporta solo il valore SQL_SC_NON_UNIQUE dell'attributo SQL_ATTR_SIMULATE_CURSOR Statement.  
  
 Sebbene la specifica ODBC supporti le chiamate a **SQLSetStmtAttr** con gli attributi SQL_ATTR_PARAM_BIND_TYPE o SQL_ATTR_ROW_BIND_TYPE dopo la chiamata a **SQLFetch** o **SQLFetchScroll** , la libreria di cursori non lo è. Prima di poter modificare il tipo di associazione nella libreria di cursori, l'applicazione deve chiudere il cursore. La libreria di cursori supporta la modifica degli attributi di istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR, SQL_ATTR_PARAM_BIND_OFFSET_PTR, SQL_ATTR_ROWS_FETCHED_PTR e SQL_ATTR_PARAMS_PROCESSED_PTR quando un cursore è aperto.  
  
 Un'applicazione può chiamare **SQLSetStmtAttr** con un **attributo** di SQL_ATTR_ROW_ARRAY_SIZE per modificare la dimensione del set di righe mentre un cursore è aperto. Le nuove dimensioni del set di righe diverranno effettive alla successiva chiamata a **SQLFetchScroll** o **SQLFetch** .  
  
 La libreria di cursori supporta l'impostazione dell'attributo di istruzione SQL_ATTR_PARAM_BIND_OFFSET_PTR o SQL_ATTR_ROW_BIND_OFFSET_PTR per abilitare gli offset di binding. L'offset dell'associazione non verrà utilizzato per le chiamate a **SQLFetch** quando la libreria di cursori viene utilizzata con ODBC 2. driver *x* .  
  
 La libreria di cursori supporta l'impostazione dell'attributo SQL_ATTR_USE_BOOKMARKS Statement su SQL_UB_VARIABLE.
