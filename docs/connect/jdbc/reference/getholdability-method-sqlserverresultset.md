---
description: Metodo getHoldability (SQLServerResultSet)
title: Metodo getHoldability (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4508d90f-c3c4-4eac-8001-fb0b93b66734
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 285e5b9547415da7fea26133f017ad07df66d92d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175693"
---
# <a name="getholdability-method-sqlserverresultset"></a>Metodo getHoldability (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera la trattenibilità di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getHoldability()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** contenente uno dei livelli di trattenibilità seguenti:  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getHoldability viene specificato dal metodo getHoldability nell'interfaccia java.sql.ResultSet.  
  
 Per impostare la trattenibilità del set di risultati, le applicazioni possono usare il metodo [setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md) della classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md). Dopo che è stato chiamato il metodo [setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md), sono stati creati l'oggetto istruzione e il rispettivo oggetto del set di risultati ed è stata eseguita l'istruzione, può essere necessario modificare di nuovo la trattenibilità.  
  
 Per i cursori sul lato server, se connessi a SQL Server 2005 o versioni successive, l'impostazione influisce solo sulla trattenibilità dei nuovi set di risultati ancora da creare in tale connessione. In SQL Server 2000, tuttavia, l'impostazione influisce sulla trattenibilità sia dei set di risultati esistenti che in quelli nuovi ancora da creare in tale connessione.  
  
 Quando viene reimpostata la trattenibilità e viene chiamato il metodo getHoldability sull'oggetto del set di risultati precedentemente creato, è possibile che il valore restituito da questo metodo sia diverso dal valore di trattenibilità restituito dai metodi seguenti: Statement.getResultSetHoldability, Connection.getHoldability o DatabaseMetaData.getResultSetHoldability.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
