---
title: Eliminare oggetti e risolvere le dipendenze
description: Informazioni su come rinominare o eliminare oggetti di database. Scoprire quali oggetti dipendenti vengono aggiornati automaticamente da SSDT e quali dipendenze è necessario aggiornare.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- Microsoft.VisualStudio.Data.Tools.Project.HelpKeywords.SqlProjectDropDatabaseConfirmationDialog
- sql.data.tools.dropdatabaseconfirmation.dialog
- sql.data.tools.dropmultipledatabasesconfirmation.dialog
ms.assetid: fb31c2b1-ca4f-4e11-a0b6-5c26430f1c8c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 72e9c09651d473c25b2df84e0b6ccecee0964959
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018231"
---
# <a name="how-to-delete-objects-and-resolve-dependencies"></a>Procedura: Eliminare oggetti e risolvere le dipendenze

Quando si rinomina o si elimina un oggetto in **Esplora oggetti di SQL Server**, in SQL Server Data Tools vengono rilevati automaticamente tutti i relativi oggetti di dipendenza e verrà preparato uno script ALTER per rinominare o eliminare la dipendenza in base alle esigenze.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-delete-a-database"></a>Per eliminare un database  
  
1.  Fare clic con il pulsante destro del mouse su un database in **Esplora oggetti di SQL Server** e selezionare **Elimina**.  
  
2.  Accettare tutte le impostazioni predefinite nella finestra di dialogo **Elimina database** e fare clic su **OK**.  
  
### <a name="to-rename-a-table"></a>Per rinominare una tabella  
  
1.  Verificare che la tabella `Customer` non sia aperta in Progettazione tabelle o nell'Editor Transact\-SQL.  
  
2.  Espandere il nodo **Tabelle** in **Esplora oggetti di SQL Server**. Fare clic con il pulsante destro del mouse sulla tabella **Customer** e selezionare **Rinomina**.  
  
3.  Impostare il nome della tabella su **Customers** e premere INVIO.  
  
4.  Si noti che viene richiamata automaticamente un'operazione **Aggiornamento database**. Tramite SSDT verrà chiamata automaticamente la stored procedure sp_rename per rinominare la tabella. Se sono presenti eventuali oggetti dipendenti come vincoli di chiave esterna, verranno aggiornati anche questi ultimi.  
  
    > [!WARNING]  
    > Dipendenze basate su script, quali i riferimenti a una tabella da una vista o stored procedure, non vengono aggiornati automaticamente da SSDT. Al termine della ridenominazione, è possibile usare il riquadro **Elenco errori** per individuare tutte le altre dipendenze e correggerle manualmente.  
  
5.  Applicare la modifica seguendo i passaggi descritti in [Procedura: Aggiornare un database connesso con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
6.  Fare di nuovo clic con il pulsante destro del mouse sulla tabella **Customers** in **Esplora oggetti di SQL Server** e selezionare **Visualizza dati**. Si noti che i dati della tabella non sono stati modificati dopo l'operazione di ridenominazione.  
  
7.  Fare clic con il pulsante destro del mouse sulla tabella **Products** e selezionare **Visualizza codice**. Si noti che il riferimento di chiave esterna è stato aggiornato automaticamente in `REFERENCES [dbo].[Customers] ([Id])` per riflettere la ridenominazione.  
  
### <a name="to-delete-a-table"></a>Per eliminare una tabella  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella **Customers** in **Esplora oggetti di SQL Server** e selezionare **Elimina**.  
  
2.  In **Azione utente** nella finestra di dialogo **Anteprima aggiornamenti database** si noti che tramite SSDT sono stati identificati tutti gli oggetti dipendenti, in questo caso un riferimento di chiave esterna che verrà eliminato.  
  
3.  Fare clic su **Aggiorna database**.  
  
4.  Fare clic con il pulsante destro del mouse sulla tabella **Products** in **Esplora oggetti di SQL Server** e selezionare **Visualizza codice**. Si noti che il riferimento di chiave esterna alla tabella `Customers` non è più presente.  
  
    > [!WARNING]  
    > Se la tabella **Products** è già aperta in Progettazione tabelle o nell'Editor Transact\-SQL quando l'operazione di eliminazione viene eseguita, non verrà aggiornata automaticamente per mostrare l'eliminazione del riferimento di chiave esterna. Inoltre, si possono visualizzare errori sui riferimenti non risolti nell'**Elenco errori**. Per risolvere questo problema, chiudere Progettazione tabelle o l'Editor Transact\-SQL e riaprire la tabella Products.  
  
