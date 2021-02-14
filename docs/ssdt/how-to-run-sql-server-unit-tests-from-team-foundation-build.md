---
title: Eseguire unit test di SQL Server da Team Foundation Build
description: Informazioni su come eseguire unit test di SQL Server da Team Foundation Build. Scoprire come creare una definizione di compilazione ed eseguire unit test in un'esecuzione dei test automatizzata.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 24f5b85d-d6f9-415f-b09f-933b78dc0b67
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2b87720ba7a4cbdb531cd19fe23be7d48d9a07ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018131"
---
# <a name="how-to-run-sql-server-unit-tests-from-team-foundation-build"></a>Procedura: Eseguire unit test di SQL Server da Team Foundation Build

È possibile usare Team Foundation Build per eseguire gli unit test di SQL Server come parte di un test di verifica della compilazione (BVT). È possibile configurare gli unit test per distribuire il database, generare dati di test e quindi eseguire i test selezionati. Se non si ha familiarità con Team Foundation Build, è consigliabile esaminare le informazioni riportate di seguito prima di applicare le procedure descritte in questo argomento:  
  
-   [Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
  
-   [Procedura: Configurare ed eseguire test pianificati dopo la compilazione dell'applicazione](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100))  
  
-   [Creare una definizione di compilazione di base](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
  
Prima di utilizzare queste procedure, è necessario innanzitutto configurare l'ambiente di lavoro effettuando le attività seguenti:  
  
-   Installare il controllo della versione di Team Foundation e Team Foundation Build. Probabilmente si dovrà installare il controllo della versione di Team Foundation e Team Foundation Build in due differenti computer.  
  
-   Installare MicrosoftSQL Server Data Tools Build Utilities sullo stesso computer in cui è installato Team Foundation Build. Per installare SQL Server Data Tools Build Utilities, per prima cosa occorre creare un punto di installazione amministrativa. Per altre informazioni sul punto di installazione amministrativa, vedere [Installare SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md). Quindi, installare SSDTBuildUtilties.msi sul server di compilazione dal percorso (/location) utilizzato per il punto di installazione amministrativa.  
  
-   Connettersi a un'istanza di Visual Studio Team Foundation Server.  
  
Dopo aver configurato l'ambiente di lavoro, è quindi necessario seguire questi passaggi:  
  
1.  Creare un progetto di database.  
  
2.  Importare o creare lo schema e gli oggetti del progetto di database.  
  
3.  Configurare le proprietà del progetto di database per la compilazione e la distribuzione.  
  
4.  Creare uno o più unit test.  
  
5.  Aggiungere la soluzione contenente il progetto di database e il progetto di unit test al controllo delle versioni e archiviare tutti i file.  
  
Nelle procedure disponibili in questo argomento viene descritto come creare una definizione di compilazione per eseguire gli unit test come parte di un'esecuzione automatica di test:  
  
1.  [Configurare le impostazioni di test per l'esecuzione di unit test del database in un agente di compilazione x64](#ConfigureX64)  
  
2.  [Assegnare test a una relativa categoria (facoltativo)](#CreateATestList)  
  
3.  [Modificare il progetto di test](#ModifyTestProject)  
  
4.  [Archiviare la soluzione](#CheckInTheTestList)  
  
5.  [Creare una definizione di compilazione](#CreateBuildDef)  
  
6.  [Eseguire la nuova definizione di compilazione](#RunBuild)  
  
**Esecuzione di unit test di SQL Server in un computer di compilazione**  
  
Quando si eseguono unit test in un computer di compilazione, potrebbe non essere possibile trovare i file di progetto del database (con estensione sqlproj). Questo problema si verifica perché il file app.config fa riferimento ai file in questione utilizzando percorsi relativi. Inoltre, l'esito degli unit test potrebbe essere negativo se non viene trovata l'istanza di SQL Server da usare per l'esecuzione di unit test. Questo problema può verificarsi se le stringhe di connessione archiviate nel file app.config non sono valide dal computer di compilazione.  
  
Per risolvere questi problemi, è necessario specificare una sezione di override nel file app.config tramite cui viene eseguito l'override di app.config con un file di configurazione specifico dell'ambiente Team Foundation Build. Per altre informazioni, vedere [Modificare il progetto di test](#ModifyTestProject) più avanti in questo argomento.  
  
## <a name="configure-test-settings-to-run-sql-server-unit-tests-on-an-x64-build-agent"></a><a name="ConfigureX64"></a>Configurare le impostazioni di test per l'esecuzione di unit test di SQL Server in un agente di compilazione x64  
Prima di poter eseguire gli unit test in un agente di compilazione x64, è necessario configurare le impostazioni di test per modificare la piattaforma processi host.  
  
#### <a name="to-specify-the-host-process-platform"></a>Per specificare la piattaforma processi host  
  
1.  Aprire la soluzione contenente il progetto di test per cui si desidera configurare le impostazioni.  
  
2.  Nella cartella **Elementi di soluzione** di **Esplora soluzioni** fare doppio clic sul file **Local.testsettings**.  
  
    Verrà visualizzata la finestra di dialogo **Impostazioni test**.  
  
3.  Nell'elenco scegliere **Host**.  
  
4.  In **Piattaforma processi host** all'interno del riquadro dei dettagli fare clic su **MSIL** per configurare i test da eseguire in un agente di compilazione x64.  
  
5.  Fare clic su **Applica**.  
  
## <a name="assign-tests-to-a-test-category-optional"></a><a name="CreateATestList"></a>Assegnare test a una relativa categoria (facoltativo)  
In genere, quando si crea una definizione di compilazione per eseguire unit test, si specificano una o più categorie di test. Tutti i test nelle categorie specificate vengono eseguiti quando viene eseguita la compilazione.  
  
#### <a name="to-assign-tests-to-a-test-category"></a>Per assegnare test a una relativa categoria  
  
1.  Aprire la finestra **Visualizzazione test**.  
  
2.  Selezionare un test.  
  
3.  Nel riquadro delle proprietà fare clic su **Categorie di test**, quindi sui puntini di sospensione (...) nella colonna più a destra.  
  
4.  Nella casella **Aggiungi nuova categoria** della finestra **Categoria test** digitare un nome per la nuova categoria di test.  
  
5.  Fare clic su **Aggiungi** e quindi fare clic su **OK**.  
  
    La nuova categoria di test verrà assegnata al test e sarà disponibile per gli altri test tramite le relative proprietà.  
  
## <a name="modify-the-test-project"></a><a name="ModifyTestProject"></a>Modificare il progetto di test  
Per impostazione predefinita, in Team Foundation Build viene creato un file di configurazione dal file app.config del progetto quando si compila il progetto di unit test. Il percorso del progetto del database viene archiviato come percorso relativo nel file app.config. I percorsi relativi utilizzabili in Visual Studio non potranno essere usati perché in Team Foundation Build i file compilati vengono inseriti in percorsi diversi in relazione alla posizione di esecuzione degli unit test. Inoltre, nel file app.config sono incluse le stringhe di connessione tramite cui viene specificato il database da testare. È inoltre necessario un file app.config separato per Team Foundation Build se gli unit test devono essere connessi a un database diverso da quello utilizzato durante la creazione del progetto di test. Apportando le modifiche indicate nella procedura successiva, è possibile impostare il progetto di test e il server di compilazione affinché in Team Foundation Build venga utilizzata una configurazione diversa.  
  
> [!IMPORTANT]  
> È necessario eseguire questa procedura per ogni progetto di test (con estensione vbproj o vsproj).  
  
#### <a name="to-specify-an-appconfig-file-for-team-foundation-build"></a>Per specificare un file app.config per Team Foundation Build  
  
1.  Fare clic con il pulsante destro del mouse sul file app.config in **Esplora soluzioni** e scegliere **Copia**.  
  
2.  Fare clic con il pulsante destro del mouse sul progetto di test e scegliere **Incolla**.  
  
3.  Fare clic con il pulsante destro del mouse sul file denominato **Copia di app.config** e scegliere Rinomina.  
  
4.  Digitare _BuildComputer_ **.sqlunitttest.config** e premere INVIO, dove *BuildComputer* è il nome del computer in cui viene eseguito l'agente di compilazione.  
  
5.  Fare doppio clic su *BuildComputer*.sqlunitttest.config.  
  
    Il file di configurazione verrà aperto nell'editor.  
  
6.  Impostare il percorso relativo dei file con estensione sqlproj aggiungendo un livello di cartella per la cartella delle origini e una sottocartella con lo stesso nome della soluzione. Ad esempio, se nel file di configurazione è inclusa inizialmente la voce seguente:  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Aggiornare il file come segue:  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Al termine, il file *BuildComputer*.sqlunitttest.config deve risultare analogo all'esempio seguente per Visual Studio 2010:  
  
    ```  
    <SqlUnitTesting_VS2010>  
        <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
            Configuration="Debug" />  
        <DataGeneration ClearDatabase="true" />  
        <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
        <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
    </SqlUnitTesting_VS2010>  
    ```  
  
    Se invece si usa Visual Studio 2012:  
  
    ```  
    <SqlUnitTesting_VS2012>  
            <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
                Configuration="Debug" />  
            <DataGeneration ClearDatabase="true" />  
            <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
            <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
        </SqlUnitTesting_VS2012>  
    ```  
  
7.  Aggiornare l'attributo ConnectionString per ExecutionContext e PrivilegedContext per specificare le connessioni al database di destinazione in cui si desidera eseguire la distribuzione.  
  
8.  Scegliere **Salva tutti** dal menu **File**.  
  
9. In Esplora soluzioni fare doppio clic sul file app.config.  
  
10. Nell'editor, per ogni nodo \<SqlUnitTesting_*VSVersion*> aggiungere `AllowConfigurationOverride="true"`. Ad esempio:  
  
    ```  
    -- Update SqlUnitTesting_VS2010 node to:  
    <SqlUnitTesting_VS2010 AllowConfigurationOverride="true">   
  
    -- Update SqlUnitTesting_VS2012 node to:  
    <SqlUnitTesting_VS2012 AllowConfigurationOverride="true">  
    ```  
  
    Apportando questa modifica, si consente a Team Foundation Build di utilizzare il file di configurazione di sostituzione che è stato creato.  
  
11. Scegliere **Salva tutti** dal menu **File**.  
  
    Successivamente è necessario aggiornare Local.testsettings per includere il file di configurazione personalizzato.  
  
#### <a name="to-customize-localtestsettings-to-deploy-the-customized-configuration-file"></a>Per personalizzare Local.testsettings per la distribuzione del file di configurazione personalizzato  
  
1.  In Esplora soluzioni fare doppio clic su Local.testsettings.  
  
    Verrà visualizzata la finestra di dialogo **Impostazioni test**.  
  
2.  Nell'elenco delle categorie scegliere **Distribuzione**.  
  
3.  Selezionare la casella di controllo **Abilita distribuzione**.  
  
4.  Fare clic su **Aggiungi file**.  
  
5.  Nella finestra di dialogo **Aggiungi file di distribuzione** specificare il file *BuildComputer*.sqlunitttest.config che è stato creato.  
  
6.  Fare clic su **Applica**.  
  
7.  Fare clic su **Close**.  
  
8.  Scegliere **Salva tutti** dal menu **File**.  
  
    Successivamente è possibile archiviare la soluzione nel controllo delle versioni.  
  
## <a name="check-in-the-solution"></a><a name="CheckInTheTestList"></a>Archiviare la soluzione  
In questa procedura vengono archiviati tutti i file della soluzione. In questi file è incluso il file di metadati di test della soluzione, contenente le associazioni delle categorie di test e i test. Ogni volta che si aggiunge, elimina, riorganizza o modifica il contenuto dei test, il file di metadati di test viene aggiornato automaticamente per riflettere queste modifiche.  
  
> [!NOTE]  
> In questa procedura vengono descritti i passaggi necessari se si utilizza il controllo della versione di Team Foundation. Se si utilizza un software per il controllo delle versioni differente, è necessario seguire i passaggi appropriati corrispondenti.  
  
#### <a name="to-check-in-the-solution"></a>Per archiviare la soluzione  
  
1.  Connettersi a un computer in cui viene eseguito Team Foundation Server.  
  
    Per altre informazioni, vedere l'argomento relativo all'[uso di Esplora controllo codice sorgente](/previous-versions/visualstudio/visual-studio-2010/ms181370(v=vs.100)).  
  
2.  Aggiungere la soluzione al controllo del codice sorgente, se non è già stata inclusa.  
  
    Per altre informazioni, vedere l'argomento relativo all'[aggiunta di un progetto o una soluzione al controllo della versione](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100)).  
  
3.  Fare clic su **Vista**, quindi scegliere **Archiviazioni in sospeso**.  
  
4.  Archiviare tutti i file della soluzione.  
  
    Per altre informazioni, vedere [Archiviare modifiche in sospeso](/previous-versions/visualstudio/visual-studio-2010/ms181411(v=vs.100)).  
  
    > [!NOTE]  
    > È possibile che le modalità di creazione e di gestione dei test automatici siano regolate da un processo di team specifico. Per il processo, ad esempio, si potrebbe richiedere di verificare localmente la compilazione prima di archiviare il codice in questione insieme ai relativi test che verranno eseguiti.  
  
    Accanto a ogni file in **Esplora soluzioni** viene visualizzata l'icona di un lucchetto per indicare che il file è archiviato. Per altre informazioni, vedere l'argomento relativo alla [visualizzazione delle proprietà di file e cartelle del controllo della versione](/previous-versions/visualstudio/visual-studio-2010/ms245468(v=vs.100)).  
  
    I test sono disponibili per Team Foundation Build. A questo punto è possibile creare una definizione di compilazione contenente i test che si desidera eseguire.  
  
## <a name="create-a-build-definition"></a><a name="CreateBuildDef"></a>Creare una definizione di compilazione  
  
#### <a name="to-create-a-build-definition"></a>Per creare una definizione di compilazione  
  
1.  In Team Explorer selezionare il progetto Team, fare clic con il pulsante destro del mouse sul nodo **Compilazioni** e scegliere **Nuova definizione di compilazione**.  
  
    Verrà visualizzata la finestra **Nuova definizione di compilazione**.  
  
2.  In **Nome definizione di compilazione** digitare il nome da usare per la definizione in questione.  
  
3.  Nella barra di navigazione fare clic su **Impostazioni predefinite compilazione**.  
  
4.  In **Copia output di compilazione nella seguente cartella di ricezione (percorso UNC, come \\\server\share)** specificare una cartella in cui copiare l'output di compilazione.  
  
    È possibile specificare una cartella condivisa nel computer locale o in qualsiasi percorso di rete per il quale verranno concesse autorizzazioni al processo di compilazione.  
  
5.  Nella barra di navigazione fare clic su **Processo**.  
  
6.  Nel gruppo **Obbligatorio** di **Elementi da compilare** fare clic sul pulsante Sfoglia (...).  
  
7.  Nella finestra di dialogo **Editor elenco di progetti di compilazione** fare clic su **Aggiungi**.  
  
8.  Specificare il file della soluzione (con estensione sln) aggiunto al controllo delle versioni precedentemente in questa procedura dettagliata e scegliere **OK**.  
  
    La soluzione verrà visualizzata nell'elenco **File di soluzione o progetto da compilare**.  
  
9. Fare clic su **OK**.  
  
10. In **Test automatizzati** nel gruppo **Base** specificare i test da eseguire. Per impostazione predefinita verranno eseguiti i test contenuti nei file denominati \*test\*.dll della soluzione.  
  
11. Nel menu **File** scegliere **Salva** *NomeProgetto*.  
  
    È stata creata una definizione di compilazione. Successivamente è possibile modificare il progetto di test.  
  
## <a name="run-the-new-build-definition"></a><a name="RunBuild"></a>Eseguire la nuova definizione di compilazione  
  
#### <a name="to-run-the-new-build-type"></a>Per eseguire il nuovo tipo di compilazione  
  
1.  In Team Explorer espandere il nodo del progetto Team, espandere il nodo Compilazioni, fare clic con il pulsante destro del mouse sulla definizione di compilazione che si desidera eseguire, quindi scegliere Accoda nuova compilazione.  
  
    Viene visualizzata la finestra di dialogo **Accoda compilazione {** _TeamProjectName_ **}** con un elenco di tutti i tipi di compilazione esistenti.  
  
2.  Se necessario, in **Definizione di compilazione** fare clic sulla nuova definizione di compilazione.  
  
3.  Verificare che i valori nei campi **Definizione di compilazione**, **Agente di compilazione** e **Cartella di ricezione per la compilazione** siano corretti, quindi fare clic su **Accoda**.  
  
    Verrà visualizzata la scheda **In coda** di **Build Explorer**. Per altre informazioni, vedere [Gestire e visualizzare le compilazioni completate (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms181730(v=vs.100)) o [Gestire le compilazioni in Build Explorer (Visual Studio 2012)](/previous-versions/ms181732(v=vs.140)).  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione di unit test di SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Creare una definizione di compilazione di base](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
[Accodare una compilazione](/previous-versions/visualstudio/visual-studio-2010/ms181722(v=vs.100))  
[Monitorare lo stato di una compilazione in esecuzione](/previous-versions/visualstudio/visual-studio-2010/ms181724(v=vs.100))  
