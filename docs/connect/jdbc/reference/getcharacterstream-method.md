---
description: Metodo getCharacterStream ()
title: Metodo getCharacterStream () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.getCharacterStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 70a5a8c8-791a-43f9-8a0e-1c390f30857c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56ebb493e4a896ddd4c09cf4931977b35da4d549
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168045"
---
# <a name="getcharacterstream-method-"></a>Metodo getCharacterStream ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce i dati **CLOB** come oggetto Reader o come flusso di caratteri.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.Reader getCharacterStream()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Reader contenente i dati **CLOB**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getCharacterStream viene specificato dal metodo getCharacterStream nell'interfaccia java.sql.Clob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Membri di SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Classe SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
