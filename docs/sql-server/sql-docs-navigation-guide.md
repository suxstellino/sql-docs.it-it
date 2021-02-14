---
title: Suggerimenti per l'esplorazione della documentazione di SQL Server
description: Suggerimenti e consigli per esplorare la documentazione tecnica di SQL Server. Vengono illustrati elementi come la pagina hub, il sommario e l'intestazione, nonché come usare i percorsi di navigazione e il filtro della versione.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: fc749f636a4d1b7b2e737e54d9ecd9418321c548
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082555"
---
# <a name="sql-server-docs-navigation-guide"></a>Guida all'esplorazione della documentazione di SQL Server

In questo argomento vengono forniti alcuni suggerimenti e consigli per esplorare lo spazio della documentazione tecnica di SQL Server.  

## <a name="hub-page"></a>Pagina hub

La pagina hub di SQL Server è disponibile all'indirizzo [https://aka.ms/sqldocs](../index.yml?WT.mc_id=akams) ed è il punto di ingresso per trovare il contenuto SQL Server di interesse.

È sempre possibile tornare a questa pagina selezionando **SQL Docs** dall'intestazione nella parte superiore di ogni pagina all'interno del set di documentazione tecnica di SQL Server: 

![SQL Docs nell'intestazione](media/sql-server-docs-navigation-guide/sql-docs-in-header.png)

## <a name="offline-documentation"></a>Documentazione offline

Per visualizzare la documentazione di SQL Server su un sistema offline sono disponibili due opzioni. È possibile creare un file PDF da qualsiasi punto della documentazione tecnica di SQL Server, oppure si può scaricare il contenuto offline mediante [Guida offline di SQL Server e Help Viewer](./sql-server-offline-documentation.md). 

Se si vuole creare un file PDF, fare clic sul collegamento **Scarica PDF** disponibile nella parte inferiore di ogni sommario.


![Scarica PDF](media/sql-server-docs-navigation-guide/download-pdf.png)

## <a name="toc-symbols"></a>Simboli del sommario 

Le voci del sommario che terminano con `>` indicano che l'utente verrà indirizzato a contenuto della documentazione tecnica con un sommario diverso. 

![Parentesi angolari singole nel sommario](media/sql-server-docs-navigation-guide/single-carrots-in-sql-docs-toc.png)

Le voci del sommario che terminano con `>>` indicano che l'utente verrà indirizzato a una pagina all'esterno di docs.microsoft.com. 

![Marcatori di spostamento nel sommario](media/sql-server-docs-navigation-guide/double-carrots-in-sql-docs-toc.png)

Se si passa a una di queste pagine, è possibile tornare alla pagina principale del contenuto tecnico di SQL Server e al sommario selezionando la voce "Benvenuto in SQL Server >" disponibile nella parte superiore di ogni sommario. 

![Tornare al sommario SQL](media/sql-server-docs-navigation-guide/navigate-back-to-sql-toc.png)

## <a name="toc-search"></a>Ricerca nel sommario 
In docs.microsoft.com è possibile cercare contenuto nel sommario usando la casella di ricerca di filtro nella parte superiore: 

![Usare la casella di filtro](media/sql-server-docs-navigation-guide/sql-docs-toc-filter.gif)

## <a name="version-filter"></a>Filtro versione
La documentazione tecnica di SQL Server include contenuto per diverse versioni ed edizioni supportate di SQL Server. Le funzionalità possono variare nelle diverse versioni di SQL Server e, di conseguenza, anche il contenuto può a volte variare. 

È possibile usare il [filtro della versione](versioning-system-monikers-ui-sql-server.md) per assicurarsi di visualizzare il contenuto per la versione appropriata di SQL Server: 

![Filtro della versione in SQL Docs](media/sql-server-docs-navigation-guide/sql-docs-version-filter.gif)

Se si seleziona **All SQL** \> **Hide nothing** (Tutto SQL > Visualizza tutto), tutto il contenuto è visibile e non viene nascosto niente con il filtro. L'opzione **Hide nothing** (Visualizza tutto) può visualizzare contenuto pertinente a diverse versioni di SQL Server nello stesso articolo, che può risultare contraddittorio, poco chiaro o fuorviante. L'opzione [**Hide nothing** (Visualizza tutto) non è pertanto consigliata per un uso di routine](versioning-system-monikers-ui-sql-server.md#anchor-allsql-hidenothing). 

## <a name="breadcrumbs"></a>Percorsi di navigazione

È possibile trovare i percorsi di navigazione sotto l'intestazione e sopra il sommario e tali percorsi indicano la posizione dell'articolo corrente nel sommario.  Queste informazioni non solo consentono di verificare il contesto del tipo di contenuto che si sta leggendo, ma anche di ripercorrere all'indietro l'albero del sommario:

![Percorsi di navigazione di SQL Docs](media/sql-server-docs-navigation-guide/sql-docs-bread-crumbs.gif)

## <a name="article-section-navigation"></a>Spostamenti tra le sezioni dell'articolo

Il riquadro di spostamento a destra consente di spostarsi rapidamente tra le sezioni all'interno di un articolo, nonché di identificare la propria posizione all'interno dell'articolo.  

![Riquadro di spostamento a destra](media/sql-server-docs-navigation-guide/sql-docs-right-hand-navigation.gif)


## <a name="submit-docs-feedback"></a>Inviare commenti e suggerimenti per la documentazione

Se si riscontra un errore all'interno di un articolo, è possibile inviare commenti e suggerimenti al team del contenuto di SQL per tale articolo scorrendo verso il basso nella parte inferiore della pagina e selezionando **Invia feedback su questa pagina**.

![Commenti e suggerimenti sul contenuto con problema Git](media/sql-server-get-help/git-issues.png)

È anche possibile inviare commenti e suggerimenti sulla documentazione in generale all'indirizzo [https://aka.ms/sqldocsfeedback](https://aka.ms/sqldocsfeedback). 

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![Modificare la documentazione SQL](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="next-steps"></a>Passaggi successivi

- Iniziare a usare la [documentazione tecnica di SQL Server](index.yml).
- Per altre informazioni sull'invio di commenti o suggerimenti o su come ottenere assistenza per SQL Server, vedere la pagina [Guida e commenti di SQL Server](sql-server-get-help.md). 
- Per accedere rapidamente a tutti gli articoli di avvio rapido e alle esercitazioni, vedere [Risorse SQL per la formazione](../sql-server/educational-sql-resources.yml).