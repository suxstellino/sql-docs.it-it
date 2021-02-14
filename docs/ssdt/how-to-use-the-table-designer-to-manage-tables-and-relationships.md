---
title: Uso di Progettazione tabelle per gestire tabelle e relazioni
description: Acquisire familiarità con Progettazione tabelle. Informazioni su come usare questo strumento per creare e modificare la struttura delle tabelle di database e per visualizzare le relazioni tra gli oggetti di database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.columnspecification.index.dialog
- sql.data.tools.design.table.columnsgrid.view
- sql.data.tools.design.table.scriptpanel
ms.assetid: 322a2903-d7a6-4f52-9048-1bd413b4c799
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a71009351c187bdda50c00b4f3b00bfee0fd2acf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018001"
---
# <a name="how-to-use-the-table-designer-to-manage-tables-and-relationships"></a>Procedura: Usare Progettazione tabelle per gestire tabelle e relazioni

Progettazione tabelle offre uno strumento visivo insieme all'Editor Transact\-SQL per la creazione e la modifica della struttura della tabella, inclusi oggetti di programmazione specifici della tabella, per i database di SQL Server.  Viene avviata quando si crea una nuova tabella per un progetto o un database connesso o quando si fa doppio clic per modificare una tabella in Esplora oggetti di SQL Server o Esplora soluzioni.  
  
La finestra di progettazione è costituita dalla Griglia colonne, dal riquadro di script e dal riquadro Contesto. Nella Griglia colonne sono elencate tutte le colonne della tabella. È possibile aggiungere, modificare ed eliminare colonne in questa griglia.  Il riquadro Contesto offre una vista logica della definizione di tabella (chiavi, indici, vincoli, trigger e così via) e consente di selezionare un oggetto per evidenziare le relative relazioni alle singole colonne. È anche possibile aggiungere nuovi oggetti alla tabella in questo riquadro nonché modificare le proprietà di un oggetto selezionato nella griglia delle proprietà. Nel riquadro di script viene mostrata la definizione della struttura della tabella e viene evidenziato lo script dell'oggetto selezionato nel riquadro Contesto o nella Griglia colonne. È possibile modificare lo script insieme alla Griglia colonne e al riquadro Contesto durante la visualizzazione. Tutte le modifiche di uno dei tre riquadri verranno propagate immediatamente negli altri due.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-create-a-new-table"></a>Per creare una nuova tabella  
  
1.  Aprire il progetto TradeDev in uso nelle procedure precedenti.  
  
2.  In **Esplora soluzioni** espandere la cartella **dbo**, fare clic con il pulsante destro del mouse sulla cartella **Tabelle** e selezionare **Aggiungi**, quindi **Tabella**.  
  
3.  Denominare la nuova tabella **Shipper** e scegliere **Aggiungi**.  
  
4.  Verrà aperta Progettazione tabelle. Nella Griglia colonne aggiungere una nuova colonna alla tabella denominata **ShipperName** e Tipo di dati **int**.  
  
5.  Si noti che è inoltre possibile modificare le proprietà di colonne nella finestra **Proprietà**. Fare clic sulla colonna **ShipperName** e, nella finestra **Proprietà**, impostare **DataType** di questa colonna su **nvarchar** e **length** su **128**. Si noti che quando si sposta lo stato attivo fuori dal campo, il riquadro di script e la Griglia colonne della finestra di progettazione vengono aggiornati automaticamente per riflettere la modifica.  
  
### <a name="to-create-a-new-foreign-key-constraint"></a>Per creare un nuovo vincolo di chiave esterna  
  
1.  Fare clic con il pulsante destro del mouse sul nodo **Chiavi esterne** nel riquadro Contesto della finestra di progettazione e selezionare **Aggiungi nuova chiave esterna**.  
  
2.  Si noti che il conteggio dei nodi viene incrementato automaticamente di 1. Premere INVIO per accettare il nome predefinito del vincolo.  
  
3.  Sostituire la definizione predefinita del vincolo nel riquadro di script con quanto riportato di seguito.  
  
    ```  
    CONSTRAINT [FK_Shipper_Products] FOREIGN KEY ([Id]) REFERENCES [dbo].[Products]([Id])  
    ```  
  
    Si noti che la creazione e la modifica di entità di database per un progetto offline è identica all'esecuzione di attività con un database connesso.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Creare oggetti di database tramite Progettazione tabelle](../ssdt/how-to-create-database-objects-using-table-designer.md)  
  
