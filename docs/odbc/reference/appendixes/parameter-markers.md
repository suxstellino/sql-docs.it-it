---
description: Marcatori di parametro
title: Marcatori di parametro | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- parameter markers [ODBC]
ms.assetid: 07213d04-cd31-45fd-a8c8-2e16e09eeaf4
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: 7ab9b0cd8682ecc722f97b563285bdd31665c1fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205341"
---
# <a name="parameter-markers"></a>Marcatori di parametro
In conformità con la specifica SQL-92, un'applicazione non può inserire marcatori di parametro nei percorsi seguenti. Per un elenco più completo, vedere la specifica di SQL-92.  
  
-   In un elenco di **selezione**  
  
-   Come entrambe le *espressioni* in un *predicato di confronto*  
  
-   Come entrambi gli operandi di un operatore binario  
  
-   Sia il primo che il secondo operando di un'operazione **between**  
  
-   Sia il primo che il terzo operando di un'operazione **between**  
  
-   Sia l'espressione che il primo valore di un'operazione **in**  
  
-   Come operando di un'operazione unaria + o-  
  
-   Come argomento di *set-Function-Reference*  
  
 Per ulteriori informazioni sugli indicatori di parametro, vedere la specifica SQL-92. Per ulteriori informazioni sui parametri, vedere [parametri di istruzione](../../../odbc/reference/develop-app/statement-parameters.md).
