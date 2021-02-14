---
description: Utilità di esecuzione pacchetti (dtexecui.exe)
title: Utilità di esecuzione pacchetti (dtexecui) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtexecui.setvalues.f1
- sql13.dts.dtexecui.reporting.f1
- sql13.dts.dtexecui.datasources.f1
- sql13.dts.dtexecui.commandfiles.f1
- sql13.dts.dtexecui.logging.f1
- sql13.dts.dtexecui.general.f1
- sql13.dts.dtexecui.verification.f1
- sql13.dts.dtexecui.executionoptions.f1
- sql13.dts.dtexecui.commandline.f1
- sql13.dts.dtexecui.configuration.f1
helpviewer_keywords:
- DTExecUI utility
ms.assetid: 3d71df39-126b-4c8e-bd77-128bbd5b0887
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14392191ab3d11165f75f1637c2fb3330aa886ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346917"
---
# <a name="execute-package-utility-dtexecui"></a>Utilità di esecuzione pacchetti (dtexecui.exe)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilizzare l' **Utilità di esecuzione pacchetti** per eseguire i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . L'utilità esegue i pacchetti archiviati in una delle tre posizioni seguenti: database di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] e file system. Questa interfaccia utente, che può essere avviata da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oppure digitando **dtexecui** al prompt dei comandi, può essere usata in alternativa all'esecuzione di pacchetti usando lo strumento del prompt dei comandi **DTExec** .  
  
 I pacchetti vengono eseguiti nello stesso processo dell'utilità **dtexecui.exe** . Questa utilità è uno strumento a 32 bit, quindi i pacchetti eseguiti usando **dtexecui.exe** in un ambiente a 64 bit vengono eseguiti in Windows on Win32 (WOW). Quando si sviluppano e si verificano i comandi usando l'utilità dtexecui.exe in un computer a 64 bit, è consigliabile eseguire la verifica in modalità a 64 bit usando la versione a 64 bit di **dtexec.exe** prima della distribuzione o della pianificazione dei comandi su un server di produzione.  
  
 L' **Utilità di esecuzione pacchetti** è un'interfaccia utente grafica per lo strumento del prompt dei comandi **DTExec** . L'interfaccia utente semplifica la configurazione delle opzioni e assembla automaticamente la riga di comando che viene passata allo strumento del prompt dei comandi **DTExec** quando il pacchetto viene eseguito dalle opzioni specificate.  
  
 L' **Utilità di esecuzione pacchetti** può essere anche usata per assemblare le righe di comando usate dall'utente durante l'esecuzione diretta di **DTExec** .  
  
### <a name="to-open-execute-package-utility-in-sql-server-management-studio"></a>Per aprire l'Utilità di esecuzione pacchetti in SQL Server Management Studio  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]scegliere **Esplora oggetti** dal menu **Visualizza**.  
  
2.  In Esplora oggetti fare clic su **Connetti**, quindi su **Integration Services**.  
  
3.  Nella finestra di dialogo **Connetti al server** immettere il nome del server nell'elenco **Nome server** e quindi fare clic su **Connetti**.  
  
4.  Espandere la cartella **Pacchetti archiviati** e le relative sottocartelle, fare clic con il pulsante destro del mouse sul pacchetto da eseguire e quindi scegliere **Esegui pacchetto**.  
  
### <a name="to-open-the-execute-package-utility-at-the-command-prompt"></a>Per aprire l'Utilità di esecuzione pacchetti dal prompt dei comandi  
  
-   In una finestra del prompt dei comandi eseguire **dtexecui**.  
  
 Nelle sezioni seguenti vengono descritte le pagine della finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="general-page"></a>Pagina Generale  
 Utilizzare la pagina **Generale** della finestra di dialogo **Utilità di esecuzione pacchetti** per specificare il nome e il percorso di un pacchetto.  
  
 L'utilità di esecuzione pacchetti (dtexecui.exe) esegue sempre un pacchetto nel computer locale anche se il pacchetto si trova su un server remoto. Se il pacchetto remoto utilizza file di configurazione che si trovano a loro volta sul server remoto, l'Utilità di esecuzione pacchetti non è in grado di individuare tali file e l'esecuzione del pacchetto ha esito negativo. Per evitare questo problema, è necessario fare riferimento ai file di configurazione tramite un nome condiviso in formato UNC (Universal Naming Convention), ad esempio \\\mioserver\miofile.  
  
### <a name="static-options"></a>Opzioni statiche  
 **Origine pacchetto**  
 Consente di specificare il percorso del pacchetto da eseguire mediante le opzioni seguenti:  
  
|valore|Descrizione|  
|-|-|  
|**SQL Server**|Selezionare questa opzione se il pacchetto si trova in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Specificare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e immettere il nome utente e la password per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ogni nome utente e password aggiunge le opzioni **/USER** _nomeutente_ e **/PASSWORD** _password_ al prompt dei comandi.|  
|**File system**|Selezionare questa opzione se il pacchetto si trova nel file system.|  
|**Archivio pacchetti SSIS**|Selezionare questa opzione se il pacchetto si trova nell'Archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|  
  
 Ad ognuna di queste impostazioni è associato il set di opzioni seguente.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
### <a name="dynamic-options"></a>Opzioni dinamiche  
  
#### <a name="package-source--sql-server"></a>Origine pacchetto = SQL Server  
 **Server**  
 Consente di digitare il nome del server in cui si trova il pacchetto o di selezionare il server nell'elenco.  
  
 **Accesso al server**  
 Consente di specificare se il pacchetto deve usare l'autenticazione di Windows o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per eseguire la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per una maggiore sicurezza è consigliabile utilizzare l'autenticazione di Windows. Se si sceglie di utilizzare l'autenticazione di Windows, non è necessario specificare il nome utente e la password.  
  
 **Usa autenticazione di Windows**  
 Selezionare questa opzione per usare l'autenticazione di Windows e accedere mediante un account utente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Usa autenticazione di SQL Server**  
 Selezionare questa opzione per usare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando un utente si connette con un nome di account di accesso e una password da una connessione non trusted, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue l'autenticazione controllando se è stato impostato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e se la password specificata corrisponde a quella registrata in precedenza. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non riesce a trovare un account di accesso, l'autenticazione ha esito negativo e viene visualizzato un messaggio di errore.  
  
> [!IMPORTANT]  
>  Se possibile, usare l'autenticazione di Windows.  
  
 **Pacchetto**  
 Digitare il nome del pacchetto o fare clic sul pulsante con i puntini di sospensione **(...)** per individuare il pacchetto mediante la finestra di dialogo **Seleziona pacchetto SSIS**.  
  
#### <a name="package-source--file-system"></a>Origine pacchetto = File system  
 **Pacchetto**  
 Digitare il nome del pacchetto o fare clic sul pulsante con i puntini di sospensione **(...)** per individuare il pacchetto mediante la finestra di dialogo Apri. Per impostazione predefinita, nella finestra di dialogo vengono elencati solo i file con estensione dtsx.  
  
#### <a name="package-source--ssis-package-store"></a>Origine pacchetto = Archivio pacchetti SSIS  
 **Server**  
 Consente di digitare il nome del computer in cui si trova il pacchetto o di selezionare il computer nell'elenco.  
  
 **Accesso al server**  
 Consente di specificare se il pacchetto deve utilizzare l'autenticazione di Microsoft Windows per eseguire la connessione all'origine del pacchetto. Per una maggiore sicurezza è consigliabile utilizzare l'autenticazione di Windows. Se si sceglie di utilizzare l'autenticazione di Windows, non è necessario specificare il nome utente e la password.  
  
 **Usa autenticazione di Windows**  
 Selezionare questa opzione per utilizzare l'autenticazione di Windows e accedere mediante un account utente di Microsoft Windows.  
  
 **Usa autenticazione di SQL Server**  
 Questa opzione non è disponibile se si esegue un pacchetto incluso nell' **Archivio pacchetti SSIS**.  
  
 **Pacchetto**  
 Digitare il nome del pacchetto o fare clic sul pulsante con i puntini di sospensione **(...)** per individuare il pacchetto mediante la finestra di dialogo **Seleziona pacchetto SSIS**.  
  
## <a name="configurations-page"></a>Pagina Configurazioni  
 Utilizzare la pagina **Configurazioni** della finestra di dialogo **Utilità di esecuzione pacchetti** per selezionare i file di configurazione da caricare in fase di esecuzione e di specificare l'ordine in cui devono essere caricati.  
  
### <a name="options"></a>Opzioni  
 **File di configurazione**  
 Elenca le configurazioni utilizzate dal pacchetto. Ogni file di configurazione aggiunge un'opzione **/CONFIGFILE nomefile** al prompt dei comandi.  
  
 **Tasti di direzione**  
 Selezionare un file di configurazione nell'elenco e utilizzare i tasti di direzione sulla destra per modificare l'ordine di caricamento. Le configurazioni vengono caricate a partire dall'inizio dell'elenco.  
  
> [!NOTE]  
>  Se più configurazioni modificano la stessa proprietà, viene utilizzata la configurazione caricata per ultima.  
  
 **Aggiungere**  
 Fare clic su questo pulsante per aggiungere configurazioni tramite la finestra di dialogo **Apri** . Per impostazione predefinita, nella finestra di dialogo sono elencati solo i file con estensione dtsconfig.  
  
 **Rimuovi**  
 Selezionare un file di configurazione nell'elenco e quindi fare clic su **Rimuovi**.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="command-files-page"></a>Pagina File di comando  
 Utilizzare la pagina **File di comando** della finestra di dialogo **Utilità di esecuzione pacchetti** per selezionare i file di comando da caricare in fase di esecuzione.  
  
### <a name="options"></a>Opzioni  
 **File di comando**  
 Consente di elencare i file di comando utilizzati dal pacchetto. Un pacchetto può utilizzare più file per l'impostazione delle opzioni della riga di comando.  
  
 **Tasti di direzione**  
 Selezionare un file di comando nell'elenco e utilizzare i tasti di direzione a destra per modificare l'ordine di caricamento. I file di comando vengono caricati in ordine a partire dalla parte superiore dell'elenco.  
  
 **Aggiungere**  
 Fare clic su questo pulsante per aggiungere un file di comando usando la finestra di dialogo **Apri** .  
  
 **Rimuovi**  
 Selezionare un file di comando nella casella di testo e quindi rimuoverlo con il pulsante **Rimuovi** .  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="connection-managers-page"></a>Pagina Gestioni connessioni  
 Utilizzare la pagina **Gestioni connessioni** della finestra di dialogo **Utilità di esecuzione pacchetti** per modificare le stringhe di connessione delle gestioni connessioni utilizzate dal pacchetto.  
  
### <a name="options"></a>Opzioni  
 **Gestione connessione**  
 Selezionare la casella di controllo corrispondente per rendere modificabile la colonna **Stringa di connessione** .  
  
 **Descrizione**  
 Consente di visualizzare una descrizione di ogni gestione connessione. Non è possibile modificare le descrizioni.  
  
 **Stringa di connessione**  
 Consente di modificare la stringa di connessione per una gestione connessione. Questo campo è modificabile solo quando è selezionata la casella di controllo **Gestione connessione** .  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="execution-options-page"></a>Pagina Opzioni di esecuzione  
 Usare la pagina **Opzioni di esecuzione** della finestra di dialogo **Utilità di esecuzione pacchetti** per specificare le opzioni di runtime per il pacchetto.  
  
### <a name="options"></a>Opzioni  
 **Interrompi il pacchetto in caso di avvisi di convalida**  
 Indica se il pacchetto deve essere interrotto quando vengono generati avvisi di convalida.  
  
 **Convalida pacchetto senza esecuzione**  
 Indica se il pacchetto viene esclusivamente convalidato.  
  
 **Numero massimo di file eseguibili simultanei**  
 Se si desidera, specificare il numero massimo di file eseguibili che possono essere eseguiti simultaneamente nel pacchetto. Dopo avere selezionato questa casella di controllo, utilizzare la casella di selezione per specificare il numero massimo di file eseguibili.  
  
 **Abilita checkpoint pacchetto**  
 Indica se abilitare i checkpoint del pacchetto.  
  
 **File del checkpoint**  
 Elenca i file del checkpoint utilizzati dal pacchetto, se si abilitano i checkpoint del pacchetto.  
  
 **Sfoglia**  
 Fare clic sul pulsante Sfoglia **(...)** per individuare il file del checkpoint tramite la finestra di dialogo **Apri**, se si abilitano i checkpoint del pacchetto. Se è già stato specificato un file del checkpoint, questo viene sostituito dal file selezionato.  
  
 **Ignora opzioni di riavvio**  
 Indica se ignorare le opzioni di riavvio, se si abilitano i checkpoint del pacchetto.  
  
 **Opzione di riavvio**  
 Consente di selezionare la modalità di utilizzo dei checkpoint, se si ignorano le opzioni di riavvio.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="reporting-page"></a>Pagina Report  
 Utilizzare la pagina **Report** dell' **Utilità di esecuzione pacchetti** per specificare le informazioni e gli eventi relativi al pacchetto da registrare nella console durante l'esecuzione del pacchetto.  
  
### <a name="options"></a>Opzioni  
 **Eventi console**  
 Consente di specificare gli eventi e i tipi di messaggi da segnalare.  
  
 **Nessuno**  
 Selezionare questa opzione per non generare report.  
  
 **Errori**  
 Selezionare questa opzione per segnalare i messaggi di errore.  
  
 **Avvisi**  
 Selezionare questa opzione per segnalare i messaggi di avviso.  
  
 **Eventi personalizzati**  
 Selezionare questa opzione per segnalare i messaggi di evento personalizzati.  
  
 **Eventi pipeline**  
 Selezionare questa opzione per segnalare i messaggi di evento dei flussi di dati.  
  
 **Informazioni**  
 Selezionare questa opzione per segnalare i messaggi informativi.  
  
 **Verbose**  
 Selezionare questa opzione per utilizzare i report dettagliati.  
  
 **Registrazione console**  
 Consente di specificare le informazioni che si desidera scrivere nel registro al verificarsi dell'evento selezionato.  
  
 **Nome**  
 Selezionare questa opzione per segnalare il nome dell'utente che ha creato il pacchetto.  
  
 **Computer**  
 Selezionare questa opzione per segnalare il nome del computer nel quale il pacchetto è in esecuzione.  
  
 **Operatore**  
 Selezionare questa opzione per segnalare il nome dell'utente che ha avviato il pacchetto.  
  
 **Nome origine**  
 Selezionare questa opzione per segnalare il nome del pacchetto.  
  
 **GUID origine**  
 Selezionare questa opzione per segnalare il GUID del pacchetto.  
  
 **GUID esecuzione**  
 Selezionare questa opzione per segnalare il GUID dell'istanza di esecuzione del pacchetto.  
  
 **Message**  
 Selezionare questa opzione per segnalare i messaggi.  
  
 **Ora di inizio e fine**  
 Selezionare questa opzione per segnalare l'ora di inizio e fine dell'esecuzione del pacchetto.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="logging-page"></a>Pagina Registrazione  
 Utilizzare la pagina **Registrazione** della finestra di dialogo **Utilità di esecuzione pacchetti** per rendere disponibili i provider di log per il pacchetto in fase di esecuzione. Specificare il tipo di provider di log del pacchetto e la stringa di connessione al log. Per ogni voce di provider di log, viene aggiunta un'opzione **/LOGGER**_idclasse_ al prompt dei comandi.  
  
### <a name="options"></a>Opzioni  
 **Provider di log**  
 Selezionare un provider di log dall'elenco.  
  
 **Stringa di configurazione**  
 Selezionare il nome della gestione connessione dal pacchetto che punta al percorso del log oppure digitare la stringa di connessione al provider di log.  
  
 **Rimuovi**  
 Selezionare un provider di log e fare clic su questo pulsante per rimuoverlo.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="set-values-page"></a>Pagina Imposta valori  
 Utilizzare la pagina **Imposta valori** della finestra di dialogo **Utilità di esecuzione pacchetti** per impostare i valori delle proprietà di pacchetti, file eseguibili, connessioni, variabili e provider di log specificando i percorsi delle proprietà e i valori di queste ultime. Ogni percorso aggiunge un'opzione **/SET**_percorsoproprietà;valore_ al prompt dei comandi.  
  
### <a name="options"></a>Opzioni  
 **Percorso proprietà**  
 Consente di immettere il percorso della proprietà. La sintassi del percorso usa una barra rovesciata (\\) per indicare che l'elemento seguente è un contenitore, il punto (.) per indicare che l'elemento seguente è una proprietà e le parentesi per indicare un membro di una raccolta. Il membro può essere identificato tramite il relativo indice o nome. Ad esempio, il percorso della proprietà di una variabile di pacchetto è \Pacchetto.Variabili[Variabile].Valore.  
  
 **Valore**  
 Consente di immettere il valore della proprietà.  
  
 **Rimuovi**  
 Selezionare un percorso di proprietà e fare clic su questo pulsante per rimuoverla.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="verification-page"></a>Pagina Verifica  
 Utilizzare la pagina **Verifica** della finestra di dialogo **Utilità di esecuzione pacchetti** per impostare i criteri di verifica del pacchetto.  
  
### <a name="options"></a>Opzioni  
 **Esegui solo pacchetti firmati**  
 Selezionare questa opzione per eseguire solo i pacchetti firmati.  
  
 **Verifica build pacchetto**  
 Selezionare questa opzione per verificare la build del pacchetto.  
  
 Compilare  
 Consente di specificare il numero di build sequenziale associato alla build.  
  
 **Verifica ID pacchetto**  
 Selezionare questa opzione per verificare l'ID del pacchetto.  
  
 ID pacchetto  
 Consente di specificare il numero di identificazione del pacchetto.  
  
 **Verifica ID versione**  
 Selezionare questa opzione per verificare l'ID della versione.  
  
 ID versione  
 Consente di specificare il numero di identificazione della versione.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="command-line-page"></a>Pagina Riga di comando  
 Utilizzare il nodo **Riga di comando** della finestra di dialogo **Utilità di esecuzione pacchetti** per modificare la riga di comando generata dalle opzioni create dalle varie finestre di dialogo.  
  
### <a name="options"></a>Opzioni  
 **Ripristina opzioni originali**  
 Fare clic su questo pulsante per ripristinare lo stato originale della riga di comando. Usare questa opzione se sono state apportate modifiche tramite l'opzione **Modifica riga di comando manualmente** e si vogliono ripristinare le opzioni delle riga di comando originali.  
  
 **Modifica riga di comando manualmente**  
 Fare clic per modificare la riga di comando nella casella di testo **Riga di comando** .  
  
 **Riga di comando**  
 Consente di visualizzare la riga di comando corrente, che risulta modificabile se si seleziona l'opzione per la modifica manuale della riga di comando.  
  
 **Eseguire**  
 Fare clic su questo pulsante per eseguire il pacchetto.  
  
 **Close**  
 Fare clic su questo pulsante per chiudere la finestra di dialogo **Utilità di esecuzione pacchetti** .  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità dtexec](../../integration-services/packages/dtexec-utility.md)  
  
  
