---
title: Creare un piano di manutenzione con l'area di progettazione
description: Informazioni su come creare un piano di manutenzione multiserver o di un singolo server usando l'area di progettazione del piano di manutenzione in SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Maintenance Plan Design Surface
ms.assetid: 2ef803ee-a9f8-454a-ad63-fedcbe6838d1
author: MikeRayMSFT
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 931aa85fbb37d8dba1c9bb3e06e00760d3f16caf
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049157"
---
# <a name="create-a-maintenance-plan-maintenance-plan-design-surface"></a>Creare un piano di manutenzione (area di progettazione del piano di manutenzione)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come creare un piano di manutenzione multiserver o di un singolo server utilizzando l'area di progettazione del piano di manutenzione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Mentre la **Creazione guidata piano di manutenzione** è ideale per la creazione di piani di manutenzione di base, la creazione di un piano tramite l'area di progettazione consente di utilizzare un flusso di lavoro avanzato.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   [Creazione di un piano di manutenzione tramite l'area di progettazione del piano di manutenzione](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Per creare un piano di manutenzione multiserver, è necessario configurare un ambiente multiserver composto da un server master e uno o più server di destinazione. I piani di manutenzione multiserver devono essere creati e gestiti nel server master. Questi piani possono essere visualizzati, ma non gestiti, nei server di destinazione.  
  
-   I membri dei ruoli **db_ssisadmin** e **dc_admin** possono essere in grado di elevare i privilegi a **sysadmin**. Questa elevazione dei privilegi può verificarsi perché tali ruoli possono modificare i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I pacchetti possono essere eseguiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il contesto di sicurezza **sysadmin** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per impedire questa elevazione dei privilegi durante l'esecuzione di piani di manutenzione, set di raccolta dati e altri pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurare i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che eseguono pacchetti in modo che usino un account proxy con privilegi limitati o aggiungere solo i membri **sysadmin** ai ruoli **db_ssisadmin** e **dc_admin** .  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per creare o gestire piani di manutenzione, è necessario essere membro del ruolo predefinito del server **sysadmin** . In Esplora oggetti il nodo **Piani di manutenzione** viene visualizzato solo per gli utenti membri del ruolo predefinito del server **sysadmin** .  
  
##  <a name="using-maintenance-plan-design-surface"></a><a name="SSMSProcedure"></a> Utilizzo dell'area di progettazione del piano di manutenzione  
  
#### <a name="to-create-a-maintenance-plan"></a>Per creare un piano di manutenzione  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il server in cui si desidera creare un piano di manutenzione.  
  
2.  Fare clic sul segno più per espandere la cartella **Gestione** .  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Piani di manutenzione** e scegliere **Nuovo piano di manutenzione**.  
  
4.  Nella finestra di dialogo **Nuovo piano di manutenzione** digitare un nome per il piano nella casella **Nome** , quindi fare clic su **OK**. Verranno visualizzate la casella degli strumenti e l'area *nome_piano_manutenzione* **[Progettazione]** con il sottopiano **Sottopiano_1** creato nella griglia principale.  
  
     Nell'intestazione dell'area di progettazione sono disponibili le opzioni seguenti.  
  
     **Aggiungi sottopiano**  
     Consente di aggiungere un sottopiano configurabile.  
  
     **Proprietà sottopiano**  
     Consente di visualizzare la finestra di dialogo **Proprietà sottopiano** per il sottopiano selezionato nella griglia principale. In alternativa, è possibile fare doppio clic su un sottopiano nella griglia per visualizzare la finestra di dialogo **Proprietà sottopiano** . Per ulteriori informazioni su questa finestra di dialogo, vedere di seguito.  
  
     **Elimina sottopiano selezionato**  
     Consente di eliminare il sottopiano selezionato.  
  
     **Pianificazione sottopiano**  
     Consente di visualizzare la finestra di dialogo **Nuova pianificazione processo** per il sottopiano selezionato.  
  
     **Rimuovi pianificazione**  
     Consente di rimuovere una pianificazione dal sottopiano selezionato.  
  
     **Gestire le connessioni**  
     Consente di visualizzare la finestra di dialogo **Gestisci connessioni** . Questa finestra di dialogo viene utilizzata per aggiungere ulteriori connessioni a istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al piano di manutenzione. Per ulteriori informazioni su questa finestra di dialogo, vedere di seguito.  
  
     **Report e registrazione**  
     Consente di visualizzare la finestra di dialogo **Report e registrazione** . Per ulteriori informazioni su questa finestra di dialogo, vedere di seguito.  
  
     **Server**  
     Visualizza la finestra di dialogo **Server** usata per selezionare i server in cui verranno eseguite le attività del sottopiano. Questa opzione è abilitata solo nei server master in ambienti multiserver. Per altre informazioni, vedere [Creazione di un ambiente multiserver](../../ssms/agent/create-a-multiserver-environment.md) e [Piano di manutenzione & #40;Server& #41;](../../relational-databases/maintenance-plans/maintenance-plan-servers.md).  
  
     **Nome**  
     Consente di visualizzare il nome del piano di manutenzione. Il nome dei nuovi piani di manutenzione viene indicato in una finestra di dialogo visualizzata prima dell'apertura della finestra di progettazione dei piani di manutenzione. Per rinominare un piano di manutenzione, fare clic con il pulsante destro del mouse sul piano in Esplora oggetti e quindi scegliere **Rinomina**.  
  
     **Descrizione**  
     Consente di visualizzare o specificare una descrizione per il piano di manutenzione. La lunghezza massima consentita per una descrizione è 512 caratteri.  
  
     **Area di progettazione**  
     Consente di progettare e gestire i piani di manutenzione. Utilizzare l'area di progettazione per aggiungere attività di manutenzione a un piano, rimuovere attività da un piano, specificare collegamenti di precedenza tra le attività e indicare le diramazioni e i parallelismi delle attività.  
  
     Un collegamento di precedenza tra due attività stabilisce una relazione tra le attività. La seconda attività, ovvero *l'attività dipendente*, viene eseguita solo se il risultato dell'esecuzione della prima attività, ovvero *l'attività precedente*, soddisfa i criteri specificati. In genere, il risultato dell'esecuzione è **Esito positivo**, **Esito negativo** o **Completamento**. Per ulteriori informazioni, vedere il passaggio **8** di seguito.  
  
5.  Nell'intestazione dell'area di progettazione fare doppio clic su **Sottopiano_1** e immettere un nome e una descrizione per il sottopiano nella finestra di dialogo **Proprietà sottopiano** .  
  
     Nella finestra di dialogo **Proprietà sottopiano** sono disponibili le opzioni seguenti.  
  
     **Nome**  
     Nome del sottopiano.  
  
     **Descrizione**  
     Breve descrizione del sottopiano.  
  
     **Pianificare**  
     Indica la pianificazione in base alla quale verrà eseguito il sottopiano. Fare clic su **Pianificazione sottopiano** per aprire la finestra di dialogo **Nuova pianificazione processo** . Fare clic su **Rimuovi pianificazione** per eliminare la pianificazione dal sottopiano.  
  
     Elenco **Esegui come**  
     Consente di selezionare l'account da utilizzare per l'esecuzione di questa sottoattività.  
  
6.  Fare clic su **Pianificazione sottopiano** per immettere i dettagli della pianificazione nella finestra di dialogo **Nuova pianificazione processo** .  
  
7.  Per compilare il sottopiano, trascinare elementi del flusso di attività dalla **Casella degli strumenti** all'area di progettazione del piano. Fare doppio clic sulle attività per aprire le finestre di dialogo per la configurazione delle relative opzioni.  
  
     Nella **Casella degli strumenti** sono disponibili le attività del piano di manutenzione seguenti:  
  
    -   **Attività Backup database**  
  
    -   **Attività Controlla integrità database**  
  
    -   **Attività Esegui processo di SQL Server Agent**  
  
    -   **Attività Esegui istruzione T-SQL**  
  
    -   **Attività Pulizia contenuto cronologia**  
  
    -   **Attività Pulizia file manutenzione**  
  
    -   **Attività Notifica operatori**  
  
    -   **Attività Ricompila indice**  
  
    -   **Attività Riorganizza indice**  
  
    -   **Attività Compatta database**  
  
    -   **Attività Aggiorna statistiche**  
  
     Per aggiungere attività alla **Casella degli strumenti**:  
  
    1.  Scegliere **Scegli elementi della Casella degli strumenti** dal menu **Strumenti**.  
  
    2.  Selezionare gli strumenti che si desidera visualizzare nella **Casella degli strumenti**, quindi fare clic su **OK**.  
  
     Le attività del piano di manutenzione aggiunte alla **Casella degli strumenti** vengono rese disponibili anche nella **Creazione guidata piano di manutenzione**. Per altre informazioni sulle singole attività illustrate in precedenza, vedere [Utilizzo della Creazione guidata piano di manutenzione](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure) nell'argomento relativo alla **Creazione guidata piano di manutenzione**.  
  
8.  Per definire un flusso di lavoro tra attività:  
  
    1.  Fare clic con il pulsante destro del mouse sull'attività precedente e scegliere **Aggiungi vincolo di precedenza**.  
  
    2.  Nella finestra di dialogo **Flusso di controllo** selezionare l'attività dipendente nell'elenco **A** , quindi fare clic su **OK**.  
  
    3.  Fare doppio clic sul connettore tra le due attività per aprire la finestra di dialogo **Editor vincoli di precedenza** .  
  
         Nella finestra di dialogo **Editor vincoli di precedenza** sono disponibili le opzioni seguenti.  
  
         **Opzioni vincolo**  
         Consente di definire il funzionamento di un vincolo tra due attività.  
  
         Elenco **Operazione valutazione**  
         Consente di specificare l'operazione di valutazione utilizzata dal vincolo di precedenza. Le operazioni sono: **Vincolo**, **Espressione**, **Espressione e vincolo** e **Espressione o vincolo**.  
  
         Elenco **Valore**  
         Specificare il valore del vincolo: **Esito positivo**, **Esito negativo** o **Completamento**. Il valore predefinito è **Esito positivo** .  
  
        > [!NOTE]  
        >  La riga del vincolo di precedenza è verde in caso di **Esito positivo**, rossa in caso di **Esito negativo** e blu in caso di **Completamento**.  
  
         **Espressione**  
         Se si usano le operazioni **Espressione**, **Espressione e vincolo** oppure **Espressione o vincolo**, digitare un'espressione. L'espressione deve restituire un valore booleano.  
  
         **Test**  
         Consente di convalidare l'espressione.  
  
         **Più vincoli**  
         Consente di definire l'interoperabilità di più vincoli e di controllare l'esecuzione dell'attività vincolata.  
  
         **AND logico**  
         Selezionare questa opzione per specificare che devono essere valutati contemporaneamente più vincoli di precedenza nello stesso file eseguibile. Tutti i vincoli devono restituire True. Questa opzione è l'impostazione predefinita.  
  
        > [!NOTE]  
        >  Questo tipo di vincolo di precedenza viene visualizzato come riga di colore verde, rosso o blu continua.  
  
         **OR logico**  
         Selezionare questa opzione per specificare che devono essere valutati contemporaneamente più vincoli di precedenza nello stesso file eseguibile. Almeno un vincolo deve restituire True.  
  
        > [!NOTE]  
        >  Questo tipo di vincolo di precedenza viene visualizzato come riga di colore verde, rosso o blu tratteggiata.  
  
9. Per aggiungere un altro sottopiano contenente attività eseguite in base a una pianificazione diversa, fare clic su **Aggiungi sottopiano** sulla barra degli strumenti per aprire la finestra di dialogo **Proprietà sottopiano** .  
  
10. Per aggiungere connessioni a server diversi:  
  
    1.  Nella barra degli strumenti dell'area di progettazione fare clic su **Gestisci connessioni**.  
  
    2.  Nella finestra di dialogo **Gestisci connessioni** fare clic su **Aggiungi**.  
  
    3.  Nella casella **Nome connessione** nella finestra di dialogo **Proprietà connessione** immettere il nome della connessione che si desidera creare.  
  
    4.  Nella finestra di dialogo **Selezionare o immettere il nome di un server** in **Specificare le informazioni seguenti per connettersi ai dati di SQL Server** immettere il nome del server SQL che si intende usare o fare clic sui puntini di sospensione **(...)** e selezionare un server nella finestra di dialogo **SQL Server**. Se si seleziona un server nella finestra di dialogo **SQL Server** , fare clic su **OK**.  
  
    5.  In **Immettere le informazioni per l'accesso al server** selezionare **Usa sicurezza integrata di Windows NT** o **Usa nome utente e password specifici**. Se si sceglie di utilizzare un nome utente e una password specifici, immettere tali informazioni rispettivamente nelle caselle **Nome utente** e **Password** .  
  
    6.  Nella finestra di dialogo **Proprietà connessione** fare clic su **OK**.  
  
    7.  Nella finestra di dialogo **Gestisci connessioni** fare clic su **Chiudi**.  
  
11. Per specificare le opzioni dei report:  
  
    1.  Nella barra degli strumenti dell'area di progettazione fare clic su **Report e registrazione**.  
  
    2.  Nella finestra di dialogo **Report e registrazione** selezionare **Genera report in un file di testo**, **Invia report a destinatario di posta elettronica** o entrambe le opzioni in **Report** .  
  
        1.  Se si seleziona **Genera report in un file di testo**, selezionare **Crea nuovo file** o **Accoda a file**.  
  
        2.  A seconda della selezione effettuata nel passaggio precedente, immettere il nome e il percorso completo del nuovo file o del file da accodare nella casella **Cartella** o **Nome file** . In alternativa, fare clic sui puntini di sospensione **(...)** e selezionare il percorso della cartella o il nome del file dalla finestra di dialogo **Individua cartella -** _nome\_server_ o **Individua file di database -** _nome\_server_.  
  
        3.  Se si seleziona **Invia report a destinatario di posta elettronica**, nell'elenco **Operatore agente** selezionare il destinatario del report da inviare tramite posta elettronica.  
  
            > [!NOTE]  
            >  È necessario configurare SQL Server Agent per l'utilizzo di Posta elettronica database per inviare il report tramite posta elettronica. Per altre informazioni, vedere [Configure SQL Server Agent Mail to Use Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
    3.  Per salvare informazioni più dettagliate, in **Registrazione** selezionare **Registra informazioni estese**.  
  
    4.  Per scrivere le informazioni relative ai risultati del piano di manutenzione in un altro server, selezionare **Registra in server remoto** e quindi selezionare una connessione server nell'elenco **Connessione** oppure fare clic su **Nuovo** e immettere le informazioni sulla connessione nella finestra di dialogo **Proprietà connessione** .  
  
    5.  Nella finestra di dialogo **Report e registrazione** fare clic su **OK**.  
  
12. Per visualizzare i risultati nel visualizzatore file di log, in **Esplora oggetti** fare clic con il pulsante destro del mouse sulla cartella **Piani di manutenzione** o sul piano di manutenzione specifico e selezionare **Visualizza cronologia**.  

     Nella finestra di dialogo **Visualizzatore file di log -** _nome\_server_ sono disponibili le opzioni seguenti.  
  
     **Carica log**  
     Consente di aprire una finestra di dialogo in cui è possibile specificare un file di log da caricare.  
  
     **Export**  
     Consente di aprire una finestra di dialogo in cui è possibile esportare in un file di testo le informazioni visualizzate nella griglia **Riepilogo file di log** .  
  
     **Aggiorna**  
     Consente di aggiornare la visualizzazione dei log selezionati. Il pulsante **Aggiorna** consente di leggere nuovamente i log selezionati dal server di destinazione applicando qualsiasi impostazione di filtro.  
  
     **Filter**  
     Consente di aprire una finestra di dialogo in cui è possibile specificare le impostazioni usate per filtrare il file di log, ad esempio **Connessione**, **Data** o altri criteri di filtro **generali** .  
  
     **Ricerca**  
     Consente di cercare testo specifico nel file di log. La ricerca con caratteri jolly non è supportata.  
  
     **Stop**  
     Consente di arrestare il caricamento delle voci del file di log. È ad esempio possibile utilizzare questa opzione se il caricamento di un file di log remoto o offline richiede parecchio tempo e si desidera visualizzare solo le voci più recenti.  
  
     **Riepilogo file di log**  
     Consente di visualizzare un riepilogo dei filtri del file di log. Se non è stato applicato alcun filtro al file, verrà visualizzato il testo **Nessun filtro applicato**. Se è stato applicato un filtro al log, verrà visualizzato il testo **Filtra voci del log in cui:** \<filter criteria>.  
  
     **Data**  
     Visualizza la data dell'evento.  
  
     **Origine**  
     Consente di visualizzare la funzionalità di origine da cui è stato creato l'evento, ad esempio il nome del servizio, come MSSQLSERVER. Questa opzione non viene visualizzata per tutti i tipi di log.  
  
     **Messaggio**  
     Consente di visualizzare i messaggi associati all'evento.  
  
     **Tipo log**  
     Consente di visualizzare il tipo di log cui appartiene l'evento. Tutti i log selezionati vengono visualizzati nella finestra di riepilogo dei log.  
  
     **Origine log**  
     Visualizza una descrizione del log di origine in cui viene acquisito l'evento.  
  
     **Dettagli riga selezionata**  
     Consente di selezionare una riga di evento nella parte inferiore della pagina per visualizzare dettagli aggiuntivi sulla riga. È possibile riordinare le colonne trascinandole su nuove posizioni all'interno della griglia. Le colonne possono inoltre essere ridimensionate trascinando verso destra o verso sinistra le corrispondenti barre di separazione nell'intestazione della griglia. Per adattare automaticamente le dimensioni della colonna al contenuto, fare doppio clic sulle barre di separazione nell'intestazione della griglia.  
  
     **Istanza**  
     Nome dell'istanza in cui si è verificato l'evento. Viene visualizzato come *nome computer*\\*nome istanza*.  
  
  
