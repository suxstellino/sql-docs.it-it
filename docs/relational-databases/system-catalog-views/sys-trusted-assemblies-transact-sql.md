---
description: sys.trusted_assemblies (Transact-SQL)
title: sys.trusted_assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trusted_assemblies_TSQL
- trusted_assemblies
- sys.trusted_assemblies_TSQL
- sys.trusted_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trusted_assemblies
ms.assetid: ''
author: VanMSFT
ms.author: vanto
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f2b26f5e646d1997959ba4856a88463ae8b1b3ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186156"
---
# <a name="systrusted_assemblies-transact-sql"></a>sys.trusted_assemblies (Transact-SQL)  
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Contiene una riga per ogni assembly attendibile per il server.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


|Nome colonna |Tipo di dati |Descrizione |
|--- |--- |--- |
|hash |varbinary(8000) |SHA2_512 hash del contenuto dell'assembly. |
|description |nvarchar(4000) |Descrizione facoltativa definita dall'utente dell'assembly. Microsoft consiglia di utilizzare il nome canonico che codifica il nome semplice, il numero di versione, le impostazioni cultura, la chiave pubblica e l'architettura dell'assembly da considerare attendibile. Questo valore identifica in modo univoco l'assembly sul lato Common Language Runtime (CLR) ed è uguale al valore di clr_name in sys. Assemblies. |
|create_date |datetime2 |Data in cui l'assembly è stato aggiunto all'elenco di assembly attendibili. |
|created_by |nvarchar(128) |Nome dell'account di accesso dell'entità che ha aggiunto l'assembly all'elenco. |
| | | |

### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
 
## <a name="remarks"></a>Commenti  
Utilizzare **[sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)** per aggiungere e **[sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)** per rimuovere gli assembly da `sys.trusted_assemblies` .

## <a name="see-also"></a>Vedere anche  
  [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)  
  [sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)  
  [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  
