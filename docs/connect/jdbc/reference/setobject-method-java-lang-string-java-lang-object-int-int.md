---
description: Metodo setObject (java.lang.String, java.lang.Object, int, int)
title: Metodo setObject (java.lang.String, java.lang.Object, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setObject (java.lang.String, java.lang.Object, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 16f5f09a-51b5-423a-b52d-8c2eaa04e9ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7905385b181b8f809fd4a5775aa86c00dd302f2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173230"
---
# <a name="setobject-method-javalangstring-javalangobject-int-int"></a>Metodo setObject (java.lang.String, java.lang.Object, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore del parametro designato tramite l'oggetto, il tipo di destinazione e la scala specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setObject(java.lang.String sCol,  
                      java.lang.Object o,  
                      int n,  
                      int m)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
 *o*  
  
 Valore **Object**.  
  
 *n*  
  
 Valore **int** che indica il tipo di destinazione come definito in java.sql.Types.  
  
 *m*  
  
 Valore **int** che indica il numero di cifre a destra del separatore decimale. Questo parametro viene ignorato per tutti i tipi a eccezione di NUMERIC e DECIMAL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setObject viene specificato dal metodo setObject nell'interfaccia java.sql.CallableStatement.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setObject &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
