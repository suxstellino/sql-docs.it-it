---
title: Navigazione di base in Ottimizzazione guidata motore di database
description: Ottimizzazione guidata motore di database (DTA, Database Engine Tuning Advisor) offre una modalità basata su un'interfaccia utente grafica per visualizzare le sessioni di ottimizzazione e i report delle raccomandazioni di ottimizzazione.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
ms.assetid: ad49b2e0-a5e3-49d2-80fd-9f4eaa3652cb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.openlocfilehash: e8c4f32eab8fa7d7791a5f4718d8d771cb1dd6cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354474"
---
# <a name="lesson-1-basic-navigation-in-database-engine-tuning-advisor-dta"></a>Lezione 1: Navigazione di base in Ottimizzazione guidata motore di database (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Ottimizzazione guidata motore di database offre una modalità basata su un'interfaccia utente grafica per visualizzare le sessioni di ottimizzazione e i report delle indicazioni di ottimizzazione. In questa lezione viene illustrato come avviare lo strumento e configurare la visualizzazione. Questa lezione consente di apprendere i diversi metodi per avviare lo strumento e per configurare la visualizzazione in modo da supportare le attività di ottimizzazione eseguite regolarmente.  

## <a name="prerequisites"></a>Prerequisiti 

Per completare questa esercitazione, sono necessari SQL Server Management Studio, l'accesso a un server che esegue SQL Server e un database AdventureWorks.

- Installare [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare i [database di esempio AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Le istruzioni per il ripristino dei database in SSMS sono disponibili in [Ripristinare un database](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Questa esercitazione è destinata agli utenti che hanno familiarità con l'uso di SQL Server Management Studio e con attività semplici di amministrazione del database. 
  

## <a name="launch-database-tuning-advisor"></a>Avviare Ottimizzazione guidata motore di database 
Per iniziare, aprire l'interfaccia utente grafica (GUI) di Ottimizzazione guidata motore di database. Al primo uso, per inizializzare l'applicazione è necessario che lo strumento Ottimizzazione guidata motore di database sia avviato da un membro del ruolo predefinito del server **sysadmin** . Dopo l'inizializzazione i membri del ruolo predefinito del database **db_owner** possono usare lo strumento Ottimizzazione guidata motore di database per ottimizzare i database di cui sono proprietari. Per altre informazioni sull'inizializzazione di Ottimizzazione guidata motore di database, vedere [Avvio e utilizzo di Ottimizzazione guidata motore di database](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
1. Avviare SQL Server Management Studio (SSMS). Nel menu **Start** di Windows selezionare **Tutti i programmi** e trovare **SQL Server Management Studio**. 
2. Dopo aver aperto SSMS, selezionare il menu **Strumenti** e selezionare **Ottimizzazione guidata motore di database**. 

  ![Avviare DTA da SSMS](media/dta-tutorials/launch-dta.png)

3. Ottimizzazione guidata motore di database viene avviato e viene visualizzata la finestra di dialogo **Connetti al server**. Verificare le impostazioni predefinite e quindi selezionare **Connetti** per connettersi a SQL Server.  
  
Per impostazione predefinita, la configurazione all'avvio dello strumento Ottimizzazione guidata motore di database è quella illustrata nella figura seguente:  
  
![Finestra predefinita di Ottimizzazione guidata motore di database](media/dta-tutorials/dta-default-gui.png)
  
> [!NOTE]  
> La scheda **Monitoraggio sessione** visualizza il nome della sessione, che corrisponde al nome dell'utente connesso e alla data corrente. 
  
Al primo avvio dell'interfaccia utente grafica dello strumento Ottimizzazione guidata motore di database vengono visualizzati due riquadri principali.  
  
-   Il riquadro sinistro contiene Monitoraggio sessione, in cui sono elencate tutte le sessioni di ottimizzazione eseguite sull'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. All'avvio dello strumento Ottimizzazione guidata motore di database, nella parte superiore del riquadro viene visualizzata una nuova sessione. È possibile assegnare un nome alla sessione nel riquadro adiacente. L'elenco contiene inizialmente una sola sessione. Si tratta della sessione predefinita creata automaticamente dallo strumento Ottimizzazione guidata motore di database per impostazione predefinita. Dopo aver eseguito l'ottimizzazione dei database, tutte le sessioni di ottimizzazione per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle quali si è connessi verranno elencate al di sotto della nuova sessione. È possibile fare clic con il pulsante destro del mouse su una sessione di ottimizzazione per rinominarla, chiuderla, eliminarla o clonarla. Se si fa clic con il pulsante destro del mouse nell'elenco è possibile ordinare le sessioni in base al nome, allo stato, alla data e all'ora di creazione oppure creare una nuova sessione. Nella parte inferiore del riquadro vengono visualizzati i dettagli della sessione di ottimizzazione selezionata. È possibile scegliere di visualizzare i dettagli organizzati in categorie usando il pulsante **Per categoria** oppure in un elenco alfabetico usando il pulsante **Per nome** . È inoltre possibile nascondere Monitoraggio sessione trascinando il bordo destro del riquadro verso il lato sinistro della finestra. Per visualizzarlo nuovamente, trascinare il bordo del riquadro verso destra. Monitoraggio sessione consente di visualizzare sessioni di ottimizzazione precedenti e di utilizzarle per la creazione di nuove sessioni con definizioni simili. È inoltre possibile utilizzare Monitoraggio sessione per valutare le indicazioni di ottimizzazione. Per altre informazioni, vedere [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md). Usare il pulsante **Indietro** nel browser per tornare a questa esercitazione.  
  
-   Il riquadro destro contiene le schede **Generale** e **Opzioni di ottimizzazione** . In questo riquadro è possibile definire la sessione di ottimizzazione del Motore di database. Nella scheda **Generale** è possibile digitare un nome per la sessione di ottimizzazione, specificare il file o la tabella del carico di lavoro da usare e selezionare i database e le tabelle che si vuole ottimizzare in questa sessione. Un carico di lavoro è un set di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite sui database che si desidera ottimizzare. Lo strumento Ottimizzazione guidata motore di database utilizza file di traccia, tabelle di traccia, script [!INCLUDE[tsql](../../includes/tsql-md.md)] o file XML come input del carico di lavoro per l'ottimizzazione dei database. Nella scheda **Opzioni di ottimizzazione** è possibile selezionare le strutture di progettazione fisica dei database (indici e viste indicizzate) e la strategia di partizionamento che verrà seguita dallo strumento Ottimizzazione guidata motore di database durante l'analisi. In questa scheda è anche possibile specificare il tempo massimo impiegato da Ottimizzazione guidata motore di database per l'ottimizzazione di un carico di lavoro. Per impostazione predefinita, lo strumento Ottimizzazione guidata motore di database eseguirà l'ottimizzazione di un carico di lavoro in un'ora.  
  
> [!NOTE]
> Lo strumento Ottimizzazione guidata motore di database può usare come input file in formato XML quando uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] viene importato dall'editor di query di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Per altre informazioni, vedere la sezione relativa all'avvio dello strumento Ottimizzazione guidata motore di database dall'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [Avvio e utilizzo di Ottimizzazione guidata motore di database](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
## <a name="configure-tool-options-and-layout"></a>Configurare il layout e le opzioni dello strumento 

1.  Scegliere **Opzioni** dal menu **Strumenti**.  

   ![Opzioni DTA](media/dta-tutorials/dta-settings.png) 
  
2.  Nella finestra di dialogo **Opzioni** visualizzare le opzioni seguenti:  
  
    -   Espandere l'elenco **All'avvio** per mostrare le visualizzazioni possibili all'avvio di Ottimizzazione guidata motore di database. Per impostazione predefinita, è selezionata l'opzione **Mostra una nuova sessione** .  
  
    -   Fare clic su **Modifica carattere** per visualizzare i tipi di carattere disponibili per gli elenchi di database e le tabelle della scheda **Generale** . I caratteri selezionati per questa opzione vengono utilizzati anche nei report e nelle griglie delle indicazioni dello strumento Ottimizzazione guidata motore di database dopo l'esecuzione dell'ottimizzazione. Per impostazione predefinita, Ottimizzazione guidata motore di database utilizza i caratteri di sistema.  
  
    -   È possibile impostare **Numero di elementi negli elenchi degli ultimi elementi utilizzati** tra **1** e **10**. In tal modo viene impostato il numero massimo di elementi visualizzati quando si sceglie **Sessioni recenti** o **File recenti** dal menu **File** . Per impostazione predefinita, questa opzione è impostata su **4**.  
  
    -   Quando l'opzione **Memorizza le ultime opzioni di ottimizzazione specificate** è selezionata, per impostazione predefinita, per la sessione di ottimizzazione successiva, lo strumento Ottimizzazione guidata motore di database usa le opzioni specificate per l'ultima sessione. Deselezionare questa casella di controllo per utilizzare le opzioni di ottimizzazione predefinite di Ottimizzazione guidata motore di database. Per impostazione predefinita, questa opzione è selezionata.  
  
    -   Per impostazione predefinita, l'opzione **Chiedi conferma prima di eliminare definitivamente le sessioni** è selezionata per evitare l'eliminazione accidentale di sessioni di ottimizzazione.  
  
    -   Per impostazione predefinita, l'opzione **Chiedi conferma prima di arrestare l'analisi della sessione** è selezionata, per evitare l'arresto accidentale di una sessione di ottimizzazione prima che Ottimizzazione guidata motore di database abbia concluso l'analisi di un carico di lavoro.  
  
## <a name="next-lesson"></a>Lezione successiva  
[Lezione 2: Uso di Ottimizzazione guidata motore di database](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
  
  
