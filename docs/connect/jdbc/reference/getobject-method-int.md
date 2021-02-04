---
description: Metodo getObject (int)
title: Metodo getObject (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getObject (jnt)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c4b8366b-c065-48e1-b712-19e2d9834228
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ee652a927a77d5080340cac5ae5eeee427c8a3d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162630"
---
# <a name="getobject-method-int"></a>Metodo getObject (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.Object getObject(int index)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che specifica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **Object**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getObject viene specificato dal metodo getObject nell'interfaccia java.sql.CallableStatement.  
  
 Il metodo restituirà il valore della colonna specificata come oggetto Java. Il tipo dell'oggetto Java sarà il tipo di oggetto Java predefinito che corrisponde al tipo SQL della colonna, in base al mapping per i tipi predefiniti indicato nella specifica JDBC. Se si tratta di un valore NULL SQL, il driver restituisce un valore Null Java.  
  
 Questo metodo può essere utilizzato anche per leggere tipi di dati astratti specifici del database. In JDBC 2.0 il comportamento del metodo getObject è stato esteso ai fini della materializzazione dei dati di tipi SQL definiti dall'utente. Quando una colonna contiene un valore di tipo Structured o Distinct, il comportamento di questo metodo è analogo a quello di una chiamata a `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 A partire da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0:  
  
-   Un valore di tipo **date** verrà restituito come oggetto java.sql.Date.  
  
-   Un valore di tipo **time** verrà restituito come oggetto java.sql.Time.  
  
-   Un valore di tipo **datetime2** verrà restituito come oggetto java.sql.Timestamp.  
  
-   Un valore di tipo **datetimeoffset** verrà restituito come oggetto microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getObject &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
