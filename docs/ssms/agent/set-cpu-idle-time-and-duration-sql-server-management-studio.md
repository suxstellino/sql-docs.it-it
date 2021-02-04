---
description: Impostare il tempo e l'intervallo di inattività della CPU
title: Impostare il tempo e l'intervallo di inattività della CPU
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- CPU [SQL Server], idle conditions
- time [SQL Server], CPU idle and duration
- duration of CPU idle [SQL Server]
- SQL Server Agent, CPU idle conditions
- idle time [SQL Server]
ms.assetid: 8647b465-d899-4cc7-9640-134a506d0a2e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 0b2d6f2c63967bba489c2f39d975be7e48b93597
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251035"
---
# <a name="set-cpu-idle-time-and-duration"></a>Impostare il tempo e l'intervallo di inattività della CPU

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene illustrato come definire la condizione di inattività della CPU per il server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. La definizione del tempo di inattività della CPU influisce sulla modalità di risposta agli eventi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Si supponga, ad esempio, di considerare la CPU inattiva quando l'utilizzo medio scende sotto il 10% e rimane tale per 10 minuti. Se sono stati definiti processi da eseguire quando la CPU del server raggiunge una condizione di inattività, questi verranno avviati quando l'utilizzo della CPU scende sotto il 10% e rimane tale per 10 minuti. Se si tratta di processi che influiscono significativamente sulle prestazioni del server, la corretta definizione della condizione di inattività della CPU è particolarmente importante.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-set-cpu-idle-time-and-duration"></a>Per impostare il tempo e l'intervallo di inattività della CPU  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent**, scegliere **Proprietà** e selezionare la pagina **Avanzate** .  
  
3.  In **Condizione di inattività CPU** eseguire le operazioni seguenti:  
  
    -   Selezionare la casella di controllo **Imposta condizione di inattività CPU**.  
  
    -   Specificare una percentuale nella casella **L'uso medio della CPU è inferiore al** (per tutte le CPU). In questo modo verrà impostato il livello di utilizzo al di sotto del quale la CPU viene considerata inattiva.  
  
    -   Specificare un numero di secondi nella casella **per almeno** . In questo modo verrà impostato l'intervallo relativo all'utilizzo minimo trascorso il quale la CPU viene considerata inattiva.  
