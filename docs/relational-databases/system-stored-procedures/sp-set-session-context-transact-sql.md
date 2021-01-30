---
description: sp_set_session_context (Transact-SQL)
title: sp_set_session_context (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_set_session_context
- sp_set_session_context_TSQL
- sys.sp_set_session_context
- sys.sp_set_session_context_TSQL
helpviewer_keywords:
- sp_set_session_context
ms.assetid: 7a3a3b2a-1408-4767-a376-c690e3c1fc5b
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4ad470228567b26a618800ec004a2a45c5f5440e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209290"
---
# <a name="sp_set_session_context-transact-sql"></a>sp_set_session_context (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Imposta una coppia chiave-valore nel contesto della sessione.  
  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_set_session_context [ @key= ] N'key', [ @value= ] 'value'  
    [ , [ @read_only = ] { 0 | 1 } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @key =] N'key '  
 Chiave da impostare, di tipo **sysname**. Le dimensioni massime della chiave sono pari a 128 byte.  
  
 [ @value =]' valore '  
 Valore per la chiave specificata, di tipo **sql_variant**. L'impostazione di un valore NULL libera la memoria. Le dimensioni massime sono pari a 8.000 byte.  
  
 [ @read_only =] {0 | 1}  
 Flag di tipo **bit**. Se è 1, il valore per la chiave specificata non può essere modificato nuovamente in questa connessione logica. Se è 0 (impostazione predefinita), il valore può essere modificato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente può impostare un contesto di sessione per la sessione.  
  
## <a name="remarks"></a>Commenti  
 Come altre stored procedure, solo i valori letterali e le variabili (non espressioni o chiamate di funzione) possono essere passati come parametri.  
  
 Le dimensioni totali del contesto della sessione sono limitate a 1 MB. Se si imposta un valore che determina il superamento di questo limite, l'istruzione ha esito negativo. È possibile monitorare l'utilizzo complessivo della memoria in [sys.dm_os_memory_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).  
  
 È possibile monitorare l'utilizzo complessivo della memoria eseguendo una query [sys.dm_os_memory_cache_counters &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) come segue: `SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT';`  
  
## <a name="examples"></a>Esempi  
R. Nell'esempio seguente viene illustrato come impostare e restituire una chiave di contesto di sessioni denominata Language con un valore di English.  
  
```  
EXEC sys.sp_set_session_context @key = N'language', @value = 'English';  
SELECT SESSION_CONTEXT(N'language');  
```  
  
 Nell'esempio seguente viene illustrato l'utilizzo del flag di sola lettura facoltativo.  
  
```  
EXEC sys.sp_set_session_context @key = N'user_id', @value = 4, @read_only = 1;  
```  

B. Nell'esempio seguente viene illustrato come impostare e recuperare una chiave di contesto della sessione denominata client_correlation_id con un valore 12323ad.
```
-- set value
EXEC sp_set_session_context 'client_correlation_id', '12323ad'; 

--check value
SELECT SESSION_CONTEXT(N'client_correlation_id');

```

## <a name="see-also"></a>Vedere anche  
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [SESSION_CONTEXT &#40;&#41;Transact-SQL ](../../t-sql/functions/session-context-transact-sql.md)   
 [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
