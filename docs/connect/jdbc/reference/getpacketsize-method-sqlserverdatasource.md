---
description: Metodo getPacketSize (SQLServerDataSource)
title: Metodo getPacketSize (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getPacketSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b2e9f01a-2e51-47e5-90bf-43c62d1be74d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1fb42a8d875cd3e3e72eebc212a8050a41e389b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162581"
---
# <a name="getpacketsize-method-sqlserverdatasource"></a>Metodo getPacketSize (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce le dimensioni del pacchetto di rete corrente usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], specificate in byte.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getPacketSize()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** contenente la dimensione del pacchetto di rete corrente.  
  
## <a name="remarks"></a>Commenti  
 Se la proprietà packetSize non è impostata, il metodo getPacketSize restituisce il valore predefinito 8000.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
