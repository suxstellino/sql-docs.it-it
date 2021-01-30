---
description: SQLGetFunctions (libreria di cursori)
title: SQLGetFunctions (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetFunctions function [ODBC], Cursor Library
ms.assetid: 931acd12-4eb6-4a78-9a77-157a18a9a2d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9414aa4d2be551ab3b6a44193f6b6c17e2deb145
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202759"
---
# <a name="sqlgetfunctions-cursor-library"></a>SQLGetFunctions (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLGetFunctions** nella libreria di cursori. Per informazioni generali su **SQLGetFunctions**, vedere [funzione SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md).  
  
 Quando si chiama **SQLGetFunctions**, la libreria di cursori restituisce il supporto di **SQLExtendedFetch**, **SQLFetchScroll**, **SQLSetPos** e **SQLSetScrollOptions**, oltre alle funzioni supportate dal driver.
