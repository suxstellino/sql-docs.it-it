---
description: Limitazioni dell'istruzione ALTER TABLE
title: Limitazioni dell'istruzione ALTER TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ALTER TABLE statement limitations
- ALTER TABLE statement limitations [ODBC]
ms.assetid: f3e88f85-edf4-47cd-a822-292b106ddb34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d36e5dfb2457a9d3453dbe3bc877c5d267731ea1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205745"
---
# <a name="alter-table-statement-limitations"></a>Limitazioni dell'istruzione ALTER TABLE
Quando si utilizza il driver dBASE o Paradox, dopo la creazione di un indice e l'aggiunta di un nuovo record, la struttura della tabella non può essere modificata dall'istruzione ALTER TABLE, a meno che l'indice non venga eliminato e il contenuto della tabella venga eliminato.  
  
 Le istruzioni ALTER TABLE non sono supportate per i driver di Microsoft Excel o di testo.  
  
> [!NOTE]  
>  Quando si utilizza il driver Paradox senza implementare il motore di database Borland, le istruzioni ALTER TABLE non sono supportate. sono consentite solo istruzioni Read e Append.
