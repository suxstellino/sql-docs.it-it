---
description: Metodo unwrap (SQLServerXADataSource)
title: Metodo unwrap (SQLServerXADataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d97c99b3-2224-4abb-8b32-40aff49fe759
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d8f15798e6a4b3e0228034fa317990f7fa49211d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160888"
---
# <a name="unwrap-method-sqlserverxadatasource"></a>Metodo unwrap (SQLServerXADataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Parametri  
 *iface*  
  
 Classe di tipo **T** che definisce un'interfaccia.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto che implementa l'interfaccia specificata.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md) è definito dall'interfaccia java.sql.Wrapper, introdotta nella specifica JDBC 4.0.  
  
 È possibile che le applicazioni debbano accedere alle estensioni dell'API JDBC specifiche di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Il metodo unwrap supporta l'annullamento del wrapping nelle classi pubbliche estese dall'oggetto, se le classi espongono estensioni del fornitore.  
  
 La classe [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) estende la classe [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md) che viene estesa dalla classe [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Quando viene chiamato questo metodo, l'oggetto annulla il wrapping nelle classi seguenti: [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md), [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md) e [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md).  
  
 Per altre informazioni, vedere [Wrapper e interfacce](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-methods.md)   
 [Membri di SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [Classe SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
