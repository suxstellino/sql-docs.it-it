---
description: Metodo execute (java.lang.String, java.lang.String)
title: Metodo execute (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.execute (java.lang.String.java.lang.String[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9451c7c2-4c0d-4d1e-9b42-a26ff28e3f6a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4103e4a10aff5f16c68cfc584d31a1e6600e723
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168476"
---
# <a name="execute-method-javalangstring-javalangstring"></a>Metodo execute (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue l'istruzione SQL specificata, che può restituire più risultati, e segnala a [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] che le chiavi generate automaticamente indicate nella matrice specificata devono essere rese disponibili per il recupero.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final boolean execute(java.lang.String sql,  
                             java.lang.String[] columnNames)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente un'istruzione SQL.  
  
 *columnNames*  
  
 Matrice di valori string che indica i nomi di colonna delle chiavi generate automaticamente che devono essere resi disponibili.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il primo risultato è un set di risultati. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo execute viene specificato dal metodo execute nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo execute &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)   
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
