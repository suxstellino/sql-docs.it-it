---
title: Componenti e configurazione di SSMS
description: Esercitazione che descrive i componenti e le opzioni di configurazione di base per l'ambiente di SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/16/2018
ms.openlocfilehash: 1688998d5a3f6b9033808701c33ff141cf8cc8c3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597188"
---
# <a name="sql-server-management-studio-components-and-configuration"></a>Componenti e configurazione di SQL Server Management Studio

In questa esercitazione vengono descritti i vari componenti finestra in SQL Server Management Studio (SSMS) e alcune opzioni di configurazione di base per l'area di lavoro. In questo articolo vengono illustrate le operazioni seguenti: 

> [!div class="checklist"]
> * Identificare i componenti che costituiscono l'ambiente di SSMS
> * Modificare il layout dell'ambiente e ripristinare quello predefinito
> * Ingrandire l'editor di query
> * Modificare il tipo di carattere
> * Configurare le opzioni di avvio
> * Ripristinare la configurazione predefinita

## <a name="prerequisites"></a>Prerequisites

Per completare questa esercitazione è necessario avere SQL Server Management Studio.  

* Installare [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

## <a name="sql-server-management-studio-components"></a>Componenti di SQL Server Management Studio

In questa sezione vengono descritti i vari componenti finestra disponibili nell'area di lavoro e come usarli.

* Per chiudere una finestra, selezionare la **X** nell'angolo destro della barra del titolo.
* Per riaprire una finestra, selezionarla nel menu **Visualizza**.

    ![Menu Visualizza](media/ssms-configuration/viewmenu.png)

* **Esplora oggetti** (F8): Esplora oggetti è una visualizzazione struttura ad albero di tutti gli oggetti di database nel server. Questa visualizzazione include i database del motore di database di SQL Server, SQL Server Analysis Services, SQL Server Reporting Services e SQL Server Integration Services. Esplora oggetti contiene informazioni su tutti i server a esso connessi. 

    ![Esplora oggetti](media/ssms-configuration/objectexplorer.png)
* **Finestra Query** (CTRL+N): dopo aver selezionato **Nuova query**, immettere le query Transact-SQL (T-SQL) in questa finestra. Qui vengono visualizzati anche i risultati delle query.

    ![Finestra Nuova query](media/ssms-configuration/newquery.png)

* **Proprietà** (F4): quando la finestra di query è aperta, sarà disponibile la visualizzazione Proprietà che include le proprietà di base della query. Indica ad esempio l'orario in cui è stata avviata una query, il numero di righe restituite e i dettagli di connessione.  

    ![Proprietà di configurazione](media/ssms-configuration/properties.png)

* **Visualizzatore modelli** (CTRL+ALT+T): il visualizzatore modelli include vari modelli T-SQL predefiniti. È possibile usare questi modelli per eseguire varie funzioni quali la creazione o il backup di database. 

    ![Visualizzatore modelli](media/ssms-configuration/templates.png)

* **Dettagli Esplora oggetti** (F7): questa visualizzazione è più granulare rispetto a quella in Esplora oggetti. È possibile usare Dettagli Esplora oggetti per modificare più oggetti contemporaneamente. In questa finestra è ad esempio possibile selezionare più database e quindi eliminarli o includerli in uno script contemporaneamente. 

    ![Visualizza](media/ssms-configuration/objectexplorerdetails.PNG) 

## <a name="change-the-environment-layout"></a>Modificare il layout ambiente 

In questa sezione viene descritto come modificare il layout dell'ambiente, ad esempio come spostare varie finestre. 

* Per spostare una finestra, premere e tenere premuto il titolo e quindi trascinare la finestra. 
* Per bloccare o sbloccare una finestra, selezionare l'icona della puntina da disegno nella barra del titolo:

    ![Bloccare un oggetto](media/ssms-configuration/pushpin.png)

* Ogni componente finestra ha un menu a discesa che consente di modificare la finestra in vari modi: 

    ![Opzioni della finestra](media/ssms-configuration/windowoptions.png)

* Quando sono aperte due o più finestre di query, è possibile raggrupparle in schede verticali o orizzontali in modo che siano entrambe visibili. Per visualizzare finestre a schede, fare clic con il pulsante destro del mouse sul titolo della query e quindi selezionare l'opzione a schede preferita:

    ![Opzioni delle schede query](media/ssms-configuration/querytabbedoptions.png)

    * Di seguito è riportato un gruppo di schede orizzontali:

      ![Esempio di un gruppo di schede orizzontali](media/ssms-configuration/horizontaltab.png)

    * Di seguito è riportato un gruppo di schede verticali:

      ![Esempio di un gruppo di schede verticali](media/ssms-configuration/verticaltabgroup.png)

    * Per unire le schede, fare clic con il pulsante destro del mouse sul titolo della query e quindi selezionare **Passa al gruppo di schede precedente** o **Passa al gruppo di schede successivo**:

      ![Unire le schede delle query](media/ssms-configuration/mergetabgroups.png)

* Per ripristinare il layout predefinito dell'ambiente, nel menu **Finestra** selezionare **Reimposta layout finestra**:

    ![Ripristinare il layout della finestra](media/ssms-configuration/resetwindowlayout.png)

## <a name="maximize-query-editor"></a>Ingrandire l'editor di query

È possibile ingrandire l'editor di query alla modalità schermo intero:

1. Fare clic in un punto qualsiasi all'interno della finestra dell'editor di query.

2. Premere MAIUSC+ALT+INVIO per passare dalla modalità normale a quella a schermo intero e viceversa. 

Questa scelta rapida da tastiera funziona con tutte le finestre dei documenti. 

## <a name="change-basic-settings"></a>Modificare le impostazioni di base

In questa sezione viene descritto come modificare alcune impostazioni di base in SSMS dal menu **Strumenti**.

  ![Strumenti - menu](media/ssms-configuration/tools.png)

* Per modificare la barra degli strumenti evidenziata, selezionare **Strumenti** > **Personalizza**:

    ![Personalizzare una barra degli strumenti](media/ssms-configuration/toolbar.png)

### <a name="change-the-font"></a>Modificare il tipo di carattere

* Per modificare il tipo di carattere, selezionare **Strumenti** > **Opzioni** > **Tipi di carattere e colori**:

     ![Modificare tipi di carattere e colori](media/ssms-configuration/fontsandcolors.png)

### <a name="change-startup-options"></a>Modificare le opzioni di avvio

* Le opzioni di avvio determinano l'aspetto dell'area di lavoro quando si apre SSMS per la prima volta. Per modificare le opzioni di avvio, selezionare **Strumenti** > **Opzioni** > **Avvio**:

    ![Modificare le opzioni di avvio](media/ssms-configuration/startup.png)

### <a name="reset-settings-to-the-default"></a>Reimpostare i valori predefiniti delle impostazioni

* È possibile esportare e importare tutte queste impostazioni dal menu. Per importare o esportare le impostazioni oppure per ripristinare le impostazioni predefinite, selezionare **Strumenti** > **Importa/Esporta impostazioni** 

    ![Importare ed esportare le impostazioni](media/ssms-configuration/settings.png)

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questa *esercitazione* e questi articoli di *procedure* sono utili per varie funzionalità disponibili in SSMS.  Questi articoli illustrano come gestire i componenti di SSMS e individuare le funzionalità usate regolarmente.

* [Connettersi ed eseguire query su un'istanza](../quickstarts/ssms-connect-query-sql-server.md)
* [Scripting](scripting-ssms.md)
* [Uso di modelli in SSMS](../template/templates-ssms.md)
* [Suggerimenti e consigli per l'uso di SSMS](ssms-tricks.md)