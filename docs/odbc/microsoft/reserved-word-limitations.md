---
description: Limitazioni delle parole chiave riservate
title: Limitazioni di parole riservate | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: ed42f083-c9e8-4ee4-9d64-d879bf955c78
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e615ab4c8222ec7ac679f96f360458dfaab5abb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182567"
---
# <a name="reserved-keyword-limitations"></a>Limitazioni delle parole chiave riservate

Evitare di utilizzare parole chiave riservate ODBC come identificatori nelle tabelle SQL o negli oggetti correlati. Se si verifica un caso dispari in cui è necessario utilizzare una parola chiave riservata come identificatore, è necessario racchiudere l'identificatore con una coppia di *apice* inverso ('). Un altro nome *per l'* *apice inverso è indietro*.

La limitazione della parola chiave riservata si applica anche a qualsiasi forma abbreviata delle parole chiave riservate.

Un elenco di parole chiave riservate ODBC è disponibile all'indirizzo:

- [Parole chiave riservate di ODBC](../reference/appendixes/reserved-keywords.md).

- Nella *Guida di riferimento per programmatori ODBC* vedere [Appendice C: grammatica SQL](../reference/appendixes/appendix-c-sql-grammar.md).