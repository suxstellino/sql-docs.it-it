---
description: Mapping di SQLFreeStmt
title: Mapping di SQLFreeStmt | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFreeStmt function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLFreeStmt
ms.assetid: 267d95f2-4f0c-47ab-9411-5afe105215a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a90ddbaab036efd22003b84b551398b0d896226b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202812"
---
# <a name="sqlfreestmt-mapping"></a>Mapping di SQLFreeStmt
Quando un'applicazione chiama **SQLFreeStmt** con un argomento *Option* di SQL_DROP tramite un driver ODBC *3. x* , la chiamata a  
  
```  
SQLFreeStmt(hstmt, SQL_DROP)   
```  
  
 viene mappato a  
  
```  
SQLFreeHandle(SQL_HANDLE_STMT,Handle)  
```  
  
 con l'argomento *handle* impostato sul valore in *HSTMT*.
