---
description: Strumenti di progettazione di Visual Database Tools
title: Strumenti di progettazione di Visual Database Tools
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- data sources [SQL Server]
- View Designer
- Visual Database Tools [SQL Server]
- Database Diagram Designer
- Query Designer [SQL Server]
- design tools [Visual Database Tools]
- Table Designer
- Visual Database Tools [SQL Server], designers
- Properties window [Visual Database Tools]
ms.assetid: bd0ca68e-6f69-42dd-bcb5-ce511673769c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 129de2adfa1b3b033551d3b41db4eac8f34891fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338769"
---
# <a name="visual-database-tool-designers"></a>Strumenti di progettazione di Visual Database Tools
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Visual Database Tools è una combinazione di strumenti di progettazione che consentono di lavorare con un'origine dati creando query, progettando o modificando una struttura di database o aggiornando dati. Tali strumenti sono Progettazione diagrammi di database, Progettazione tabelle e Progettazione query e Progettazione viste.  
  
## <a name="properties-window"></a>Finestra Proprietà  
La finestra delle proprietà non è specifica di Visual Database Tools, ma è l'area in cui è possibile eseguire gran parte delle modifiche. Visualizza le proprietà dell'elemento attualmente selezionato, ad esempio una tabella, e consente di eseguire numerose operazioni, dalla modifica del nome delle proprietà al confronto di una colonna. Alcune proprietà sono visibili nella finestra delle proprietà, ma possono essere modificate solo tramite un altro strumento.  
  
## <a name="database-diagram-designer"></a>Progettazione diagrammi di database  
Progettazione diagrammi di database è costituito da una finestra in cui è possibile creare, modificare e rappresentare graficamente le tabelle e le relazioni di un database.  
  
Per visualizzare Progettazione diagrammi di database, aprire un diagramma esistente o fare clic con il pulsante destro del mouse sul nodo del database in Esplora oggetti e scegliere **Nuovo diagramma database** dal menu a discesa.  
  
Dopo avere aperto la finestra di progettazione, nel menu principale risulterà disponibile il menu **Diagramma di database** . Questo menu è il punto di accesso alle caratteristiche speciali dello strumento di progettazione.  
  
> [!NOTE]  
> Questo strumento di progettazione interagisce con i database di Microsoft SQL Server.  
>   
> Questa versione di Visual Database Tools non supporta Microsoft SQL Server 7 e versioni precedenti.  
  
## <a name="table-designer"></a>Progettazione tabelle  
Progettazione tabelle è uno strumento grafico che consente di progettare e rappresentare graficamente una singola tabella di un database di Microsoft SQL Server a cui si è connessi.  
  
Progettazione tabelle è strutturato in due riquadri. L'area superiore contiene una griglia in cui ciascuna riga descrive una colonna del database. Per ciascuna colonna del database, la griglia indica gli attributi fondamentali: nome della colonna, tipo di dati e impostazione relativa all'accettazione di valori Null.  
  
Nell'area inferiore di Progettazione tabelle, ovvero la scheda Proprietà colonne, vengono visualizzati attributi aggiuntivi relativi alla colonna eventualmente evidenziata nell'area superiore.  
  
Da Progettazione tabelle è inoltre possibile fare clic con il pulsante destro del mouse nella sezione della griglia per accedere a finestre di dialogo che consentono di creare e modificare relazioni, vincoli, indici e chiavi per la tabella.  
  
Per visualizzare Progettazione tabelle, aprire una tabella esistente o fare clic con il pulsante destro del mouse sul nodo delle **Tabelle** in Esplora oggetti e scegliere **Aggiungi nuova tabella** dal menu a discesa.  
  
Dopo avere aperto la finestra di progettazione, nel menu principale risulterà disponibile il menu Progettazione tabelle. Questo menu è il punto di accesso alle caratteristiche speciali dello strumento di progettazione.  
  
> [!NOTE]  
> Questo strumento di progettazione interagisce con i database di Microsoft SQL Server.  
>   
> Questa versione di Visual Database Tools non supporta Microsoft SQL Server 7 e versioni precedenti.  
  
## <a name="query-and-view-designer"></a>Progettazione query e Progettazione viste  
Progettazione query e Progettazione viste in realtà è costituito da due strumenti che funzionano in modo molto simile. Di seguito sono riportate alcune delle differenze principali:  
  
-   Le viste vengono salvate con il database, mentre una query viene salvata con un progetto di database di Visual Studio.  
  
-   Progettazione query interagisce praticamente con qualsiasi origine dati, mentre Progettazione viste interagisce unicamente con SQL Server.  
  
-   Progettazione query consente di progettare istruzioni DML SELECT, INSERT, UPDATE e DELETE, mentre le viste possono contenere solo istruzioni SELECT.  
  
### <a name="view-designer"></a>Progettazione viste  
Progettazione viste consente di progettare e rappresentare graficamente una vista esistente o di crearne una nuova in un database di Microsoft SQL Server a cui si è connessi.  
  
In Progettazione viste sono presenti quattro riquadri: Diagramma, Criteri, SQL e Risultati. Per informazioni dettagliate su ognuno di questi riquadri, vedere [Strumenti di progettazione di query e viste &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md).  
  
Per visualizzare Progettazione viste, aprire una vista esistente o fare clic con il pulsante destro del mouse sul nodo delle **Viste** in Esplora oggetti e scegliere **Aggiungi nuova vista** dal menu a discesa.  
  
Dopo avere aperto la finestra di progettazione, nel menu principale risulterà disponibile il menu **Progettazione query** . Questo menu è il punto di accesso alle caratteristiche speciali dello strumento di progettazione.  
  
> [!NOTE]  
> Questo strumento di progettazione interagisce con i database di Microsoft SQL Server.  
>   
> Questa versione di Visual Database Tools non supporta Microsoft SQL Server 7 e versioni precedenti.  
  
## <a name="see-also"></a>Vedere anche  
[Progettare diagrammi di database &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-database-diagrams-visual-database-tools.md)  
[Progettazione di tabelle &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)  
[Procedure per la progettazione di query e viste &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
