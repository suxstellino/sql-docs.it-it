---
description: 'Lezione 2-2: Aggiungere e configurare il contenitore Ciclo Foreach'
title: 'Passaggio 2: Aggiungere e configurare il contenitore Ciclo Foreach | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c63288019fa6938f4481bbded2e5b29c29198d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354317"
---
# <a name="lesson-2-2-add-and-configure-the-foreach-loop-container"></a>Lezione 2-2: Aggiungere e configurare il contenitore Ciclo Foreach

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In questa attività verrà aggiunta la capacità di creare un ciclo in una cartella di file flat e applicare la trasformazione del flusso di dati della lezione 1 a ognuno di questi file flat. Ciò si ottiene tramite l'aggiunta e la configurazione di un contenitore Ciclo Foreach al flusso di controllo.  
  
Il contenitore Ciclo Foreach che si aggiunge deve essere in grado di collegarsi a ogni file flat della cartella. Dal momento che tutti i file della cartella hanno lo stesso formato, il contenitore Ciclo Foreach può utilizzare la stessa gestione connessione file flat per la connessione a tali file. La gestione connessione file flat usata dal contenitore è quella creata nella lezione 1.  
  
Al momento, la gestione connessione file flat della lezione 1 si connette a un solo file flat specifico. Per connettersi in modo iterativo a ogni file flat nella cartella, è necessario configurare il contenitore Ciclo Foreach e la gestione connessione file flat come segue:  
  
-   **Contenitore Ciclo Foreach:** il valore enumerato del contenitore verrà mappato a una variabile di pacchetto definita dall'utente. Il contenitore usa poi questa variabile per modificare in modo dinamico la proprietà **ConnectionString** della gestione connessione file flat e connettersi in modo iterativo a ogni file flat nella cartella.  
  
-   **Gestione connessione file flat:** la gestione connessione creata nella lezione 1 viene modificata usando una variabile definita dall'utente per popolare la proprietà **ConnectionString** della gestione connessione.  
  
Le procedure in questa attività mostrano come creare e modificare il contenitore Ciclo Foreach per usare una variabile di pacchetto definita dall'utente e aggiungere l'attività Flusso di dati al ciclo. Si vedrà come modificare la gestione connessione file flat per usare tale variabile definita dall'utente nell'attività successiva.  
  
Dopo aver apportato tali modifiche al pacchetto, quando questo viene eseguito, il contenitore Ciclo Foreach esegue un'iterazione su tutti i file nella cartella Sample Data. Ogni volta che viene individuato un file che corrisponde ai criteri, il contenitore Ciclo Foreach popola la nuova variabile con il nome file, esegue il mapping di tale variabile alla proprietà **ConnectionString** della gestione connessione file flat per i dati della valuta di esempio e quindi esegue il flusso di dati su tale file. In questo modo, in ogni iterazione del Ciclo Foreach, l'attività Flusso di dati utilizza un file flat diverso.  
  
> [!NOTE]  
> Dal momento che [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] separa il flusso di controllo dal flusso dei dati, i cicli aggiunti al flusso di controllo non richiederanno la modifica del flusso di dati. Pertanto, il flusso di dati della lezione 1 non deve essere modificato.  
  
## <a name="add-a-foreach-loop-container"></a>Aggiungere un contenitore Ciclo Foreach  
  
1.  In **SQL Server Data Tools** selezionare la scheda **Flusso di controllo**.  
  
2.  Nella **Casella degli strumenti SSIS** espandere **Contenitori**, quindi trascinare **Contenitore Ciclo Foreach** sulla superficie di progettazione della scheda **Flusso di controllo** .  
  
3.  Fare clic con il pulsante destro del mouse sul nuovo **Contenitore Ciclo Foreach** e scegliere **Modifica**.  
  
4.  Nella pagina **Generale** della finestra di dialogo **Editor ciclo Foreach** immettere **Foreach File in Folder** per **Nome**. Selezionare **OK**.  
  
5.  Fare clic con il pulsante destro del mouse sul contenitore Ciclo Foreach, scegliere **Proprietà** e verificare che la proprietà **LocaleID** sia impostata su **Inglese (Stati Uniti)** nella finestra **Proprietà**.  
  
## <a name="configure-the-enumerator-for-the-foreach-loop-container"></a>Configurare l'enumeratore per il contenitore Ciclo Foreach  
  
1.  Fare doppio clic su **Foreach File in Folder** per riaprire l'**Editor ciclo Foreach**.  
  
2.  Selezionare **Raccolta**.  
  
3.  Nella pagina **Raccolta** selezionare **Enumeratore Foreach File**.  
  
4.  Nel gruppo **Configurazione enumeratore** selezionare **Sfoglia**.  
  
5.  Nella finestra di dialogo **Sfoglia per cartelle** individuare nel computer la cartella contenente i file Currency_*.txt inclusi con i dati di esempio.

6.  Nella casella **File** immettere **Currency_\*.txt**.  
  
## <a name="map-the-enumerator-to-a-user-defined-variable"></a>Eseguire il mapping dell'enumeratore a una variabile definita dall'utente  
  
1.  Selezionare **Mapping variabili**.  
  
2.  Nella colonna **Variabile** della pagina **Mapping variabili** selezionare la cella vuota e selezionare **\<New Variable...>** .  
  
3.  Nella finestra di dialogo **Aggiungi variabile** immettere **varFileName** per **Nome**.  
  
    > [!NOTE]  
    > Per i nomi delle variabili viene fatta distinzione tra maiuscole e minuscole.  
  
4.  Selezionare **OK**.  
  
5.  Selezionare di nuovo **OK** per chiudere la finestra di dialogo **Editor ciclo Foreach**.  
  
## <a name="add-the-data-flow-task-to-the-loop"></a>Aggiungere l'attività Flusso di dati al ciclo  
  
-   Trascinare l'attività Flusso di dati **Extract Sample Currency Data** nel contenitore Ciclo Foreach denominato **Foreach File in Folder**.  
  
## <a name="go-to-next-task"></a>Esecuzione del passaggio successivo  
[Passaggio 3: Modificare la gestione connessione file flat](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Vedere anche  
[Configurare un contenitore Ciclo Foreach](./control-flow/foreach-loop-container.md)  
[Usare variabili nei pacchetti](./integration-services-ssis-variables.md)  
  
  
