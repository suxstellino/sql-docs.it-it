---
description: Mapping di SQLAllocStmt
title: Mapping di SQLAllocStmt | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLAllocStmt
- SQLAllocStmt function [ODBC], mapping
ms.assetid: a2449dbb-1b6c-4b49-81b9-ebdddd4442fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa2b3392ce9cce9b6cf98d18fe6672bb0034bf2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202961"
---
# <a name="sqlallocstmt-mapping"></a>Mapping di SQLAllocStmt
Quando un'applicazione chiama **SQLAllocStmt** tramite un driver ODBC *3. x* , la chiamata a:  
  
```  
SQLAllocStmt(hdbc, phstmt)  
```  
  
 viene mappato a **SQLAllocHandle** da Gestione driver nel driver come indicato di seguito:  
  
```  
SQLAllocHandle(SQL_HANDLE_STMT, InputHandle, OutputHandlePtr)  
```  
  
 con *InputHandle puntare* impostato su *HDBC* e *OutputHandlePtr* impostato su *phstmt*.
