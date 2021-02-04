---
description: Metodo getEncrypt (SQLServerDataSource)
title: Metodo getEncrypt (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- getEncrypt Method (SQLServerDataSource)
apilocation:
- getEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 1cdb12dd-6e6f-4bbd-8f5f-9e630f3ee2c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b9bbcb5adb686825132b2ae166beecd5846693f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175856"
---
# <a name="getencrypt-method-sqlserverdatasource"></a>Metodo getEncrypt (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce un valore **Boolean** che indica se la proprietà di crittografia è abilitata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean getEncypt()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se la crittografia è abilitata. In caso contrario, **false**.  
  
## <a name="remarks"></a>Osservazioni  
 Se la proprietà di crittografia è impostata su **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fa in modo che in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] venga usata la crittografia TLS per tutti i dati inviati tra il client e il server, se nel server è installato un certificato.  
  
 Se la proprietà encrypt non è specificata o è impostata su **false**, il driver non impone il supporto della crittografia TLS a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è configurata in modo da imporre la crittografia TLS, viene stabilita una connessione senza crittografia. Se l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è configurata per imporre la crittografia TLS, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] abilita questo tipo di crittografia automaticamente durante l'esecuzione in un ambiente Java Virtual Machine (JVM) correttamente configurato. In caso contrario, la connessione viene terminata e il driver genera un errore. Se la proprietà di crittografia non è impostata, il metodo [getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md) restituisce il valore predefinito **false**.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
