---
title: Stored procedure estese (Transact-SQL)
description: Informazioni sulle stored procedure estese che è possibile usare quando si lavora con i database abilitati per l'estensione. Vedere come riconciliare le colonne ed eseguire altre attività.
titleSuffix: SQL Server Stretch Database
ms.custom: seo-dt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Stretch Database, stored procedures
ms.assetid: bda29952-4b8b-4295-ab78-f24dcb0b03c6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 556e74812ca328a42f1332f20b2576b4cc1b9359
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178064"
---
# <a name="stretch-database-extended-stored-procedures-transact-sql"></a>Stretch Database stored procedure estese (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

 In questa sezione vengono descritte le stored procedure estese correlate a Stretch Database.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
[sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) Rimuove la connessione autenticata tra un database locale abilitato per l'estensione e il database di Azure remoto.

[sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md) Ottiene il numero di ore di dati migrati che SQL Server conserva in una tabella di staging per garantire un ripristino completo del database di Azure remoto, se è necessario un ripristino.
  
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) Ripristina la connessione autenticata tra un database locale abilitato per l'estensione e il database remoto.
  
 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)  
 Riconcilia l'ID batch archiviato nella tabella SQL Server abilitata per l'estensione per i dati migrati più di recente con l'ID batch archiviato nella tabella remota di Azure. 
 
[sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md) Consente di riconciliare le colonne della tabella remota di Azure con le colonne nella tabella SQL Server abilitata per l'estensione.
 
 [sys.sp_rda_reconcile_indexes](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-indexes-transact-sql.md) Accoda un'attività dello schema per riconciliare gli indici nella tabella remota.
 
 [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) Specifica se le query eseguite sul database corrente abilitato per l'estensione e sulle relative tabelle restituiscono dati locali e remoti (impostazione predefinita) o solo dati locali.
 
 [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) Imposta il numero di ore di dati migrati che SQL Server conserva in una tabella di staging per garantire un ripristino completo del database di Azure remoto, se è necessario un ripristino.
 
 [sys.sp_rda_test_connection](../../relational-databases/system-stored-procedures/sys-sp-rda-test-connection-transact-sql.md) Verifica la connessione da SQL Server al server Azure remoto e segnala problemi che possono impedire la migrazione dei dati.
 
## <a name="see-also"></a>Vedere anche  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
