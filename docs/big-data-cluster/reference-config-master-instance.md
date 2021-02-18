---
title: Proprietà di configurazione dell'istanza master di SQL Server
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per le proprietà di configurazione per l'istanza master di SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343516"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>Proprietà di configurazione dell'istanza di SQL Server Master-versione pre-CU9

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> Le informazioni seguenti sono applicabili solo ai cluster di versioni precedenti a CU9 che non sono abilitati per la configurazione e richiedono MSSQL-conf per configurare l'istanza di SQL Server master. I cluster CU9 e versione successiva sfruttano le funzionalità di gestione della configurazione e non richiedono più un file MSSQL-conf. Le configurazioni disponibili per l'istanza master di SQL Server e altri componenti BDC sono reperibili [qui](reference-config-bdc-overview.md).

## <a name="properties"></a>Proprietà

È possibile configurare le seguenti opzioni di SQL Server per l'istanza master in fase di distribuzione.

|Proprietà|Opzioni|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>Esempi

L'esempio seguente abilita SQL Server Agent e la telemetria, imposta un PID per l'edizione Enterprise e abilita il flag di traccia 1204.

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

Per istruzioni, vedere [Configurare l'istanza master di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md).

## <a name="next-steps"></a>Passaggi successivi

[Configurare l'istanza master di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md)
