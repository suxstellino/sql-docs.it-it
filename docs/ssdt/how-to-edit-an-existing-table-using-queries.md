---
title: Modificare una tabella esistente tramite query
description: Informazioni su come usare una query Transact-SQL per modificare la definizione o i dati di una tabella. Visualizzare esempi di modifica di una definizione di tabella e di inserimento di righe in una tabella.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 58f4de8e-97b4-4bcb-953f-f3d428432491
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bd6ea94af644c981d368608a0fe864dbdf237181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018221"
---
# <a name="how-to-edit-an-existing-table-using-queries"></a>Procedura: Modificare una tabella esistente tramite query

È possibile modificare la definizione di una tabella o dei relativi dati scrivendo una query Transact\-SQL. Per visualizzare o immettere dati in una tabella in modo visivo, usare l'editor dei dati come descritto in [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-edit-the-definition-of-an-existing-table"></a>Per modificare la definizione di una tabella esistente  
  
1.  Espandere il nodo **Tabelle** del database **Trade** in **Esplora oggetti di SQL Server** e fare clic con il pulsante destro del mouse su **dbo.Suppliers**.  
  
2.  Selezionare **Progettazione viste** per visualizzare lo schema della tabella in Progettazione tabelle.  
  
3.  Selezionare la casella **Consenti valori Null** per la colonna **Address**. Si noti che il codice corrispondente nel riquadro di script viene impostato immediatamente su `NULL`.  
  
4.  Aggiornare il database seguendo i passaggi nell'argomento [Procedura: Aggiornare un database connesso con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
### <a name="to-populate-data-in-new-tables-using-a-transact-sql-query"></a>Per popolare i dati in nuove tabelle tramite una query Transact\-SQL  
  
1.  Fare clic con il pulsante destro del mouse sul nodo del database **Trade** e selezionare **Nuova query**.  
  
2.  Nel riquadro di script incollare il codice riportato di seguito.  
  
    ```  
    insert into dbo.Suppliers values  
    (1, 'NorthWind Traders', 'Seattle, WA'),  
    (2, 'Contoso', 'Tacoma, WA')  
    GO  
  
    insert dbo.Customer values  
    (1, 'Fourth Coffee')  
    GO  
  
    insert dbo.Products values  
    (1, 'Apples', 0, 1, 1),  
    (2, 'Instant Coffee', 1, 2, 1)  
    GO  
    ```  
  
3.  Fare clic sul pulsante **Esegui query** per eseguire questa query. Di seguito nel riquadro **Messaggio** viene indicato che le righe vengono aggiunte correttamente alle tabelle.  
  
**(2 righe interessate)(1 riga interessata)(2 righe interessate)**  
  
4.  Sostituire il codice nel riquadro di script con quanto indicato di seguito ed eseguire la query. Si tenterà di aggiungere una nuova riga alla tabella `Products` con un valore `ShelfLife` di 6.  
  
    ```  
    insert dbo.Products values  
    (3, 'Potato Chips', 6, 1, 1)  
    GO  
    ```  
  
5.  Nel riquadro **Messaggio** viene indicato che l'istruzione `INSERT` è in conflitto con il vincolo CHECK esistente, mediante il quale il valore di `ShelfLife` non può essere superiore a 5. La tabella Products non viene aggiornata poiché l'istruzione causa un errore nel vincolo esistente.  
  
6.  Modificare il codice nella parte riportata di seguito ed eseguire di nuovo la query. Si noti che, questa volta, la riga viene aggiornata correttamente.  
  
    ```  
    insert dbo.Products values  
    (3, 'Potato Chips', 2, 1, 1)  
    GO  
    ```  
  
## <a name="see-also"></a>Vedere anche  
[Gestire tabelle e relazioni e correggere errori](../ssdt/manage-tables-relationships-and-fix-errors.md)  
[Usare l'Editor Transact-SQL per modificare ed eseguire script](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)  
  
