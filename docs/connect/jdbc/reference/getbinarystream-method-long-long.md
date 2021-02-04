---
description: Metodo getBinaryStream (long, long)
title: Metodo getBinaryStream (long, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30bc8882-04b4-4efd-95e4-7d3a2a8c1d47
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da3fcf0ab4e97c19458981feea8f7e2af4c5a133
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168226"
---
# <a name="getbinarystream-method-long-long"></a>Metodo getBinaryStream (long, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce un oggetto flusso di input che contiene un valore BLOB parziale tramite la lunghezza e la posizione iniziale specificate.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.InputStream getBinaryStream(long pos, long length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Offset rispetto al primo byte del valore parziale da recuperare.  
  
 *length*  
  
 Lunghezza in byte del valore parziale da recuperare.  
  
## <a name="return-value"></a>Valore restituito  
 Flusso di input che contiene i dati BLOB.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getBinaryStream viene specificato dal metodo getBinaryStream nell'interfaccia java.sql.Blob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membri di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Classe SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
