---
description: SQL_C_TCHAR
title: SQL_C_TCHAR | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_c_tchar [ODBC]
- pseudo-type identifiers [ODBC], SQL_C_TCHAR
- data types [ODBC], pseudo-type identifiers
ms.assetid: 9e27c8bd-ee15-4ce9-b70a-34cf1bf16f4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a399570a15f04d8ace61664a445c5283d629bf6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193050"
---
# <a name="sql_c_tchar"></a>SQL_C_TCHAR
L'identificatore del tipo di SQL_C_TCHAR non identifica effettivamente un tipo di dati. si tratta di una macro presente nel file di intestazione per la conversione Unicode. Viene sostituito da SQL_C_CHAR o SQL_C_WCHAR a seconda dell'impostazione del **#define** Unicode. È utile per un'applicazione che trasferisce dati di tipo carattere compilati come un'applicazione ANSI e Unicode.
