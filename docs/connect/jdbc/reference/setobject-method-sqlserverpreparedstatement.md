---
description: Metodo setObject (SQLServerPreparedStatement)
title: Metodo setObject (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 93a2b22c-82b4-48c7-a428-369ebe98a372
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da892f57476bd1fbb0b7afd67c2ad1f9d8b41c13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173206"
---
# <a name="setobject-method-sqlserverpreparedstatement"></a>Metodo setObject (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore del parametro designato tramite l'oggetto specificato.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="overload-list"></a>Elenco degli overload  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[setObject (int, java.lang.Object)](../../../connect/jdbc/reference/setobject-method-int-java-lang-object.md)|Imposta il valore del parametro designato tramite l'oggetto specificato.|  
|[setObject (int, java.lang.Object, int)](../../../connect/jdbc/reference/setobject-method-int-java-lang-object-int.md)|Imposta il valore del parametro designato tramite l'oggetto e il tipo di destinazione specificati.|  
|[setObject (int, java.lang.Object, int, int)](../../../connect/jdbc/reference/setobject-method-int-java-lang-object-int-int.md)|Imposta il valore del parametro designato tramite l'oggetto, il tipo di destinazione e la scala specificati.|  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
