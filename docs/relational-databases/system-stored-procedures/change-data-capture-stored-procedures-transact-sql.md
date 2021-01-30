---
description: Stored procedure Change Data Capture (Transact-SQL)
title: Stored procedure Change Data Capture (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system stored procedures [SQL Server], change data capture
- change data capture [SQL Server], stored procedures
ms.assetid: 7da7068d-6388-465a-b708-a2f27ded1efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1b0794e4c35038a16f09a41098cad3b7384783a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203756"
---
# <a name="change-data-capture-stored-procedures-transact-sql"></a>Stored procedure Change Data Capture (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Change Data Capture rende disponibile in un formato relazionale appropriato il record storico dell'attività DML (Data Manipulation Language) che si è verificata nelle tabelle abilitate. Le stored procedure seguenti vengono utilizzate per configurare Change Data Capture, gestire i processi dell'agente di Change Data Capture e fornire i metadati correnti ai consumer dei dati delle modifiche.  

:::row:::
    :::column:::
        [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)

        [sys.sp_cdc_change_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md)

        [sys.sp_cdc_cleanup_change_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-cleanup-change-table-transact-sql.md)

        [sys.sp_cdc_disable_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)

        [sys.sp_cdc_disable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)

        [sys.sp_cdc_drop_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md)

        [sys.sp_cdc_enable_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)

        [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)

        [sys.sp_cdc_get_captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)

        [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)

        [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)

        [sys.sp_cdc_help_jobs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)

        [sys.sp_cdc_scan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md)

        [sys.sp_cdc_start_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md)

        [sys.sp_cdc_stop_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Tabelle Change Data Capture &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)  
  
  
