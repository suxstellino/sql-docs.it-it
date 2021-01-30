---
description: ALTER RESOURCE GOVERNOR (Transact-SQL)
title: ALTER RESOURCE GOVERNOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER RESOURCE GOVERNOR
- ALTER_RESOURCE_GOVERNOR_TSQL
- ALTER RESOURCE GOVERNOR RECONFIGURE
- ALTER_RESOURCE_GOVERNOR_RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE GOVERNOR
- RECONFIGURE, ALTER RESOURCE GOVERNOR
ms.assetid: 442c54bf-a0a6-4108-ad20-db910ffa6e3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cd2d0ecbf0dde509c57afb8991b4cd2676eaf82b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204072"
---
# <a name="alter-resource-governor-transact-sql"></a>ALTER RESOURCE GOVERNOR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa istruzione viene usata per eseguire in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le azioni di Resource Govenor indicate di seguito:  
  
-   Applicare le modifiche di configurazione specificate quando vengono eseguite le istruzioni CREATE|ALTER|DROP WORKLOAD GROUP o CREATE|ALTER|DROP RESOURCE POOL o CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
-   Abilitare o disabilitare Resource Governor.  
  
-   Configurare la classificazione per le richieste in arrivo.  
  
-   Reimpostare le statistiche del pool di risorse e del gruppo del carico di lavoro.  
  
-   Impostare il numero massimo di operazioni di I/O per ogni volume del disco.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER RESOURCE GOVERNOR   
      { DISABLE | RECONFIGURE }  
    | WITH ( CLASSIFIER_FUNCTION = { schema_name.function_name | NULL } )  
    | RESET STATISTICS  
    | WITH ( MAX_OUTSTANDING_IO_PER_VOLUME = value )   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 DISABLE  
 Disabilita Resource Governor, determinando i seguenti risultati:  
  
-   La funzione di classificazione non viene eseguita.  
  
-   Tutte le nuove connessioni vengono automaticamente classificate nel gruppo predefinito.  
  
-   Le richieste avviate dal sistema vengono classificate nel gruppo di carico di lavoro interno.  
  
-   Tutte le impostazioni del gruppo del carico di lavoro e del pool di risorse esistenti vengono reimpostate ai valori predefiniti. In questo caso non viene generato alcun evento quando vengono raggiunti i limiti.  
  
-   Il normale monitoraggio del sistema non viene modificato.  
  
-   Le modifiche alla configurazione non hanno effetto fino all'abilitazione di Resource Governor.  
  
-   Al riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non verrà caricata la configurazione di Resource Governor, ma solo i gruppi e i pool interni e predefiniti.  
  
 RECONFIGURE  
 Se non è abilitato, RECONFIGURE consente di abilitare Resource Governor determinando i seguenti risultati:  
  
-   Viene eseguita la funzione di classificazione per le nuove connessioni, in modo che il relativo carico di lavoro possa essere assegnato ai gruppi del carico di lavoro.  
  
-   Vengono imposti e applicati i limiti delle risorse specificati nella configurazione di Resource Governor.  
  
-   Le richieste esistenti prima dell'abilitazione di Resource Governor sono interessate dalle modifiche alla configurazione effettuate quando Resource Governor è stato disabilitato.  
  
 Quando Resource Governor è in esecuzione, RECONFIGURE applica ogni modifica di configurazione richiesta durante l'esecuzione delle istruzioni CREATE|ALTER|DROP WORKLOAD GROUP o CREATE|ALTER|DROP RESOURCE POOL o CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
> [!IMPORTANT]  
>  ALTER RESOURCE GOVERNOR RECONFIGURE deve essere eseguito per rendere operativa ogni modifica di configurazione.  
  
 CLASSIFIER_FUNCTION = { _schema_name_ **.** _function_name_ | NULL }  
 Registra la funzione di classificazione specificata da *schema_name.function_name*. La funzione classifica ogni nuova sessione e assegna le richieste e le query della sessione a un gruppo del carico di lavoro. Quando viene utilizzato NULL, le nuove sessioni vengono assegnate automaticamente al gruppo del carico di lavoro predefinito.  
  
 RESET STATISTICS  
 Reimposta le statistiche dei pool di risorse e dei gruppi del carico di lavoro. Per altre informazioni, vedere [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) e [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
 MAX_OUTSTANDING_IO_PER_VOLUME = *value*  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Imposta il numero massimo di operazioni di I/O in coda per ogni volume del disco. Queste operazioni di I/O possono essere letture o scritture di qualsiasi dimensione.  Il valore massimo per MAX_OUTSTANDING_IO_PER_VOLUME è 100. Non è una percentuale. Questa impostazione è progettata per ottimizzare la governance delle risorse rispetto alle caratteristiche di I/O di un volume del disco. È consigliabile provare diversi valori e usare uno strumento di calibrazione come IOMeter, [DiskSpd](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223), o SQLIO (deprecato) per identificare il valore massimo per il sottosistema di archiviazione usato. Questa impostazione fornisce un controllo di sicurezza a livello di sistema che consente a SQL Server di soddisfare l'IOPS minimo per i pool di risorse anche se per gli altri pool MAX_IOPS_PER_VOLUME è impostato su illimitato. Per altre informazioni su MAX_IOPS_PER_VOLUME, vedere [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 ALTER RESOURCE GOVERNOR DISABLE, ALTER RESOURCE GOVERNOR RECONFIGURE e ALTER RESOURCE GOVERNOR RESET STATISTICS non possono essere utilizzate in una transazione utente.  
  
 Il parametro RECONFIGURE appartiene alla sintassi di Resource Governor e non deve essere confuso con [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), che è un'istruzione DDL diversa.  
  
 Prima di eseguire istruzioni DDL, è consigliabile acquisire familiarità con gli stati di Resource Governor. Per altre informazioni, vedere [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL SERVER.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-starting-the-resource-governor"></a>R. Avvio di Resource Governor  
 Alla prima installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Resource Governor risulta disabilitato. Nell'esempio seguente viene avviato Resource Governor. Una volta avviato Resource Governor mediante l'istruzione precedente, sarà possibile utilizzare i gruppi del carico di lavoro e i pool di risorse predefiniti.  
  
```sql  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="b-assigning-new-sessions-to-the-default-group"></a>B. Assegnazione di nuove sessioni al gruppo predefinito  
 Nell'esempio seguente vengono assegnate tutte le nuove sessioni al gruppo del carico di lavoro predefinito rimuovendo qualsiasi funzione di classificazione esistente dalla configurazione di Resource Governor. Quando nessuna funzione viene definita come funzione di classificazione, tutte le nuove sessioni vengono assegnate al gruppo del carico di lavoro predefinito. Questa modifica viene applicata solo alle nuove sessioni e non a quelle esistenti.  
  
```sql  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = NULL);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="c-creating-and-registering-a-classifier-function"></a>C. Creazione e registrazione di una funzione di classificazione  
 Nell'esempio seguente viene creata una funzione di classificazione denominata `dbo.rgclassifier_v1`. La funzione classifica ogni nuova sessione in base al nome utente o al nome dell'applicazione e assegna le richieste e le query della sessione a un gruppo del carico di lavoro specifico. Le sessioni sulle quali non viene eseguito il mapping ai nomi utente o dell'applicazione specifici vengono assegnate al gruppo del carico di lavoro predefinito. La funzione di classificazione viene quindi registrata e viene applicata la modifica della configurazione.  
  
```sql  
-- Store the classifier function in the master database.  
USE master;  
GO  
SET ANSI_NULLS ON;  
GO  
SET QUOTED_IDENTIFIER ON;  
GO  
CREATE FUNCTION dbo.rgclassifier_v1() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
-- Declare the variable to hold the value returned in sysname.  
    DECLARE @grp_name AS sysname  
-- If the user login is 'sa', map the connection to the groupAdmin  
-- workload group.   
    IF (SUSER_NAME() = 'sa')  
        SET @grp_name = 'groupAdmin'  
-- Use application information to map the connection to the groupAdhoc  
-- workload group.  
    ELSE IF (APP_NAME() LIKE '%MANAGEMENT STUDIO%')  
        OR (APP_NAME() LIKE '%QUERY ANALYZER%')  
            SET @grp_name = 'groupAdhoc'  
-- If the application is for reporting, map the connection to  
-- the groupReports workload group.  
    ELSE IF (APP_NAME() LIKE '%REPORT SERVER%')  
        SET @grp_name = 'groupReports'  
-- If the connection does not map to any of the previous groups,  
-- put the connection into the default workload group.  
    ELSE  
        SET @grp_name = 'default'  
    RETURN @grp_name  
END;  
GO  
-- Register the classifier user-defined function and update the   
-- the in-memory configuration.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION=dbo.rgclassifier_v1);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="d-resetting-statistics"></a>D. Reimpostazione delle statistiche  
 Nell'esempio seguente vengono reimpostate tutte le statistiche del gruppo del carico di lavoro e del pool di risorse.  
  
```sql 
ALTER RESOURCE GOVERNOR RESET STATISTICS;  
```  
  
### <a name="e-setting-the-max_outstanding_io_per_volume-option"></a>E. Impostare l'opzione MAX_OUTSTANDING_IO_PER_VOLUME  
 Nell'esempio seguente l'opzione MAX_OUTSTANDING_IO_PER_VOLUME viene impostata su 20.  
  
```sql  
ALTER RESOURCE GOVERNOR  
WITH (MAX_OUTSTANDING_IO_PER_VOLUME = 20);   
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)  
  
  
