---
description: Metodo createClob (SQLServerConnection)
title: Metodo createClob (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 58b0865a-1cde-4046-9761-51e471294023
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0f95d0c8deeb56653c9572b03fc895dfbc8b9010
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168580"
---
# <a name="createclob-method-sqlserverconnection"></a>Metodo createClob (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un oggetto Clob senza dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Clob createClob()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Clob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo createClob viene specificato dal metodo createClob nell'interfaccia java.sql.Connection.  
  
 Questo metodo sostituisce la necessità del [costruttore SQLServerClob &#40;SQLServerConnection, java.lang.String&#41;](../../../connect/jdbc/reference/sqlserverclob-constructor-sqlserverconnection-java-lang-string.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
