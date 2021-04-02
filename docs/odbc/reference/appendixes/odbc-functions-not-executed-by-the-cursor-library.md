---
description: Funzioni ODBC non eseguite dalla libreria di cursori
title: Funzioni ODBC non eseguite dalla libreria di cursori | Microsoft Docs
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
ms.assetid: f2941522-75eb-4db9-9468-4800b884dac2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db4fac295d8f2426637ca90584df86267eb20dc9
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980879"
---
# <a name="odbc-functions-not-executed-by-the-cursor-library"></a>Funzioni ODBC non eseguite dalla libreria di cursori
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori non esegue le funzioni seguenti. Quando un'applicazione chiama una di queste funzioni, gestione driver richiama il driver, non la libreria di cursori.  
  
:::row:::
   :::column span="":::
      **SQLFetch**<br>      **SQLGetConnectAttr**<br>      **SQLGetDiagField**<br>      **SQLGetDiagRec**
   :::column-end:::
   :::column span="":::
      **SQLGetEnvAttr**<br>      **SQLSetDescRec**<br>      **SQLSetEnvAttr**  
   :::column-end:::
:::row-end:::

