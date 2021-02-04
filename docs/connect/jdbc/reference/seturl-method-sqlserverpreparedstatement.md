---
description: Metodo setURL (SQLServerPreparedStatement)
title: Metodo setURL (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d853b2f3-fb72-4d4b-8997-f4a45a9dfefc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f2983f05d8bd2132b1ed3a7b04833f09e794852d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172916"
---
# <a name="seturl-method-sqlserverpreparedstatement"></a>Metodo setURL (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul valore di URL specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setURL(int parameterIndex,  
                         java.net.URL x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterindex*  
  
 Valore **int** che indica il numero di parametro.  
  
 *x*  
  
 Oggetto URL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setURL viene specificato dal metodo setURL nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
