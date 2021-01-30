---
description: SQLGetStmtAttr (libreria di cursori)
title: SQLGetStmtAttr (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6c34e1ef-4273-4afb-a7d3-f9017ab69c5e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 19f40228dea31ccd3c3491aa522877464983870b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202740"
---
# <a name="sqlgetstmtattr-cursor-library"></a>SQLGetStmtAttr (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLGetStmtAttr** nella libreria di cursori. Per informazioni generali su **SQLGetStmtAttr**, vedere [funzione SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md).  
  
 La libreria di cursori supporta gli attributi di istruzione seguenti con **SQLGetStmtAttr**:  

:::row:::
    :::column:::
        SQL_ATTR_CONCURRENCY  
        SQL_ATTR_CURSOR_TYPE  
        SQL_ATTR_FETCH_BOOKMARK_PTR  
        SQL_ATTR_PARAM_BIND_OFFSET_PTR  
        SQL_ATTR_PARAM_BIND_TYPE  
    :::column-end:::
    :::column:::
        SQL_ATTR_ROW_ARRAY_SIZE  
        SQL_ATTR_ROW_BIND_OFFSET_PTR  
        SQL_ATTR_ROW_BIND_TYPE  
        SQL_ATTR_ROW_NUMBER  
        SQL_ATTR_SIMULATE_CURSOR  
    :::column-end:::
:::row-end:::
