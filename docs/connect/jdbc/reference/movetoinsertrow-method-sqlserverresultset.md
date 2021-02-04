---
description: Metodo moveToInsertRow (SQLServerResultSet)
title: Metodo moveToInsertRow (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.moveToInsertRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f3c54bfe-d5b7-4f6e-ae6c-3e8954e5b1c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 431e9dbc8570d9505b9a2bfd1b8922229b22e3e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177040"
---
# <a name="movetoinsertrow-method-sqlserverresultset"></a>Metodo moveToInsertRow (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Sposta il cursore nella riga di inserimento.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void moveToInsertRow()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo moveToInsertRow viene specificato dal metodo moveToInsertRow nell'interfaccia java.sql.ResultSet.  
  
 La posizione corrente del cursore viene memorizzata mentre il cursore viene posizionato sulla riga di inserimento. La riga di inserimento è una riga speciale associata a un set di risultati aggiornabile. Si tratta essenzialmente di un buffer in cui è possibile costruire una nuova riga chiamando i metodi di aggiornamento prima di aggiungere la riga al set di risultati.  
  
 È possibile chiamare solo i metodi updater, getter e [insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md) quando il cursore si trova sulla riga di inserimento. È necessario assegnare un valore a tutte le colonne di un set di risultati ogni volta che questo viene chiamato e prima di chiamare il metodo insertRow. Un metodo di aggiornamento deve essere chiamato prima che uno di richiamo possa essere chiamato su un valore della colonna.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
