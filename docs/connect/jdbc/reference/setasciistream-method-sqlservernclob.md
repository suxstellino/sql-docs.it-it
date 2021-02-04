---
description: Metodo setAsciiStream (SQLServerNClob)
title: Metodo setAsciiStream (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 617ece92-0fb1-4f95-b32d-29b5b56eb3fb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b51a216aa622b5cbaa3a60f9d19798df425066a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173975"
---
# <a name="setasciistream-method-sqlservernclob"></a>Metodo setAsciiStream (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un flusso da usare per scrivere caratteri ASCII nel valore **NCLOB** rappresentato da questo oggetto **java.sql.NClob**, a partire dalla posizione specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.OutputStream setAsciiStream(long pos)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Posizione di inizio della scrittura nell'oggetto **NCLOB**. La prima posizione è 1.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto OutputStream che rappresenta il flusso in cui possono essere scritti i caratteri ASCII.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setAsciiStream viene specificato dal metodo setAsciiStream nell'interfaccia java.sql.NClob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [Membri di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [Classe SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
