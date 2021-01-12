---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 98e5e9b90449836a4370771c35e12ea82007aed6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099751"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga con le impostazioni correnti per le proprietà delle risorse cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificate in questo argomento. Non verrà restituito alcun dato se questa vista viene eseguita in un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Queste proprietà vengono utilizzate per impostare i valori che influiscono sul rilevamento degli errori, sui tempi di risposta agli errori e sulla registrazione per il monitoraggio dello stato integrità dell'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

|Nome colonna|Proprietà|Descrizione|  
|-----------------|--------------|-----------------|  
|VerboseLogging|bigint|Livello di registrazione per il cluster di failover di SQL Server. La registrazione dettagliata può essere attivata per fornire dettagli aggiuntivi nei log degli errori per la risoluzione dei problemi. Uno dei valori seguenti:<br /><br /> 0: la registrazione è disabilitata (impostazione predefinita)<br /><br /> 1: solo errori<br /><br /> 2: errori e avvisi<br /><br /> Per ulteriori informazioni, vedere [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|bigint|I flag di dump di SQLDumper determinano il tipo dei file di dump generati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'impostazione predefinita è 0.|  
|SqlDumperDumpPath|nvarchar(260)|Percorso in cui l'utilità SQLDumper genera i file di dump.|  
|SqlDumperDumpTimeOut|bigint|Valore di timeout in millisecondi prima che l'utilità SQLDumper generi un dump in caso di errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore predefinito è 0.|  
|FailureConditionLevel|bigint|Consente di impostare le condizioni in cui il cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve restituire un errore o essere riavviato. Il valore predefinito è 3. Per una spiegazione dettagliata o per modificare le impostazioni delle proprietà, vedere [configurare le impostazioni delle proprietà FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|bigint|Valore di timeout che consente di definire il tempo di attesa da parte della DLL risorse del motore di database di SQL Server relativo alla restituzione delle informazioni sull'integrità del server prima che venga stabilita la mancata risposta dell'istanza di SQL Server. Il valore di timeout è espresso in millisecondi. Il valore predefinito è 60000. Per altre informazioni o per modificare questa impostazione della proprietà, vedere [configurare le impostazioni delle proprietà HealthCheckTimeout](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede le autorizzazioni VIEW SERVER STATE sull'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato sys.dm_os_cluster_properties per restituire le impostazioni della proprietà per la risorsa cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 Set di risultati di esempio:  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60000|  
  
  
