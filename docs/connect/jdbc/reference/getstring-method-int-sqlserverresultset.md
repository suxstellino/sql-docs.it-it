---
description: Metodo getString (int) (SQLServerResultSet)
title: Metodo getString (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getString (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bfa493c4-fe07-449b-b4d0-384e1a1fce48
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d679ea82b07c7df4aef09b47c7cc74e537a94070
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174964"
---
# <a name="getstring-method-int-sqlserverresultset"></a>Metodo getString (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore dell'indice di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come **String** nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getString(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getString viene specificato dal metodo getString nell'interfaccia java.sql.ResultSet.  
  
 Tutte le colonne in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] possono essere restituite come stringa. Pertanto, possono essere restituite una rappresentazione **String** di tutti i tipi numerici e basati su caratteri e una rappresentazione stringa esadecimale di colonne binarie quali binary, varbinary, varbinary(max), image, timestamp e uniqueidentifier.  
  
 I tipi dipendenti dai percorsi quali money, smallmoney, datetime, smalldatetime, float, real, decimal e numeric restituiranno il formato canonico toString() per il valore sottostante del tipo.  
  
 I tipi definiti dall'utente vengono restituiti come valori **String** esadecimali.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getString &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
