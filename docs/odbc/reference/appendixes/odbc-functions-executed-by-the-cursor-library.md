---
description: Funzioni ODBC eseguite dalla libreria di cursori
title: Funzioni ODBC eseguite dalla libreria di cursori | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- cursor library [ODBC], functions
- functions [ODBC], cursor library
- ODBC functions [ODBC], cursor library
- ODBC cursor library [ODBC], functions
ms.assetid: 2f1d3386-7e59-4d55-a5b4-3440b61343a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 68d5c4f12b2b2a3d408e2bd47d5f3c1d138b105e
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980991"
---
# <a name="odbc-functions-executed-by-the-cursor-library"></a>Funzioni ODBC eseguite dalla libreria di cursori
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori esegue le funzioni seguenti. Quando un'applicazione chiama una funzione in questo elenco, gestione driver richiama la libreria di cursori, non il driver. Si noti che la libreria di cursori può chiamare il driver durante l'esecuzione della funzione.  
  
:::row:::
   :::column span="":::
      **SQLBindCol**<br>      **SQLBindParam**<br>      **SQLBindParameter**<br>      **SQLCloseCursor**<br>      **SQLEndTran**<br>      **SQLExtendedFetch**<br>      **SQLFetchScroll**<br>      **SQLFreeHandle**<br>      **SQLFreeStmt**<br>      **SQLGetData**<br>      **SQLGetDescField**<br>      **SQLGetDescRec**<br>      **SQLGetFunctions**<br>      **SQLGetInfo**<br>      **SQLGetStmtAttr**
   :::column-end:::
   :::column span="":::
      **SQLGetStmtOption**<br>      **SQLNativeSql**<br>      **SQLNumParams**<br>      **SQLParamOptions**<br>      **SQLRowCount**<br>      **SQLSetConnectAttr**<br>      **SQLSetConnectOption**<br>      **SQLSetDescField**<br>      **SQLSetDescRec**<br>      **SQLSetPos**<br>      **SQLSetScrollOptions**<br>      **SQLSetStmtAttr**<br>      **SQLSetStmtOption**<br>      **SQLTransact**
   :::column-end:::
:::row-end:::
