---
title: Uso di modelli in SQL Server Management Studio
description: Informazioni sui modelli Transact-SQL (T-SQL) predefiniti disponibili in SQL Server Management Studio (SSMS).
keywords: SQL Server, SSMS, SQL Server Management Studio, Modelli
author: MashaMSFT
ms.author: mathoma
ms.topic: tutorial
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
helpviewer_keywords:
- templates [SQL Server], SQL Server Management Studio
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: ee33e30ef69b0c4a464e91576dc33e6d15ca4890
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596972"
---
# <a name="use-templates-in-sql-server-management-studio"></a>Usare modelli in SQL Server Management Studio

Questa esercitazione illustra i modelli predefiniti di Transact-SQL (T-SQL) che sono disponibili in SQL Server Management Studio (SSMS). In questo articolo vengono illustrate le operazioni seguenti:

## <a name="prerequisites"></a>Prerequisites

Per completare questa esercitazione, è necessario SQL Server Management Studio e l'accesso a un server SQL.

* Installare [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

* Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).

## <a name="use-template-browser"></a>Usare il visualizzatore modelli

Questa sezione illustra come individuare e usare il visualizzatore modelli.

1. Aprire SQL Server Management Studio.

2. Nel menu **Visualizza** selezionare **Visualizzatore modelli** (CTRL+ALT+T):

    ![Aprire il visualizzatore modelli](media/templates-ssms/templatebrowser.png)

    È possibile visualizzare i modelli usati di recente nella parte inferiore del visualizzatore modelli.

3. Espandere il nodo desiderato. Fare clic con il pulsante destro sul modello e quindi selezionare **Apri**:

    ![Aprire un modello](media/templates-ssms/opentemplate.png)

    È anche possibile fare doppio clic sul nome del modello per aprirlo.

4. Viene visualizzata una nuova finestra di query. Lo script T-SQL è già popolato.

5. Modificare il modello in base alle esigenze e selezionare **Esegui** per eseguire la query:

    ![Creare un modello di database](media/templates-ssms/createdbtemplate.png)

## <a name="edit-an-existing-template"></a>Modificare un modello esistente

È anche possibile modificare i modelli esistenti nel visualizzatore modelli.  

1. Nel visualizzatore modelli passare al modello da usare.

2. Fare clic con il pulsante destro del mouse sul modello e quindi scegliere **Modifica**:

    ![Modificare un modello](media/templates-ssms/edittemplate.png)

3. Nella finestra di query aperta apportare le modifiche desiderate.

4. Per salvare il modello, selezionare **File** > **Salva** (CTRL+S).

5. Chiudere la finestra di query.

6. Riaprire il modello. Vengono visualizzate le modifiche apportate.

## <a name="locate-templates-on-disk"></a>Individuare i modelli su disco

Quando un modello è aperto, è possibile individuare i modelli che si trovano su disco.

1. Nel visualizzatore modelli selezionare un modello e quindi scegliere **Modifica**.

2. Fare clic con il pulsante destro del mouse su **Titolo query**, quindi selezionare **Apri cartella superiore**. I modelli archiviati su disco vengono visualizzati in Esplora risorse: 

   ![Modelli su disco](media/templates-ssms/templatesondisk.png)
  
## <a name="create-a-new-template"></a>Creare un nuovo modello

È anche possibile creare un nuovo modello nel visualizzatore modelli. La procedura seguente illustra come creare una nuova cartella e quindi creare un nuovo modello nella cartella. È anche possibile seguire la procedura per creare un modello personalizzato in una cartella esistente. 

1. Aprire il visualizzatore modelli.

2. Fare clic con il pulsante destro del mouse su **Modelli di SQL Server** e quindi selezionare **Nuovo** > **Cartella**.

3. Denominare la cartella **Modelli personalizzati**:

    ![Creare una cartella di modelli personalizzati](media/templates-ssms/creatingcustomtemplate.png)

4. Fare clic con il pulsante destro del mouse sulla cartella Modelli personalizzati appena creata e quindi selezionare **Nuovo** > **Modello**. Immettere un nome per il modello:

    ![Creare un modello personalizzato](media/templates-ssms/createnewtemplate.png)

5. Fare clic con il pulsante destro del mouse sul modello creato e quindi scegliere **Modifica**. Viene visualizzata la nuova finestra di query.

6. Immettere il testo T-SQL che si vuole salvare.

7. Scegliere **Salva** dal menu **File**.

8. Chiudere la finestra di query esistente e aprire il nuovo modello personalizzato.

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questa *esercitazione* e questi articoli di *procedure* sono utili per varie funzionalità disponibili in SSMS.  Questi articoli illustrano come gestire i componenti di SSMS e individuare le funzionalità usate regolarmente.

* [Connettersi ed eseguire query su un'istanza](../quickstarts/ssms-connect-query-sql-server.md)
* [Scripting](../tutorials/scripting-ssms.md)
* [Configurazione di SSMS](../tutorials/ssms-configuration.md)
* [Suggerimenti e consigli per l'uso di SSMS](../tutorials/ssms-tricks.md)