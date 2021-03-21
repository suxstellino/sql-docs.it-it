---
title: Gerarchia delle autorizzazioni (motore di database) | Microsoft Docs
description: Informazioni sulla gerarchia di entità che possono essere protette con le autorizzazioni, chiamate entità a protezione diretta, nel motore di database di SQL Server.
ms.custom: ''
ms.date: 03/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.server.permissions.f1--May use common.permissions
helpviewer_keywords:
- security [SQL Server], denying access
- hierarchies [SQL Server], permissions
- securables [SQL Server]
- security [SQL Server], permissions
- permissions [SQL Server], hierarchy
- security [SQL Server], granting access
ms.assetid: f6d20a55-ef03-4e14-85f9-009902889866
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd021a3306d7bca7d72f02430d52d787c06a23a2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748061"
---
# <a name="permissions-hierarchy-database-engine"></a>Gerarchia delle autorizzazioni (Motore di database)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestisce una raccolta gerarchica di entità che possono essere protette attraverso l'uso di autorizzazioni. Queste entità sono denominate *entità a sicurezza diretta*. Le entità a protezione diretta più significative sono server e database, ma è possibile impostare autorizzazione distinte a un livello più specifico. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] regola le azioni delle entità sulle entità a protezione diretta verificando che siano state concesse le autorizzazioni corrette.  
  
 Nella figura seguente vengono illustrate le relazioni esistenti tra le gerarchie di autorizzazioni di [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Il sistema di autorizzazioni funziona allo stesso modo in tutte le versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../includes/ssdw-md.md)]e [!INCLUDE[ssAPS](../../includes/ssaps-md.md)], ma alcune funzionalità non sono disponibili in tutte le versioni. Le autorizzazioni a livello di server, ad esempio, non possono essere configurate nei prodotti Azure.  
  
 ![Diagramma delle gerarchie di autorizzazioni del motore di database](../../relational-databases/security/media/wj-security-layers.gif "Diagramma delle gerarchie di autorizzazioni del motore di database")  
  
## <a name="chart-of-sql-server-permissions"></a>Grafico delle autorizzazioni di SQL Server  
 Per un'anteprima di grandi dimensioni di tutte le autorizzazioni del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in formato pdf, vedere [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  
  
## <a name="working-with-permissions"></a>Utilizzo delle autorizzazioni  
 Le autorizzazioni possono essere manipolate attraverso le ben note query GRANT, DENY e REVOKE [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le informazioni sulle autorizzazioni sono visualizzate nelle viste di catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) . Le funzioni predefinite offrono inoltre supporto per query sulle informazioni relative alle autorizzazioni.  
  
 Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza di SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
