---
description: Metodo valueOf (java.sql.Timestamp, java.util.Calendar)
title: Metodo valueOf (java.sql.Timestamp, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7320c383-0b06-446d-963b-7005e50324a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 232eeb75b783d463df17d7a836ab95b9b1b0ac8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181138"
---
# <a name="valueof-method-javasqltimestamp-javautilcalendar"></a>Metodo valueOf (java.sql.Timestamp, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un oggetto **DateTimeOffset** che rappresenta un punto nel tempo in un particolare offset rispetto a GMT in base a un valore java.sql.Timestamp e un valore java.util.Calendar che indica l'offset.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public static DateTimeOffset valueOf(java.sql.Timestamp timestamp, java.util.Calendar calendar)  
```  
  
#### <a name="parameters"></a>Parametri  
 *timestamp*  
  
 Valore java.sql.Timestamp.  
  
 *calendario*  
  
 Valori di offset.  I componenti di data e ora di *calendar* verranno impostati in base al valore di *timestamp*.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un oggetto DateTimeOffset che rappresenta il punto nel tempo specificato dall'oggetto java.sql.Timestamp in corrispondenza del fuso orario dell'oggetto java.util.Calendar specificato.  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo imposta inoltre l'oggetto Java.util.Calendar sul momento specificato dall'oggetto java.sql.Timestamp.  
  
## <a name="see-also"></a>Vedere anche  
 [Classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Membri di DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
