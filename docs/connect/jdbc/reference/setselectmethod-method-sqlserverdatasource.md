---
description: Metodo setSelectMethod (SQLServerDataSource)
title: Metodo setSelectMethod (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setSelectMethod
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7934276d-5ac9-4cbc-a2a0-2c65c93733ac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f31db4197a0d2b714bb1e1889648d24ca5a4fc3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178643"
---
# <a name="setselectmethod-method-sqlserverdatasource"></a>Metodo setSelectMethod (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il tipo di cursore predefinito usato per tutti i set di risultati creati tramite questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setSelectMethod(java.lang.String selectMethod)  
```  
  
#### <a name="parameters"></a>Parametri  
 *selectMethod*  
  
 Valore **String** contenente il tipo di cursore predefinito.  
  
## <a name="remarks"></a>Commenti  
 selectMethod è il tipo di cursore predefinito utilizzato per un set di risultati. Questa proprietà è utile in caso di set di risultati di grandi dimensioni e se non desidera archiviare tutto il set di risultati in memoria nel lato client. Impostando la proprietà su "cursor", è possibile creare un cursore sul lato server che può recuperare i blocchi di dati più piccoli in una sola volta. Se la proprietà selectMethod non è impostata, il metodo [getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md) restituisce il valore predefinito "direct".  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
