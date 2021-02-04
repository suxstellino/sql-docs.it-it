---
description: Metodo updateBytes (SQLServerResultSet)
title: Metodo updateBytes (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3050c836-fbb3-4475-99e5-05637a48a932
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 28414c4d58a5a1b2ef0cada57457de960722e8d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188302"
---
# <a name="updatebytes-method-sqlserverresultset"></a>Metodo updateBytes (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con una matrice di valori **byte**.  
  
## <a name="overload-list"></a>Elenco degli overload  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[updateBytes (int, byte&#91;&#93;)](../../../connect/jdbc/reference/updatebytes-method-int-byte.md)|Aggiorna la colonna designata con una matrice di valori **byte** in base all'indice della colonna.|  
|[updateBytes (java.lang.String, byte&#91;&#93;)](../../../connect/jdbc/reference/updatebytes-method-java-lang-string-byte.md)|Aggiorna la colonna designata con una matrice di valori **byte** in base al nome della colonna.|  
  
## <a name="remarks"></a>Commenti  
 In una versione precedente di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] era possibile usare SQLServerResultSet.updateBytes per convertire valori tra matrici di byte e i tipi di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**date**, **time**, **datetime2** e **datetimeoffset**. In questa versione l'utilizzo del metodo con questi tipi di dati provoca un'eccezione indicante che la conversione non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
