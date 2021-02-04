---
description: Metodo setResponseBuffering (SQLServerStatement)
title: Metodo setResponseBuffering (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apilocation:
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apitype: Assembly
ms.assetid: 9f489835-6cda-4c8c-b139-079639a169cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fecbee60be6ffe48ffd72d66bf12e78de40a93c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178665"
---
# <a name="setresponsebuffering-method-sqlserverstatement"></a>Metodo setResponseBuffering (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) su un valore String **full** o **adaptive** senza distinzione tra maiuscole e minuscole.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setResponseBuffering(java.lang.String value)  
```  
  
#### <a name="parameters"></a>Parametri  
 *value*  
  
 Valore **String** contenente la modalità di memorizzazione delle risposte nel buffer. La modalità valida può essere una delle stringhe senza distinzione tra maiuscole e minuscole seguenti: **full** o **adaptive**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Il valore **adaptive** specifica la memorizzazione nel buffer della quantità di dati minima possibile, quando necessario.  
  
 Il valore **full** specifica la lettura dell'intero risultato dal server in fase di esecuzione.  
  
 Adaptive è il valore predefinito nel driver JDBC versione 2.0 e 3.0. Full era il valore predefinito prima della versione 2.0 del driver JDBC.  
  
 Il metodo [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) consente di eseguire l'override della proprietà **String** della connessione **responseBuffering** per l'oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) corrente. Per altre informazioni sull'uso della modalità di buffering delle risposte, vedere [Uso del buffer adattivo](../../../connect/jdbc/using-adaptive-buffering.md).  
  
 Se l'applicazione specifica un valore del parametro non valido nel metodo [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md), viene generato un oggetto [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)   
 [Utilizzo del buffer adattivo](../../../connect/jdbc/using-adaptive-buffering.md)  
  
  
