---
description: Limitazioni dell'istruzione DELETE
title: Limitazioni dell'istruzione DELETE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE statement limitations [ODBC]
- ODBC SQL grammar, DELETE statement limitations
ms.assetid: 084761fe-e65b-4f38-ba4f-69884b2a7700
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b188d782f82e23d031b820bba5f56a30dad414bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181584"
---
# <a name="delete-statement-limitations"></a>Limitazioni dell'istruzione DELETE
L'istruzione DELETE non è supportata per Microsoft Excel o driver di testo. Si noti che l'istruzione INSERT è supportata per il driver di testo.  
  
 Il driver dBASE non supporta la compressione di una tabella per rimuovere i valori "eliminati".  
  
 Affinché il driver Paradox elimini una riga da una tabella, è necessario che la tabella disponga di un indice univoco (chiave primaria Paradox).
