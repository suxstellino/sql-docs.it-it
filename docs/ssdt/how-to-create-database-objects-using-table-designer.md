---
title: Creare oggetti di database tramite Progettazione tabelle
description: Informazioni su come creare un nuovo database in Esplora oggetti di SQL Server. Scoprire come creare nuove tabelle, vincoli e riferimenti di chiave esterna in Progettazione tabelle.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.scriptpanel
- sql.data.tools.design.table.context.view
- Microsoft.Data.Relational.Design.PW.RelationshipsDescriptor.OnDelete
ms.assetid: 9c9479c1-9bfc-4039-837e-e53fce67723d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a055bd1f8de41686e1c3477e3e4810e78d6b5025
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018311"
---
# <a name="how-to-create-database-objects-using-table-designer"></a>Procedura: Creare oggetti di database tramite Progettazione tabelle

Il nuovo nodo **SQL Server** in **Esplora oggetti di SQL Server** non solo è visivamente molto simile a SSMS, ma consente di creare nuovi oggetti tramite menu contestuali con un funzionamento è simile ai corrispondenti di SSMS.  
  
È ad esempio possibile creare un nuovo database nel nodo **Database**. Analogamente, è possibile selezionare un database specifico e creare o modificare definizioni di tabella e relativi oggetti di programmazione correlati immediatamente tramite la nuova Progettazione tabelle. Da Progettazione tabelle è possibile passare a un riquadro di script che consente di modificare direttamente lo script mediante il quale viene definita questa tabella.  
  
### <a name="to-create-a-new-database"></a>Per creare un nuovo database  
  
1.  Nel nodo **SQL Server** in **Esplora oggetti di SQL Server** espandere l'istanza del server connessa.  
  
2.  Fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiungi nuovo database**.  
  
3.  Rinominare il nuovo database in **Trade**.  
  
### <a name="to-create-new-tables-using-the-table-designer"></a>Per creare nuove tabelle utilizzando Progettazione tabelle  
  
1.  Espandere il nodo **Trade** appena creato. Fare clic con il pulsante destro del mouse sul nodo **Tabelle** e selezionare **Aggiungi nuova tabella**.  
  
2.  Progettazione tabelle verrà aperta in una nuova finestra. La finestra di progettazione è costituita dalla Griglia colonne, dal riquadro di script e dal riquadro Contesto. Nella Griglia colonne sono elencate tutte le colonne della tabella. Altri componenti della finestra di progettazione verranno presentati nelle procedure più avanti.  
  
3.  Nel riquadro di script rinominare la nuova tabella in `Suppliers`. In particolare, sostituire  
  
    ```  
    CREATE TABLE [dbo].[Table1]  
    ```  
  
    con  
  
    ```  
    CREATE TABLE [dbo].[Suppliers]  
    ```  
  
4.  Fare clic sulla riga vuota nella Griglia colonne per aggiungere una nuova colonna alla tabella.  Immettere **CompanyName** per il campo **Nome**, **nvarchar (128)** per **Tipo di dati** e deselezionare il campo **Consenti valori Null**. Quando si esce dai campi, si noti che il riquadro di script viene immediatamente aggiornato.  
  
5.  Aggiungere un'altra colonna. Immettere **Address** per il campo **Name**, **nvarchar (MAX)** per **Tipo di dati** e deselezionare il campo **Consenti valori Null**.  
  
    > [!WARNING]  
    > Se si modificano gli oggetti da un database connesso, non salvarli nell'unità locale. Per salvare correttamente le modifiche nel database, seguire i passaggi indicati nella prossima procedura [Procedura: Aggiornare un database connesso con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
6.  Ripetere i passaggi elencati in precedenza per creare un'altra tabella denominata **Customer**. Questa volta, aggiungere alla tabella Customer le colonne seguenti utilizzando la Griglia colonne. Inoltre, modificare lo script in modo che il nome della tabella sia `[dbo].[Customer]`.  
  
    |Nome|Tipo di dati|**Consenti valori NULL**|  
    |--------|-------------|-------------------|  
    |ID|INT|non selezionata|  
    |Nome|nvarchar (128)|non selezionata|  
  
7.  Creare un'ulteriore tabella denominata **Products**. Aggiungere alla tabella Products le colonne seguenti utilizzando la Griglia colonne. Inoltre, modificare lo script in modo che il nome della tabella sia `[dbo].[Products]`.  
  
    |Nome|Tipo di dati|**Consenti valori NULL**|  
    |--------|-------------|-------------------|  
    |ID|INT|non selezionata|  
    |Nome|nvarchar (128)|non selezionata|  
    |ShelfLife|INT|selezionata|  
    |SupplierId|INT|selezionata|  
    |CustomerId|INT|selezionata|  
  
### <a name="to-create-a-new-check-constraint-using-the-table-designer"></a>Per creare un nuovo vincolo CHECK utilizzando Progettazione tabelle  
  
1.  Il riquadro Contesto di Progettazione tabelle offre una vista logica della definizione di tabella (chiavi, vincoli, trigger e così via) e consente di selezionare un oggetto per evidenziare le relative relazioni alle singole colonne.  
  
    Per la tabella Products, fare clic con il pulsante destro del mouse sul nodo **Vincoli CHECK** nel riquadro Contesto di Progettazione tabelle e selezionare **Aggiungi nuovo vincolo CHECK**.  
  
2.  Si noti che il conteggio dei nodi viene incrementato automaticamente di 1.  
  
3.  Fare clic sul riquadro di script e sostituire la definizione predefinita del vincolo con quanto riportato di seguito.  
  
    ```  
    CONSTRAINT [CK_Products_ShelfLife] CHECK ([ShelfLife] <5),  
    ```  
  
    Con questo vincolo il valore di ShelfLife per una riga non potrà essere superiore a 5.  
  
### <a name="to-create-new-foreign-key-references-using-the-table-designer"></a>Per creare nuovi riferimenti di chiave esterna utilizzando Progettazione tabelle  
  
1.  Per la tabella Products, fare clic con il pulsante destro del mouse sul nodo **Chiavi esterne** nel riquadro Contesto e selezionare **Aggiungi nuova chiave esterna**.  
  
2.  Si noti che il conteggio dei nodi viene incrementato automaticamente di 1.  
  
3.  Fare clic sul riquadro di script e sostituire la definizione predefinita del riferimento di chiave esterna con quanto riportato di seguito.  
  
    ```  
    CONSTRAINT [FK_Products_SupplierId] FOREIGN KEY ([SupplierId]) REFERENCES [dbo].[Suppliers] ([Id]),  
    ```  
  
4.  Ripetere i passaggi elencati in precedenza per aggiungere un altro riferimento di chiave esterna alla tabella Products. Questa volta, sostituire la definizione predefinita con quanto riportato di seguito.  
  
    ```  
    CONSTRAINT [FK_Products_CustomerId] FOREIGN KEY ([CustomerId]) REFERENCES [dbo].[Customer] ([Id])  
    ```  
  
## <a name="see-also"></a>Vedere anche  
[Gestire tabelle e relazioni e correggere errori](../ssdt/manage-tables-relationships-and-fix-errors.md)  
  
