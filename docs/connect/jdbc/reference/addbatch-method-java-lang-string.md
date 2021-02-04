---
description: Metodo addBatch (java.lang.String)
title: Metodo addBatch (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.addBatch (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 093f6c3b-49a6-4043-9993-bd0482de04dd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0771dc065700ee23454ff621a3f890857f9b9e06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163587"
---
# <a name="addbatch-method-javalangstring"></a>Metodo addBatch (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiunge il comando SQL specificato all'elenco corrente di comandi per questo oggetto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void addBatch(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente un'istruzione SQL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo addBatch viene specificato dal metodo addBatch nell'interfaccia java.sql.Statement.  
  
 La chiamata di questo metodo genera un'eccezione perché l'istruzione SQL per l'oggetto SQLServerPreparedStatement viene specificata quando viene creato l'oggetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo addBatch &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
