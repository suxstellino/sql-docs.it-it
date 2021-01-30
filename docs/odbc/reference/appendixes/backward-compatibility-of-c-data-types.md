---
description: Compatibilità con le versioni precedenti dei tipi di dati C
title: Compatibilità con le versioni precedenti dei tipi di dati C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], C data types
- compatibility [ODBC], C data types
- data types [ODBC], backward compatibility
- C data types [ODBC], backward compatibility
ms.assetid: b1453a65-ae03-4061-b0cf-a8434d8bc40b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 75ea0c53091faaece23f9d7316c9a6f242677a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212506"
---
# <a name="backward-compatibility-of-c-data-types"></a>Compatibilità con le versioni precedenti dei tipi di dati C
SQL_C_SHORT, SQL_C_LONG e SQL_C_TINYINT sono stati sostituiti in ODBC da tipi firmati e non firmati: SQL_C_SSHORT e SQL_C_USHORT, SQL_C_SLONG e SQL_C_ULONG e SQL_C_STINYINT e SQL_C_UTINYINT. Un driver ODBC *3. x* che dovrebbe funzionare con le applicazioni ODBC *2. x* deve supportare SQL_C_SHORT, SQL_C_LONG e SQL_C_TINYINT, perché quando vengono chiamati, gestione driver li passa al driver.
