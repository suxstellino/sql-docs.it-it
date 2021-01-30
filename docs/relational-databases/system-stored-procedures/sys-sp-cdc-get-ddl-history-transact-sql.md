---
description: sys.sp_cdc_get_ddl_history (Transact-SQL)
title: sys.sp_cdc_get_ddl_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_get_ddl_history
- sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sp_cdc_get_ddl_history
- sys.sp_cdc_get_ddl_history
ms.assetid: 4dee5e2e-d7e5-4fea-8037-a4c05c969b3a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b83e018c8e8e7c5e2f4daa704e5b7a13e7789f0b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205954"
---
# <a name="syssp_cdc_get_ddl_history-transact-sql"></a>sys.sp_cdc_get_ddl_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce la cronologia delle modifiche Data Definition Language (DDL) associata all'istanza di acquisizione specificata a partire dall'abilitazione di Change Data Capture per l'istanza di acquisizione. Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_get_ddl_history [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @capture_instance =]'*capture_instance*'  
 Nome dell'istanza di acquisizione associata a una tabella di origine. *capture_instance* è di **tipo sysname** e non può essere null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nome dello schema della tabella di origine.|  
|source_table|**sysname**|Nome della tabella di origine.|  
|capture_instance|**sysname**|Nome dell'istanza di acquisizione.|  
|required_column_update|**bit**|Indica che la modifica DDL ha richiesto una colonna della tabella delle modifiche che deve essere modificata per riflettere una modifica del tipo di dati apportata alla colonna di origine.|  
|ddl_command|**nvarchar(max)**|Istruzione DDL applicata alla tabella di origine.|  
|ddl_lsn|**binary(10)**|Numero di sequenza del file di log (LSN) associato alla modifica DDL.|  
|ddl_time|**datetime**|Ora associata alla modifica DDL.|  
  
## <a name="remarks"></a>Commenti  
 Le modifiche DDL apportate alla tabella di origine che modificano la struttura della colonna della tabella di origine, ad esempio l'aggiunta o l'eliminazione di una colonna o la modifica del tipo di dati di una colonna esistente, vengono mantenute nella tabella [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) . È possibile creare un report su queste modifiche utilizzando questa stored procedure. Le voci di cdc.ddl_history vengono create quando il processo di acquisizione legge la transazione DDL nel log.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner per la restituzione delle righe relative a tutte le istanze di acquisizione del database. Per tutti gli altri utenti, è richiesta l'autorizzazione SELECT su tutte le colonne acquisite nella tabella di origine e, se è stato definito un ruolo di controllo per l'istanza di acquisizione, l'appartenenza a tale ruolo del database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiunta una colonna alla tabella di origine `HumanResources.Employee` e viene eseguita la stored procedure `sys.sp_cdc_get_ddl_history` per creare un report sulle modifiche DDL che si applicano alla tabella di origine associata all'istanza di acquisizione `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
ALTER TABLE HumanResources.Employee  
ADD Test_Column int NULL;  
GO  
-- Pause 10 seconds to allow the event to be logged.   
WAITFOR DELAY '00:00:10';  
GO   
EXECUTE sys.sp_cdc_get_ddl_history   
    @capture_instance = 'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
