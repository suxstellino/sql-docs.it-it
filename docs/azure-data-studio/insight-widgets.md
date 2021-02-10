---
title: Usare i widget di informazioni dettagliate per monitorare i server e i database
description: Informazioni su come usare i widget di informazioni dettagliate di Azure Data Studio per trasformare le query che monitorano server e database in visualizzazioni dettagliate.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19, seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: 2b8f54b66852df0ce0300e950ba712a803a7a012
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040171"
---
# <a name="manage-servers-and-databases-with-insight-widgets-in-azure-data-studio"></a>Gestire server e database con i widget di informazioni dettagliate in Azure Data Studio

I widget di informazioni dettagliate acquisiscono le query Transact-SQL (T-SQL) usate per monitorare server e database e le trasformano in visualizzazioni dettagliate.

Le informazioni dettagliate sono rappresentate in grafici personalizzabili che vengono aggiunti ai dashboard di monitoraggio di server e database. Dopo aver visualizzato le informazioni dettagliate immediate su server e database, è possibile analizzarle più nel dettaglio e avviare le azioni di gestione prestabilite.

È possibile creare eccellenti dashboard di gestione di server e database, come quello visualizzato nell'esempio seguente:

![Dashboard del database](media/insight-widgets/database-dashboard.png)

Per iniziare a creare diversi tipi di widget di informazioni dettagliate, seguire le esercitazioni seguenti:

- [Creare un widget di informazioni dettagliate personalizzato](tutorial-build-custom-insight-sql-server.md)
- *Abilitare i widget di informazioni dettagliate predefiniti*
  - [Abilitare le informazioni dettagliate sul monitoraggio delle prestazioni](tutorial-qds-sql-server.md)
  - [Abilitare il widget sullo spazio usato dalle tabelle](tutorial-table-space-sql-server.md)

## <a name="sql-queries"></a>Query SQL

Azure Data Studio tenta di evitare la necessità di introdurre un'altra lingua o un'interfaccia utente complessa e prova quindi a usare quanto più possibile T-SQL con una configurazione JSON minima. La configurazione di widget di informazioni dettagliate con T-SQL sfrutta l'ingente numero di origini esistenti di utili query T-SQL che possono essere convertite in widget dettagliati.

I widget di informazioni dettagliate sono costituiti da una o due query T-SQL:
* *La query del widget di informazioni dettagliate* è obbligatoria ed è la query che restituisce i dati visualizzati nel widget.
* *La query dei dettagli di informazioni dettagliate* è necessaria solo se si sta creando una pagina di dettagli di informazioni dettagliate.

Una query del widget di informazioni dettagliate definisce un set di dati che restituisce un conteggio o un grafico. Le query dei dettagli di informazioni dettagliate vengono usate invece per elencare le informazioni dettagliate rilevanti in un formato tabulare nel pannello dei dettagli di informazioni dettagliate. 

Azure Data Studio esegue le query del widget di informazioni dettagliate, esegue il mapping del set di risultati di ogni query al set di dati di un grafico e ne esegue il rendering. Quando un utente apre i dettagli delle informazioni dettagliate, viene eseguita la query dei dettagli e il risultato viene visualizzato in una griglia all'interno della finestra di dialogo.

L'idea di base è quella di scrivere una query T-SQL in modo che possa essere usata come set di dati di un widget di conteggio o di grafico. 

## <a name="summary"></a>Summary

La query T-SQL e il set di risultati determinano il comportamento del widget di informazioni dettagliate. Per creare un widget di informazioni dettagliate efficace è fondamentale saper scrivere una query per un tipo di grafico o eseguire il mapping del tipo di grafico appropriato per una query esistente.



## <a name="additional-resources"></a>Risorse aggiuntive
- [Editor query](tutorial-sql-editor.md)

