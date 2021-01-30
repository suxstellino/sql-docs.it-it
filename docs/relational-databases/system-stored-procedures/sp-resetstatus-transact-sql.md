---
description: sp_resetstatus (Transact-SQL)
title: sp_resetstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_resetstatus
- sp_resetstatus_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_resetstatus
ms.assetid: b892727f-ea3b-4b94-88d9-f2386ad4962c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ca7fa3d697fe5714066d677ba04b666817c597d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194385"
---
# <a name="sp_resetstatus-transact-sql"></a>sp_resetstatus (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reimposta lo stato di un database sospetto.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER database](../../t-sql/statements/alter-database-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_resetstatus [ @dbname = ] 'database'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @dbname =]'*database*'  
 Nome del database di cui si desidera reimpostare lo stato. il *database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_resetstatus disattiva il flag di stato sospetto di un database e aggiorna le colonne della modalità e dello stato per il database specificato in sys.databases. Prima di eseguire questa procedura, è necessario analizzare il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e risolvere tutti i problemi esistenti. Dopo l'esecuzione di sp_resetstatus, arrestare e riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Un database può risultare sospetto per svariati motivi. È ad esempio possibile che il sistema operativo abbia negato l'accesso a una risorsa del database oppure che uno o più file di database siano danneggiati o non disponibili.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene reimpostato lo stato del database `AdventureWorks2012`.  
  
```  
EXEC sp_resetstatus 'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
