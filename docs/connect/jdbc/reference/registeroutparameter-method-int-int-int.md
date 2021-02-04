---
description: Metodo registerOutParameter (int, int, int)
title: Metodo registerOutParameter (int, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.registerOutParameter (int, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d902d4e0-881f-4182-814c-0ede9a8da7fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71413f131d3ec27b5a6c2bc31a2bcdce151a6da9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176747"
---
# <a name="registeroutparameter-method-int-int-int"></a>Metodo registerOutParameter (int, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Registra il parametro OUT nella posizione ordinale specificata nel tipo e nella scala JDBC specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void registerOutParameter(int index,  
                                 int sqlType,  
                                 int scale)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che indica la posizione ordinale del parametro.  
  
 *sqlType*  
  
 Codice di tipo JDBC come definito in java.sql.Types.  
  
 *scale*  
  
 Valore **int** che indica il numero di cifre da posizionare a destra del separatore decimale.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo registerOutParameter viene specificato dal metodo registerOutParameter nell'interfaccia java.sql.CallableStatement.  
  
 A partire dal driver JDBC 3.0 di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], quando *sqlType* è di tipo java.sql.Types.TIME, il comportamento di questo metodo viene modificato dalla proprietà di connessione **sendTimeAsDatetime** ([Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md)) e [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo registerOutParameter &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
