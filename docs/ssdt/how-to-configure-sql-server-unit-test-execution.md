---
title: Configurare l'esecuzione di unit test di SQL Server
description: Informazioni su come configurare unit test di SQL Server. Scoprire come specificare le stringhe di connessione e come distribuire uno schema di database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: e0179429-13ce-4d23-ae27-e6419de0a575
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 569262aece2d8b1212704c0b2ba443c6c63f2b07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018411"
---
# <a name="how-to-configure-sql-server-unit-test-execution"></a>Procedura: Configurare l'esecuzione di unit test di SQL Server

Con la configurazione del progetto di test è possibile specificare diverse impostazioni tramite cui vengono controllati gli aspetti della modalità di esecuzione degli unit test di SQL Server. Queste impostazioni di configurazione vengono archiviate nel file app.config del progetto di test. Se si modifica direttamente il file, i nuovi valori vengono visualizzati nella finestra di dialogo relativa alla configurazione di test.  
  
La soluzione può contenere più progetti di test. Ogni progetto di test contiene un file app.config, ovvero un set di impostazioni di configurazione. Pertanto nella soluzione possono essere contenuti diversi set di unit test (uno per ogni progetto di test) configurati per essere eseguiti in modo diverso.  
  
Tramite queste impostazioni vengono controllate la modalità di connessione del test al database che verrà testato, nonché la modalità di distribuzione di uno schema da un progetto di database al database in questione:  
  
-   **Connessioni di database**. Utilizzare questa impostazione per specificare le stringhe di connessione utilizzate per connettersi al database su cui si esegue il test. Per altre informazioni, vedere [Specificare stringhe di connessione](#SpecifyConnectionStrings)  
  
-   **Distribuzione dello schema**. Un progetto di database è una rappresentazione offline del database. Il progetto di database rappresenta la struttura degli oggetti di database, ma non contiene dati. Dopo aver apportato le modifiche allo schema in un progetto di database, è possibile eseguirne il test in un database effettivo. Nel passaggio relativo alla distribuzione dello schema gli oggetti di database che si desidera testare vengono copiati dal progetto di database nel database su cui si eseguono i test. Per altre informazioni sulla distribuzione dello schema, vedere [Distribuire uno schema di database](#DeployingDBSchema).  
  
    > [!NOTE]  
    > I test non vengono eseguiti nella cartella della soluzione, ma in una cartella distinta sul disco rigido locale. Sebbene sia possibile configurare gli aspetti della distribuzione di test, in genere questa operazione non è necessaria per gli unit test. Per altre informazioni sulla distribuzione di test, vedere [Esecuzione di test](/previous-versions/visualstudio/visual-studio-2010/dd286680(v=vs.100)).  
  
## <a name="specify-connection-strings"></a><a name="SpecifyConnectionStrings"></a>Specificare stringhe di connessione  
  
#### <a name="to-specify-database-connection-strings"></a>Per specificare stringhe di connessione al database  
  
1.  Fare clic con il pulsante destro del mouse sul progetto di unit test in **Esplora soluzioni** e scegliere **Configurazione test di SQL Server**.  
  
    Verrà visualizzata la finestra di dialogo **Configurazione test di SQL Server -'<projectname>'** .  
  
2.  In **Connessioni database** è possibile effettuare le operazioni seguenti:  
  
    -   Fare clic sulla connessione al database su cui si desidera eseguire gli unit test.  
  
    -   Selezionare la casella di controllo **Utilizza una connessione dati secondaria per convalidare gli unit test** e fare clic su una connessione al database nell'elenco se si vuole che l'esecuzione del test venga convalidata su una connessione al database diversa.  
  
    -   Fare clic su **Nuova connessione** per aggiungere una connessione a qualsiasi elenco. È inoltre possibile fare clic su **Modifica connessione** per modificare le impostazioni in una connessione esistente.  
  
    Con questo passaggio viene creata la stringa di connessione `ExecutionContext`, utilizzata per eseguire lo script di test nello unit test. Se si specifica anche una connessione secondaria, viene creata anche la stringa di connessione `PrivilegedContext`. Questa connessione viene utilizzata per testare le interazioni con il database all'esterno dello script di test nello unit test. Per altre informazioni, vedere [Panoramica delle stringhe di connessione e delle autorizzazioni](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
3.  Fare clic su **OK** per chiudere la finestra di dialogo **Configurazione test di SQL Server -'<projectname>'** .  
  
4.  Ricompilare il progetto di test per applicare le modifiche alla configurazione.  
  
## <a name="deploy-a-database-schema"></a><a name="DeployingDBSchema"></a>Distribuire uno schema di database  
  
#### <a name="to-deploy-to-a-database-the-schema-of-a-database-project"></a>Per distribuire a un database lo schema di un progetto di database  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto di database, quindi scegliere **Compila**.  
  
    Quando si compila il progetto di database, si genera uno script Transact\-SQL. Quando si esegue questo script su un database, viene ricreata la struttura del progetto di database in tale database.  
  
2.  Selezionare il progetto di test che si desidera configurare.  
  
3.  Fare clic con il pulsante destro del mouse sul progetto di unit test in **Esplora soluzioni** e scegliere **Configurazione test di SQL Server**.  
  
    Verrà visualizzata la finestra di dialogo **Configurazione test di SQL Server -'<projectname>'** .  
  
4.  In **Distribuzione** è possibile effettuare le operazioni seguenti:  
  
    -   Selezionare la casella di controllo **Distribuisci automaticamente i progetti di database prima dell'esecuzione degli unit test** per assicurarsi che per tutte le modifiche dello schema apportate al progetto di database venga eseguito il commit prima dell'esecuzione dei test.  
  
    -   In **Progetto di database** fare clic sul progetto di database da distribuire oppure fare clic sui puntini di sospensione per individuare un altro progetto. I file del progetto di database hanno l'estensione dbproj.  
  
    -   In **Configurazione distribuzione** fare clic sulla configurazione del progetto da usare per la distribuzione. Le scelte disponibili sono **Debug**, **Predefinito** o **Release**. Se tuttavia si crea una configurazione per eseguire gli unit di test, la configurazione viene visualizzata anche come opzione.  
  
5.  Fare clic su **OK** per chiudere la finestra di dialogo **Configurazione test di SQL Server -'<projectname>'** .  
  
    All'inizio dell'esecuzione del test, viene eseguito lo script Transact\-SQL generato nel passaggio 1. Tramite questa operazione viene distribuito lo schema nel database di destinazione.  
  
6.  Ricompilare il progetto di unit test per applicare le modifiche alla configurazione.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
