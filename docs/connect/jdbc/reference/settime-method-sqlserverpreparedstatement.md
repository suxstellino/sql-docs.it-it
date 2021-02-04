---
description: Metodo setTime (SQLServerPreparedStatement)
title: Metodo setTime (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setTime
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b3a83ea3-6636-4096-842b-71b37340fa07
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32783882345d0f63725a81e667a1e2eefb7f3358
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173110"
---
# <a name="settime-method-sqlserverpreparedstatement"></a>Metodo setTime (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul valore di ora specificato.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="overload-list"></a>Elenco degli overload  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[setTime (int, java.sql.Time)](../../../connect/jdbc/reference/settime-method-int-java-sql-time.md)|Imposta il parametro designato sul valore di ora specificato.|  
|[setTime (int, java.sql.Time, java.util.Calendar)](../../../connect/jdbc/reference/settime-method-int-java-sql-time-java-util-calendar.md)|Imposta il parametro designato sui valori relativi all'ora e al calendario specificati.|  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
