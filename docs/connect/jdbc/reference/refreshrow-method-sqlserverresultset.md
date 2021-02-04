---
description: Metodo refreshRow (SQLServerResultSet)
title: Metodo refreshRow (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.refreshRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 048fe245-157f-4fd8-be75-ce54b83e02b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c99d1bfda98734585d3364d1a81cc45a888a0f2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176700"
---
# <a name="refreshrow-method-sqlserverresultset"></a>Metodo refreshRow (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la riga corrente con il relativo valore più recente nel database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void refreshRow()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo refreshRow viene specificato dal metodo refreshRow nell'interfaccia java.sql.ResultSet.  
  
 Non è possibile chiamare questo metodo quando il cursore si trova sulla riga di inserimento.  
  
 Tramite questo metodo un'applicazione può indicare in modo esplicito al driver JDBC di ripetere il recupero delle righe dal database. In un'applicazione è possibile che la chiamata a questo metodo sia necessaria durante l'operazione di memorizzazione nella cache o di prelettura di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] ai fini del recupero dell'ultimo valore di una riga dal database. È possibile che il driver JDBC aggiorni effettivamente più righe contemporaneamente se la dimensione del recupero è maggiore di uno.  
  
 Il recupero viene ripetuto per tutti i valori in base al livello di isolamento transazione e alla sensibilità del cursore. Se questo metodo viene chiamato dopo la chiamata a un metodo di aggiornamento, ma prima della chiamata al metodo [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md), gli aggiornamenti apportati alla riga andranno persi. Se questo metodo viene chiamato frequentemente, è possibile che si verifichi una riduzione della prestazioni.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
