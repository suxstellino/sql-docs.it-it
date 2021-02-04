---
description: Metodo getMaxFieldSize (SQLServerStatement)
title: Metodo getMaxFieldSize (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getMaxFieldSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ed7bbcb8-660b-4e9b-8241-e216c42826f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4b1cbb5bb3cb35b4286a0b822168c9991c228576
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162811"
---
# <a name="getmaxfieldsize-method-sqlserverstatement"></a>Metodo getMaxFieldSize (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero massimo di byte che possono essere restituiti per i valori di colonna binaria o di tipo carattere in un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato da questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final int getMaxFieldSize()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero massimo di byte che può essere contenuto in una colonna oppure 0 se non esiste alcun limite.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMaxFieldSize viene specificato dal metodo getMaxFieldSize nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-methods.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
