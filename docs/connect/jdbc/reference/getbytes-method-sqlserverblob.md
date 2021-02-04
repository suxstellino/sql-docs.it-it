---
description: Metodo getBytes (SQLServerBlob)
title: Metodo getBytes (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea1b810-b5c1-466d-bdc4-561468214632
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32fd57b6be09a09036d3bd29c5e22ee9934e7ab8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168111"
---
# <a name="getbytes-method-sqlserverblob"></a>Metodo getBytes (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ottiene i dati BLOB come matrice di byte.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public byte[] getBytes(long pos,  
                       int length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Posizione di inizio, a partire da 1 (non 0).  
  
 *length*  
  
 Lunghezza dei dati da ottenere.  
  
## <a name="return-value"></a>Valore restituito  
 Matrice di **byte** contenente i dati richiesti.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getBytes viene specificato dal metodo getBytes nell'interfaccia java.sql.Blob.  
  
 Se è presente un BLOB Null o di lunghezza zero e si prova a ottenere esattamente zero byte in corrispondenza della posizione 1, viene restituita una matrice di **byte** vuota (matrice di byte di lunghezza 0).  
  
 Se si dispone di una lunghezza BLOB Null o zero e si tenta di ottenere una qualsiasi lunghezza di byte in una posizione diversa da 1, verrà generata un'eccezione di posizione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membri di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Classe SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
