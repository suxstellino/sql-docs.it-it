---
description: Interfaccia ISQLServerStatement
title: Interfaccia ISQLServerStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7f83b514-6e76-4f37-baf3-a10db2010e7c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4d15d4a75ad5a4c6e3a3921305fdc8849bf9f757
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177310"
---
# <a name="isqlserverstatement-interface"></a>Interfaccia ISQLServerStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rappresenta l'implementazione di base della funzionalità dell'istruzione JDBC. Questa interfaccia è stata aggiunta in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 **Pacchetto:** com.microsoft.sqlserver.jdbc  
  
 **Estende:** java.sql.Statement  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public interface ISQLServerStatement  
```  
  
## <a name="remarks"></a>Osservazioni  
 Questa interfaccia viene implementata dalla [classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
 L'interfaccia espone i metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] seguenti:  
  
|Metodo|Per ulteriori informazioni, vedere|  
|------------|-------------------------------|  
|public String getResponseBuffering|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|  
|public void setResponseBuffering|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sull'API del driver JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
