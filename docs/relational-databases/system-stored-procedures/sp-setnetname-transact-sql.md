---
description: sp_setnetname (Transact-SQL)
title: sp_setnetname (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setnetname
- sp_setnetname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setnetname
ms.assetid: f416ba81-3835-4588-b0a3-2fe75589490e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f84120579d3eae3b2755eaab89eff137af6190db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193495"
---
# <a name="sp_setnetname-transact-sql"></a>sp_setnetname (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta i nomi di rete in **sys. Servers** sui nomi dei computer di rete effettivi per le istanze remote di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questa stored procedure può essere utilizzata per abilitare l'esecuzione di chiamate a stored procedure remote in computer il cui nome di rete contiene identificatori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non validi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_setnetname  
@server = 'server',   
     @netname = 'network_name'  
```  
  
## <a name="arguments"></a>Argomenti  
 **@server ='** *server* **'**  
 Nome del server remoto specificato nella sintassi di chiamata a stored procedure remote codificata dall'utente. Per usare questo *Server*, è necessario che esista già una riga in **sys. Servers** . *server* è di tipo **sysname** e non prevede alcun valore predefinito.  
  
 **@netname ='** *network_name* **'**  
 Nome di rete del computer a cui vengono effettuate chiamate a stored procedure remote. *network_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 Questo nome deve corrispondere al nome del computer [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows e può includere caratteri non consentiti negli identificatori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Se il nome del computer contiene identificatori non validi, potrebbero verificarsi problemi durante l'esecuzione di alcune chiamate a stored procedure remote a computer Windows.  
  
 I server collegati e i server remoti sono inclusi nello stesso spazio dei nomi. Devono quindi avere nomi diversi. Tuttavia, è possibile definire un server collegato e un server remoto in un server specificato assegnando nomi diversi e usando **sp_setnetname** per impostare il nome di rete di uno di essi sul nome di rete del server sottostante.  
  
```  
--Assume sqlserv2 is actual name of SQL Server   
--database server  
EXEC sp_addlinkedserver 'sqlserv2';  
GO  
EXEC sp_addserver 'rpcserv2';  
GO  
EXEC sp_setnetname 'rpcserv2', 'sqlserv2';  
```  
  
> [!NOTE]  
>  L'utilizzo di **sp_setnetname** per puntare un server collegato di nuovo al server locale non è supportato. I server a cui viene fatto riferimento in questo modo non possono partecipare a transazioni distribuite.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza ai ruoli predefiniti del server **sysadmin** e **setupadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrata una tipica sequenza di amministrazione utilizzata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per eseguire la chiamata a una stored procedure remota.  
  
```  
USE master;  
GO  
EXEC sp_addserver 'Win_1';  
EXEC sp_setnetname 'Win_1','Win-1';  
EXEC Win_1.master.dbo.sp_who;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
