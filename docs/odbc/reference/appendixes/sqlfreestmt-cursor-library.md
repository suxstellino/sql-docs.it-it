---
description: SQLFreeStmt (libreria di cursori)
title: SQLFreeStmt (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFreeStmt function [ODBC], Cursor Library
ms.assetid: 47bfbd4d-9453-4609-958d-1e05794cb223
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b7ebdcfc5a9997efb4301852b1aae3c1408464d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202821"
---
# <a name="sqlfreestmt-cursor-library"></a>SQLFreeStmt (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLFreeStmt** nella libreria di cursori. Per informazioni generali su **SQLFreeStmt**, vedere [funzione SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md).  
  
 Se un'applicazione chiama **SQLFreeStmt** con l'opzione SQL_UNBIND dopo aver chiamato **SQLExtendedFetch**, **SQLFetch** o **SQLFetchScroll**, la libreria di cursori restituisce un errore. Prima di poter separare le colonne del set di risultati, un'applicazione deve chiamare **SQLCloseCursor** o **SQLFreeStmt** con l'opzione SQL_CLOSE.
