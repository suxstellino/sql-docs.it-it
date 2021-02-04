---
description: Metodo setCharacterStream (SQLServerNClob)
title: Metodo setCharacterStream (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 09042ee9-dfb1-4d0b-82bd-d1224b0aea80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a21bb749f191e9269c3a1086317d6a9ef1c6236c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173650"
---
# <a name="setcharacterstream-method-sqlservernclob"></a>Metodo setCharacterStream (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un flusso da usare per scrivere un flusso di caratteri Unicode nel valore **NCLOB** rappresentato da questo oggetto **java.sql.NClob**, a partire dalla posizione specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.Writer setCharacterStream(long pos)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Posizione di inizio della scrittura nel valore **NCLOB**. La prima posizione è 1.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Writer che rappresenta il flusso in cui possono essere scritti i caratteri Unicode.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setCharacterStream viene specificato dal metodo setCharacterStream nell'interfaccia java.sql.NClob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [Membri di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [Classe SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
