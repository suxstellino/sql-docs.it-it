---
description: Metodo toString (DateTimeOffset)
title: Metodo toString (DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e77b9be3-1a02-4769-8acf-ac71d48d6a76
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4599b8152bbb7041565cc4858199ec88d1bb0510
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158832"
---
# <a name="tostring-method-datetimeoffset"></a>Metodo toString (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce una rappresentazione stringa dell'oggetto **DateTimeOffset**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public String toString()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Rappresentazione stringa dell'oggetto **DateTimeOffset**.  
  
## <a name="remarks"></a>Osservazioni  
 La stringa ha il formato `YYYY-MM-DD HH:mm:ss[.fffffff] [+|-]HH:mm`.  
  
 Nei secondi frazionari della stringa restituita vengono aggiunti zero alla precisione dichiarata. Ad esempio, un **datetimeoffset(6)** con il valore "2010-03-10 12:34:56.78-08:00" verrà formattato da DateTimeOffset.toString come "2010-03-10 12:34:56.780000 -08:00".  
  
## <a name="see-also"></a>Vedere anche  
 [Classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Membri di DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
