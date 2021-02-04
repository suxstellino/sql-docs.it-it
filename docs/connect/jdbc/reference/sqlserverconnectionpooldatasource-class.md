---
description: Classe SQLServerConnectionPoolDataSource
title: Classe SQLServerConnectionPoolDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b00e5a90-2af7-4d04-8ef8-256183777dcf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 152dcc1f6aa2d8b463b3e859f79ade54b5620334
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172831"
---
# <a name="sqlserverconnectionpooldatasource-class"></a>Classe SQLServerConnectionPoolDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rappresenta connessioni di database fisiche per le gestioni dei pool di connessioni.  
  
 **Pacchetto:** com.microsoft.sqlserver.jdbc  
  
 **Estende:** [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
 **Implementa:** javax.sql.ConnectionPoolDataSource  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public class SQLServerConnectionPoolDataSource  
```  
  
## <a name="remarks"></a>Osservazioni  
 SQLServerConnectionPoolDataSource viene in genere usato negli ambienti dei server applicazioni Java che supportano il pool di connessioni predefinito e richiedono un oggetto ConnectionPoolDataSource per fornire connessioni fisiche, come i server applicazioni Java EE (Java Platform, Enterprise Edition) che forniscono il pool di connessioni della specifica dell'API JDBC 3.0.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-members.md)   
 [Informazioni di riferimento sull'API del driver JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
