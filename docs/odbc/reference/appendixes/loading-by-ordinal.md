---
description: Caricamento per ordinale
title: Caricamento in base al numero ordinale | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], loading by ordinal
- compatibility [ODBC], loading by ordinal
- loading by ordinal [ODBC]
ms.assetid: 337d90ab-68eb-4940-a2f3-f7d5693ee766
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ffb2516a79144a5ae79b21e6621882056b1f69ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184370"
---
# <a name="loading-by-ordinal"></a>Caricamento per ordinale
In ODBC *2. x* è possibile eseguire il caricamento in base al numero ordinale per migliorare le prestazioni del processo di connessione. Un driver ODBC *2. x* esporta una funzione fittizia con l'ordinale 199; Quando la gestione driver la rileva, risolve gli indirizzi delle funzioni ODBC in base al numero ordinale, non in base al nome. Questa funzionalità è ancora supportata per i driver ODBC *2. x* , ma non è supportata per i driver ODBC *3. x* .
