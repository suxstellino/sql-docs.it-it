---
description: Metodo setBinaryStream (SQLServerBlob)
title: Metodo setBinaryStream (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBinaryStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: abcec31f-1a60-4765-9725-8cf7e9f1f8ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0f72c246530b4f2298af9bf846bad15fd8380d5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173859"
---
# <a name="setbinarystream-method-sqlserverblob"></a>Metodo setBinaryStream (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un flusso che può essere utilizzato per scrivere nel valore BLOB.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.OutputStream setBinaryStream(long pos)  
```  
  
#### <a name="parameters"></a>Parametri  
 *Pos*  
  
 Posizione nel valore BLOB in corrispondenza della quale iniziare la scrittura.  
  
## <a name="return-value"></a>Valore restituito  
 Flusso di output.  
  
## <a name="exceptions"></a>Eccezioni  
 java.sql.SQLException  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBinaryStream viene specificato dal metodo setBinaryStream nell'interfaccia java.sql.Blob.  
  
 I dati nel valore BLOB vengono sovrascritti dal flusso di output a partire dalla posizione specificata e possono superare la lunghezza iniziale di tale valore. Se si specifica un valore posizione+1, verranno aggiunti byte. Se si passa un valore posizione+2 o superiore (o zero o inferiore) verrà generato un errore di posizione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membri di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Classe SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
