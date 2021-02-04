---
description: Metodo executeUpdate (java.lang.String) (SQLServerStatement)
title: Metodo executeUpdate (java.lang.String) (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 85e7c3a2-f2da-49bf-9d90-5fd246fd60e1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8d518ab7d043a5b54cf363a4be96523fed473a79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168511"
---
# <a name="executeupdate-method-javalangstring-sqlserverstatement"></a>Metodo executeUpdate (java.lang.String) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue l'istruzione SQL specificata, che può essere un'istruzione INSERT, UPDATE o DELETE, oppure un'istruzione SQL che non restituisce nulla, ad esempio un'istruzione SQL DDL. A partire dalla versione 3.0 del driver JDBC per [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il metodo executeUpdate restituirà il numero corretto di righe aggiornate in un'operazione MERGE.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int executeUpdate(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente l'istruzione SQL.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero di righe interessate oppure 0 se si usa un'istruzione DDL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo executeUpdate viene specificato dal metodo executeUpdate nell'interfaccia java.sql.Statement.  
  
 Se l'esecuzione di una stored procedure restituisce un conteggio di aggiornamenti maggiore di uno o genera più set di risultati, usare il metodo [execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md) per eseguire la stored procedure.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo executeUpdate &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)   
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
