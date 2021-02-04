---
description: Metodo updateBlob (int, java.io.InputStream)
title: Metodo updateBlob (int, java.io.InputStream) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d0263018-d326-4a7b-bf6f-5f508db899d4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 442900b54a1e50f75d2a31463406fda25025d1d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183228"
---
# <a name="updateblob-method-int-javaioinputstream"></a>Metodo updateBlob (int, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata tramite il flusso di input specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateBlob(int columnIndex,  
                       java.io.InputStream inputStream)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *inputStream*  
  
 Oggetto InputStream.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateBlob viene specificato dal metodo updateBlob nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateBlob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
