---
description: Limitazioni della clausola WHERE
title: Limitazioni della clausola WHERE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, WHERE clause limitations
- WHERE clause limitations [ODBC]
ms.assetid: 46b54f74-e4a3-4318-87cf-8a97c38a2718
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80a0d29b57e5498c6d14e5d1e79600751d6f85c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176511"
---
# <a name="where-clause-limitations"></a>Limitazioni della clausola WHERE
Il numero massimo di clausole in una clausola WHERE è 40.  
  
 Le colonne LONGVARBINARY e LONGVARCHAR possono essere confrontate con valori letterali fino a 255 caratteri di lunghezza, ma non possono essere confrontati utilizzando parametri.
