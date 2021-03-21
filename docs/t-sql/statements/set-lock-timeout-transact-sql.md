---
description: SET LOCK_TIMEOUT (Transact-SQL)
title: SET LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LOCK_TIMEOUT_TSQL
- SET_LOCK_TIMEOUT_TSQL
- SET LOCK_TIMEOUT
- LOCK_TIMEOUT
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- releasing locks
- LOCK_TIMEOUT option
- SET LOCK_TIMEOUT statement
- locking [SQL Server], time-outs
- wait time for lock releases [SQL Server]
ms.assetid: dd0c389e-956d-435e-bf71-e16624a0a215
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ed18868e9b5509e4e8d47e6bf5b80d55bc4bf62
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754291"
---
# <a name="set-lock_timeout-transact-sql"></a>SET LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Specifica l'intervallo in millisecondi durante il quale un'istruzione rimane in attesa del rilascio di un blocco.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SET LOCK_TIMEOUT timeout_period  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *timeout_period*  
 Intervallo di attesa, in millisecondi, prima che [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisca un errore di blocco. Il valore -1 (predefinito) corrisponde a nessun periodo di timeout, ovvero a un'attesa infinita.  
  
 Quando l'attesa per un blocco supera il valore di timeout, viene restituito un errore. Un valore uguale a 0 significa nessuna attesa e non appena viene incontrato un blocco viene visualizzato un messaggio.  
  
## <a name="remarks"></a>Commenti  
 All'inizio di una connessione tale impostazione è uguale a -1. Se viene modificata, la nuova impostazione rimane attiva per il resto della connessione.  
  
 L'opzione SET LOCK_TIMEOUT viene impostata in fase di esecuzione, non in fase di analisi.  
  
 L'hint di blocco READPAST è un'alternativa all'opzione SET.  
  
 Le istruzioni CREATE DATABASE, ALTER DATABASE e DROP DATABASE non rispettano l'impostazione di SET LOCK_TIMEOUT.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-set-the-lock-timeout-to-1800-milliseconds"></a>A. Impostare il timeout di blocco su 1800 millisecondi.  
 Nell'esempio seguente il timeout per l'attesa del blocco viene impostato su `1800` millisecondi.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-set-the-lock-timeout-to-wait-forever-for-a-lock-to-be-released"></a>B. Impostare il timeout di blocco sull'attesa infinita per il rilascio di un blocco.  
 Nell'esempio seguente il timeout di blocco è impostato per l'attesa infinita e non ha scadenza. Questo comportamento predefinito è già impostato all'inizio di ogni connessione.  
  
```sql  
SET LOCK_TIMEOUT -1;  
```  
  
 Nell'esempio seguente il timeout per l'attesa del blocco viene impostato su `1800` millisecondi. In questa versione, [!INCLUDE[ssDW](../../includes/ssdw-md.md)] analizza correttamente l'istruzione, ma ignora il valore 1800 e continua a usare il comportamento predefinito.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

