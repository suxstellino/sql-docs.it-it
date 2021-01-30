---
description: sys.dm_exec_input_buffer (Transact-SQL)
title: sys.dm_exec_input_buffer (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_input_buffer
- sys.dm_exec_input_buffer _tsql
- dm_exec_input_buffer
- dm_exec_input_buffer_tsql
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_input_buffer dynamic management function
ms.assetid: fb34a560-bde9-4ad9-aa96-0d4baa4fc104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1560c1147df8373cf75fa5640ca8ee6c6ce97bdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159871"
---
# <a name="sysdm_exec_input_buffer-transact-sql"></a>sys.dm_exec_input_buffer (Transact-SQL)

[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]

Restituisce informazioni sulle istruzioni inviate a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="syntax"></a>Sintassi

```
sys.dm_exec_input_buffer ( session_id , request_id )
```

## <a name="arguments"></a>Argomenti

*session_id* ID della sessione in cui è in esecuzione il batch da cercare. *session_id* è **smallint**. è possibile ottenere *session_id* dagli oggetti a gestione dinamica seguenti:

- [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)

*request_id* Request_id da [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). *request_id* è di **tipo int**.

## <a name="table-returned"></a>Tabella restituita

|Nome colonna|Tipo di dati|Descrizione|
|-----------------|---------------|-----------------|
|**event_type**|**nvarchar(256)**|Tipo di evento nel buffer di input per lo SPID specificato.|
|**parameters**|**smallint**|Tutti i parametri forniti per l'istruzione.|
|**event_info**|**nvarchar(max)**|Testo dell'istruzione nel buffer di input per lo SPID specificato.|

## <a name="permissions"></a>Autorizzazioni

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se l'utente dispone dell'autorizzazione View Server state, l'utente visualizzerà tutte le sessioni in esecuzione nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . in caso contrario, l'utente visualizzerà solo la sessione corrente.

> [!IMPORTANT]
> L'esecuzione di questa DMV al di fuori di SQL Server Management Studio rispetto SQL Server senza autorizzazioni VIEW SERVER STATE, ad esempio in un trigger, stored procedure o Function, genera un errore di autorizzazione nel database master.

In [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , se l'utente è il proprietario del database, l'utente visualizzerà tutte le sessioni in esecuzione sul. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in caso contrario, l'utente visualizzerà solo la sessione corrente.

> [!IMPORTANT]
> L'esecuzione di questa DMV all'esterno dei SQL Server Management Studio nel database SQL di Azure senza autorizzazioni del proprietario, ad esempio in un trigger, stored procedure o funzione, genera un errore di autorizzazione nel database master.

## <a name="remarks"></a>Commenti

Questa funzione a gestione dinamica può essere usata in combinazione con sys.dm_exec_sessions o sys.dm_exec_requests eseguendo **Cross Apply**.

## <a name="examples"></a>Esempi

### <a name="a-simple-example"></a>R. Esempio semplice

Nell'esempio seguente viene illustrato il passaggio di un ID di sessione (SPID) e di un ID richiesta alla funzione.

```sql
SELECT * FROM sys.dm_exec_input_buffer (52, 0);
GO
```

### <a name="b-using-cross-apply-to-additional-information"></a>B. Utilizzo di Cross Apply per informazioni aggiuntive

Nell'esempio seguente viene elencato il buffer di input per le sessioni con ID sessione maggiore di 50.

```sql
SELECT es.session_id, ib.event_info
FROM sys.dm_exec_sessions AS es
CROSS APPLY sys.dm_exec_input_buffer(es.session_id, NULL) AS ib
WHERE es.session_id > 50;
GO
```

## <a name="see-also"></a>Vedere anche

- [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
- [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)
