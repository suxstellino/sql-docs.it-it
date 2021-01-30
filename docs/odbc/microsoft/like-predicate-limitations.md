---
description: Limitazioni del predicato LIKE
title: Limitazioni del predicato LIKE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- LIKE predicate limitations [ODBC]
- ODBC SQL grammar, LIKE predicate limitations
ms.assetid: dbd39099-caf6-4c4c-9ad8-f6c63c1bd5e4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4925ff83d8dafc1cce7c0a58a17685fcf30371f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199895"
---
# <a name="like-predicate-limitations"></a>Limitazioni del predicato LIKE
Se i dati in una colonna hanno una lunghezza superiore a 255 caratteri, il confronto LIKE sarà basato solo sui primi 255 caratteri.  
  
 Un oggetto LIKE usato in una routine è supportato solo con i modelli costanti. I driver di database desktop supportano SQL-92 come i criteri di ricerca.  
  
 L'uso di una clausola escape in un predicato LIKE non è supportato.  
  
 Un confronto LIKE non deve essere eseguito su una colonna contenente dati di tipo numerico o float. I risultati potrebbero essere imprevedibili. Per ulteriori informazioni, vedere la *Guida per programmatori Microsoft Jet motore di database*.
