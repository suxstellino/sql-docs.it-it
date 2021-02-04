---
description: Metodo setObject (int, java.lang.Object)
title: Metodo setObject (int, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 61f19faa-3006-4a1c-974c-55951e3b3000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d21949a1d33107cd5f10a9352aa6282a8ad2da79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178771"
---
# <a name="setobject-method-int-javalangobject"></a>Metodo setObject (int, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore del parametro designato tramite l'oggetto specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setObject(int index,  
                            java.lang.Object obj)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che indica il numero di parametro.  
  
 *obj*  
  
 Oggetto.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setObject viene specificato dal metodo setObject nell'interfaccia java.sql.PreparedStatement.  
  
 Prima di chiamare il metodo setObject, l'applicazione potrebbe impostare il parametro specificato tramite uno dei metodi seguenti:  
  
-   I metodi set\<Type> della classe SQLServerPreparedStatement o della classe SQLServerCallableStatement  
  
-   I metodi setNull della classe SQLServerPreparedStatement o della classe SQLServerCallableStatement  
  
-   Metodo [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) della classe SQLServerCallableStatement  
  
 In tale caso, il tipo del parametro viene impostato automaticamente. Se l'applicazione chiama il metodo setObject con un valore obj NULL, il driver presuppone che il tipo del parametro sia un tipo impostato dal metodo chiamato in precedenza.  
  
 Se il valore obj è NULL e le informazioni sul tipo per tale parametro non possono essere determinate, il metodo setObject converte il parametro specificato in un valore CHAR prima di inviarlo al database.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setObject &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
