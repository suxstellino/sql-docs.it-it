---
title: Visualizzare e modificare dati in una tabella
description: Informazioni su come usare l'editor dei dati per visualizzare, modificare ed eliminare dati in una tabella esistente. Scoprire come visualizzare le modifiche nel modulo di script e salvarle in un file di script.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.QUERYRESULTS.F1
- sql.data.tools.queryresults.executequerydeletingrow
ms.assetid: bb67ce83-a87a-4e14-84cd-9a5930fe74c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: ebe23b534590bfb45c4a0bffce78d2139ef8912c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017991"
---
# <a name="how-to-view-and-edit-data-in-a-table"></a>Procedura: Visualizzare e modificare dati in una tabella

È possibile visualizzare, modificare ed eliminare dati di una tabella esistente utilizzando un editor dei dati visivo.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-edit-data-in-a-table-visually-using-the-data-editor"></a>Per modificare i dati in una tabella utilizzando visivamente l'Editor dati  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella **Products** in **Esplora oggetti di SQL Server** e selezionare **Visualizza dati**.  
  
2.  Verrà avviato l'Editor dati. Si notino le righe aggiunte alla tabella nelle procedure precedenti.  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella **Fruits** in Esplora oggetti di SQL Server e selezionare **Visualizza dati**.  
  
4.  Nell'Editor dati digitare **1** per **Id** e **True** per **Perishable**, quindi premere INVIO o il tasto TAB per spostare lo stato attivo dalla nuova riga affinché sia possibile eseguirne il commit nel database.  
  
5.  Ripetere il passaggio sopra indicato per immettere **2**, **False** e **3**, **False** nella tabella.  
  
    Si noti che quando si modifica una riga, è sempre possibile ripristinare le modifiche apportate a una cella premendo ESC.  
  
6.  È possibile visualizzare le modifiche come script facendo clic sul pulsante **Script** nella barra degli strumenti. In alternativa, è possibile usare il pulsante **Genera script nel file** per salvarle in un file di script con estensione sql da eseguire in un secondo momento.  
  
7.  Fare clic con il pulsante destro del mouse sul database **Trade** in **Esplora oggetti di SQL Server** e selezionare **Nuova query**. Nell'editor digitare `select * from dbo.PerishableFruits` e premere il pulsante **Esegui query** per restituire i dati rappresentati dalla vista `PerishableFruits`.  
  
