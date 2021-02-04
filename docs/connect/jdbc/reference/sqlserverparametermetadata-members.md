---
description: Membri di SQLServerParameterMetaData
title: Membri di SQLServerParameterMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: f9ebb203-2013-4feb-94f5-494b7f098f9a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90f01cbefe8a09d89f8e26a4f4c0e7e4101bddb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176591"
---
# <a name="sqlserverparametermetadata-members"></a>Membri di SQLServerParameterMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Nome|Descrizione|  
|----------|-----------------|  
|java.sql.ParameterMetaData|parameterModeIn, parameterModeInOut, parameterModeOut, parameterModeUnknown, parameterNoNulls, parameterNullable, parameterNullableUnknown|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[getParameterClassName](../../../connect/jdbc/reference/getparameterclassname-method-sqlserverparametermetadata.md)|Recupera il nome completo della classe Java le cui istanze devono essere passate al metodo [setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md) della classe [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).|  
|[getParameterCount](../../../connect/jdbc/reference/getparametercount-method-sqlserverparametermetadata.md)|Recupera il numero di parametri nell'oggetto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) per cui questo oggetto [SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md) contiene informazioni.|  
|[getParameterMode](../../../connect/jdbc/reference/getparametermode-method-sqlserverparametermetadata.md)|Recupera la modalità del parametro designato.|  
|[getParameterType](../../../connect/jdbc/reference/getparametertype-method-sqlserverparametermetadata.md)|Recupera il tipo SQL del parametro designato.|  
|[getParameterTypeName](../../../connect/jdbc/reference/getparametertypename-method-sqlserverparametermetadata.md)|Recupera il nome del tipo specifico del database del parametro designato.|  
|[getPrecision](../../../connect/jdbc/reference/getprecision-method-sqlserverparametermetadata.md)|Recupera il numero di cifre decimali per il parametro designato.|  
|[getScale](../../../connect/jdbc/reference/getscale-method-sqlserverparametermetadata.md)|Recupera il numero di cifre a destra del separatore decimale per il parametro designato.|  
|[isNullable](../../../connect/jdbc/reference/isnullable-method-sqlserverparametermetadata.md)|Recupera un valore che indica se nel parametro designato sono consentiti valori Null.|  
|[isSigned](../../../connect/jdbc/reference/issigned-method-sqlserverparametermetadata.md)|Recupera un valore che indica se i valori per il parametro designato possono essere numeri con segno.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)  
  
  
