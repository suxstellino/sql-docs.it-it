---
description: Metodo executeUpdate (java.lang.String, int[])
title: Metodo executeUpdate (java.lang.String, int[]) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate (java.lang.String, int[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7b3d5b60-4285-4047-b13e-106754ca0d98
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c08e1ff1746926b8944466377bbfaf64331d62c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163393"
---
# <a name="executeupdate-method-javalangstring-int"></a>Metodo executeUpdate (java.lang.String, int[])
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue l'istruzione SQL specificata e segnala a [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] che le chiavi generate automaticamente indicate nella matrice specificata devono essere rese disponibili per il recupero.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final int executeUpdate(java.lang.String sql,  
                               int[] columnIndexes)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente un'istruzione SQL.  
  
 *columnIndexes*  
  
 Matrice di valori int che indicano gli indici di colonna delle chiavi generate automaticamente che devono essere resi disponibili.  
  
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
  
  
