---
description: 'Da C a SQL: dati binari'
title: 'Da C a SQL: binario | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- binary data type [ODBC]
- data conversions from C to SQL types [ODBC], binary
- binary data transfers [ODBC]
- converting data from c to SQL types [ODBC], binary
ms.assetid: 3e9083f3-357b-41aa-833c-2c8aac2226cd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2e3e1d197a1999c6502848d92222f1c3d6ad5d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212431"
---
# <a name="c-to-sql-binary"></a>Da C a SQL: dati binari
Identificatore per il tipo di dati ODBC C binario:  
  
 SQL_C_BINARY  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL in cui è possibile convertire i dati binari C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Lunghezza in byte del <dati = lunghezza in byte colonna<br /><br /> Lunghezza in byte dei dati > lunghezza in byte della colonna|n/d<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Lunghezza in caratteri della <dati = lunghezza carattere colonna<br /><br /> Lunghezza in caratteri della lunghezza dei caratteri > colonna|n/d<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER<br /><br /> SQL_BIGINT<br /><br /> SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE<br /><br /> SQL_BIT SQL_TYPE_DATE<br /><br /> SQL_TYPE_TIME<br /><br /> SQL_TYPE_TIMESTAMP|Lunghezza in byte dei dati = lunghezza dati SQL<br /><br /> Lunghezza in byte dei dati <> lunghezza dati SQL|n/d<br /><br /> 22003|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|Lunghezza dei dati <= lunghezza della colonna<br /><br /> Lunghezza dei dati > lunghezza della colonna|n/d<br /><br /> 22001|
