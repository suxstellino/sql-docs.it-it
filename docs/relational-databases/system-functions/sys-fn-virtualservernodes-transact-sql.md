---
description: sys.fn_virtualservernodes (Transact-SQL)
title: sys.fn_virtualservernodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_virtualservernodes
- fn_virtualservernodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- nodes [Faillover Clustering], virtual servers
- nodes [Faillover Clustering]
- virtual servers [Faillover Clustering]
- failover clustering [SQL Server], nodes
- fn_virtualservernodes function
- sys.fn_virtualservernodes function
ms.assetid: 257f3b8d-93c0-4444-87f1-ea211bd8cad0
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b1f721eeb58fbb2d1b072a4156511bf24b191cb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482489"
---
# <a name="sysfn_virtualservernodes-transact-sql"></a>sys.fn_virtualservernodes (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Restituisce un elenco di nodi di istanze cluster di failover in cui è possibile eseguire un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tali informazioni risultano utili in ambienti con clustering di failover.  
  
> [!IMPORTANT]
>  Questa [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] funzione di sistema è inclusa per la compatibilità con le versioni precedenti. È invece consigliabile utilizzare [sys.dm_os_cluster_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_virtualservernodes()  
```  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Se il server corrente è un server in cluster, **fn_virtualservernodes** restituisce un elenco di nodi di istanze cluster di failover in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stata definita questa istanza di.  
  
 Se l'istanza del server corrente non è un server cluster, **fn_virtualservernodes** restituisce un set di righe vuoto.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione VIEW SERVER STATE per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente la funzione `fn_virtualservernodes` viene utilizzata per eseguire una query su un'istanza del server del cluster.  
  
```  
SELECT * FROM fn_virtualservernodes();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 NodeName  
  
 -------\-  
  
 SS3-CLUSN1  
  
 SS3-CLUSN2  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_os_cluster_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)  
  
  
