---
description: Metodo acceptsURL (SQLServerDriver)
title: Metodo acceptsURL (SQLServerDriver) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDriver.acceptsURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fc744566-7191-4b15-9f76-b4b8087fb14a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0269ab57fb0be55f57c8fb97c5d39f6cda37359e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168746"
---
# <a name="acceptsurl-method-sqlserverdriver"></a>Metodo acceptsURL (SQLServerDriver)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Verifica se l'URL specificato è valido.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean acceptsURL(java.lang.String url)  
```  
  
#### <a name="parameters"></a>Parametri  
 *url*  
  
 Valore **String** contenente l'URL usato per la connessione al database.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se l'URL specificato è valido. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo acceptsURL viene specificato dal metodo acceptsURL nell'interfaccia java.sql.Driver.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-methods.md)   
 [Membri di SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-members.md)   
 [Classe SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-class.md)  
  
  
