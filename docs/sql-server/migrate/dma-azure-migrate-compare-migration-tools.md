---
title: Confrontare strumenti di migrazione dati SQL
titleSuffix: SQL Server
description: 'Confrontare strumenti di migrazione dati SQL per determinare lo strumento più adatto alle proprie esigenze aziendali, ad esempio Data Migration Assistant (DMA), Azure Migrate, servizio migrazione del database di Azure, SQL Server Migration Assistant (SSMA), Database Experimentation Assistant (DEA). '
ms.date: 03/15/2021
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajpo
ms.author: rajpo
ms.openlocfilehash: fc406a39dc0b5d1a80e4f357a28b6945da0bf453
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603794"
---
# <a name="compare-sql-data-migration-tools"></a>Confrontare strumenti di migrazione dati SQL

Microsoft offre una suite di strumenti e servizi progettati per aiutare gli utenti a eseguire la migrazione di vari tipi di database di origine a un'ampia gamma di ambienti di destinazione. 

Questo articolo fornisce una breve panoramica degli strumenti disponibili per la migrazione a SQL Server e Azure SQL. 

## <a name="azure-migrate"></a>Azure Migrate

Azure Migrate offre un hub centralizzato per individuare e valutare i server, l'infrastruttura, le applicazioni e i dati locali in Azure su larga scala.  Azure Migrate offrire una migrazione unificata tra server, database e applicazioni. 

Usare Azure Migrate per individuare tutti i server SQL nell'data center, valutare le dipendenze dell'applicazione, comprendere la conformità di questi server SQL che eseguono la migrazione ad Azure SQL, ottenere raccomandazioni Microsoft, l'opzione di distribuzione SQL di Azure ottimale e lo SKU corretto che possono soddisfare le esigenze di prestazioni per i carichi di lavoro.  È anche possibile ottenere le stime mensili che eseguono questi database in Azure SQL per soddisfare i vantaggi offerti dalle licenze. 

Usare Azure Migrate negli scenari seguenti: 
- Valutare e individuare i dati SQL Server. 
- Ottenere le raccomandazioni per la distribuzione di Azure SQL, le dimensioni di destinazione e le stime mensili.
- È possibile sollevare e spostare l'intero patrimonio di dati per SQL Server nelle macchine virtuali di Azure. 

Per ulteriori informazioni, vedere la [documentazione Azure migrate](/azure/migrate/). 

## <a name="data-migration-assistant-dma"></a>Data Migration Assistant (DMA)

Il Data Migration Assistant (DMA) consente di eseguire l'aggiornamento a una piattaforma dati moderna individuando i problemi di compatibilità che possono influito sulle funzionalità del database nella nuova versione di SQL Server o nel database SQL di Azure. DMA consiglia miglioramenti per le prestazioni e l'affidabilità per l'ambiente di destinazione e consente di spostare lo schema, i dati e gli oggetti non contenuti dal server di origine al server di destinazione.

Usare DMA negli scenari seguenti: 
- Aggiornamento di SQL Server 2005 e versioni successive a SQL Server 2012, SQL Server 2014, SQL Server 2016, SQL Server 2017 e SQL Server 2019 in Windows e Linux e SQL Server in una macchina virtuale di Azure. 
- Rilevare i problemi di compatibilità che possono influito sulle funzionalità del database in una versione di destinazione più recente di SQL Server o Azure SQL e fornire misure di attenuazione. 
- Spostare lo schema, i dati e gli oggetti non contenuti da un server di origine a SQL Server o Azure SQL. 

Per ulteriori informazioni, vedere la [documentazione Data Migration Assistant](../../dma/dma-overview.md). 

## <a name="sql-server-migration-assistant-ssma"></a>SQL Server Migration Assistant (SSMA)

SQL Server Migration Assistant (SSMA) è uno strumento progettato per automatizzare la migrazione del database per SQL Server e SQL di Azure da motori di database alternativi. 

Usare SSMA nello scenario seguente:
- Eseguire la migrazione dei database di Microsoft Access, DB2, MySQL, Oracle e SAP ASE a SQL Server.
- Eseguire la migrazione dei database di Microsoft Access, DB2, MySQL, Oracle e SAP ASE in Azure SQL.

Per ulteriori informazioni, vedere la [documentazione di SQL Server Migration Assistant](../../ssma/sql-server-migration-assistant.md)

## <a name="azure-database-migration-service-dms"></a>Servizio Migrazione del database di Azure

Servizio Migrazione del database di Azure consente migrazioni senza problemi da più origini di database alle piattaforme dati di Azure con tempi di inattività minimi.  Il servizio migrazione del database fornisce una pipeline di migrazione resiliente e affidabile che richiede un coinvolgimento minimo dell'utente durante il processo di migrazione globale. 

Utilizzare il servizio migrazione del database negli scenari seguenti:
- Esegui la migrazione di entrambi i database in Azure SQL, soprattutto su larga scala e per le migrazioni di grandi dimensioni (in termini di numero e dimensione dei database). 
- Eseguire la migrazione dei database al database di Azure.

Per altre informazioni, vedere la [documentazione del servizio migrazione del database di Azure](/azure/dms/). 

## <a name="database-experimentation-assistant-dea"></a>Database Experimentation Assistant

Database Experimentation Assistant (DEA) è una soluzione di sperimentazione per gli aggiornamenti di SQL Server. DEA può aiutarti a valutare una versione di destinazione di SQL Server per un carico di lavoro specifico. I clienti che eseguono l'aggiornamento da versioni precedenti di SQL Server (a partire da 2005) a versioni più recenti di SQL Server possono usare la metrica di analisi fornita dallo strumento.

Usare il Database Experimentation Assistant nello scenario seguente:
- Acquisizione del carico di lavoro di un ambiente di SQL Server di origine e valutazione del carico di lavoro in una SQL Server di origine per preparare la migrazione. 
- Identificare gli errori di compatibilità e le possibili query degradate per lo scenario di migrazione SQL Server. 

Per ulteriori informazioni, vedere la [documentazione database Experimentation Assistant](../../dea/database-experimentation-assistant-overview.md).


## <a name="quick-comparison"></a>Confronto rapido

Usare il grafico seguente per confrontare le funzionalità degli strumenti di migrazione SQL:


| Funzionalità |Azure Migrate  |Data Migration Assistant (DMA)  |SQL Server Migration Assistant (SSMA)  | Servizio Migrazione del database di Azure | Database Experimentation Assistant|
|---------|---------|---------|---------|---|---|
|Individuare e valutare le proprietà di dati SQL| Su larga scala | Sì |No |No|No|
|Eseguire la migrazione di oggetti SQL Server al database SQL o a SQL Istanza gestita| No| Sì | No  | Sì|No|
|SQL Server Lift-and-Shift per SQL Server nella macchina virtuale di Azure | Sì | No | No | No| No |
|Eseguire la migrazione (e/o l'aggiornamento) SQL Server SQL Server nella macchina virtuale di Azure | No | Sì| No | No | No| 
|Eseguire la migrazione di oggetti non SQL </br> (Oracle, Access, DB2 e così via) | No |No|Sì|No|No|
|Eseguire la migrazione di database open source </br> (MySQL, PostgreSQL, MariaDB e così via)| No | No | No | Sì | No|
|Confrontare i carichi di lavoro tra SQL Server di origine e di destinazione |No|No|No|No|Sì|
|||||||

## <a name="next-steps"></a>Passaggi successivi

Introduzione alla migrazione a [SQL Server](../../ssma/sql-server-migration-assistant.md) da un altro motore di database, alla migrazione a [SQL di Azure](/azure/azure-sql/migration-guides/)o alla valutazione dei dati SQL con [Azure migrate](/azure/migrate/how-to-create-azure-sql-assessment). 

