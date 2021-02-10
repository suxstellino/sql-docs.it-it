---
title: Eseguire la migrazione di SQL Server al database SQL di Azure usando il Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant per eseguire la migrazione di un SQL Server locale al database SQL di Azure
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 7bd846f4c4abd2a6b9fbcdc434964b789f32d047
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030639"
---
# <a name="migrate-on-premises-sql-server-or-sql-server-on-azure-vms-to-azure-sql-database-using-the-data-migration-assistant"></a>Eseguire la migrazione di SQL Server o SQL Server locali in macchine virtuali di Azure al database SQL di Azure usando il Data Migration Assistant

Il Data Migration Assistant fornisce valutazioni senza problemi della SQL Server locale e degli aggiornamenti a versioni successive di SQL Server o migrazioni a SQL Server in macchine virtuali di Azure o nel database SQL di Azure.

Questo articolo fornisce istruzioni dettagliate per la migrazione di SQL Server database locale al database SQL di Azure usando il Data Migration Assistant.

## <a name="create-a-new-migration-project"></a>Creare un nuovo progetto di migrazione

1. Nel riquadro sinistro selezionare **nuovo** (+), quindi selezionare il tipo di progetto di **migrazione** .

2. Impostare il tipo di origine su **SQL Server** e il tipo di server di destinazione sul **database SQL di Azure**.

3. Selezionare **Crea**.

   ![Crea progetto di migrazione](../dma/media/NewCreate1.png)

## <a name="specify-the-source-server-and-database"></a>Specificare il server e il database di origine

1. Per l'origine, in **Connetti al server di origine**, nella casella di testo **nome server** , immettere il nome dell'istanza di SQL Server di origine.

2. Selezionare il **tipo di autenticazione** supportato dall'istanza di SQL Server di origine.

   > [!NOTE]
   > È consigliabile crittografare la connessione selezionando la casella di controllo **Crittografa connessione** in **Connection poperties**.

    ![Selezione server di origine](../dma/media/select-source-server.png)

3. Selezionare **Connetti**.

4. Selezionare un singolo database di origine di cui eseguire la migrazione al database SQL di Azure.

   > [!NOTE]
   > Se si desidera valutare il database e visualizzare e applicare le correzioni consigliate prima della migrazione, selezionare la casella di controllo **valuta database prima della migrazione?** .

    ![Seleziona database di origine](../dma/media/select-source-database.png)

5. Selezionare **Avanti**.

## <a name="specify-the-target-server-and-database"></a>Specificare il server e il database di destinazione

1. Per la destinazione, in **Connetti al server di destinazione**, nella casella di testo **nome server** , immettere il nome dell'istanza del database SQL di Azure. 

2. Selezionare il **tipo di autenticazione** supportato dall'istanza del database SQL di Azure di destinazione.

   > [!NOTE]
   > È consigliabile crittografare la connessione selezionando la casella di controllo **Crittografa connessione** in **Connection poperties**.

     ![Selezione server di destinazione](../dma/media/select-target-server.png)

3. Selezionare **Connetti**.

4. Selezionare un singolo database di destinazione in cui eseguire la migrazione.

   > [!NOTE]
   > Se si intende eseguire la migrazione degli utenti di Windows, nella casella di testo **nome di dominio dell'utente esterno di destinazione** verificare che il nome di dominio dell'utente esterno di destinazione sia specificato correttamente.

    ![Selezione database di destinazione](../dma/media/select-target-database.png)

5. Selezionare **Avanti**.

## <a name="select-schema-objects"></a>Selezionare gli oggetti dello schema

1. Selezionare gli oggetti dello schema dal database di origine di cui si vuole eseguire la migrazione al database SQL di Azure.

    ![Selezionare gli oggetti dello schema](../dma/media/select-schema-objects.png)

    > [!NOTE]
    > Vengono presentati alcuni degli oggetti che non possono essere convertiti così come sono, offrendo l'opportunità di eseguire la correzione automatica. Facendo clic su questi oggetti nel riquadro sinistro, vengono visualizzate le correzioni suggerite nel riquadro destro. Esaminare le correzioni e scegliere se applicare o ignorare tutte le modifiche, per ogni singolo oggetto. Si noti che il fatto di applicare o ignorare tutte le modifiche per un oggetto non influisce sulle modifiche degli altri oggetti di database. Le istruzioni che non possono essere convertite o corrette automaticamente vengono riprodotte nel database di destinazione e impostate come commenti.

    ![Correzione suggerita](../dma/media/suggested-fix.png)

2. Selezionare **script SQL generale**.

3. Esaminare lo script generato.

    ![Script generato](../dma/media/generated-script.png)

## <a name="deploy-schema"></a>Distribuire lo schema

1. Selezionare **Distribuisci schema**.

2. Esaminare i risultati della distribuzione dello schema.

    ![Risultati della distribuzione dello schema](../dma/media/schema-deployment-results.png)

3. Selezionare **Migrate data** per avviare il processo di migrazione dei dati.

4. Selezionare le tabelle con i dati di cui eseguire la migrazione.

    ![Selezionare le tabelle di cui eseguire la migrazione](../dma/media/select-tables-to-migrate.png) 

5. Selezionare **Avvia migrazione dati**.

La schermata finale mostra lo stato complessivo.

   ![Stato migrazione](../dma/media/migration-status.png) 

## <a name="see-also"></a>Vedi anche

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: impostazioni di configurazione](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedure consigliate](../dma/dma-bestpractices.md)
