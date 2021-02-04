---
description: Metodo updateBlob (int, java.io.InputStream, long)
title: Metodo updateBlob (int, java.io.InputStream, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2edf9b51-63e1-4c28-afdf-2d4af593d5f7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c26c94227ce70d1a623093c75f7bd5be2ca6ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183250"
---
# <a name="updateblob-method-int-javaioinputstream-long"></a>Metodo updateBlob (int, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata tramite il flusso di input specificato, che conterrà il numero specificato di byte.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateBlob(int columnIndex,  
                       java.io.InputStream inputStream,  
                                              long length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *inputStream*  
  
 Oggetto InputStream.  
  
 *length*  
  
 Valore **long** che indica la lunghezza del flusso.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateBlob viene specificato dal metodo updateBlob nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateBlob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
