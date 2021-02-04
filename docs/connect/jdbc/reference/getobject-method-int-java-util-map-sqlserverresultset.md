---
description: Metodo getObject (int, java.util.Map) (SQLServerResultSet)
title: Metodo getObject (int, java.util.Map) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getObject (int, java.util.Map)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: df85a514-ab43-4bf6-98dd-f7f37fad1850
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bfc8688546099bcdc334af9cdd62e6b86aeab1ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175410"
---
# <a name="getobject-method-int-javautilmap-sqlserverresultset"></a>Metodo getObject (int, java.util.Map) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ottiene il valore dell'indice di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto nel linguaggio di programmazione Java, usando l'oggetto Map specificato.  
  
> [!NOTE]  
>  Questo metodo non è attualmente supportato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. L'utilizzo di questo metodo restituirà sempre il mapping predefinito.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.Object getObject(int i,  
                                  java.util.Map map)  
```  
  
#### <a name="parameters"></a>Parametri  
 *i*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *map*  
  
 Oggetto Map.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **Object**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getObject viene specificato dal metodo getObject nell'interfaccia java.sql.ResultSet.  
  
 Il metodo restituirà il valore della colonna specificata come oggetto Java. Il tipo dell'oggetto Java sarà il tipo di oggetto Java predefinito che corrisponde al tipo SQL della colonna, in base al mapping per i tipi predefiniti indicato nella specifica JDBC. Se si tratta di un valore NULL SQL, il driver restituisce un valore Null Java.  
  
 Questo metodo può essere utilizzato anche per leggere tipi di dati astratti specifici del database. Nell'API di JDBC 2.0, il comportamento del metodo getObject è esteso ai fini della materializzazione dei dati di tipi SQL definiti dall'utente. Quando una colonna contiene un valore di tipo Structured o Distinct, il comportamento di questo metodo è analogo a quello di una chiamata a `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 A partire da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0:  
  
-   Un valore di tipo date sarà restituito come oggetto java.sql.Date.  
  
-   Un valore di tipo time sarà restituito come oggetto java.sql.Time.  
  
-   Un valore di tipo datetime2 sarà restituito come oggetto java.sql.Timestamp.  
  
-   Un valore di tipo datetimeoffset sarà restituito come oggetto microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getObject &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
