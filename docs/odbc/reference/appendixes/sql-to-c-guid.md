---
description: 'Da SQL a C: GUID'
title: 'Da SQL a C: GUID | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], GUID
- data conversions from SQL to C types [ODBC], guid
- GUID data type [ODBC]
ms.assetid: cf56c684-c261-4b89-994a-db14ab2241d6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b55b88414dfa78b80c49987af6dab82ba4abeda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203029"
---
# <a name="sql-to-c-guid"></a>Da SQL a C: GUID
L'identificatore per il tipo di dati SQL ODBC GUID è:  
  
 SQL_GUID  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati di SQL GUID. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificatore di tipo C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Lunghezza in byte *BufferLength* > caratteri|Data|36|n/d|  
||*BufferLength* < 37|Non definito|Non definito|22003|  
|SQL_C_WCHAR|Lunghezza in caratteri > *bufferLength*|Data|36|n/d|  
||*BufferLength* < 37|Non definito|Non definito|22003|  
|SQL_C_BINARY|Lunghezza in byte dei dati \< =  *bufferLength*|Data|Lunghezza dei dati in byte|n/d|  
||Lunghezza in byte dei dati > *bufferLength*|Non definito|Non definito|22003|  
|SQL_C_GUID|Nessuno [a]|Data|16 [b]|n/d|  
  
 [a] il valore di *bufferLength* viene ignorato per la conversione. Il driver presuppone che le dimensioni di **TargetValuePtr* siano le dimensioni del tipo di dati C.  
  
 [b] corrisponde alla dimensione del tipo di dati C corrispondente.
