---
description: Mapping di SQLSetParam
title: Mapping di SQLSetParam | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLSetParam
- SQLSetParam function [ODBC], mapping
ms.assetid: 022dfbc0-8d18-4c35-8a28-d9eb16063188
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6e2488c689a4da1152e115017287274252c6b405
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202631"
---
# <a name="sqlsetparam-mapping"></a>Mapping di SQLSetParam
**SQLSetParam** continua a essere mappato su **SQLBINDPARAMETER** come in ODBC 2. *x*. Anche se concettualmente è simile a **SQLBindParam**, gestione driver non esegue il mapping di **SQLSetParam** a **SQLBindParam**. Questo è dovuto a un certo ODBC 2 esistente. i driver *x* utilizzano il valore speciale di *bufferLength* (SQL_SETPARAM_VALUE_MAX) generato da Gestione driver quando esegue il mapping di **SQLSetParam** su **SQLBindParameter** per determinare quando viene chiamato da 1. applicazione ODBC *x* .  
  
 Una chiamata a  
  
```  
SQLSetParam(hstmt, ipar, fCType, fSqlType, cbColDef, ibScale, rgbValue, pcbValue)  
```  
  
 comporterà quanto segue:  
  
```  
SQLBindParameter(StatementHandle, ParameterNumber, SQL_PARAM_INPUT_OUTPUT, ValueType, ParameterType, ColumnSize, DecimalDigits, ParameterValuePtr, SQL_SETPARAM_VALUE_MAX, StrLen_or_IndPtr)  
```
