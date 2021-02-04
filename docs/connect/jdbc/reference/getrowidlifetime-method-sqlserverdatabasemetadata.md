---
description: Metodo getRowIdLifetime (SQLServerDatabaseMetaData)
title: Metodo getRowIdLifetime (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 317c0b44-fe3f-4142-9cab-e40e4c4fe070
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4da5d30b347d30acfa3a6fd7b96214bc7955278d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162392"
---
# <a name="getrowidlifetime-method-sqlserverdatabasemetadata"></a>Metodo getRowIdLifetime (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce uno stato che indica se è supportato il tipo di dati SQL RowId. Se il tipo è supportato, restituisce la durata di validità di un oggetto RowId.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.RowIdLifetime getRowIdLifetime()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto RowIdLifetime.  
  
> [!NOTE]  
>  Nel driver JDBC versione 2,0 questo metodo restituisce il valore java.sql.RowIdLifetime.ROWID_UNSUPPORTED.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getRowIdLifetime viene specificato dal metodo getRowIdLifetime nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
