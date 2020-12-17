---
description: Proprietà SQL Server Agent (pagina Connessione)
title: Proprietà SQL Server Agent (pagina Connessione)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.connection.f1
ms.assetid: d6a677ff-60ad-47ba-a0cb-df4193b165e0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 42a7dc1230057e390de4a1afc5ed334fe7c2a655
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474372"
---
# <a name="sql-server-agent-properties-connection-page"></a>Proprietà SQL Server Agent (pagina Connessione)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Usare questa pagina per visualizzare e modificare le impostazioni della connessione del servizio [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Opzioni  
**Alias server host locale**  
Consente di specificare l'alias da utilizzare per la connessione all'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non è possibile utilizzare le opzioni di connessione predefinite per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, definire un alias per l'istanza e specificarlo qui.  
  
**Usa autenticazione di Windows**  
Consente di impostare l'autenticazione di [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows come metodo di autenticazione predefinito per la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent si connette come l'account con cui viene eseguito il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
**Usa autenticazione di SQL Server**  
Consente di impostare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come metodo di autenticazione predefinito per la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L'autenticazione è disponibile per garantire la compatibilità con le versioni precedenti. Per un livello di sicurezza migliore, utilizzare l'autenticazione di Windows, se possibile.  
  
**Accesso**  
Consente di specificare il nome dell'account di accesso per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
**Password**  
Consente di specificare la password per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
