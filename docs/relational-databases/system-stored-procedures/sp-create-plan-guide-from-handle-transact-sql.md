---
description: sp_create_plan_guide_from_handle (Transact-SQL)
title: sp_create_plan_guide_from_handle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide_from_handle_TSQL
- sp_create_plan_guide_from_handle
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide_from_handle
ms.assetid: 02cfb76f-a0f9-4b42-a880-1c3e7d64fe41
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2020abc49ccd29491f99adc77910868071e16e00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186222"
---
# <a name="sp_create_plan_guide_from_handle-transact-sql"></a>sp_create_plan_guide_from_handle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una o più guide di piano da un piano di query nella cache dei piani. È possibile utilizzare questa stored procedure per garantire che Query Optimizer utilizzi sempre un determinato piano di query per una query specificata. Per altre informazioni sulle guide di piano, vedere [Guide di piano](../../relational-databases/performance/plan-guides.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_create_plan_guide_from_handle [ @name = ] N'plan_guide_name'  
    , [ @plan_handle = ] plan_handle  
    , [ [ @statement_start_offset = ] { statement_start_offset | NULL } ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @name =] N'*plan_guide_name*'  
 Nome della guida di piano. I nomi delle guide di piano vengono definiti a livello dell'ambito del database corrente. *plan_guide_name* devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md) e non possono iniziare con il simbolo di cancelletto (#). La lunghezza massima consentita di *plan_guide_name* è 124 caratteri.  
  
 [ @plan_handle =] *plan_handle*  
 Identifica un batch nella cache dei piani. *plan_handle* è di tipo **varbinary (64)**. è possibile ottenere *plan_handle* dalla vista a gestione dinamica [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 [ @statement_start_offset =] { *statement_start_offset* | NULL}]  
 Identifica la posizione iniziale dell'istruzione all'interno del batch del *plan_handle* specificato. *statement_start_offset* è di **tipo int** e il valore predefinito è null.  
  
 L'offset dell'istruzione corrisponde alla colonna statement_start_offset nella vista a gestione dinamica [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 Specificando un valore NULL o non specificando alcun offset dell'istruzione, viene creata una guida di piano per ogni istruzione nel batch utilizzando il piano di query per l'handle di piano specificato. Le guide di piano risultanti equivalgono alle guide di piano che utilizzano l'hint per la query USE PLAN per forzare l'utilizzo di un piano specifico.  
  
## <a name="remarks"></a>Commenti  
 Non è possibile creare una guida di piano per tutti i tipi di istruzione. Se non è possibile creare una guida di piano per un'istruzione nel batch, la stored procedure ignora l'istruzione e passa all'istruzione successiva nel batch. Se un'istruzione è presente più volte nello stesso batch, viene abilitato il piano per l'ultima occorrenza, disabilitando i piani precedenti per l'istruzione. Se non è possibile utilizzare alcuna istruzione nel batch in una guida di piano, viene generato l'errore 10532 e l'istruzione ha esito negativo. Si consiglia di ottenere sempre l'handle di piano dalla vista a gestione dinamica sys.dm_exec_query_stats, al fine di evitare l'insorgere dell'errore.  
  
> [!IMPORTANT]  
>  sp_create_plan_guide_from_handle crea guide di piano basate sui piani seguendone la visualizzazione nella cache dei piani. Ciò significa che il testo del batch, le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e Showplan XML vengono acquisiti carattere per carattere (includendo qualsiasi valore letterale passato alla query) dalla cache dei piani alla guida di piano risultante. Tali stringhe di testo possono contenere informazioni riservate che vengono quindi archiviate nei metadati del database. Gli utenti con le autorizzazioni appropriate possono visualizzare queste informazioni utilizzando la vista del catalogo sys.plan_guides e la finestra di dialogo **Proprietà Guida di piano** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Per garantire che le informazioni riservate non vengano divulgate mediante una guida di piano, si consiglia di esaminare le guide di piano create dalla cache dei piani.  
  
## <a name="creating-plan-guides-for-multiple-statements-within-a-query-plan"></a>Creazione di guide di piano per più istruzioni all'interno di un piano di query  
 Analogamente a sp_create_plan_guide, sp_create_plan_guide_from_handle rimuove il piano di query per il batch o il modulo applicato dalla cache dei piani. Questa operazione consente a tutti gli utenti di iniziare a utilizzare la nuova guida di piano. Quando si crea una guida di piano per più istruzioni all'interno di un unico piano di query, è possibile posticipare la rimozione del piano dalla cache creando tutte le guide di piano in una transazione esplicita. Questo metodo consente al piano di rimanere nella cache fino al completamento della transazione e alla creazione di una guida di piano per ogni istruzione specificata. Vedere l'esempio B.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW SERVER STATE`. In aggiunta, sono richieste autorizzazioni singole per ogni guida di piano creata utilizzando sp_create_plan_guide_from_handle. Per creare una guida di piano di tipo OBJECT è necessaria `ALTER` l'autorizzazione per l'oggetto a cui si fa riferimento. Per creare una guida di piano di tipo SQL o TEMPLATE è necessaria `ALTER` l'autorizzazione per il database corrente. Per determinare il tipo di guida di piano che verrà creato, eseguire la query seguente:  
  
```sql  
SELECT cp.plan_handle, sql_handle, st.text, objtype   
FROM sys.dm_exec_cached_plans AS cp  
JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st;  
```  
  
 Nella riga che contiene l'istruzione per la quale si sta creando la guida di piano, esaminare la colonna `objtype` nel set di risultati. Un valore `Proc` indica che la guida di piano è di tipo OBJECT. Altri valori, come ad esempio `AdHoc` o `Prepared` indicano che la guida di piano è di tipo SQL.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-plan-guide-from-a-query-plan-in-the-plan-cache"></a>R. Creazione di una guida di piano da un piano di query nella cache dei piani  
 Nell'esempio seguente viene creata una guida di piano per una singola istruzione SELECT specificando un piano di query dalla cache dei piani. Viene innanzitutto eseguita una semplice istruzione `SELECT` per la quale verrà creata la guida di piano. Il piano per la query viene esaminato utilizzando le viste a gestione dinamica `sys.dm_exec_sql_text` e `sys.dm_exec_text_query_plan`. La guida di piano viene quindi creata per la query specificando il piano di query nella cache dei piani a essa associata. Nell'esempio, l'istruzione finale verifica l'esistenza della guida di piano.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT WorkOrderID, p.Name, OrderQty, DueDate  
FROM Production.WorkOrder AS w   
JOIN Production.Product AS p ON w.ProductID = p.ProductID  
WHERE p.ProductSubcategoryID > 4  
ORDER BY p.Name, DueDate;  
GO  
-- Inspect the query plan by using dynamic management views.  
SELECT * FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle)  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
-- Create a plan guide for the query by specifying the query plan in the plan cache.  
DECLARE @plan_handle varbinary(64);  
DECLARE @offset int;  
SELECT @plan_handle = plan_handle, @offset = qs.statement_start_offset  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
  
EXECUTE sp_create_plan_guide_from_handle   
    @name =  N'Guide1',  
    @plan_handle = @plan_handle,  
    @statement_start_offset = @offset;  
GO  
-- Verify that the plan guide is created.  
SELECT * FROM sys.plan_guides  
WHERE scope_batch LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
```  
  
### <a name="b-creating-multiple-plan-guides-for-a-multistatement-batch"></a>B. Creazione di più guide di piano per un batch costituito da più istruzioni  
 Nell'esempio seguente viene creata una guida di piano per due istruzioni all'interno di un batch costituito da più istruzioni. Le guide di piano vengono create all'interno di una transazione esplicita, in modo da non rimuovere dalla cache dei piani il piano di query per il batch, dopo la creazione della prima guida di piano. Viene innanzitutto eseguito un batch costituito da più istruzioni. Il piano per il batch viene esaminato utilizzando le viste a gestione dinamica. Si noti che viene restituita una riga per ogni istruzione nel batch. Viene quindi creata una guida di piano per la prima e la terza istruzione nel batch specificando il parametro `@statement_start_offset`. Nell'esempio, l'istruzione finale verifica l'esistenza delle guide di piano.  
  
 [!code-sql[PlanGuides#Create_From_Handle2](../../relational-databases/system-stored-procedures/codesnippet/tsql/sp-create-plan-guide-fro_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [Guide di piano](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_text_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
  
  
