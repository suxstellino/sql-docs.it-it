---
title: Panoramica della migrazione di SQL Server Integration Services ad Azure | Microsoft Docs
description: Questo articolo illustra i processi e gli strumenti per eseguire la migrazione di SQL Server Integration Services ad Azure.
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f443bc4209365253cf6e4e4005d19afe9ee11268
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061466"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Eseguire la migrazione di carichi di lavoro SSIS locali a SSIS in ADF

Questo articolo elenca i processi e gli strumenti utili per eseguire la migrazione di carichi di lavoro SQL Server Integration Services (SSIS) a SSIS in ADF.

In [Panoramica della migrazione](/azure/data-factory/scenario-ssis-migration-overview) viene presentato il processo di migrazione globale dei carichi di lavoro ETL da SSIS locale a SSIS in ADF.

Il processo di migrazione è costituito da due fasi: [Valutazione](/azure/data-factory/scenario-ssis-migration-overview#assessment) e [Migrazione](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="assessment"></a>Valutazione

Data Migration Assistant (DMA) è uno strumento scaricabile gratuitamente per questo scopo, che può essere installato ed eseguito in locale. È possibile creare un progetto di valutazione DMA di tipo Integration Services per valutare i pacchetti SSIS in batch e identificare i problemi di compatibilità.

Ottenere [Database Migration Assistant](../../dma/dma-overview.md) ed [eseguire la valutazione dei pacchetti](../../dma/dma-assess-ssis.md).

## <a name="migration"></a>Migrazione

A seconda dei tipi di archiviazione dei pacchetti SSIS di origine e della destinazione di migrazione dei carichi di lavoro del database, i passaggi per eseguire la migrazione di pacchetti SSIS e processi di SQL Server Agent che pianificano le esecuzioni di pacchetti SSIS possono variare. Per altre informazioni, vedere [questa pagina](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="next-steps"></a>Passaggi successivi

- [Eseguire la migrazione di pacchetti SSIS a Istanza gestita di SQL di Azure](/azure/dms/how-to-migrate-ssis-packages-managed-instance).
- [Eseguire la migrazione di processi SSIS ad Azure Data Factory (ADF) con SQL Server Management Studio (SSMS)](/azure/data-factory/how-to-migrate-ssis-job-ssms).