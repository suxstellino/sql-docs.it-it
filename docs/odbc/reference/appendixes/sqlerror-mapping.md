---
description: Mapping di SQLError
title: Mapping di SQLError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLError
- SQLError function [ODBC], mapping
ms.assetid: 802ac711-7e5d-4152-9698-db0cafcf6047
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 95cb5d4c22ff0baec48e20da2be582d0fbab97c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202881"
---
# <a name="sqlerror-mapping"></a>Mapping di SQLError
Quando un'applicazione chiama **SQLError** attraverso un driver ODBC *3. x* , la chiamata a  
  
```  
SQLError(henv, hdbc, hstmt, szSqlState, pfNativeError, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)   
```  
  
 viene mappato a  
  
```  
SQLGetDiagRec(HandleType, Handle, RecNumber, szSqlstate, pfNativeErrorPtr, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)  
```  
  
 con l'argomento *HandleType* impostato sul valore SQL_HANDLE_ENV, SQL_HANDLE_DBC o SQL_HANDLE_STMT, in base alle esigenze, e l'argomento *handle* impostato sul valore in *HENV*, *HDBC* o *HSTMT*, a seconda dei casi. L'argomento *RecNumber* è determinato da Gestione driver.
