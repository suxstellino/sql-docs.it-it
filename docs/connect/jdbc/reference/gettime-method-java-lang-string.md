---
description: Metodo getTime (java.lang.String)
title: Metodo getTime (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTime (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ca0a3b29-30d1-4d20-bc8d-d3d9ed19ff50
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbaf986b61066809edb89647eb0ca7ba43c1e981
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162125"
---
# <a name="gettime-method-javalangstring"></a>Metodo getTime (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto java.sql.Time nel linguaggio di programmazione Java in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Time getTime(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Time.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getTime viene specificato dal metodo getTime nell'interfaccia java.sql.CallableStatement.  
  
 Vedere il grafico denominato "Conversioni dei metodi di richiamo" in [Informazioni sulle conversioni dei tipi di dati](../../../connect/jdbc/understanding-data-type-conversions.md) per visualizzare i tipi di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che possono essere recuperati con questo metodo.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getTime &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
