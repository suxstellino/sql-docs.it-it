---
description: 'Da SQL a C: ora'
title: 'Da SQL a C: ora | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], time
- time data type [ODBC]
- data conversions from SQL to C types [ODBC], time
ms.assetid: 6dc59973-7bb5-40f1-87c8-5bf68b3bf2ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db1882befdb1b295ce247cb62601bbec4ba3db5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203010"
---
# <a name="sql-to-c-time"></a>Da SQL a C: ora
Identificatore per il tipo di dati time ODBC SQL:  
  
 SQL_TYPE_TIME  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati SQL. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificatore di tipo C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Lunghezza in byte *BufferLength* > caratteri<br /><br /> *9*  <=  *BufferLength* <= lunghezza in byte carattere<br /><br /> *BufferLength* < 9|Data<br /><br /> Dati troncati [a]<br /><br /> Non definito|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Lunghezza in caratteri > *bufferLength*<br /><br /> *9*  <=  *BufferLength* <= Lunghezza carattere<br /><br /> *BufferLength* < 9|Data<br /><br /> Dati troncati [a]<br /><br /> Non definito|Lunghezza dei dati in caratteri<br /><br /> Lunghezza dei dati in caratteri<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Lunghezza in byte dei dati <= *bufferLength*<br /><br /> Lunghezza in byte dei dati > *bufferLength*|Data<br /><br /> Non definito|Lunghezza dei dati in byte<br /><br /> Non definito|n/d<br /><br /> 22003|  
|SQL_C_TYPE_TIME|Nessuno [b]|Data|6 [d]|n/d|  
|SQL_C_TYPE_TIMESTAMP|Nessuno [b]|Dati [c]|16 [d]|n/d|  
  
 [a] i secondi frazionari del tempo vengono troncati.  
  
 [b] il valore di *bufferLength* viene ignorato per la conversione. Il driver presuppone che le dimensioni di **TargetValuePtr* siano le dimensioni del tipo di dati C.  
  
 [c] i campi relativi alla data della struttura del timestamp vengono impostati sulla data corrente e il campo dei secondi frazionari della struttura del timestamp è impostato su zero.  
  
 [d] corrisponde alla dimensione del tipo di dati C corrispondente.  
  
 Quando i dati SQL vengono convertiti in dati di tipo carattere C, la stringa risultante si trova nel formato "*HH*:*mm*:*SS*". Questo formato non è influenzato dall'impostazione di Windows® Country.
