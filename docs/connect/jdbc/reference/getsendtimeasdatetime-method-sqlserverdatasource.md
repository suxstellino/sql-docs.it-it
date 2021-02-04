---
description: Metodo getSendTimeAsDatetime (SQLServerDataSource)
title: Metodo getSendTimeAsDatetime (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 02287122-5dc1-455d-987f-95fd9a69d503
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba9fd31fdbc5626fc3baa81c853bc78ea2a81abc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175071"
---
# <a name="getsendtimeasdatetime-method-sqlserverdatasource"></a>Metodo getSendTimeAsDatetime (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Questo metodo è stato aggiunto in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Restituisce l'impostazione della proprietà di connessione **sendTimeAsDatetime**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean getSendTimeAsDatetime();  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se i valori java.sql.Time verranno inviati al server come tipo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **datetime** di . **false** se i valori java.sql.Time verranno inviati al server come tipo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **time** di .  
  
## <a name="remarks"></a>Osservazioni  
 Per altre informazioni sulla proprietà di connessione **sendTimeAsDatetime**, vedere [Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 L'oggetto [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md) consente di impostare la proprietà di connessione **sendTimeAsDatetime** a livello di codice.  
  
 Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
