---
description: "Esercitazione: uso dell'origine OData"
title: "Esercitazione: uso dell'origine OData | Microsoft Docs"
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2c64cf8b-5edb-48df-8ffe-697096258f71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b0bb777c05b30e4b794fc889dc6322e24c4dd5ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339729"
---
# <a name="tutorial-using-the-odata-source"></a>Esercitazione: uso dell'origine OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In questa esercitazione viene eseguito il processo per estrarre la raccolta **Dipendenti** del servizio **Northwind** OData di esempio (https://services.odata.org/V3/Northwind/Northwind.svc/) e caricarla in un file flat.  
  
## <a name="1-create-an-integration-services-project"></a>1. Creare un progetto di Integration Services  
  
1.  Avviare **SQL Server Data Tools** o [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
2.  Fare clic su **File**, scegliere **Nuovo** e quindi fare clic su **Progetto**.  
  
3.  Nella finestra di dialogo **Nuovo progetto** espandere **Installato**, **Modelli**, **Business Intelligence**, quindi scegliere **Integration Services**.  
  
4.  Selezionare **Progetto di Integration Services** come tipo di progetto.  
  
5.  Immettere un **nome** e selezionare un **percorso** per il progetto, quindi fare clic su **OK**.  
  
## <a name="2-add-and-configure-an-odata-source"></a>2. Aggiungere e configurare un'origine OData 
  
1.  Trascinare un' **Attività Flusso di dati** da **Casella degli strumenti SSIS** all'area di progettazione del flusso di controllo del pacchetto SSIS.  
  
2.  Fare clic sulla scheda **Flusso di dati** oppure fare doppio clic sull'**attività Flusso di dati** per aprire l'area di progettazione del flusso di dati.  
  
3.  Trascinare **Origine Odata** dal gruppo **Comune** in **Casella degli strumenti SSIS**.
  
4.  Fare doppio clic sul componente **Origine OData** per aprire la finestra di dialogo **Editor origine OData**.  
  
5.  Fare clic su **Nuovo** per aggiungere una nuova gestione connessione OData.  
  
6.  Immettere l'URL del servizio OData in **percorso documento di servizio**. Può essere l'URL del documento di servizio o l'URL di un'entità o di un feed specifico. Ai fini di questa esercitazione, immettere l'URL del documento di servizio: [https://services.odata.org/V3/Northwind/Northwind.svc/](https://services.odata.org/V3/Northwind/Northwind.svc/).  
  
7.  Verificare che sia selezionato **Autenticazione di Windows** come **autenticazione** da utilizzare per accedere al servizio OData. **Autenticazione di Windows** è selezionato per impostazione predefinita.  
  
8.  Fare clic su **Test connessione** per testare la connessione e fare clic su **OK** per completare la creazione di un'istanza della gestione connessione OData.  
  
9. Nella finestra di dialogo **Editor origine OData** , verificare che **Raccolta** sia selezionato per l'opzione **Utilizza raccolta in percorso risorse** .  
  
10. Selezionare **Dipendenti** nell'elenco a discesa **Raccolta**.  
  
11. Immettere tutti i filtri o le opzioni query OData aggiuntive per **Opzioni query**. Ad esempio: `$orderby=CompanyName&$top=100`. Ai fini di questa esercitazione immettere `$top=5`.  
  
12. Fare clic su **Anteprima** per visualizzare un'anteprima dei dati.  
  
13. Fare clic su **Colonne** nel riquadro di navigazione sinistro per passare alla pagina **Colonne** .  
  
14. Scegliere **EmployeeID**, **FirstName** e **LastName** da **Colonne esterne disponibili** selezionando le caselle di controllo.  
  
15. Fare clic su **OK** per chiudere la finestra di dialogo **Editor origine OData** .  
  
## <a name="3-add-and-configure-a-flat-file-destination"></a>3. Per aggiungere e configurare una destinazione file flat
  
1.  Trascinare una **Destinazione file flat** dalla **casella degli strumenti SSIS** all'area di progettazione del flusso di dati sotto il componente **Origine Odata** .  
  
2.  Connettere il componente **Origine Odata** al componente **Destinazione file flat** utilizzando la freccia blu.  
  
3.  Fare doppio clic su **Destinazione file flat**. Viene visualizzata la finestra di dialogo **Editor destinazione file flat** .  
  
4.  Nella finestra di dialogo **Editor destinazione file flat** fare clic su **Nuovo** per creare una nuova gestione connessione file flat.  
  
5.  Nella finestra di dialogo **Formato file flat** selezionare **Delimitato**. Viene visualizzata la finestra di dialogo **Editor gestione connessione file flat**.  
  
6.  Nella finestra di dialogo **Editor gestione connessione file flat** immettere `c:\Employees.txt` in **Nome file**.  
  
7.  Fare clic su **Colonne** nel pannello di navigazione sinistro. In questa pagina è possibile visualizzare l'anteprima dei dati.  
  
8.  Fare clic su OK per chiudere la finestra di dialogo **Editor gestione connessione file flat** .  
  
9. Nella finestra di dialogo **Editor destinazione file flat** fare clic su **Mapping** nel riquadro di navigazione sinistro. Controllare i mapping.  
  
10. Fare clic su OK per chiudere la finestra di dialogo **Editor destinazione file flat** .  

## <a name="4-run-the-package"></a>4. Eseguire il pacchetto
Eseguire il pacchetto SSIS. Verificare che il file di output venga creato con l'ID, il nome e il cognome di cinque dipendenti dal feed OData.
  
  
