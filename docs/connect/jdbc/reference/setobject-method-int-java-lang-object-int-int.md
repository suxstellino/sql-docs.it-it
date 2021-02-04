---
description: Metodo setObject (int, java.lang.Object, int, int)
title: Metodo setObject (int, java.lang.Object, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d190ee20-d669-4c6f-a081-d5cfec2f72ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e8e5db6e0e63d46768a3bad13c377c91669059e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178774"
---
# <a name="setobject-method-int-javalangobject-int-int"></a>Metodo setObject (int, java.lang.Object, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore del parametro designato tramite l'oggetto, il tipo di destinazione e la scala specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setObject(int n,  
                            java.lang.Object obj,  
                            int targetSqlType,  
                            int scale)  
```  
  
#### <a name="parameters"></a>Parametri  
 *n*  
  
 Valore **int** che indica il numero di parametro.  
  
 *obj*  
  
 Oggetto.  
  
 *targetSqlType*  
  
 Valore **int** che indica il tipo di destinazione come definito in java.sql.Types.  
  
 *scale*  
  
 Valore **int** che indica il numero di cifre a destra del separatore decimale. Questo parametro viene ignorato per tutti i tipi diversi da NUMERIC e DECIMAL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setObject viene specificato dal metodo setObject nell'interfaccia java.sql.PreparedStatement.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setObject &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
