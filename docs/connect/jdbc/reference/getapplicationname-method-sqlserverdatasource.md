---
description: Metodo getApplicationName (SQLServerDataSource)
title: Metodo getApplicationName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getApplicationName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f71e501c-ccd7-4a1e-b6ea-4d47a81c18c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 45e4ce6a37e24ee4d3e8692fe4b145e8e8f019b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168419"
---
# <a name="getapplicationname-method-sqlserverdatasource"></a>Metodo getApplicationName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il nome dell'applicazione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getApplicationName()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome dell'applicazione o " [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] " se non è impostato alcun valore.  
  
## <a name="remarks"></a>Osservazioni  
 Il nome dell'applicazione viene usato per identificare l'applicazione specifica nei diversi strumenti di profiling e registrazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se il nome dell'applicazione non è impostato, il metodo getApplicationName restituisce la stringa non localizzata " [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] ".  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
