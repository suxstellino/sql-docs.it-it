---
description: Transizioni dei descrittori
title: Transizioni di descrittori | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC], descriptor
- transitioning states [ODBC], descriptor
- descriptor transitions [ODBC]
ms.assetid: 0cf24fe6-5e3c-45fa-81b8-4f52ddf8501d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d312b372a0817ae79ad07bdbf149f90ba0c71939
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194885"
---
# <a name="descriptor-transitions"></a>Transizioni dei descrittori
I descrittori ODBC presentano i tre stati seguenti.  
  
|State|Descrizione|  
|-----------|-----------------|  
|D0|Descrittore non allocato|  
|D1i|Descrittore allocato in modo implicito|  
|D1e|Descrittore allocato in modo esplicito|  
  
 Nelle tabelle seguenti viene illustrato il modo in cui ogni funzione ODBC influiscono sullo stato del descrittore.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|D1i [1]|--|--|  
|D1e [2]|--|--|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|IH|--|--|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|--[1]|D0|--|  
|IH 2|(HY017)|D0|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField e SQLGetDescRec  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|IH|--|--|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField e SQLSetDescRec  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|IH 1|--|--|  
  
 [1] Questa riga Mostra le transizioni quando *DescriptorHandle* è l'handle di un ARD, APD o dpi oppure (per **SQLSetDescField**) quando *DescriptorHandle* è l'handle di un IRD e *FieldIdentifier* è stato SQL_DESC_ARRAY_STATUS_PTR o SQL_DESC_ROWS_PROCESSED_PTR.  
  
## <a name="all-other-odbc-functions"></a>Tutte le altre funzioni ODBC  
  
|D0<br /><br /> Non allocato|D1i<br /><br /> Implicita|D1e<br /><br /> Esplicita|  
|------------------------|----------------------|----------------------|  
|--|--|--|
