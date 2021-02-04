---
description: Metodo getBlob (java.lang.String)
title: Metodo getBlob (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBlob (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3fe74b50-9ccd-4973-a93a-6da2c20a4154
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2b52b721a1c9bc672af5f081c390a1d72e7de538
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168162"
---
# <a name="getblob-method-javalangstring"></a>Metodo getBlob (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro BLOB JDBC designato come oggetto Blob nel linguaggio di programmazione Java in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Blob getBlob(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
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
  
  
