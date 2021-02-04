---
description: Metodo getColumnDisplaySize (SQLServerResultSetMetaData)
title: Metodo getColumnDisplaySize (SQLServerResultSetMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSetMetaData.getColumnDisplaySize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 21c25443-bd2b-4b60-9798-4efe2c158952
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f38f061e0789b745786f92820ab00d05e6f0b2b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168096"
---
# <a name="getcolumndisplaysize-method-sqlserverresultsetmetadata"></a>Metodo getColumnDisplaySize (SQLServerResultSetMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce la larghezza massima normale della colonna designata espressa in caratteri.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getColumnDisplaySize(int column)  
```  
  
#### <a name="parameters"></a>Parametri  
 *column*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica la larghezza massima. Se la larghezza non è nota, restituisce 0.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getColumnDisplaySize viene specificato dal metodo getColumnDisplaySize nell'interfaccia java.sql.ResultSetMetaData.  
  
 Il driver JDBC 3.0 per [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] presenta modifiche funzionali nella colonna COLUMN_SIZE. Per altre informazioni, vedere [SQLServerDatabaseMetaData.getColumns](../../../connect/jdbc/reference/getcolumns-method-sqlserverdatabasemetadata.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-members.md)   
 [Classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
