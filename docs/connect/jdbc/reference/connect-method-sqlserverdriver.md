---
description: Metodo connect (SQLServerDriver)
title: Metodo connect (SQLServerDriver) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDriver.connect
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 43813a4c-1cc7-4659-ba27-f1786f1371eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d41a7158f354a730cebe496c7adb1caa6e31f36f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166020"
---
# <a name="connect-method-sqlserverdriver"></a>Metodo connect (SQLServerDriver)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea una connessione al database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Connection connect(java.lang.String Url,  
                                   java.util.Properties suppliedProperties)  
```  
  
#### <a name="parameters"></a>Parametri  
 *Url*  
  
 Valore **String** contenente l'URL usato per la connessione al database.  
  
 *suppliedProperties*  
  
 Set di coppie di valori stringa utilizzato come argomenti di connessione.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto connessione.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo connect viene specificato dal metodo connect nell'interfaccia java.sql.Driver.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-methods.md)   
 [Membri di SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-members.md)   
 [Classe SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-class.md)  
  
  
