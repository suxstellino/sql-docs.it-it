---
description: Funzione SQLRemoveDefaultDataSource
title: Funzione SQLRemoveDefaultDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDefaultDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDefaultDataSource
helpviewer_keywords:
- SQLRemoveDefaultDataSource function [ODBC]
ms.assetid: db803266-57df-4864-a41b-901247549c1f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326522986785d7e76bc781fa4cc912401f2c237e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192519"
---
# <a name="sqlremovedefaultdatasource-function"></a>Funzione SQLRemoveDefaultDataSource
**Conformità**  
 Versione introdotta: ODBC 1,0, deprecato  
  
 **Summary**  
 In ODBC 3,0 la funzione **SQLRemoveDefaultDataSource** è stata sostituita da una chiamata a [SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md) con un argomento *fRequest* di ODBC_REMOVE_DEFAULT_DSN. Se un programma di installazione ODBC 2 *. x* chiama questa funzione, il programma di installazione ODBC eseguirà il mapping alla chiamata **SQLConfigDataSource** seguente:  
  
```cpp  
SQLConfigDataSource (NULL, ODBC_REMOVE_DEFAULT_DSN, NULL, NULL)  
```
