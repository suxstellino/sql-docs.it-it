---
description: SET REMOTE_PROC_TRANSACTIONS (Transact-SQL)
title: SET REMOTE_PROC_TRANSACTIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- REMOTE_PROC_TRANSACTIONS_TSQL
- SET REMOTE_PROC_TRANSACTIONS
- REMOTE_PROC_TRANSACTIONS
- SET_REMOTE_PROC_TRANSACTIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- SET REMOTE_PROC_TRANSACTIONS statement
- distributed transactions [SQL Server], starting
- REMOTE_PROC_TRANSACTIONS option
- active transactions
ms.assetid: 4d284ae9-3f5f-465a-b0dd-1328a4832a03
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ecfcda70eb37c4b7b10db2ac49a02996140060da
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100694"
---
# <a name="set-remote_proc_transactions-transact-sql"></a>SET REMOTE_PROC_TRANSACTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Specifica che, quando una transazione locale è attiva, l'esecuzione di una stored procedure remota comporta l'avvio di una transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)] gestita da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Questa opzione è disponibile per la compatibilità con le versioni precedenti per le applicazioni che utilizzano stored procedure remote. Anziché pubblicare chiamate della stored procedure remota, utilizzare query distribuite che fanno riferimento a server collegati che vengono definiti tramite [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET REMOTE_PROC_TRANSACTIONS { ON | OFF }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 ON | OFF  
 Quando è impostata su ON, viene avviata una transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)] quando in una transazione locale viene eseguita una stored procedure remota. Quando è impostata su OFF, la chiamata di una stored procedure remota in una transazione locale non avvia una transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="remarks"></a>Osservazioni  
 Quando l'opzione REMOTE_PROC_TRANSACTIONS è impostata su ON, la chiamata di una stored procedure remota comporta l'avvio di una transazione distribuita e l'integrazione della transazione in MS DTC. L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene chiamata la stored procedure remota corrisponde all'origine della transazione e ne controlla il completamento. Quando per la connessione viene successivamente eseguita un'istruzione COMMIT TRANSACTION o ROLLBACK TRANSACTION, l'istanza di controllo richiede che il completamento della transazione distribuita nei computer interessati venga gestito da MS DTC.  
  
 Dopo l'avvio di una transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)], è possibile chiamare stored procedure remote in altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che sono state definite come server remoti. Tutti i server remoti sono integrati nella transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)]. MS DTC assicura inoltre che la transazione venga completata in ogni server remoto.  
  
 L'opzione REMOTE_PROC_TRANSACTIONS è un'impostazione a livello di connessione che consente di ignorare l'opzione **sp_configure remote proc trans** a livello di istanza.  
  
 Quando l'opzione REMOTE_PROC_TRANSACTIONS viene impostata su OFF, le chiamate di stored procedure remote non diventano parte di una transazione locale. Al completamento della stored procedure remota viene eseguito il commit o il rollback delle modifiche apportate dalla stored procedure. Le successive istruzioni COMMIT TRANSACTION o ROLLBACK TRANSACTION eseguite dalla connessione che ha chiamato la stored procedure remota non hanno alcun effetto sull'elaborazione eseguita dalla procedura.  
  
 L'opzione REMOTE_PROC_TRANSACTIONS è disponibile per compatibilità con le versioni precedenti e ha effetto solo sulle chiamate di stored procedure remote indirizzate a istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che sono state definite come server remoti tramite **sp_addserver**. L'opzione non viene applicata alle query distribuite che eseguono una stored procedure in un'istanza definita come server collegato tramite **sp_addlinkedserver**.  
  
 L'opzione SET REMOTE_PROC_TRANSACTIONS viene impostata in fase di esecuzione, non in fase di analisi.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
