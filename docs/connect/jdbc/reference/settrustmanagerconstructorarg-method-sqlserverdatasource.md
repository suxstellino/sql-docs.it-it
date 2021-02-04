---
description: Metodo setTrustManagerConstructorArg (SQLServerDataSource)
title: Metodo setTrustManagerConstructorArg (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setTrustManagerConstructorArg
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c22b2bb90b2996500b47aa62caa9979efe48b0b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172963"
---
# <a name="settrustmanagerconstructorarg-method-sqlserverdatasource"></a>Metodo setTrustManagerConstructorArg (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore String della proprietà di connessione TrustManagerConstructorArg.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setTrustManagerConstructorArg(java.lang.String trustManagerClass)  
```  
  
#### <a name="parameters"></a>Parametri  
 *trustManagerClass*  
  
 Valore **String** che contiene il nome completo della classe di un oggetto javax.net.ssl.TrustManager personalizzato.
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
