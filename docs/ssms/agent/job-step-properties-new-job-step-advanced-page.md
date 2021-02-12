---
title: Proprietà del nuovo passaggio di processo (pagina Avanzate)
description: Proprietà passaggio processo - Nuovo passaggio di processo (pagina Avanzate)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepadvanced.f1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2120c2f5d86c4d8ad4c549f95ba8090574506015
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978842"
---
# <a name="job-step-properties---new-job-step-advanced-page"></a>Proprietà passaggio processo - Nuovo passaggio di processo (pagina Avanzate)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di database SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Usare questa pagina per visualizzare e modificare le proprietà di un passaggio di processo di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="options"></a>Opzioni  
**Azione in caso di esito positivo**  
Consente di impostare l'azione che [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent deve eseguire in caso di esito positivo del passaggio di processo.  
  
**Tentativi**  
Consente di impostare il numero di tentativi eseguiti da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent per ripetere un passaggio di processo non riuscito.  
  
**Intervallo tra i tentativi (minuti)**  
Consente di impostare l'intervallo di attesa di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent tra l'esecuzione di un tentativo e l'altro.  
  
**Azione in caso di esito negativo**  
Consente di impostare l'azione che [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent deve eseguire in caso di esito negativo del passaggio di processo.  
  
## <a name="options-for-transact-sql-job-steps"></a>Opzioni per i passaggi del processo Transact-SQL  
**File di output**  
Consente di impostare il file da utilizzare per l'output del passaggio di processo. Questa opzione è disponibile solo per i membri del ruolo predefinito del server **sysadmin** .  
  
**...**  
Selezionare il file da utilizzare per l'output del passaggio di processo.  
  
**Visualizza**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], questo pulsante è disabilitato per la visualizzazione di file di output. Usare invece il blocco note per visualizzare i file di output dei passaggi di processo.  
  
**Accoda output a file esistente**  
Consente di accodare l'output al contenuto già esistente nel file. Diversamente, il precedente contenuto del file viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Registra nella tabella**  
Consente di registrare l'output del passaggio di processo nella tabella **nella sysjobstepslogs** del database **msdb** .  
  
**Visualizza**  
Dopo l'esecuzione del passaggio di processo almeno una volta, selezionare **Visualizza** per visualizzare l'output nella tabella.  
  
**Accoda output a voce esistente nella tabella**  
Consente di accodare l'output al contenuto già esistente nella tabella. Diversamente, il precedente contenuto della tabella viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Includi output passaggio nella cronologia**  
Selezionare questa opzione per includere l'output del passaggio di processo nella cronologia processo.  
  
**Esegui come utente**  
Se si è membri del ruolo predefinito del server **sysadmin** , è possibile selezionare un altro account di accesso SQL per eseguire questo passaggio di processo.  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>Opzioni per i passaggi del processo Sistema operativo (CmdExec)  
**File di output**  
Consente di impostare il file da utilizzare per l'output del passaggio di processo.  
  
**...**  
Selezionare il file da utilizzare per l'output del passaggio di processo.  
  
**Visualizza**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], questo pulsante è disabilitato per la visualizzazione di file di output. Usare invece il blocco note per visualizzare i file di output dei passaggi di processo.  
  
**Accoda output a file esistente**  
Accoda l'output del passaggio di processo al contenuto del file precedente ogni volta che viene eseguito.  
  
**Registra nella tabella**  
Consente di registrare l'output del passaggio di processo nella tabella **nella sysjobstepslogs** del database **msdb** .  
  
**Visualizza**  
Dopo l'esecuzione del passaggio di processo almeno una volta, selezionare **Visualizza** per visualizzare l'output nella tabella.  
  
**Accoda output a voce esistente nella tabella**  
Consente di accodare l'output al contenuto già esistente nella tabella. Diversamente, il precedente contenuto della tabella viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Includi output passaggio nella cronologia**  
Selezionare questa opzione per includere l'output del passaggio di processo nella cronologia processo.  
  
## <a name="options-for-powershell-job-steps"></a>Opzioni per i passaggi di processo di PowerShell  
**File di output**  
Consente di impostare il file da utilizzare per l'output del passaggio di processo.  
  
**...**  
Selezionare il file da utilizzare per l'output del passaggio di processo.  
  
**Visualizza**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], questo pulsante è disabilitato per la visualizzazione di file di output. Usare invece il blocco note per visualizzare i file di output dei passaggi di processo.  
  
**Accoda output a file esistente**  
Accoda l'output del passaggio di processo al contenuto del file precedente ogni volta che viene eseguito.  
  
**Registra nella tabella**  
Consente di registrare l'output del passaggio di processo nella tabella **nella sysjobstepslogs** del database **msdb** .  
  
**Visualizza**  
Dopo l'esecuzione del passaggio di processo almeno una volta, selezionare **Visualizza** per visualizzare l'output nella tabella.  
  
**Accoda output a voce esistente nella tabella**  
Consente di accodare l'output al contenuto già esistente nella tabella. Diversamente, il precedente contenuto della tabella viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Includi output passaggio nella cronologia**  
Selezionare questa opzione per includere l'output del passaggio di processo nella cronologia processo.  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>Opzioni per i passaggi del processo Lettura coda repliche  
**Server**  
Consente di impostare il server da utilizzare per un passaggio di processo Lettura coda repliche.  
  
**Database**  
Consente di impostare il database da utilizzare per un passaggio di processo Lettura coda repliche.  
  
## <a name="options-for-sql-server-analysis-services-job-steps"></a>Opzioni per i passaggi del processo SQL Server Analysis Services  
**File di output**  
Consente di impostare il file da utilizzare per l'output del passaggio di processo. Questa opzione è disponibile solo per i membri del ruolo predefinito del server **sysadmin** .  
  
**...**  
Selezionare il file da utilizzare per l'output del passaggio di processo.  
  
**Visualizza**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], questo pulsante è disabilitato per la visualizzazione di file di output. Usare invece il blocco note per visualizzare i file di output dei passaggi di processo.  
  
**Accoda output a file esistente**  
Consente di accodare l'output al contenuto già esistente nel file. Diversamente, il precedente contenuto del file viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Registra nella tabella**  
Consente di registrare l'output del passaggio di processo nella tabella **nella sysjobstepslogs** del database **msdb** .  
  
**Visualizza**  
Dopo l'esecuzione del passaggio di processo almeno una volta, selezionare **Visualizza** per visualizzare l'output nella tabella.  
  
**Accoda output a voce esistente nella tabella**  
Consente di accodare l'output al contenuto già esistente nella tabella. Diversamente, il precedente contenuto della tabella viene sovrascritto ogni volta che il passaggio di processo viene eseguito.  
  
**Includi output passaggio nella cronologia**  
Selezionare questa opzione per includere l'output del passaggio di processo nella cronologia processo.  
  
## <a name="see-also"></a>Vedere anche

- [Gestire passaggi di processo](manage-job-steps.md)
