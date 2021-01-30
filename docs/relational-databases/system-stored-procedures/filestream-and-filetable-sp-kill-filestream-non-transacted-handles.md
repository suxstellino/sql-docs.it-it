---
description: sp_kill_filestream_non_transacted_handles (Transact-SQL)
title: sp_kill_filestream_non_transacted_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_kill_filestream_non_transacted_handles_TSQL
- sp_kill_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sp_kill_filestream_non_transacted_handles
ms.assetid: 7188353e-ab29-49a0-8f25-7fb8ab122589
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd3ba3d653b8f1626f89c7bb979580c666dd830a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165234"
---
# <a name="sp_kill_filestream_non_transacted_handles-transact-sql"></a>sp_kill_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di chiudere handle di file non transazionali per dati di tabelle FileTable.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
sp_kill_filestream_non_transacted_handles [[ @table_name = ] 'table_name', [[ @handle_id = ] @handle_id]]  
```  
  
## <a name="arguments"></a>Argomenti  
 *table_name*  
 Nome della tabella in cui chiudere handle non transazionali.  
  
 È possibile passare *table_name* senza *handle_id* per chiudere tutti gli handle non transazionali aperti per la tabella FileTable.  
  
 È possibile passare NULL come valore di *table_name* per chiudere tutti gli handle non transazionali aperti per tutte le tabelle FileTable nel database corrente. Il valore predefinito è NULL.  
  
 *handle_id*  
 ID facoltativo del singolo handle da chiudere. È possibile ottenere il *handle_id* dalla vista a gestione dinamica [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md) . Ogni ID è univoco in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si specifica *handle_id*, è necessario fornire anche un valore per *table_name*.  
  
 È possibile passare NULL come valore di *handle_id* per chiudere tutti gli handle non transazionali aperti per la tabella FileTable specificata da *table_name*. Il valore predefinito è NULL.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-set"></a>Set di risultati  
 Nessuna.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Il *handle_id* richiesto da **sp_kill_filestream_non_transacted_handles** non è correlato al session_id o all'unità di lavoro utilizzata in altri comandi **Kill** .  
  
 Per altre informazioni, vedere [Gestire le tabelle FileTable](../../relational-databases/blob/manage-filetables.md).  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sugli handle di file non transazionali aperti, eseguire una query sulla vista a gestione dinamica [sys.dm_filestream_non_transacted_handles &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione **View database state** per ottenere gli handle di file dalla vista a gestione dinamica **sys.dm_FILESTREAM_non_transacted_handles** e per eseguire **sp_kill_filestream_non_transacted_handles**.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti viene illustrato come chiamare **sp_kill_filestream_non_transacted_handles** per chiudere gli handle di file non transazionali per i dati della tabella FileTable.  
  
```sql  
-- Close all open handles in the current database.  
sp_kill_filestream_non_transacted_handles  
  
-- Close all open handles in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable'  
  
-- Close a specific handle in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable', @handle_id = 0xFFFAAADD  
```  
  
 Nell'esempio seguente viene illustrato come utilizzare uno script per ottenere un *handle_id* e chiuderlo.  
  
```sql  
DECLARE @handle_id varbinary(16);  
DECLARE @table_name sysname;  
  
SELECT TOP 1 @handle_id = handle_id, @table_name = Object_name(table_id)  
FROM sys.dm_FILESTREAM_non_transacted_handles;  
  
EXEC sp_kill_filestream_non_transacted_handles @dbname, @table_name, @handle_id;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire tabelle FileTable](../../relational-databases/blob/manage-filetables.md)  
 [Viste a gestione dinamica FILESTREAM e FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
 <br>[Viste del catalogo FILESTREAM e FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
 <br>[sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
