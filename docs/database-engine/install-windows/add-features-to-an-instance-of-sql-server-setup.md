---
title: Aggiungere funzionalità a un'istanza di SQL Server (programma di installazione)
description: Questo articolo offre una procedura dettagliata per l'aggiunta di funzionalità specifiche dell'istanza in un'istanza di SQL Server 2019.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- " SQL Server, features"
- adding features to  SQL Server
ms.assetid: 97931fdc-d943-48dd-81b9-ae8b8d2c6dad
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 09/07/2019
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2f756136657d20a694cbaa2263500624eeaf8c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347871"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>Aggiungere funzionalità a un'istanza di SQL Server (programma di installazione)

[!INCLUDE [ SQL Server - Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Questo articolo offre una procedura dettagliata per aggiungere funzionalità a un'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Alcuni componenti o servizi di SQL Server sono specifici di un'istanza di SQL Server. Essi sono anche noti come specifici dell'istanza. Condividono la stessa versione dell'istanza che li ospita e vengono utilizzati esclusivamente per quell'istanza. È possibile aggiungere i componenti specifici dell'istanza a un'istanza di SQL Server, insieme ai componenti condivisi, se non sono già installati. Per un elenco delle funzionalità supportate dalle edizioni di SQL Server, vedere [Edizioni e funzionalità supportate per SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) o [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

Per aggiungere funzionalità a un'istanza di SQL Server dal prompt dei comandi, vedere [Installare SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md).

## <a name="prerequisites"></a>Prerequisiti

Prima di continuare, vedere gli articoli in [Pianificazione di un'installazione di SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).

> [!NOTE]
> Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa SQL Server da una condivisione remota, è necessario usare un account di dominio che abbia le autorizzazioni di lettura per la condivisione remota.  
  
> [!NOTE]
> Quando si aggiungono funzionalità a un'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], le impostazioni delle segnalazioni relative all'utilizzo delle funzionalità esistenti vengono applicate alle funzionalità aggiunte. Per modificare queste impostazioni, usare lo strumento **Segnalazione errori e utilizzo funzionalità di SQL Server**, disponibile nel menu **Strumenti di configurazione** di SQL Server.

## <a name="procedures"></a>Procedure

#### <a name="to-add-features-to-an-instance-of-sscurrent"></a>Per aggiungere funzionalità a un'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

1. Inserire il supporto di installazione di SQL Server. Nella cartella radice fare doppio clic sul file setup.exe. Per eseguire l'installazione da una condivisione di rete, accedere alla cartella radice nella condivisione, quindi fare doppio clic sul file setup.exe. Se viene visualizzata la finestra di dialogo del programma di installazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], selezionare **OK** per installare i prerequisiti, quindi selezionare **Annulla** per uscire dall'installazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  

2. L'Installazione guidata consentirà di avviare il Centro installazione SQL Server. Per aggiungere una nuova funzionalità in un'istanza esistente di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], selezionare **Installazione** nell'area di navigazione di sinistra, quindi selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

3. Controllo configurazione sistema consentirà di eseguire un'operazione di individuazione nel computer. Per visualizzare i dettagli della verifica, selezionare **Visualizza dettagli**. Per continuare, selezionare **OK**.

4. Nella pagina Aggiornamenti prodotto vengono visualizzati gli aggiornamenti di prodotto più recenti disponibili per SQL Server. Se non si desidera includere gli aggiornamenti, deselezionare la casella di controllo **Includi aggiornamenti prodotti SQL Server**. Se non viene individuato alcun aggiornamento del prodotto, il programma di installazione di SQL Server non visualizza questa pagina e viene aperta automaticamente la pagina **Installazione dei file di installazione**.

5. Nella pagina Installa i file di installazione viene mostrato lo stato di avanzamento del download, dell'estrazione e dell'installazione dei file di installazione. Se viene trovato un aggiornamento del programma di installazione di SQL Server e ne viene specificata l'inclusione, verrà installato anche questo aggiornamento. Selezionare **Installa** per installare i file di supporto per l'installazione.  

6. Controllo configurazione sistema consentirà di verificare lo stato del sistema del computer prima che l'installazione continui.  

7. Nella pagina Tipo di installazione selezionare l'opzione **Aggiungi funzionalità a un'istanza esistente di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]** e quindi selezionare l'istanza da aggiornare.

8. Nella pagina Selezione funzionalità selezionare i componenti per l'installazione. Una volta selezionato il nome della funzionalità desiderata, nel riquadro a destra verrà visualizzata una descrizione per ogni gruppo di componenti. È possibile selezionare qualsiasi combinazione di caselle di controllo. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) e [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md). Ogni componente può essere installato una sola volta in un'istanza specificata di SQL Server. Per installare più componenti, è necessario installare un'istanza aggiuntiva di SQL Server.

    I prerequisiti per le funzionalità selezionate vengono visualizzati nel riquadro di destra. Il programma di installazione di SQL Server consentirà di installare i prerequisiti che non sono già stati installati durante la procedura di installazione descritta più avanti in questo argomento.

    Controllo configurazione sistema consentirà di verificare lo stato del sistema del computer prima che l'installazione continui. Selezionare **Avanti** per continuare.

9. Nella pagina Requisiti di spazio su disco viene calcolato lo spazio su disco necessario per le funzionalità specificate e vengono confrontati i requisiti con lo spazio su disco disponibile nel computer in cui è in esecuzione il programma di installazione.

10. Il flusso di lavoro relativo alla parte rimanente di questo articolo dipende dalle funzionalità specificate per l'installazione. Le pagine visualizzate dipendono dalle selezioni effettuate.

11. Nella pagina Configurazione server - Account di servizio specificare gli account di accesso per i servizi SQL Server. I servizi effettivamente configurati in questa pagina dipendono dalle caratteristiche selezionate per l'installazione.

    È possibile assegnare lo stesso account di accesso a tutti i servizi SQL Server oppure configurare singolarmente l'account di ogni servizio. È inoltre possibile specificare se i servizi verranno avviati automaticamente, manualmente o se sono disabilitati. Microsoft consiglia di configurare gli account del servizio singolarmente per fornire privilegi minimi per ogni servizio, in modo che i servizi SQL Server abbiano le autorizzazioni minime necessarie per completare le attività. Per altre informazioni, vedere [Configurazione Server - Account di servizio](./install-sql-server.md) e [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

    Per specificare lo stesso account di accesso per tutti gli account del servizio nell'istanza corrente di SQL Server, immettere le credenziali nei campi visualizzati nella parte inferiore della pagina.

    **Nota sulla sicurezza** : [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]

    Dopo aver specificato le informazioni di accesso per i servizi SQL Server, selezionare **Avanti**.

12. Usare la scheda **Configurazione server - Regole di confronto** per specificare regole di confronto non predefinite per il motore di database e Analysis Services. Per altre informazioni, vedere [Configurazione del server - Regole di confronto](./install-sql-server.md).

13. Utilizzare la pagina Configurazione Motore di database - Provisioning account per specificare gli elementi seguenti:  

    - Modalità di sicurezza: selezionare l'autenticazione di Windows o l'autenticazione in modalità mista per l'istanza di SQL Server. Se si seleziona Autenticazione in modalità mista, è necessario specificare una password complessa per l'account amministratore di sistema predefinito di SQL Server.

        Quando viene stabilita la connessione tra un dispositivo e SQL Server, il meccanismo di sicurezza è lo stesso sia in modalità mista che di autenticazione di Windows. Per altre informazioni, vedere [Configurazione del motore di database - Configurazione del server](./install-sql-server.md).  

    - Amministratori di SQL Server: è necessario specificare almeno un amministratore di sistema per l'istanza di SQL Server. Per aggiungere l'account usato per eseguire il programma di installazione di SQL Server, selezionare **Aggiungi utente corrente**. Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per l'istanza di SQL Server. Per altre informazioni, vedere [Configurazione del motore di database - Configurazione del server](./install-sql-server.md).

    Dopo aver modificato l'elenco, selezionare **OK**. Verificare l'elenco di amministratori nella finestra di dialogo di configurazione. Quando l'elenco è completo, selezionare **Avanti**.

14. Usare la pagina Configurazione del motore di database - Directory dati per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.  

    > [!IMPORTANT]
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di SQL Server. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di SQL Server.

    Per altre informazioni, vedere [Configurazione del motore di database - Directory dati](./install-sql-server.md).

15. Usare la pagina Configurazione del motore di database - FILESTREAM per abilitare la funzione FILESTREAM per l'istanza di SQL Server. Per altre informazioni su FILESTREAM, vedere [Configurazione del motore di database - Filestream](./install-sql-server.md). Per continuare, selezionare Avanti.

16. Usare la pagina Configurazione di Analysis Services - Provisioning account per specificare la modalità server e gli utenti o gli account che avranno le autorizzazioni di amministratore per Analysis Services. La modalità server determina quali sottosistemi di memoria e archiviazione vengono utilizzati nel server. Tipi di soluzione diversi eseguiti nelle modalità server diverse. Se si intende eseguire database di cubi multidimensionali nel server, scegliere l'opzione predefinita, vale a dire quella relativa alla modalità server multidimensionale e di data mining. Relativamente alle autorizzazioni di amministratore, è necessario specificare almeno un amministratore di sistema per Analysis Services. Per aggiungere l'account usato per eseguire il programma di installazione di SQL Server, selezionare **Aggiungi utente corrente**. Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per Analysis Services. Per altre informazioni sulle autorizzazioni della modalità server e amministratore, vedere [Configurazione di Analysis Services - Provisioning account](./install-sql-server.md).

    Dopo aver modificato l'elenco, selezionare **OK**. Verificare l'elenco di amministratori nella finestra di dialogo di configurazione. Quando l'elenco è completo, selezionare **Avanti**.

17. Usare la pagina Configurazione di Analysis Services - Directory dati per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.

    > [!IMPORTANT]
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di SQL Server. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di SQL Server.

    Per altre informazioni, vedere [Configurazione di Analysis Services - Directory dati](./install-sql-server.md).

18. Usare la pagina Configurazione di Reporting Services per specificare il tipo di installazione di Reporting Services da creare. Per altre informazioni sulle modalità di configurazione di Reporting Services, vedere [Opzioni di configurazione di Reporting Services &#40;SSRS&#41;](./install-sql-server.md).

19. Utilizzare la pagina di configurazione del controller di Riesecuzione distribuita per specificare gli utenti a cui si desidera concedere autorizzazioni amministrative per il servizio controller di Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative disporranno di accesso illimitato al servizio controller di Riesecuzione distribuita.

    Selezionare il pulsante **Aggiungi utente corrente** per aggiungere gli utenti a cui concedere le autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita. Selezionare il pulsante **Aggiungi** per aggiungere autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita. Selezionare il pulsante **Rimuovi** per rimuovere le autorizzazioni di accesso dal servizio controller di Riesecuzione distribuita.

    Per continuare, selezionare **Avanti**.

20. Utilizzare la pagina di configurazione del client Riesecuzione distribuita per specificare gli utenti a cui si desidera concedere autorizzazioni amministrative per il servizio client Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative disporranno di accesso illimitato al servizio client Riesecuzione distribuita.

    **Nome controller** è un parametro facoltativo e il valore predefinito è \<*blank*>. Immettere il nome del controller che il computer client comunicherà con per il servizio client Riesecuzione distribuita. Tenere presente quanto segue:

    - Se è già stato configurato un controller, immettere il nome del controller durante la configurazione di ciascuno client.

    - In caso contrario, è possibile lasciare vuoto il nome del controller. Tuttavia, è necessario immettere manualmente il nome del controller nel file **configurazione client** .

    Specificare la **Directory di lavoro** per il servizio client Riesecuzione distribuita. La directory di lavoro predefinita è \<*drive letter*>:\Programmi\Microsoft\SQL Server\DReplayClient\WorkingDir.

    Specificare la **Directory dei risultati** per il servizio client Riesecuzione distribuita. La directory dei risultati predefinita è \<*drive letter*>:\Programmi\Microsoft\SQL Server\DReplayClient\ResultDir.

    Per continuare, selezionare **Avanti**.

21. Nella pagina Segnalazione errori specificare le informazioni che si vuole inviare a Microsoft per contribuire a migliorare SQL Server. Per impostazione predefinita, l'opzione per la segnalazione di errori è abilitata.

22. Controllo configurazione sistema eseguirà uno o più set di regole per convalidare la configurazione del computer con le funzionalità di SQL Server specificate.  

23. Nella pagina Inizio installazione è presente una visualizzazione albero delle opzioni specificate durante l'installazione. In questa pagina, tramite il programma di installazione vengono indicati l'eventuale abilitazione o disabilitazione della funzionalità di aggiornamento del prodotto e la versione dell'aggiornamento finale. Dopo aver selezionato l'installazione, il programma di installazione di SQL Server installerà prima i prerequisiti obbligatori per le funzionalità selezionate e successivamente le funzionalità.  

24. Durante l'installazione, nella pagina Stato dell'installazione è possibile monitorare lo stato di avanzamento del processo.  

25. Al termine dell'installazione nella pagina Operazione completata viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti. Per completare il processo di installazione di SQL Server, selezionare **Chiudi**.

26. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni sui file di log del programma di installazione, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="next-steps"></a>Passaggi successivi

Configurare l'installazione di SQL Server

- Per ridurre la superficie di attacco di un sistema, SQL Server installa e attiva in modo selettivo i servizi e le funzionalità principali. Per ulteriori informazioni, vedere [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).

## <a name="see-also"></a>Vedere anche
- [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Convalidare un'installazione di SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)
- [Ripristinare un'installazione non riuscita di SQL Server 2016](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
- [Eseguire l'aggiornamento a SQL Server usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
- [Installare SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md)
