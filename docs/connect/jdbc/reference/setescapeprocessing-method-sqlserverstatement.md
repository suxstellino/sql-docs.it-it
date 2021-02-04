---
description: Metodo setEscapeProcessing (SQLServerStatement)
title: Metodo setEscapeProcessing (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.setEscapeProcessing
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6ac0682e-e04c-4fdb-893b-92408d42051e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c60d547ed0a72a573bda8f16b4051ff796261dd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173462"
---
# <a name="setescapeprocessing-method-sqlserverstatement"></a>Metodo setEscapeProcessing (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta la modalità di elaborazione di escape.  
  
> [!NOTE]  
>  L'elaborazione delle sequenze di escape per [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] è sempre abilitata. L'impostazione di questo metodo su false non ha effetti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setEscapeProcessing(boolean enable)  
```  
  
#### <a name="parameters"></a>Parametri  
 *enable*  
  
 **true** per abilitare l'elaborazione delle sequenze di escape. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setEscapeProcessing viene specificato dal metodo setEscapeProcessing nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
