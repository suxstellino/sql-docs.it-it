---
description: Metodo getBlob (int)
title: Metodo getBlob (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBlob (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bef3ef12-cdda-4a18-90d6-4a501b8e30f0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54e9c032fa43e2f0ea18494a3f39afedb60fc86b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168172"
---
# <a name="getblob-method-int"></a>Metodo getBlob (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro BLOB JDBC designato come oggetto Blob nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Blob getBlob(int index)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che specifica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Blob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getBlob viene specificato dal metodo getBlob nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getBlob &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
