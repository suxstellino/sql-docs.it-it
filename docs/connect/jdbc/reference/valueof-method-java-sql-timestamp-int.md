---
description: Metodo valueOf (java.sql.Timestamp, int)
title: Metodo valueOf (java.sql.Timestamp, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 114f55af-62ab-4c60-8724-0affbbbbbcdc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56a9bde3e6480c973ae173137f81149aced6c7b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181145"
---
# <a name="valueof-method-javasqltimestamp-int"></a>Metodo valueOf (java.sql.Timestamp, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un oggetto **DateTimeOffset** che rappresenta un punto nel tempo in un particolare offset GMT in base a un valore java.sql.Timestamp e un valore che indica l'offset in minuti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public static DateTimeOffset valueOf(java.sql.Timestamp timestamp, int minutesOffset)  
```  
  
#### <a name="parameters"></a>Parametri  
 *timestamp*  
  
 Valore java.sql.Timestamp.  
  
 *minutesOffset*  
  
 Offset in minuti.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un oggetto DateTimeOffset che rappresenta il punto nel tempo specificato dall'oggetto java.sql.Timestamp in corrispondenza dell'offset specificato, in minuti, da GMT.  
  
## <a name="see-also"></a>Vedere anche  
 [Classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Membri di DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
