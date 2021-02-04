---
description: Membri di SQLServerResultSetMetaData
title: Membri di SQLServerResultSetMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 37587981-2979-49a3-a6ab-df4bfb9b8748
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10abafa4c3fb2f3bccc7d0ee897cc55a2c92e37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180075"
---
# <a name="sqlserverresultsetmetadata-members"></a>Membri di SQLServerResultSetMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Nome|Descrizione|  
|----------|-----------------|  
|java.sql.ResultSetMetaData|columnNoNulls, columnNullable, columnNullableUnknown|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[getCatalogName](../../../connect/jdbc/reference/getcatalogname-method-sqlserverresultsetmetadata.md)|Ottiene il nome del catalogo per la tabella che include la colonna designata.|  
|[getColumnClassName](../../../connect/jdbc/reference/getcolumnclassname-method-sqlserverresultsetmetadata.md)|Restituisce il nome completo della classe Java di cui vengono prodotte le istanze se viene chiamato il metodo [getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md) della classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) per recuperare un valore dalla colonna.|  
|[getColumnCount](../../../connect/jdbc/reference/getcolumncount-method-sqlserverresultsetmetadata.md)|Restituisce il numero di colonne nel set di risultati.|  
|[getColumnDisplaySize](../../../connect/jdbc/reference/getcolumndisplaysize-method-sqlserverresultsetmetadata.md)|Restituisce la larghezza massima normale della colonna designata espressa in caratteri.|  
|[getColumnLabel](../../../connect/jdbc/reference/getcolumnlabel-method-sqlserverresultsetmetadata.md)|Ottiene il titolo suggerito da utilizzare in stampe e visualizzazioni della colonna designata.|  
|[getColumnName](../../../connect/jdbc/reference/getcolumnname-method-sqlserverresultsetmetadata.md)|Ottiene il nome della colonna designata.|  
|[getColumnType](../../../connect/jdbc/reference/getcolumntype-method-sqlserverresultsetmetadata.md)|Recupera il tipo SQL della colonna designata.|  
|[getColumnTypeName](../../../connect/jdbc/reference/getcolumntypename-method-sqlserverresultsetmetadata.md)|Recupera il nome del tipo specifico del database della colonna designata.|  
|[getPrecision](../../../connect/jdbc/reference/getprecision-method-sqlserverresultsetmetadata.md)|Ottiene il numero di cifre decimali per la colonna designata.|  
|[getScale](../../../connect/jdbc/reference/getscale-method-sqlserverresultsetmetadata.md)|Ottiene il numero di cifre a destra del separatore decimale per la colonna designata.|  
|[getSchemaName](../../../connect/jdbc/reference/getschemaname-method-sqlserverresultsetmetadata.md)|Ottiene il nome dello schema di tabella per la colonna designata.|  
|[getTableName](../../../connect/jdbc/reference/gettablename-method-sqlserverresultsetmetadata.md)|Ottiene il nome di tabella della colonna designata.|  
|[isAutoIncrement](../../../connect/jdbc/reference/isautoincrement-method-sqlserverresultsetmetadata.md)|Indica se la colonna designata viene numerata automaticamente e, pertanto, resa di sola lettura.|  
|[isCaseSensitive](../../../connect/jdbc/reference/iscasesensitive-method-sqlserverresultsetmetadata.md)|Indica se per una colonna viene fatta distinzione tra maiuscole e minuscole.|  
|[isCurrency](../../../connect/jdbc/reference/iscurrency-method-sqlserverresultsetmetadata.md)|Indica se la colonna designata è un valore di cassa.|  
|[isDefinitelyWritable](../../../connect/jdbc/reference/isdefinitelywritable-method-sqlserverresultsetmetadata.md)|Indica se un'operazione di scrittura nella colonna designata riuscirà sicuramente.|  
|[isNullable](../../../connect/jdbc/reference/isnullable-method-sqlserverresultsetmetadata.md)|Indica il supporto dei valori Null nella colonna designata.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverresultsetmetadata.md)|Indica se la colonna designata non è accessibile in scrittura.|  
|[isSearchable](../../../connect/jdbc/reference/issearchable-method-sqlserverresultsetmetadata.md)|Indica se la colonna designata può essere utilizzata in una clausola WHERE SQL.|  
|[isSigned](../../../connect/jdbc/reference/issigned-method-sqlserverresultsetmetadata.md)|Indica se i valori della colonna designata sono numeri con firma.|  
|[isSparseColumnSet](../../../connect/jdbc/reference/issparsecolumnset-method-sqlserverresultsetmetadata.md)|Indica se una colonna in un set di risultati è un set di colonne di tipo sparse.|  
|[isWritable](../../../connect/jdbc/reference/iswritable-method-sqlserverresultsetmetadata.md)|Indica se è possibile che un'operazione di scrittura nella colonna designata venga completata.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
