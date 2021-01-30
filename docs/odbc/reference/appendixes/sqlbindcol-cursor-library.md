---
description: SQLBindCol (libreria di cursori)
title: SQLBindCol (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocStmt function [ODBC], Cursor Library
ms.assetid: f4dd546a-0a6c-4397-8ee7-fafa6b9da543
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64b51f58fd51b9bcad1abd86750b024be131a8a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202943"
---
# <a name="sqlbindcol-cursor-library"></a>SQLBindCol (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLBindCol** nella libreria di cursori. Per informazioni generali su **SQLBindCol**, vedere [funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
 Un'applicazione alloca uno o più buffer affinché la libreria di cursori restituisca il set di righe corrente in. Chiama **SQLBindCol** una o più volte per associare questi buffer al set di risultati.  
  
 Un'applicazione può chiamare **SQLBindCol** per riassociare le colonne del set di risultati dopo aver chiamato **SQLExtendedFetch**, **SQLFetch** o **SQLFetchScroll**, purché il tipo di dati C, le dimensioni della colonna e le cifre decimali della colonna associata rimangano invariate. L'applicazione non deve chiudere il cursore per riassociare le colonne a indirizzi diversi.  
  
 La libreria di cursori supporta l'impostazione dell'attributo SQL_ATTR_ROW_BIND_OFFSET_PTR Statement per l'utilizzo degli offset di binding. Non è necessario chiamare **SQLBindCol** per eseguire questa operazione di riassociazione. Se la libreria di cursori viene utilizzata con un driver ODBC *3. x* , l'offset di binding non viene utilizzato quando viene chiamato **SQLFetch** . L'offset di binding viene utilizzato se **SQLFetch** viene chiamato quando la libreria di cursori viene utilizzata con un driver ODBC *2. x* perché viene quindi eseguito il mapping di **SQLFetch** a **SQLExtendedFetch**.  
  
 La libreria di cursori supporta la chiamata a **SQLBindCol** per associare la colonna del segnalibro.  
  
 Quando si utilizza un driver ODBC *2. x* , la libreria di cursori restituisce SQLSTATE HY090 (lunghezza di stringa o di buffer non valida) quando viene chiamato **SQLBindCol** per impostare la lunghezza del buffer per una colonna del segnalibro su un valore diverso da 4. Quando si utilizza un driver ODBC *3. x* , la libreria di cursori consente che il buffer sia di qualsiasi dimensione.
