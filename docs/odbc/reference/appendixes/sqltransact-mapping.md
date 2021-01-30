---
description: Mapping di SQLTransact
title: Mapping SQLTransact | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLTransact
- SQLTransact function [ODBC], mapping
ms.assetid: 8a01041f-3572-46f9-8213-b817f3cf929c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 09dbfe0c97de5b7b2800869dbb02c1290fd3df3a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202569"
---
# <a name="sqltransact-mapping"></a>Mapping di SQLTransact
**SQLTransact** è ora sostituito da **SQLEndTran**. La differenza principale tra le due funzioni è che **SQLEndTran** contiene un argomento *HandleType*, che specifica l'ambito del lavoro da eseguire. L'argomento *HandleType* può specificare l'ambiente o l'handle di connessione. La chiamata seguente a **SQLTransact**:  
  
```  
SQLTransact(henv, hdbc, fType)  
```  
  
 viene mappato a  
  
```  
SQLEndTran(SQL_HANDLE_DBC, ConnectionHandle, CompletionType);  
```  
  
 Se *connectionHandle* è diverso da SQL_NULL_HDBC. L'argomento *connectionHandle* è impostato sul valore di *HDBC*.  
  
 **SQL_Transact** è mappato a  
  
```  
SQLEndTran (SQL_HANDLE_ENV, EnvironmentHandle, CompletionType);  
```  
  
 Se *connectionHandle* è uguale a SQL_NULL_HDBC. L'argomento *EnvironmentHandle* è impostato sul valore di *HENV*.  
  
 In entrambi i casi precedenti, l'argomento *CompletionType* è impostato sullo stesso valore di *ftype*.
