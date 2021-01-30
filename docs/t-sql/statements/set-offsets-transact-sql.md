---
description: SET OFFSETS (Transact-SQL)
title: SET OFFSETS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_OFFSETS_TSQL
- OFFSETS_TSQL
- SET OFFSETS
- OFFSETS
dev_langs:
- TSQL
helpviewer_keywords:
- position relative to start of statement [SQL Server]
- OFFSETS option
- offsets [SQL Server]
- SET OFFSETS statement
ms.assetid: c7bcc697-0930-4630-acae-d8ccbfa4414c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7226c450ed3c854847d609e2ddadf2f250ea5691
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189492"
---
# <a name="set-offsets-transact-sql"></a>SET OFFSETS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il valore di offset (posizione relativa all'inizio di un'istruzione) delle parole chiave specificate in istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] alle applicazioni DB-Library.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET OFFSETS keyword_list { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *keyword_list*  
 Elenco delimitato da virgole di costrutti [!INCLUDE[tsql](../../includes/tsql-md.md)] che include SELECT, FROM, ORDER, TABLE, PROCEDURE, STATEMENT, PARAM ed EXECUTE.  
  
## <a name="remarks"></a>Commenti  
 L'opzione SET OFFSETS viene utilizzata solo in applicazioni DB-Library.  
  
 L'opzione SET OFFSETS viene impostata in fase di analisi, non in fase di esecuzione. Con l'impostazione in fase di analisi, se l'istruzione SET è inclusa nel batch o nella stored procedure l'impostazione diventa effettiva indipendentemente dal fatto che l'esecuzione del codice raggiunga effettivamente il punto. L'istruzione SET ha inoltre effetto prima di qualsiasi altra istruzione eseguita. Ad esempio, se l'istruzione SET è inclusa in un blocco di istruzione IF...ELSE che non viene mai raggiunto in fase di esecuzione, l'istruzione SET viene comunque eseguita perché il blocco IF...ELSE viene analizzato.  
  
 Se l'opzione SET OFFSETS viene impostata in una stored procedure, il valore dell'opzione SET OFFSETS viene ripristinato al termine della stored procedure. Un'istruzione SET OFFSETS specificata nel linguaggio SQL dinamico pertanto non ha alcun effetto sulle istruzioni successive.  
  
 SET PARSEONLY restituisce valori di offset se l'opzione OFFSETS è impostata su ON e non si verificano errori.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET PARSEONLY &#40;Transact-SQL&#41;](../../t-sql/statements/set-parseonly-transact-sql.md)  
  
  
