---
title: Esplorare le risorse SQL di Azure con Azure Resource Explorer
description: Informazioni su come esplorare e gestire server di Azure SQL, database SQL di Azure e Istanza gestita di SQL di Azure tramite Azure Resource Explorer.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 5d28054e9f459bc3334a863e7ee5edb3bd98afea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048731"
---
# <a name="explore-and-manage-azure-sql-resources-with-azure-resource-explorer"></a>Esplorare e gestire risorse SQL di Azure con Azure Resource Explorer

Questo documento illustra come esplorare e gestire risorse server di Azure SQL, database SQL di Azure e Istanza gestita di SQL di Azure tramite Azure Resource Explorer in [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

>[!NOTE]
>Azure Resource Explorer è supportato in SQL Server 2019. Successivamente, sarà possibile installare l'estensione tramite il [gestore estensioni](./extensions/add-extensions.md) o tramite **File** > **Install Package from VSIX Package** (Installa pacchetto da pacchetto VSIX).

## <a name="connect-to-azure"></a>Connettersi ad Azure

Dopo aver installato il plug-in di SQL, sulla barra dei menu a sinistra compare un'icona di Azure. Fare clic sull'icona per aprire Azure Resource Explorer. Se l'icona di Azure non compare, fare clic con il pulsante destro del mouse sulla barra dei menu a sinistra e scegliere **Azure Resource Explorer**.

### <a name="add-an-azure-account"></a>Aggiungere un account Azure

Per visualizzare le risorse SQL associate a un account Azure, è necessario prima di tutto aggiungere l'account a [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

1. Aprire la finestra di dialogo **Account collegati** tramite l'icona di gestione account in basso a sinistra o tramite il collegamento **Accedi ad Azure...** in Azure Resource Explorer.

    ![Accedere ad Azure](media/azure-resource-explorer/sign-in-to-azure.png)

2. Nella finestra di dialogo **Account collegati** fare clic su **Aggiungi un account**.

    ![Aggiungere un account Azure](media/azure-resource-explorer/add-an-azure-account.png)

3. Fare clic su **Copia e apri** per aprire il browser per l'autenticazione.

    ![Aprire la pagina di autenticazione nel browser](media/azure-resource-explorer/open-authentication-in-browser.png)

4. Incollare il **Codice utente** nella pagina Web e fare clic su **Continua** per eseguire l'autenticazione.

    ![Autenticazione nel browser](media/azure-resource-explorer/authenticate-in-browser.png)

5. A questo punto, in [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] l'account Azure connesso dovrebbe comparire nella finestra di dialogo **Account collegati**.

    ![Account Azure connesso](media/azure-resource-explorer/signed-in-azure-account.png)

### <a name="add-more-azure-accounts"></a>Aggiungere altri account Azure

In [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] sono supportati più account Azure. Per aggiungere altri account Azure, fare clic sul pulsante in alto a destra nella finestra di dialogo **Account collegati** e seguire la stessa procedura illustrata nella sezione Aggiungere un account Azure per aggiungere altri account.

![Aggiunta di account Azure](media/azure-resource-explorer/add-more-azure-account.png)

### <a name="remove-an-azure-account"></a>Rimuovere un account Azure

Per rimuovere un account Azure connesso esistente:

1. Aprire la finestra di dialogo **Account collegati** tramite l'icona di gestione account in basso a sinistra.
2. Fare clic sul pulsante **X** a destra dell'account Azure per rimuoverlo.

    ![Rimozione di account Azure](media/azure-resource-explorer/remove-azure-account.png)

## <a name="filter-subscription"></a>Filtrare la sottoscrizione

Una volta effettuato l'accesso a un account Azure, tutte le sottoscrizioni associate all'account vengono visualizzate in Azure Resource Explorer. È possibile filtrare le sottoscrizioni per ogni account Azure.

1. Fare clic sul pulsante **Seleziona sottoscrizione** a destra dell'account Azure.

   ![Filtrare la sottoscrizione](media/azure-resource-explorer/filter-subscription.png)

2. Selezionare le caselle di controllo per le sottoscrizioni dell'account che si vogliono esplorare e quindi fare clic su **OK**.

   ![Seleziona sottoscrizione](media/azure-resource-explorer/select-subscription.png)

## <a name="explore-azure-sql-resources"></a>Esplorare le risorse SQL di Azure

Per passare a una risorsa SQL di Azure in Azure Resource Explorer, espandere gli account Azure e il gruppo del tipo di risorsa.

Attualmente Azure Resource Explorer supporta server di Azure SQL, database SQL di Azure e Istanza gestita di SQL di Azure.

## <a name="connect-to-azure-sql-resources"></a>Connettersi a risorse SQL di Azure

Azure Resource Explorer consente di connettersi rapidamente a database e server SQL per eseguire query e attività di gestione.

1. Esplorare la risorsa SQL a cui si vuole eseguire la connessione dalla visualizzazione albero.
2. Fare clic con il pulsante destro del mouse sulla risorsa e scegliere **Connetti**. Il pulsante Connetti è anche disponibile a destra della risorsa.

   ![Connettersi alla risorsa SQL di Azure](media/azure-resource-explorer/connect-to-azure-sql-resource.png)

3. Nella finestra di dialogo **Connessione** aperta immettere la password e fare clic su **Connetti**.

   ![Finestra di dialogo della connessione SQL](media/azure-resource-explorer/sql-connection-dialog.png)
4. Una volta stabilita la connessione, la finestra **Server** si apre automaticamente con il nuovo database/server SQL connesso.

## <a name="next-steps"></a>Passaggi successivi

- [Usare [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] per connettersi ed eseguire query nel database SQL di Azure](quickstart-sql-database.md)
- [Usare [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] per connettersi ed eseguire query sui dati in Azure Synapse Analytics](quickstart-sql-dw.md)