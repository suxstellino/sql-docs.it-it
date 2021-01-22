---
description: 'Lezione 3-2: Aggiungere e configurare le funzionalità di registrazione'
title: 'Passaggio 2: Aggiungere e configurare le funzionalità di registrazione | Microsoft Docs'
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 56105f3f-e500-4669-8c8e-acf434527727
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 32daaf012159bd42a6f73330bc8371157e459609
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125619"
---
# <a name="lesson-3-2-add-and-configure-logging"></a>Lezione 3-2: Aggiungere e configurare le funzionalità di registrazione

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In questa attività viene abilitata la registrazione per il flusso di dati del pacchetto Lesson 3.dtsx. In seguito viene configurato un provider di log per file di testo in modo da registrare gli eventi PipelineExecutionPlan e PipelineExecuteTrees. Il provider di log per file di testo crea log facili da visualizzare e spostare. La semplicità di questi file di log li rende utili nella fase di test di base di un pacchetto. Le voci di log possono essere visualizzate anche nella finestra **Registra eventi** di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)].  
  
## <a name="add-logging-to-the-package"></a>Aggiungere le funzionalità di registrazione al pacchetto  
  
1.  Scegliere **Registrazione** dal menu **SSIS**.  
    In Visual Studio 2019 il menu **SSIS** si trova in Estensioni-> SSIS. Verificare che sia selezionata la scheda **Flusso di dati** e non **Flusso di controllo**
  
2.  Nel riquadro **Contenitori** della finestra di dialogo **Configura log SSIS** verificare che sia selezionato l'oggetto in prima posizione. Tale oggetto rappresenta il pacchetto della lezione 3.
  
3.  Nella casella **Tipo provider** della scheda **Provider e log** selezionare **Provider di log SSIS per file di testo** e quindi **Aggiungi**.  
  
    Integration Services consente di aggiungere un nuovo provider di log per file di testo al pacchetto con il nome predefinito **Provider di log SSIS per file di testo**. È ora possibile configurare il nuovo provider di log.  
  
4.  Nella colonna **Nome** digitare **Lesson 3 Log File**.  
  
5.  Facoltativamente, modificare la **Descrizione**.  
  
6.  Nella colonna **Configurazione** selezionare **\<New Connection>** per specificare dove [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] scrive le informazioni sul log.  
  
    Nella finestra di dialogo **Editor gestione connessione file** per **Tipo di utilizzo** selezionare **Crea file** e quindi **Sfoglia**. Per impostazione predefinita, la finestra di dialogo **Seleziona file** apre la cartella di progetto; è tuttavia possibile salvare le informazioni sulla registrazione in qualsiasi posizione.  
  
7.  Nella casella **Nome file** della finestra di dialogo **Seleziona file** digitare **TutorialLog.log** e selezionare **Apri**.
  
8.  Selezionare **OK** per chiudere la finestra di dialogo **Editor gestione connessione file**.  
  
9. Nel riquadro **Contenitori** espandere tutti i nodi della gerarchia del contenitore del pacchetto e quindi deselezionare tutte le caselle di controllo, compresa **Extract Sample Currency Data** . Selezionare quindi la casella di controllo **Extract Sample Currency Data** per ottenere solo gli eventi relativi a tale nodo.  
  
    > [!NOTE]  
    > Se la casella di controllo **Extract Sample Currency Data** viene visualizzata come non disponibile anziché selezionata, l'attività usa le impostazioni log del contenitore padre e non è possibile attivare gli eventi specifici dell'attività. Per risolvere questa istanza, deselezionare la casella di controllo padre.
  
10. Nella colonna **Eventi** della scheda **Dettagli** selezionare gli eventi **PipelineExecutionPlan** e **PipelineExecutionTrees** .  
  
11. Selezionare **Avanzate** per rivedere i dettagli scritti dal provider di log per ogni evento. Per impostazione predefinita, tutte le categorie delle informazioni vengono automaticamente selezionate per gli eventi specificati.  
  
12. Selezionare **Base** per nascondere le categorie delle informazioni.  
  
13. Nella colonna **Nome** della scheda **Provider e log** selezionare **Lesson 3 Log File**. Dopo aver creato un provider di log per il pacchetto, è possibile disabilitare la registrazione senza la necessità di eliminarlo e ricrearlo.  
  
14. Selezionare **OK**.  
  
## <a name="go-to-next-task"></a>Esecuzione del passaggio successivo  
[Passaggio 3: Testare il pacchetto della lezione 3](../integration-services/lesson-3-3-testing-the-lesson-3-tutorial-package.md)  
  
