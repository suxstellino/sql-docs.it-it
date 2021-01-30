---
description: Limitazioni della funzione CONVERT
title: Converti limitazioni funzione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, CONVERT function limitations
- Convert function limitations [ODBC]
ms.assetid: 3c81fc58-57f0-4dd7-be16-2b146eb15cbc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7f953f272b30095e31e68fa1bc2b6972bb4b6d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159552"
---
# <a name="convert-function-limitations"></a>Limitazioni della funzione CONVERT
Gli errori di conversione dei tipi comportano l'impostazione della colonna interessata su NULL.  
  
 Né il tipo di dati DATE né TIMESTAMP possono essere convertiti in un altro tipo di dati (o in sé) tramite la funzione CONVERT.
