---
description: Metodo setBytes (long, byte)
title: Metodo setBytes (long, byte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBytes (long.byte[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ffb8f107-0f9d-4410-957f-62b718e1e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2329bb9598c56ecabb7282faaf6b7b7f5565b068
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173767"
---
# <a name="setbytes-method-long-byte"></a>Metodo setBytes (long, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Scrive la matrice di byte specificata nell'oggetto BLOB a partire dalla posizione specificata, quindi restituisce il numero di byte scritti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Posizione (in base 1) nell'oggetto BLOB in corrispondenza della quale iniziare la scrittura dei dati.  
  
 *bytes*  
  
 Matrice di byte da scrivere nell'oggetto BLOB.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **long** che specifica il numero di byte scritti.  
  
## <a name="exceptions"></a>Eccezioni  
 java.sql.SQLException  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBytes viene specificato dal metodo setBytes nell'interfaccia java.sql.Blob.  
  
 I dati vengono sovrascritti a partire dalla posizione specificata e possono superare la lunghezza iniziale dell'oggetto BLOB. Se si specifica un valore posizione+1, verranno aggiunti byte. Se si passa un valore posizione+2 o superiore (o zero o inferiore) verrà generato un errore di posizione. Se si passa una matrice **byte** di lunghezza zero verrà restituito zero in quanto non sono stati scritti byte.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setBytes &#40;SQLServerBlob&#41;](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)   
 [Metodi di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membri di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Classe SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
