---
title: Aggiornare il motore di database | Microsoft Docs
description: Questo articolo mette a disposizione i collegamenti a risorse utili per l'aggiornamento del motore di database di SQL Server da una versione precedente di SQL Server a SQL Server 2019.
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433859
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c855e1a72e1cbd0f6012e4af19701fbaf6c71e68
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765526"
---
# <a name="upgrade-database-engine"></a>Aggiornare il motore di database

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
  Gli articoli di questa sezione descrivono come eseguire l'aggiornamento del motore di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
1.  [Scegliere un metodo di aggiornamento del motore di database](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md). Prima di iniziare a eseguire un aggiornamento, è necessario comprendere i vari metodi di aggiornamento. Questo articolo descrive i metodi di aggiornamento e i passaggi previsti per ognuno.  
  
2.  [Pianificare e testare il piano di aggiornamento del motore di database](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md). Dopo aver esaminato i metodi di aggiornamento, si è pronti per sviluppare il metodo di aggiornamento appropriato per l'ambiente e per testarlo prima di aggiornare l'ambiente esistente. Questo articolo illustra lo sviluppo di un piano di aggiornamento e il relativo test.  
  
3.  [Completare l'aggiornamento del motore di database](../../database-engine/install-windows/complete-the-database-engine-upgrade.md). Dopo che il motore di database è stato aggiornato e i database sono online, è necessario eseguire passaggi aggiuntivi, tra cui l'esecuzione di un nuovo backup, l'aggiornamento della funzionalità database per abilitare nuove funzionalità e il ripopolamento di cataloghi full-text. Questo articolo tratta tali passaggi.  
  
4.  Aggiornare il [livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) (**si applica a:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]). Uno dei passaggi da eseguire quando i database sono online nella nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] può essere l'aggiornamento della modalità di funzionamento dei database per abilitare le nuove funzionalità modificando il livello di compatibilità del database. Questa operazione può essere eseguita manualmente o tramite Assistente ottimizzazione query. 

    - [Modificare la modalità di compatibilità del database e usare Query Store](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Dopo aver modificato manualmente il livello di compatibilità del database, usare Query Store per monitorare le prestazioni e identificare le possibili regressioni. Questo articolo descrive il processo consigliato e propone un flusso di lavoro consigliato.  

    - [Modificare la modalità di compatibilità del database con Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md). In alternativa alla modifica manuale, usare **Assistente ottimizzazione query** per eseguire in modo guidato il processo consigliato di modifica del livello di compatibilità del database. Questo articolo illustra questo processo e fornisce istruzioni sul flusso di lavoro di Assistente ottimizzazione query.  

    Per altre informazioni sulle nuove funzionalità e sui comportamenti migliorati disponibili dopo la modifica del livello di compatibilità del database, vedere [Differenze tra i livelli di compatibilità](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-stored-procedures).

5.  [Scoprire le nuove funzionalità di SQL Server](https://www.microsoft.com/sql-server/sql-server-2019). Infine, dopo aver completato i passaggi precedenti, si è pronti per sfruttare i vantaggi offerti dai nuovi miglioramenti specifici del motore di database. Questo articolo suggerisce alcuni di questi miglioramenti e fornisce collegamenti ad altre informazioni.  
  
  
