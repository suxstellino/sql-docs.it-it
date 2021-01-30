---
description: Mapping di SQLAllocConnect
title: Mapping di SQLAllocConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocConnect function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLAllocConnect
ms.assetid: ac89dd1f-c565-47cc-8fa3-6fa5f80b5d63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd1659e4faaee713a2b9d942deb53fe00ea367c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202981"
---
# <a name="sqlallocconnect-mapping"></a>Mapping di SQLAllocConnect
Quando un'applicazione chiama **SQLAllocConnect** tramite un ODBC 3. driver *x* , viene eseguito il mapping della chiamata a **SQLAllocConnect**(*HENV*, *phdbc*) a **SQLAllocHandle** nel modo seguente:  
  
1.  Gestione driver alloca una connessione e la restituisce all'applicazione.  
  
2.  Quando l'applicazione stabilisce una connessione, gestione driver chiama  
  
    ```  
    SQLAllocHandle(SQL_HANDLE_DBC, InputHandle, OutputHandlePtr)  
    ```  
  
     nel driver con *InputHandle puntare* impostato su *HENV* e *OutputHandlePtr* impostato su *phdbc*.
