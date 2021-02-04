---
description: Metodo getUser (SQLServerDataSource)
title: Metodo getUser (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getUser
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3513dd7f-6ae5-4010-bde0-454ac4365bce
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4bd3ac0a7c17d0bd10e8709194c8a13eef7eb25d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154722"
---
# <a name="getuser-method-sqlserverdatasource"></a>Metodo getUser (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il nome utente utilizzato per la connessione all'origine dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getUser()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome utente.  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo [setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md) imposta il nome utente che verrà usato durante la connessione all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se il valore del nome utente non è impostato, il metodo getUser restituisce il valore predefinito Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
