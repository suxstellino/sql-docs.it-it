---
description: Definizioni di ruolo - Ruoli predefiniti
title: Definizioni di ruolo - Ruoli predefiniti | Microsoft Docs
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- security [Reporting Services], defaults
- default security
- role-based security [Reporting Services], defaults
ms.assetid: 6b46db51-7c30-467d-a251-50f50647fe21
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 19846d90b09ce660d2ffbd2e1ef996984057d420
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570445"
---
# <a name="role-definitions---predefined-roles"></a>Definizioni di ruolo - Ruoli predefiniti

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene installato con ruoli predefiniti che è possibile utilizzare per concedere l'accesso alle operazioni del server di report. Ogni ruolo predefinito definisce una raccolta di attività correlate. È possibile assegnare account utente e di gruppo ai ruoli predefiniti per fornire accesso immediato alle operazioni del server di report.  
  
## <a name="how-to-use-predefined-roles"></a>Come usare i ruoli predefiniti  
  
1. Rivedere i ruoli predefiniti per determinare se è possibile utilizzarli così come sono. Se è necessario modificare le attività o definire ruoli aggiuntivi, eseguire queste operazioni prima di iniziare ad assegnare gli utenti a ruoli specifici. Per creare o modificare i ruoli personalizzati, usare SQL Server Management Studio. Per altre informazioni, vedere [Creare, eliminare o modificare un ruolo &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md).
  
2. Individuare gli utenti e i gruppi che devono accedere al server di report e il livello di autorizzazioni richiesto. La maggior parte degli utenti dovrebbe essere assegnata al ruolo **Visualizzazione** o al ruolo **Generatore report** . Il ruolo **Server di pubblicazione** dovrebbe essere utilizzato per un numero più limitato di utenti. Il ruolo **Gestione contenuto** dovrebbe essere assegnato a pochissimi utenti.  

3. Quando si è pronti ad assegnare account utente e di gruppo a specifici ruoli, usare il portale Web. Per altre informazioni, vedere [Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md).  
  
##  <a name="predefined-role-definitions"></a><a name="bkmk_rolelist"></a> Definizioni di ruoli predefiniti  
 I ruoli predefiniti sono determinati dalle attività che supportano. È possibile modificare questi ruoli o sostituirli con altri ruoli personalizzati.  
  
 L'*ambito* definisce i limiti entro i quali vengono utilizzati i ruoli. I ruoli a livello di elemento forniscono vari livelli di accesso agli elementi del server di report e alle operazioni che influiscono su questi elementi. I ruoli a livello di elemento sono definiti sul nodo radice (Home) e su tutti gli elementi nell'intera gerarchia di cartelle del server di report. I ruoli a livello di sistema autorizzano l'accesso al livello del sito. I ruoli a livello di elemento e di sistema si escludono reciprocamente, ma vengono utilizzati insieme per fornire autorizzazioni complete per il contenuto e le operazioni del server di report.  
  
 Nella tabella seguente è descritto l'ambito predefinito dei ruoli:  
  
|Ruolo predefinito|Scope|Descrizione|  
|---------------------|-----------|-----------------|  
|[Ruolo Gestione contenuto](#bkmk_content)|Elemento|Può gestire il contenuto nel server di report, inclusi cartelle, report e risorse.|  
|[Ruolo Server di pubblicazione](#bkmk_publisher)|Elemento|Può pubblicare report e report collegati nel server di report.|  
|[Ruolo Visualizzazione](#bkmk_browser)|Elemento|Può visualizzare cartelle e report e sottoscrivere i report.|  
|[Ruolo Generatore report](#bkmk_reportbuilder)|Elemento|Può visualizzare le definizioni dei report.|  
|[Ruolo Report personali](#bkmk_myreports)|Elemento|Può pubblicare report e report collegati e gestire cartelle, report e risorse nella cartella Report personali di un utente.|  
|[Ruolo Amministratore sistema](#bkmk_systemadministrator)|Sistema|Visualizza e modifica le assegnazioni di ruolo a livello di sistema, le definizioni di ruolo a livello di sistema, le proprietà di sistema e le pianificazioni condivise, oltre a creare le definizioni dei ruoli e a gestire i processi in Management Studio.|  
|[Ruolo Utente sistema](#bkmk_systemuser)|Sistema|Visualizza le proprietà di sistema e le pianificazioni condivise e consente l'uso di Generatore Report o di altri client che eseguono le definizioni dei report.|  
  
##  <a name="content-manager-role"></a><a name="bkmk_content"></a> Ruolo Gestione contenuto  
 Il ruolo **Gestione contenuto** è un ruolo predefinito che include attività utili per gli utenti che gestiscono report e contenuto Web, ma che non devono necessariamente progettare report oppure gestire un'istanza di un server Web o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un utente con questo ruolo distribuisce i report, gestisce i modelli di report e le connessioni alle origini dei dati e decide le modalità di utilizzo dei report. Per la definizione del ruolo **Gestione contenuto** tutte le attività a livello di elemento sono selezionate per impostazione predefinita.  
  
 Il ruolo **Gestione contenuto** viene spesso utilizzato in combinazione con il ruolo **Amministratore sistema** . Insieme, in queste due definizioni del ruolo è incluso un set completo di attività per gli utenti che devono disporre di accesso completo a tutti gli elementi in un server di report. Nonostante il ruolo **Gestione contenuto** consenta l'accesso completo a report, modelli di report, cartelle e altri elementi nella gerarchia di cartelle, questo ruolo non consente l'accesso a operazioni o elementi a livello di sito. Attività come la creazione e gestione di pianificazioni condivise, l'impostazione delle proprietà del server e la gestione delle definizioni di ruolo sono operazioni a livello di sistema incluse nel ruolo **Amministratore sistema** . È pertanto consigliabile creare a livello di sito un'altra assegnazione di ruolo che consenta l'accesso alle pianificazioni condivise.  
  
### <a name="content-manager-tasks"></a>Attività del ruolo Gestione contenuto  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Gestione contenuto**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Aggiungere commenti sui report|Creazione, visualizzazione, modifica ed eliminazione di commenti nei report.| 
|Utilizzo di report|Leggere le definizioni dei report.|  
|Creazione di report collegati|Creare report collegati basati su un report non collegato.|  
|Gestione di tutte le sottoscrizioni|Visualizzare, modificare ed eliminare sottoscrizioni associate ai report e ai report collegati, indipendentemente dal proprietario della sottoscrizione. Questa attività supporta la creazione di sottoscrizioni guidate dai dati. Supporta anche la modifica e l'esecuzione di un [aggiornamento pianificato per i file di Power BI (con estensione pbix) in un server di report di Power BI](/power-bi/report-server/configure-scheduled-refresh).|  
|Gestisci commenti|Elimina i commenti degli altri utenti nei report.| 
|Gestire le origini dati|Creare ed eliminare origini dei dati condivise e visualizzare e modificare le proprietà e il contenuto delle origini dei dati.|  
|Gestione di cartelle|Creare, visualizzare ed eliminare cartelle e visualizzarne e modificarne le proprietà.| 
|Gestione di sottoscrizioni individuali|Creare, visualizzare, modificare ed eliminare sottoscrizioni personali degli utenti per report e report collegati. Questa attività supporta anche la modifica e l'esecuzione di un [aggiornamento pianificato per i file di Power BI (con estensione pbix) in un server di report di Power BI](/power-bi/report-server/configure-scheduled-refresh).|  
|Gestire i modelli|Creare, visualizzare ed eliminare modelli e visualizzare e modificare le proprietà dei modelli.|  
|Gestione della cronologia dei report|Creare, visualizzare ed eliminare la cronologia del report, visualizzare le proprietà della cronologia del report e visualizzare e modificare le impostazioni che determinano i limiti per la cronologia dello snapshot e il funzionamento della memorizzazione nella cache.|  
|Gestione di report|Aggiungere ed eliminare report, modificare i parametri dei report, visualizzare e modificare le proprietà dei report, visualizzare e modificare le origini dei dati per il contenuto dei report, visualizzare e modificare le definizioni dei report e impostare i criteri di sicurezza a livello dei report.|  
|Gestione di risorse|Creare, modificare ed eliminare risorse e visualizzarne e modificarne le proprietà.|  
|Impostazione della sicurezza per singoli elementi|Definire criteri di sicurezza per report, report collegati, cartelle, risorse e origini dei dati. Per altre informazioni, vedere [Elementi a sicurezza diretta](../../reporting-services/security/securable-items.md).|  
|Visualizzazione di origini dei dati|Visualizzare origini dei dati condivise nella gerarchia di cartelle.|  
|Visualizzazione di cartelle|Visualizzare il contenuto delle cartelle ed esplorare la gerarchia di cartelle.|  
|Visualizzazione di modelli|Visualizzare i modelli nella gerarchia di cartelle, utilizzare i modelli come origine dei dati per un report ed eseguire query in base al modello per recuperare i dati.|  
|Visualizzazione di report|Eseguire report e visualizzare le proprietà dei report.|  
|Visualizzazione di risorse|Visualizzare risorse e proprietà delle risorse.|  
  
### <a name="customizing-the-content-manager-role"></a>Personalizzazione del ruolo Gestione contenuto  
 Questo ruolo è riservato a utenti trusted che hanno completa responsabilità di gestione e manutenzione del contenuto del server di report. È possibile rimuovere attività da questa definizione, ma ciò può causare ambiguità relativamente agli elementi che possono essere gestiti. Se ad esempio si rimuove l'attività "Visualizzazione di report" da questa definizione di ruolo, gli utenti con ruolo **Gestione contenuto** non potranno visualizzare il contenuto dei report e pertanto non potranno verificare le modifiche apportate alle impostazioni dei parametri e delle credenziali.  
  
 Il ruolo **Gestione contenuto** è utilizzato nella sicurezza predefinita.  
  
##  <a name="publisher-role"></a><a name="bkmk_publisher"></a> Ruolo Server di pubblicazione  
 Il ruolo **Server di pubblicazione** è una definizione di ruolo predefinita che include attività che consentono agli utenti di aggiungere contenuti a un server di report. Questo ruolo è predefinito e non richiede ulteriori operazioni di configurazione. Questo ruolo non viene utilizzato fino a quando non si creano assegnazioni di ruolo che lo includono ed è destinato agli utenti che progettano report o modelli in Progettazione report o Progettazione modelli e quindi li pubblicano in un server di report.  
  
> [!CAUTION]  
> Le autorizzazioni per la pubblicazione di elementi in un server di report devono essere concesse solo a utenti trusted. Tramite il ruolo Server di pubblicazione vengono concesse autorizzazioni estese grazie alle quali gli utenti possono caricare qualsiasi tipo di file in un server di report. Se un file HTML o un report caricato contiene uno script dannoso, questo script viene eseguito con le credenziali di qualsiasi utente che faccia clic sul report o sul documento HTML.  
  
 Le definizioni dei report possono includere script e altri elementi vulnerabili ad attacchi intrusivi nel codice HTML quando viene eseguito il rendering del report in formato HTML in fase di esecuzione. Se un report pubblicato contiene uno script dannoso, qualsiasi utente che esegue il report provoca inavvertitamente l'esecuzione dello script al momento dell'apertura del report. Se l'utente dispone di autorizzazioni elevate, lo script viene eseguito con queste autorizzazioni.  
  
 Per ridurre il rischio di esecuzione involontaria di script dannosi da parte degli utenti, limitare il numero di utenti che dispongono delle autorizzazioni per la pubblicazione di contenuto e verificare che gli utenti pubblichino solo documenti e report provenienti da origini attendibili. Se non si è certi che una definizione del report possa essere pubblicata senza problemi, aprire il file con estensione rdl in un editor di testo e verificare se sono presenti tag script. Gli script dannosi possono essere celati in espressioni e URL, ad esempio un URL in un'azione di navigazione.  
  
### <a name="publisher-tasks"></a>Attività del server di pubblicazione  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Server di pubblicazione**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Creazione di report collegati|Creare report collegati e pubblicarli in una cartella del server di report.|  
|Gestisci commenti|Elimina i commenti degli altri utenti nei report.| 
|Gestire le origini dati|Creare ed eliminare elementi delle origini dei dati condivise e visualizzare e modificare le proprietà e il contenuto delle origini dei dati.|  
|Gestione di cartelle|Creare, visualizzare ed eliminare cartelle e visualizzare e modificare le proprietà delle cartelle.|  
|Gestire i modelli|Creare, visualizzare ed eliminare modelli di report e visualizzare e modificare le proprietà dei modelli di report.|  
|Gestione di report|Aggiungere ed eliminare report, modificare i parametri dei report, visualizzare e modificare le proprietà dei report, visualizzare e modificare le origini dei dati per il contenuto dei report, visualizzare e modificare le definizioni dei report.|  
|Gestione di risorse|Creare, modificare ed eliminare risorse e visualizzare e modificare le proprietà delle risorse.|  
  
### <a name="customizing-the-publisher-role"></a>Personalizzazione del ruolo Server di pubblicazione  
 È possibile modificare il ruolo **Server di pubblicazione** in base alle esigenze. È possibile ad esempio rimuovere l'attività "Creazione di report collegati" se non si desidera che gli utenti possano creare e pubblicare report collegati, oppure è possibile aggiungere l'attività "Visualizzazione di cartelle" in modo che gli utenti possano spostarsi nella gerarchia di cartelle e selezionare la posizione in cui archiviare un nuovo elemento.  
  
 Per gli utenti che devono pubblicare report da Progettazione report è necessaria almeno l'attività "Gestione di report" che consente di aggiungere un report al server di report. Se gli utenti devono pubblicare report che utilizzano origini dei dati condivise o file esterni, sono necessarie anche le attività "Gestione di origini dei dati" e "Gestione di risorse". Se gli utenti devono creare una cartella come parte del processo di pubblicazione, è necessario includere anche l'attività "Gestione di cartelle".  
  
##  <a name="browser-role"></a><a name="bkmk_browser"></a> Ruolo Visualizzazione  
 Il ruolo **Visualizzazione** è un ruolo predefinito in cui sono incluse attività utili per gli utenti che visualizzano report ma che non devono necessariamente crearli o gestirli. Questo ruolo consente di disporre di funzionalità di base per l'utilizzo convenzionale di un server di report. Senza queste attività, potrebbe risultare difficile per un utente utilizzare un server di report.  
  
 È consigliabile utilizzare il ruolo **Visualizzazione** in combinazione con il ruolo **Utente sistema** . Insieme, queste due definizioni di ruolo includono un set completo di attività per gli utenti che devono interagire con gli elementi disponibili in un server di report. Sebbene il ruolo **Visualizzazione** consenta l'accesso completo a report, modelli di report, cartelle e altri elementi nella gerarchia delle cartelle, questo ruolo non consente l'accesso a elementi a livello di sito come le pianificazioni condivise, utili per la creazione di sottoscrizioni. È pertanto consigliabile creare a livello di sito un'altra assegnazione di ruolo che consenta l'accesso alle pianificazioni condivise.  
  
### <a name="browser-tasks"></a>Attività del ruolo Visualizzazione  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Visualizzazione**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Aggiungere commenti sui report|Creazione, visualizzazione, modifica ed eliminazione di commenti nei report.| 
|Gestione di sottoscrizioni individuali|Creare, visualizzare, modificare ed eliminare sottoscrizioni personali degli utenti per report e report collegati e creare pianificazioni per supportare queste sottoscrizioni.| 
|Visualizzazione di cartelle|Visualizzare il contenuto delle cartelle ed esplorare la gerarchia di cartelle.| 
|Visualizzazione di modelli|Visualizzare i modelli nella gerarchia di cartelle, utilizzare i modelli come origine dei dati per un report ed eseguire query in base al modello per recuperare i dati.| 
|Visualizzazione di report|Eseguire report e visualizzare le proprietà dei report.|  
|Visualizzazione di risorse|Visualizzare risorse e proprietà delle risorse.|  
   
### <a name="customizing-the-browser-role"></a>Personalizzazione del ruolo Visualizzazione  
 È possibile modificare il ruolo **Visualizzazione** in base alle esigenze. È ad esempio possibile rimuovere l'attività "Gestione di sottoscrizioni individuali" se non si desidera supportare le sottoscrizioni oppure rimuovere l'attività "Visualizzazione di risorse" se non si desidera che gli utenti visualizzino la documentazione correlata o altri elementi che potrebbero venire caricati nel server di report.  
  
 Per questo ruolo devono essere selezionate almeno le attività "Visualizzazione di report" e "Visualizzazione di cartelle", in modo che siano supportate la visualizzazione e la navigazione all'interno delle cartelle. Non è consigliabile rimuovere l'attività "Visualizzazione di cartelle", a meno che non si desideri impedire la navigazione all'interno delle cartelle. In modo analogo, non è consigliabile rimuovere l'attività "Visualizzazione di report", a meno che non si desideri impedire agli utenti la visualizzazione dei report. Per apportare questo tipo di modifiche, è consigliabile creare una definizione di ruolo personalizzata che possa essere applicata in modo selettivo a gruppi di utenti specifici.  
  
##  <a name="report-builder-role"></a><a name="bkmk_reportbuilder"></a> Ruolo Generatore report  
 **Generatore report** è un ruolo predefinito in cui sono incluse attività per caricare i report in Generatore report, visualizzare la gerarchia di cartelle e spostarsi al suo interno. Per creare e modificare report in Generatore report, è necessario disporre di un'assegnazione di ruolo a livello di sistema che includa l'attività "Esecuzione delle definizioni dei report", richiesta per elaborare i report a livello locale in Generatore report.  
  
### <a name="report-builder-tasks"></a>Attività incluse nel ruolo Generatore report  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Generatore report**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Aggiungere commenti sui report|Creazione, visualizzazione, modifica ed eliminazione di commenti nei report.| 
|Utilizzo di report|Leggere le definizioni dei report.|  
|Gestione di sottoscrizioni individuali|Creare, visualizzare, modificare ed eliminare sottoscrizioni personali degli utenti per report e report collegati e creare pianificazioni per supportare queste sottoscrizioni.|  
|Visualizzazione di cartelle|Visualizzare il contenuto delle cartelle ed esplorare la gerarchia di cartelle.|  
|Visualizzazione di modelli|Visualizzare i modelli nella gerarchia di cartelle, utilizzare i modelli come origine dei dati per un report ed eseguire query in base al modello per recuperare i dati.|  
|Visualizzazione di report|Eseguire report e visualizzare le proprietà dei report.|  
|Visualizzazione di risorse|Visualizzare risorse e proprietà delle risorse.|  
  
### <a name="customizing-the-report-builder-role"></a>Personalizzazione del ruolo Generatore report  
 È possibile modificare il ruolo **Generatore report** in base alle esigenze. Le indicazioni sono in genere identiche a quelle per il ruolo **Visualizzatore** , cioè rimuovere l'attività "Gestione di sottoscrizioni individuali" se non si desidera supportare le sottoscrizioni, rimuovere l'attività "Visualizzazione di risorse" se non si desidera che gli utenti visualizzino le risorse e mantenere le attività "Visualizzazione di report" e "Visualizzazione di cartelle" per supportare la visualizzazione e la navigazione all'interno delle cartelle.  
  
 L'attività più importante in questa definizione di ruolo è "Utilizzo di report", che consente a un utente di caricare una definizione di report dal server di report in un'istanza locale di Generatore report. Se non si desidera supportare questa attività, è possibile eliminare questa definizione del ruolo e utilizzare il ruolo **Visualizzazione** per supportare l'accesso generale a un server di report.  
  
##  <a name="my-reports-role"></a><a name="bkmk_myreports"></a> Ruolo Report personali  
 Il ruolo **Report personali** è un ruolo predefinito in cui è incluso un set di attività destinate agli utenti della funzionalità Report personali. Questa definizione di ruolo include attività che garantiscono agli utenti autorizzazioni amministrative per la cartella Report personali di cui sono proprietari.  
  
 Sebbene sia possibile scegliere un altro ruolo da utilizzare con la funzionalità Report personali, è comunque consigliabile sceglierne uno che venga utilizzato esclusivamente per la sicurezza di questa funzionalità. Per altre informazioni, vedere [Proteggere i report personali](../../reporting-services/security/secure-my-reports.md).  
                          
### <a name="my-reports-tasks"></a>Attività di Report personali  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Report personali**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Aggiungere commenti sui report|Creazione, visualizzazione, modifica ed eliminazione di commenti nei report.| 
|Creazione di report collegati|Creare report collegati basati su report archiviati nella cartella Report personali dell'utente.|  
|Gestisci commenti|Elimina i commenti degli altri utenti nei report.| 
|Gestire le origini dati|Creare ed eliminare origini dei dati condivise e visualizzare e modificare le proprietà e il contenuto delle origini dei dati.|  
|Gestione di cartelle|Creare, visualizzare ed eliminare cartelle e visualizzarne e modificarne le proprietà.|  
|Gestione di sottoscrizioni individuali|Creare, visualizzare, modificare ed eliminare sottoscrizioni per report e report collegati.|  
|Gestione della cronologia dei report|Creare, visualizzare ed eliminare la cronologia del report, visualizzare le proprietà della cronologia del report e visualizzare e modificare le impostazioni che determinano i limiti per la cronologia dello snapshot e il funzionamento della memorizzazione nella cache.|  
|Gestione di report|Aggiungere ed eliminare report, modificare i parametri dei report, visualizzare e modificare le proprietà dei report, visualizzare e modificare le origini dei dati per il contenuto dei report, visualizzare e modificare le definizioni dei report e impostare i criteri di sicurezza a livello di report.|  
|Gestione di risorse|Creare, modificare ed eliminare le risorse e visualizzare e modificare le proprietà delle risorse.|  
|Visualizzazione di origini dei dati|Visualizzare origini dei dati condivise nella gerarchia di cartelle.|  
|Visualizzazione di cartelle|Visualizzare il contenuto delle cartelle.|  
|Visualizzazione di report|Eseguire report archiviati nella cartella Report personali dell'utente e visualizzare le proprietà dei report.|  
|Visualizzazione di risorse|Visualizzare risorse e proprietà delle risorse.|  
  
### <a name="customizing-the-my-reports-role"></a>Personalizzazione del ruolo Report personali  
 È possibile modificare il ruolo Report personali in base alle esigenze. È tuttavia consigliabile mantenere le attività "Gestione di report" e "Gestione di cartelle" per assicurare l'esecuzione delle operazioni di base relative alla gestione del contenuto. È inoltre consigliabile impostare questo ruolo in modo che supporti tutte le attività di visualizzazione per consentire agli utenti di visualizzare il contenuto delle cartelle e di eseguire i report che gestiscono personalmente.  
  
 Sebbene l'attività "impostazione della sicurezza per singoli elementi" non faccia parte della definizione di ruolo per impostazione predefinita, è possibile aggiungere questa attività al ruolo **report personali** in modo che gli utenti possano personalizzare le impostazioni di sicurezza per sottocartelle e report.  
  
##  <a name="system-administrator-role"></a><a name="bkmk_systemadministrator"></a> Ruolo Amministratore sistema  
 Il ruolo **Amministratore sistema** è un ruolo predefinito in cui sono incluse attività utili per un amministratore di server di report che ha la completa responsabilità di un server di report, ma non necessariamente del contenuto di questo server.  
  
 Per creare un'assegnazione di ruolo che includa questo ruolo, usare la pagina Impostazioni sito nel portale Web oppure i comandi che vengono visualizzati facendo clic con il pulsante destro del mouse sul nodo del server di report in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Il ruolo **Amministratore sistema** non include l'intera gamma di autorizzazioni di cui un amministratore locale può disporre in un computer. Il ruolo **Amministratore sistema** include invece operazioni eseguite a livello del sito e non a livello di elemento. Per gli utenti che desiderano l'accesso alle operazioni a livello di sito e agli elementi archiviati nel server di report, creare nella cartella principale una seconda assegnazione di ruolo che includa il ruolo **Gestione contenuto** . Insieme, in queste due definizioni del ruolo è incluso un set completo di attività per gli utenti che devono disporre di accesso completo a tutti gli elementi in un server di report.  
  
### <a name="system-administrator-tasks"></a>Attività del ruolo Amministratore sistema  
 Nella tabella seguente vengono elencate le attività incluse nel ruolo **Amministratore sistema**:  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Esecuzione delle definizioni dei report|Avviare l'esecuzione della definizione del report senza pubblicarlo in un server di report.|  
|Gestire i processi|Visualizzare e annullare processi in esecuzione. Per altre informazioni, vedere [Gestire un processo in esecuzione](../../reporting-services/subscriptions/manage-a-running-process.md).|  
|Gestione delle proprietà del server di report|Visualizzare e modificare proprietà relative al server di report e agli elementi gestiti dal server di report.<br /><br /> Questa attività supporta la ridenominazione del portale Web, l'attivazione della funzionalità Report personali e la definizione delle impostazioni predefinite della cronologia del report.|  
|Gestione della sicurezza del server di report|Visualizzare e modificare le assegnazioni di ruolo a livello di sistema.|  
|Gestire i ruoli|Creare, visualizzare, modificare ed eliminare definizioni di ruolo.<br /><br /> I membri del ruolo **Amministratore sistema** possono utilizzare la pagina Impostazioni sito per gestire i ruoli.|  
|Gestione di pianificazioni condivise|Creare, visualizzare, modificare ed eliminare pianificazioni condivise che vengono utilizzate per eseguire o aggiornare i report.|  
  
 Il ruolo **Amministratore sistema** è utilizzato nella sicurezza predefinita.  
  
##  <a name="system-user-role"></a><a name="bkmk_systemuser"></a> Ruolo Utente sistema  
Il ruolo **Utente sistema** è un ruolo predefinito in cui sono incluse attività che consentono agli utenti di visualizzare informazioni di base sul server di report. Include inoltre il supporto per caricare un report in Generatore report. Generatore report è un'applicazione client in grado di elaborare un report indipendentemente da un server di report. L'attività "Esecuzione delle definizioni dei report" è stata progettata per essere utilizzata con Generatore report. Se non si utilizza Generatore report, è possibile rimuovere questa attività dal ruolo **Utente sistema** .  

Nella tabella seguente vengono elencate le attività incluse nella definizione del ruolo **Utente sistema**:  
  
### <a name="system-user-tasks"></a>Attività del ruolo Utente sistema  
  
|Attività|Descrizione|  
|----------|-----------------|  
|Esecuzione delle definizioni dei report|Eseguire un report senza pubblicarlo in un server di report.|  
|Visualizzazione delle proprietà del server di report|Visualizzare proprietà relative al server di report, ad esempio il nome dell'applicazione, lo stato di attivazione dell'impostazione Report personali e le impostazioni predefinite della cronologia del report.<br /><br /> Se si rimuove questa attività dal ruolo **Utente sistema** , la pagina Impostazioni sito non sarà più disponibile. Non verrà inoltre visualizzato il titolo dell'applicazione nella parte superiore di ogni pagina. Per impostazione predefinita, il titolo per il portale Web è "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]".|  
|Visualizzazione di pianificazioni condivise|Visualizzare le pianificazioni condivise che sono utilizzate per eseguire o aggiornare i report.<br /><br /> Se si rimuove questa attività dal ruolo **Utente sistema** , gli utenti non potranno selezionare le pianificazioni condivise da utilizzare con sottoscrizioni e altre operazioni pianificate.|  
  
 Il ruolo **Utente sistema** può essere utilizzato per integrare la sicurezza predefinita. È possibile includerlo in nuove assegnazioni di ruolo per estendere le autorizzazioni di accesso al server di report agli utenti dei report. Per altre informazioni, vedere [Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md).  
  
## <a name="see-also"></a>Vedere anche  
[Creare, eliminare o modificare un ruolo &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md)  
[Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[Modificare o eliminare un'assegnazione di ruolo &#40;portale Web SSRS&#41;](../../reporting-services/security/role-assignments-modify-or-delete.md)  
[Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
[Attività e autorizzazioni](../../reporting-services/security/tasks-and-permissions.md)
