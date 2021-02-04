---
description: Metodo getLong (int)
title: Metodo getLong (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getLong (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b7078ca7-fd2a-4474-ab29-989ae28c77e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b7b157c7c59564003cd5fb785a884ed6a5fa37c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175673"
---
# <a name="getlong-method-int"></a>Metodo getLong (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto **long** nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public long getLong(int index)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che specifica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **long**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getLong viene specificato dal metodo getLong nell'interfaccia java.sql.CallableStatement.  
  
 Questo metodo è supportato solo nei tipi di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che possono restituire in modo sicuro un valore integer, ad esempio bigint, int, smallint, tinyint e bit. L'utilizzo di questo metodo su qualsiasi altro tipo di dati genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getLong &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
