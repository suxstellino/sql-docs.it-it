---
description: Metodo setEncrypt (SQLServerDataSource)
title: Metodo setEncrypt (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setEncrypt Method (SQLServerDataSource)
apilocation:
- setEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 0c85a9c1-f27c-457e-8461-403cc03e2d17
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a041a866e825cef59441cbfd42ca408d1d3cfd0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179014"
---
# <a name="setencrypt-method-sqlserverdatasource"></a>Metodo setEncrypt (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta un valore **Boolean** che indica se la proprietà encrypt è abilitata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setEncypt(boolean encrypt)  
```  
  
#### <a name="parameters"></a>Parametri  
 *encrypt*  
  
 **true** se la crittografia Transport Layer Security (TLS), precedentemente nota come Secure Sockets Layer (SSL), è abilitata tra il client e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. In caso contrario, **false**.  
  
## <a name="remarks"></a>Osservazioni  
 Se la proprietà encrypt è impostata su **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fa in modo che in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] venga usata la crittografia TLS per tutti i dati inviati tra il client e il server, se nel server è installato un certificato. Il valore predefinito è **false**.  
  
 Il driver JDBC rileva l'ambiente Java Virtual Machine (JVM) in cui viene eseguito durante il tentativo di stabilire un handshake TLS.  
  
 Se la proprietà encrypt è impostata su **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] usa il provider di sicurezza JSSE predefinito di JVM per negoziare la crittografia TLS con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. È possibile che il provider di sicurezza predefinito non supporti tutte le funzionalità necessarie per negoziare la crittografia TLS. Tale provider può, ad esempio, non supportare la dimensione della chiave pubblica RSA usata nel certificato TLS/SSL di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. In questo caso, è possibile che venga generato un errore dal provider di sicurezza predefinito, a causa del quale la connessione viene terminata dal driver JDBC. Per risolvere il problema, eseguire una delle operazioni seguenti:  
  
-   Configurare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con un certificato del server con una chiave pubblica RSA di dimensione inferiore  
  
-   Configurare JVM per l'uso di un provider di sicurezza JSSE diverso nel file delle proprietà di sicurezza "\<java-home>/lib/security/java.security"  
  
-   Utilizzare una versione di JVM diversa.  
  
 Se la proprietà encrypt non è specificata o è impostata su **false**, il driver non impone il supporto della crittografia TLS a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è configurata in modo da imporre la crittografia TLS, viene stabilita una connessione senza crittografia. Se l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è configurata per imporre la crittografia TLS, questa viene abilitata automaticamente da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] durante l'esecuzione in un ambiente JVM correttamente configurato. In caso contrario, la connessione viene terminata e il driver genera un errore.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
