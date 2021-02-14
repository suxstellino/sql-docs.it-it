---
description: Lezione 1-4 - Aggiunta delle configurazioni dei pacchetti
title: 'Passaggio 4: Aggiunta delle configurazioni dei pacchetti | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e04a5321-63d5-4ec5-85b9-cb4eaf6c87f6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ca2d3bb08a1059970b8c72da5541b2ad3d9d1c8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354337"
---
# <a name="lesson-1-4---adding-package-configurations"></a>Lezione 1-4 - Aggiunta delle configurazioni dei pacchetti

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In questa attività si procederà all'aggiunta di una configurazione a ogni pacchetto. Le configurazioni consentono di aggiornare i valori delle proprietà dei pacchetti e gli oggetti dei pacchetti in fase di esecuzione.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] include diversi tipi di configurazioni. È possibile archiviare le configurazioni in variabili di ambiente, voci del Registro di sistema, variabili definite dall'utente, tabelle di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e file XML. Per offrire maggiore flessibilità, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] supporta l'utilizzo di configurazioni indirette, ovvero l'utilizzo di una variabile di ambiente per specificare il percorso della configurazione nella quale sono specificati i valori effettivi. I pacchetti del progetto Deployment Tutorial utilizzano una combinazione di file di configurazione XML e configurazioni indirette. In un file di configurazione XML è possibile includere configurazioni per più proprietà e, quando opportuno, farvi riferimento con più pacchetti. In questa esercitazione verrà utilizzato un file di configurazione separato per ogni pacchetto.  
  
Nei file di configurazione sono spesso contenute informazioni riservate, ad esempio le stringhe di connessione. È pertanto consigliabile utilizzare un elenco di controllo di accesso per limitare l'accesso al percorso o alla cartella in cui vengono archiviati i file e concederlo solo a utenti o account autorizzati a eseguire i pacchetti. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../integration-services/security/security-overview-integration-services.md#files).  
  
Le configurazioni sono necessarie per eseguire correttamente dopo la distribuzione nel server di destinazione i pacchetti DataTransfer e LoadXMLData aggiunti al progetto Deployment Tutorial nell'attività precedente. Per implementare le configurazioni, verranno innanzitutto create le configurazioni indirette per i file di configurazione XML che verranno successivamente creati.  
  
Si creeranno due file di configurazione, DataTransferConfig.dtsConfig e LoadXMLData.dtsConfig. Questi file contengono coppie nome/valore che aggiornano le proprietà in pacchetti che specificano il percorso dei dati e i file di log utilizzati dal pacchetto. Nell'ambito del successivo processo di distribuzione si procederà all'aggiornamento dei valori nei file di configurazione affinché riflettano il nuovo percorso dei file nel computer di destinazione.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] riconosce DataTransferConfig.dtsConfig e LoadXMLData.dtsConfig come dipendenze dei pacchetti DataTransfer e LoadXMLData e include automaticamente i file di configurazione nel pacchetto di distribuzione che verrà creato nella lezione successiva.  
  
### <a name="to-create-indirect-configuration-for-the-datatransfer-package"></a>Per creare la configurazione indiretta per il pacchetto DataTransfer  

Verificare il modello di distribuzione corrente del progetto e impostarlo su **Modello di distribuzione del pacchetto** se necessario. Scegliere **Converti nel modello di distribuzione del pacchetto** dal menu **Progetto**
  
1.  In Esplora soluzioni fare doppio clic su DataTransfer.dtsx.  
  
2.  Nella finestra di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] fare clic in un punto qualsiasi dello sfondo dell'area di progettazione del flusso di controllo.  
  
3.  Scegliere **Configurazioni pacchetto** dal menu **SSIS**.  
  
4.  Nella finestra di dialogo **Libreria configurazioni pacchetto** selezionare **Abilita configurazioni pacchetto** , se non è già selezionata, e quindi fare clic su **Aggiungi**.  
  
5.  Nella pagina iniziale di Configurazione guidata pacchetto fare clic su **Avanti**.  
  
6.  Nella pagina Selezione tipo di configurazione selezionare **File di configurazione XML** nell'elenco **Tipo configurazione** , selezionare l'opzione **Percorso della configurazione memorizzato in una variabile di ambiente** e digitare **DataTransfer** oppure selezionare la variabile di ambiente **DataTransfer** nell'elenco.  
  
    > [!NOTE]  
    > Dopo aver aggiunto la variabile di ambiente, potrebbe essere necessario riavviare il computer affinché risulti disponibile nell'elenco. Se non si desidera riavviare il computer, è possibile digitare il nome della variabile di ambiente.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina Completamento procedura guidata digitare **DataTransfer EV Configuration** nella casella **Nome configurazione** , esaminare il contenuto della configurazione nel riquadro **Anteprima** e quindi fare clic su **Fine**.  
  
9. Chiudere la finestra di dialogo **Libreria configurazioni pacchetto**.  
  
### <a name="to-create-the-xml-configuration-for-the-datatransfer-package"></a>Per creare il file di configurazione XML per il pacchetto DataTransfer  
  
1.  In Esplora soluzioni fare doppio clic su DataTransfer.dtsx.  
  
2.  Nella finestra di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] fare clic in un punto qualsiasi dello sfondo dell'area di progettazione del flusso di controllo.  
  
3.  Scegliere **Configurazioni pacchetto** dal menu **SSIS**.  
  
4.  Nella finestra di dialogo Libreria configurazioni pacchetto selezionare **Abilita configurazioni pacchetto** e quindi fare clic su **Aggiungi**.  
  
5.  Nella pagina iniziale di Configurazione guidata pacchetto fare clic su **Avanti**.  
  
6.  Nella pagina Selezione tipo di configurazione selezionare **File di configurazione XML** nell'elenco **Tipo di configurazione** e quindi fare clic su **Sfoglia**.  
  
7.  Nella finestra di dialogo **Selezionare il percorso del file di configurazione** passare a C:\DeploymentTutorial e digitare **DataTransferConfig** nella casella **Nome file** e quindi fare clic su **Salva**.  
  
8.  Nella pagina Selezione tipo di configurazione fare clic su **Avanti**.  
  
9. Nella pagina Selezione proprietà da esportare espandere DataTransfer, Gestioni connessioni, Deployment Tutorial Log e Properties e quindi selezionare la casella di controllo **Stringa di connessione** .  
  
10. In Gestioni connessioni espandere NewCustomers e quindi selezionare la casella di controllo **Stringa di connessione** .  
  
11. Fare clic su **Avanti**.  
  
12. Nella pagina Completamento procedura guidata digitare **DataTransfer Configuration** nella casella **Nome configurazione** , esaminare il contenuto della configurazione e quindi fare clic su **Fine**.  
  
13. Nella finestra di dialogo **Libreria configurazioni pacchetto** verificare che DataTransfer EV Configuration sia elencata per prima e DataTransfer Configuration per seconda e quindi fare clic su **Chiudi**.  
  
### <a name="to-create-indirect-configuration-for-the-loadxmldata-package"></a>Per creare la configurazione indiretta per il pacchetto LoadXMLData  
  
1.  In Esplora soluzioni fare doppio clic sul pacchetto LoadXMLData.dtsx.  
  
2.  Nella finestra di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] fare clic in un punto qualsiasi dello sfondo dell'area di progettazione del flusso di controllo.  
  
3.  Scegliere **Configurazioni pacchetto** dal menu **SSIS**.  
  
4.  Nella finestra di dialogo **Libreria configurazioni pacchetto** fare clic su **Aggiungi**.  
  
5.  Nella pagina iniziale di Configurazione guidata pacchetto fare clic su **Avanti**.  
  
6.  Nella pagina Selezione tipo di configurazione selezionare **File di configurazione XML** nell'elenco **Tipo configurazione** , selezionare l'opzione **Percorso della configurazione memorizzato in una variabile di ambiente** e digitare **LoadXMLData** oppure selezionare la variabile di ambiente **LoadXMLData** nell'elenco.  
  
    > [!NOTE]  
    > Dopo aver aggiunto la variabile di ambiente, potrebbe essere necessario riavviare il computer affinché risulti disponibile nell'elenco.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina Completamento procedura guidata digitare **LoadXMLData EV Configuration** nella casella **Nome configurazione** , esaminare il contenuto della configurazione e quindi fare clic su **Fine**.  
  
### <a name="to-create-the-xml-configuration-for-the-loadxmldata-package"></a>Per creare il file di configurazione XML per il pacchetto LoadXMLData  
  
1.  In Esplora soluzioni fare doppio clic sul pacchetto LoadXMLData.dtsx.  
  
2.  Nella finestra di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] fare clic in un punto qualsiasi dello sfondo dell'area di progettazione del flusso di controllo.  
  
3.  Scegliere **Configurazioni pacchetto** dal menu **SSIS**.  
  
4.  Nella finestra di dialogo Libreria configurazioni pacchetto selezionare **Abilita configurazioni pacchetto** e quindi fare clic su **Aggiungi**.  
  
5.  Nella pagina iniziale di Configurazione guidata pacchetto fare clic su **Avanti**.  
  
6.  Nella pagina Selezione tipo di configurazione selezionare **File di configurazione XML** nell'elenco **Tipo di configurazione** e quindi fare clic su **Sfoglia**.  
  
7.  Nella finestra di dialogo **Selezionare il percorso del file di configurazione** passare a C:\DeploymentTutorial e digitare **LoadXMLDataConfig** nella casella **Nome file** e quindi fare clic su **Salva**.  
  
8.  Nella pagina Selezione tipo di configurazione fare clic su **Avanti**.  
  
9. Nella pagina Selezione proprietà da esportare espandere LoadXMLData, File eseguibili, Load XML Data e Properties, quindi selezionare le caselle di controllo **[XMLSource].[XMLData]** e **[XMLSource].[XMLSchemaDefinition]** .  
  
10. Fare clic su **Avanti**.  
  
11. Nella pagina Completamento procedura guidata digitare **LoadXMLData Configuration** nella casella **Nome configurazione** , esaminare il contenuto della configurazione e quindi fare clic su **Fine**.  
  
12. Nella finestra di dialogo **Libreria configurazioni pacchetto** verificare che LoadXMLData EV Configuration sia elencata per prima e LoadXMLData Configuration per seconda e quindi fare clic su **Chiudi**.  
  
## <a name="next-task-in-lesson"></a>Attività successiva della lezione  
[Passaggio 5: Test dei pacchetti aggiornati](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="see-also"></a>Vedere anche  
[SSIS](./packages/legacy-package-deployment-ssis.md)  
[Creazione di configurazioni dei pacchetti](./packages/legacy-package-deployment-ssis.md)  
[Accesso ai file utilizzati dai pacchetti](../integration-services/security/security-overview-integration-services.md#files)