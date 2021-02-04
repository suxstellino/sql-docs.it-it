---
description: Metodo updateNClob (int, java.sql.NClob)
title: Metodo updateNClob (int, java.sql.NClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6e9bca18-f64e-46a4-8541-2c9c39b393b5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 79ff369ae9d28abf61d278bd044d755227fd5835
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194634"
---
# <a name="updatenclob-method-int-javasqlnclob"></a>Metodo updateNClob (int, java.sql.NClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con un valore **NClob**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateNClob(int columnIndex,  
                        java.sql.NClob nClob)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *nClob*  
  
 Oggetto NClob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateNClob viene specificato dal metodo updateNClob nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo è supportato solo nelle colonne **nvarchar(max)**, **ntext** e **xml**. L'utilizzo di questo metodo su qualsiasi altro tipo di dati genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateNClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
