---
description: Metodo getString (java.lang.String)
title: Metodo getString (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getString (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f67371e0-e879-4188-85fc-ecb85f0be2a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad5a411e77b52130e9e1e4d43ebd38daff5844ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162219"
---
# <a name="getstring-method-javalangstring"></a>Metodo getString (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto **String** nel linguaggio di programmazione Java in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getString(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getString viene specificato dal metodo getString nell'interfaccia java.sql.CallableStatement.  
  
 Tutte le colonne in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] possono essere restituite come stringa. Pertanto, possono essere restituite una rappresentazione di stringa di tutti i tipi numerici e basati su caratteri e una rappresentazione in stringa esadecimale di colonne binarie quali binary, varbinary, varbinary(max), image, timestamp e uniqueidentifier.  
  
 I tipi dipendenti dai percorsi quali money, smallmoney, datetime, smalldatetime, float, real, decimal e numeric restituiranno il formato canonico toString() per il valore sottostante del tipo.  
  
 I tipi definiti dall'utente vengono restituiti come valori di stringa esadecimali.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getString &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
