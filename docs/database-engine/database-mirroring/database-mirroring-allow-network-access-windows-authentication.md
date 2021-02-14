---
title: Concedere l'accesso alla rete a un endpoint per il mirroring del database usando l'autenticazione di Windows | Microsoft Docs
description: Informazioni su come usare l'autenticazione di Windows per la connessione degli endpoint del mirroring di database di due istanze di SQL Server, che può richiedere la configurazione manuale.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 28c8fec5-5feb-4c84-8d72-f2bd1ae3b40d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: dfe10e2d812d133571d4df64d05ea7705d076464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353189"
---
# <a name="database-mirroring---allow-network-access---windows-authentication"></a>Concedere l'accesso alla rete a un endpoint per il mirroring del database usando l'autenticazione di Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'utilizzo dell'autenticazione di Windows per la connessione degli endpoint del mirroring di database di due istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede la configurazione manuale degli account di accesso nelle seguenti condizioni:  
  
-   Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione come servizi con diversi account di dominio (in domini uguali o attendibili), è necessario creare l'account di accesso di ogni account in **master** in ognuna delle istanze del server remoto e a quell'account di accesso devono essere concesse le autorizzazioni CONNECT sull'endpoint.  
  
-   Se le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono in esecuzione come account del servizio di rete, l'account di accesso di ogni account del computer host (*DomainName* **\\** _ComputerName$_ ) deve essere creato in **master** in ognuna delle istanze del server remoto e a quell'account di accesso devono essere concesse le autorizzazioni CONNECT sull'endpoint. Ciò avviene in quanto un'istanza del server che è in esecuzione con l'account del servizio di rete esegue l'autenticazione utilizzando l'account di dominio del computer host.  
  
> [!NOTE]  
>  Assicurarsi che sia disponibile un endpoint per ogni istanza del server. Per altre informazioni, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
### <a name="to-configure-logins-for-windows-authentication"></a>Per configurare gli account di accesso per l'autenticazione di Windows  
  
1.  Per l'account utente di ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], creare un account di accesso nelle altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilizzare un'istruzione [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) con la clausola FROM WINDOWS.  
  
     Per altre informazioni, vedere [Creazione di un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md).  
  
2.  Per assicurarsi inoltre che l'utente di accesso sia in grado di accedere all'endpoint, utilizzare l'istruzione [GRANT](../../t-sql/statements/grant-transact-sql.md) per concedere le autorizzazioni di connessione sull'endpoint per l'account di accesso. Si noti che la concessione di autorizzazioni di accesso per l'endpoint non è necessaria se l'utente è un amministratore.  
  
     Per altre informazioni, vedere [Grant a Permission to a Principal](../../relational-databases/security/authentication-access/grant-a-permission-to-a-principal.md).  
  
## <a name="example"></a>Esempio  
 Nel seguente esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] viene creato un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per un account utente denominato `Otheruser` che appartiene a un dominio denominato `Adomain`. Nell'esempio vengono quindi concesse all'utente autorizzazioni per la connessione a un endpoint di mirroring del database preesistente denominato `Mirroring_Endpoint`.  
  
```  
USE master;  
GO  
CREATE LOGIN [Adomain\Otheruser] FROM WINDOWS;  
GO  
GRANT CONNECT on ENDPOINT::Mirroring_Endpoint TO [Adomain\Otheruser];  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
