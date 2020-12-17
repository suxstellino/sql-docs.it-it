---
description: Polling dei server
title: Polling dei server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- target servers [SQL Server], polling interval
- polling master servers [SQL Server]
- server polling [SQL Server]
- master servers [SQL Server], polling
- polling interval [SQL Server]
ms.assetid: 96f5fd43-3edd-4418-9dd0-4d34e618890e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: f28cb7829046b27ad0579ce546373834d6199d02
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477012"
---
# <a name="poll-servers"></a>Polling dei server
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> On [Istanza gestita di SQL di Azure/docs.microsoft.com/azure/sql-database/sql-database-managed-instance), sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Quando viene implementata un'amministrazione multiserver, i server di destinazione contattano periodicamente il server master per caricare le informazioni relative ai processi eseguiti e per eseguire il download di nuovi processi. L'operazione in cui viene contattato il server master, chiamata *polling del server* , viene eseguita a *intervalli di polling* regolari.  
  
## <a name="polling-intervals"></a>Intervalli di polling  
L'intervallo di polling, la cui impostazione predefinita è un minuto, controlla la frequenza con cui il server di destinazione contatta il server master per eseguire il download delle istruzioni e per caricare i risultati dell'esecuzione dei processi.  
  
Quando un server di destinazione esegue il polling del server master, legge le operazioni assegnate al server di destinazione dalla tabella **sysdownloadlist** del database **msdb** . Queste operazioni controllano i processi multiserver e vari aspetti del funzionamento del server di destinazione. Sono esempi di operazioni l'eliminazione, l'inserimento e l'avvio di un processo o l'aggiornamento dell'intervallo di polling di un server di destinazione.  
  
Le operazioni vengono inviate alla tabella **sysdownloadlist** in uno dei modi seguenti:  
  
-   In modo esplicito tramite la stored procedure **sp_post_msx_operation** .  
  
-   In modo implicito tramite altre stored procedure di processo.  
  
Se si utilizzano stored procedure di processo per modificare passaggi o pianificazioni di processo multiserver, oppure oggetti SQL-DMO (SQL Distributed Management Object) per controllare processi multiserver, eseguire il comando seguente dopo la modifica delle pianificazioni o dei passaggi di un processo multiserver:  
  
```  
EXECUTE msdb.dbo.sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
Il comando consente di mantenere i server di destinazione sincronizzati con la definizione del processo corrente.  
  
Se si usano gli elementi seguenti non è necessario inviare operazioni in modo esplicito:  
  
-   Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per controllare i processi multiserver.  
  
-   Stored procedure di processo che non modificano pianificazioni o passaggi di processo.  
  
**Per forzare un server di destinazione a eseguire il polling del server master**  
  
-   [SQL Server Management Studio](../../ssms/agent/force-a-target-server-to-poll-the-master-server.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione di eventi](../../ssms/agent/manage-events.md)  
