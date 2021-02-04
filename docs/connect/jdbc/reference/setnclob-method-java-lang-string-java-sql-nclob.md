---
description: Metodo setNClob (java.lang.String, java.sql.NClob)
title: Metodo setNClob (java.lang.String, java.sql.NClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4e30d242-0c1b-45db-b75f-41b041692f31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65b425d8c194d2cdb00ed7319e1b5531e871bdc5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173293"
---
# <a name="setnclob-method-javalangstring-javasqlnclob"></a>Metodo setNClob (java.lang.String, java.sql.NClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sull'oggetto NClob specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setNClob(java.lang.String parameterName,  
              java.sql.NClob value)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Valore **String** contenente il nome del parametro.  
  
 *value*  
  
 Oggetto NClob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo deve essere usato per i tipi di dati dei parametri **NCHAR**, **NVARCHAR**, **NTEXT** e **XML**.  
  
 Questo metodo setNClob viene specificato dal metodo setNClob nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setNClob &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
