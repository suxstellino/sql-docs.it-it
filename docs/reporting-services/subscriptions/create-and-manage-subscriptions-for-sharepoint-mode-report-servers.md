---
title: Creare e gestire sottoscrizioni per server di report in modalità SharePoint | Microsoft Docs
description: Informazioni su come creare una sottoscrizione di Reporting Services per il recapito di report da un'app Web di SharePoint integrata con un server di report in modalità SharePoint.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], creating
- subscriptions [Reporting Services], deleting
- subscriptions [Reporting Services], managing
ms.assetid: 44be7ee2-33ce-46e4-9d1a-a20aaf43a227
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bf68742926ef0f26c5a12a5d001ea9b14f9b199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080094"
---
# <a name="create-and-manage-subscriptions-for-sharepoint-mode-report-servers"></a>Creare e gestire sottoscrizioni per server di report in modalità SharePoint
  È possibile creare sottoscrizioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per il recapito di report da un'applicazione Web di SharePoint integrata con un server di report in modalità SharePoint. Le sottoscrizioni possono recapitare report a una raccolta documenti o a una cartella di file oppure come messaggio di posta elettronica. Questo argomento fornisce un riepilogo dei requisiti e dei passaggi necessari per la creazione di una sottoscrizione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modalità SharePoint &#124; SharePoint 2010 e SharePoint 2013|  
  
 Quando si crea una sottoscrizione, il recapito può essere specificato in tre modi:  
  
-   **Raccolta documenti**: è possibile creare una sottoscrizione che recapita un documento basato sul report originale a una raccolta disponibile nello stesso sito di SharePoint del report originale. Non è possibile recapitare il documento a una raccolta disponibile in un altro server o in un altro sito della stessa raccolta siti. Per recapitare il documento, è necessario disporre dell'autorizzazione Aggiunta elementi per la raccolta a cui viene recapitato il report.  
  
-   **Cartella di file:** è possibile recapitare un documento basato sul report originale a una cartella condivisa nel file system. È necessario selezionare una cartella esistente accessibile tramite una connessione di rete.  
  
-   **Posta elettronica:** se il server di report è configurato in modo da utilizzare l'estensione del server di report per il recapito tramite posta elettronica, sarà possibile creare una sottoscrizione che invia un report o un file di report esportato (salvato in uno dei formati di output disponibili) alla propria cartella di posta in arrivo. Per ricevere solo la notifica, senza il report o l'URL del report, deselezionare le caselle di controllo **Includi collegamento al report** e **Visualizza report all'interno del messaggio** .  
  
 **Contenuto dell'argomento:**  
  
-   [Requisiti generali per le sottoscrizioni](#bkmk_subscription_requirements)  
  
-   [Per creare una sottoscrizione per recapitare un report a una raccolta di SharePoint](#bkmk_tosharepoint_library)  
  
-   [Per creare una sottoscrizione per recapitare un report a una raccolta di SharePoint](#bkmk_tosharepoint_library)  
  
-   [Per creare una sottoscrizione per il recapito tramite posta elettronica del server di report](#bkmk_subscription_for_email)  
  
-   [Per visualizzare o modificare una sottoscrizione](#bkmk_to_modify_subscription)  
  
-   [Per eliminare una sottoscrizione](#bkmk_to_delete_subscription)  
  
##  <a name="general-requirements-for-subscriptions"></a><a name="bkmk_subscription_requirements"></a> Requisiti generali per le sottoscrizioni  
 Per creare una sottoscrizione, è necessario che il report utilizzi credenziali archiviate ed è necessario disporre delle autorizzazioni per la visualizzazione del report e la creazione di avvisi.  
  
 Quando si crea una sottoscrizione, è possibile selezionare un formato di file di output. Non tutti i formati sono appropriati per tutti i report. Prima di selezionare un formato in una sottoscrizione, aprire il report e provare a esportarlo in vari formati, per verificare che venga visualizzato come previsto.  
  
 Per poter creare sottoscrizioni di **, gli utenti necessitano dell'autorizzazione di elenco** Modifica elementi [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in SharePoint. Per altre informazioni, vedere [Informazioni di riferimento sulle autorizzazioni relative a elenchi e siti di SharePoint per gli elementi del server di report](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)  
  
> [!IMPORTANT]  
>  Una sottoscrizione che recapita un report a una raccolta o a una cartella condivisa crea un nuovo file statico, che è basato sul report originale ma non è una vera e propria definizione di report eseguibile in una web part Visualizzatore report. Se il report originale include funzionalità interattive (ad esempio, collegamenti drill-through) o contenuto dinamico, tali funzionalità non saranno disponibili nel file statico recapitato al percorso di destinazione. Se si seleziona una pagina Web è possibile mantenere un certo livello di interattività, ma poiché il documento non è un file con estensione rdl eseguito nel visualizzatore di report, quando si fa clic per scorrere il report vengono create nuove pagine nella sessione del browser, pagine che sarà poi necessario scorrere per tornare al sito.  
  
 Non è possibile modificare in rdl l'estensione del nome di file di un report esportato ed eseguirlo nella web part Visualizzatore report. Se si desidera creare una sottoscrizione che fornisce un report vero e proprio, utilizzare l'estensione del server di report per il recapito tramite posta elettronica e impostare le opzioni relative all'inclusione dei collegamenti nel report.  
  
 Le impostazioni di controllo delle versioni per la raccolta contenente il documento recapitato consentono di determinare se a ogni recapito del documento deve essere creata una nuova versione. Per impostazione predefinita, le impostazioni di controllo delle versioni sono abilitate per tutte le raccolte. A meno che non si scelga **Nessun controllo delle versioni**, a ogni recapito viene creata una nuova versione principale del documento. Vengono create solo versioni principali del documento. Le versioni secondarie non vengono mai create in seguito al recapito di una sottoscrizione, anche se si seleziona un'opzione di controllo delle versioni che lo consente. Se si limita il numero di versioni principali mantenute, i recapiti meno recenti verranno sostituiti dai nuovi quando il limite viene raggiunto.  
  
 I formati di output selezionati per una sottoscrizione sono basati sulle estensioni per il rendering installate nel server di report. È pertanto possibile scegliere solo formati di output supportati dalle estensioni per il rendering del server di report.  
  
###  <a name="to-create-a-subscription-to-deliver-a-report-to-a-sharepoint-library"></a><a name="bkmk_tosharepoint_library"></a> Per creare una sottoscrizione per recapitare un report a una raccolta di SharePoint  
  
1.  Passare a una raccolta di SharePoint contenente il report.  
  
2.  Fare clic sulla freccia GIÙ accanto al report, quindi selezionare **Gestisci sottoscrizioni**.  
  
3.  Fare clic su **Aggiungi sottoscrizione**.  
  
4.  In **Estensione per il recapito** selezionare **Raccolta documenti di SharePoint**.  
  
5.  In **Raccolta documenti** selezionare una raccolta nello stesso sito.  
  
6.  In **Opzioni file** specificare il nome di file e il titolo del documento che verrà creato dalla sottoscrizione.  
  
7.  In **Formato di output** selezionare il formato dell'applicazione.  
  
     L'impostazione predefinita è MHTML (archivio Web), che produce un file HTML autonomo, ma non mantiene le funzionalità interattive eventualmente presenti nel report originale.  
  
8.  In **Opzioni sovrascrittura** specificare un'opzione che determina se sovrascrivere i file durante i recapiti successivi. Se si desidera mantenere i file recapitati in precedenza, selezionare l'opzione per **creare un file con nome univoco**. Per creare il nome di file univoco, ai nomi dei nuovi file viene automaticamente aggiunto un numero.  
  
9. In **Evento di recapito** specificare una pianificazione o un evento che determina l'esecuzione della sottoscrizione. È possibile creare una pianificazione personalizzata, selezionare una pianificazione condivisa, se disponibile, oppure eseguire la sottoscrizione ogni volta che vengono aggiornati i dati per un report eseguito con i dati di uno snapshot. Per altre informazioni sulle pianificazione e l'elaborazione dei dati, vedere [Impostare le opzioni di elaborazione &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Se si sta creando una sottoscrizione per un report con parametri, specificare in **Parametri** i valori da utilizzare con il report durante l'elaborazione della sottoscrizione. La sezione dei parametri non è visibile in questa pagina se il report selezionato non contiene parametri. Per altre informazioni sui parametri, vedere [Impostare i parametri per un report pubblicato &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-shared-folder-delivery"></a><a name="bkmk_subscription_for_sharedfolder"></a> Per creare una sottoscrizione per il recapito a una cartella condivisa  
  
1.  Passare a una raccolta di SharePoint contenente il report.  
  
2.  Fare clic sulla freccia GIÙ accanto al report, quindi selezionare **Gestisci sottoscrizioni**.  
  
3.  Fare clic su **Aggiungi sottoscrizione**.  
  
4.  In **Estensione per il recapito** selezionare **Condivisione file di Windows**.  
  
5.  In **Nome file** immettere il nome del file da creare nella cartella condivisa.  
  
6.  In **Percorso** immettere un percorso di cartella in formato UNC (Uniform Naming Convention), includendo anche il nome di rete del computer. Non includere barre rovesciate finali nel percorso della cartella. È ad esempio possibile utilizzare un percorso quale `\\ComputerName01\Public\MyReports`, dove Public e MyReports sono cartelle condivise.  
  
7.  In **Formato di rendering** selezionare il formato di applicazione per il report.  
  
8.  In **Modalità scrittura** selezionare **Nessuna**, **Incremento automatico** o **Sovrascrivi**. Tali opzioni determinano se sovrascrivere o meno i file durante i recapiti successivi. Se si desidera conservare i file recapitati in precedenza, selezionare **Incremento automatico**. Per creare il nome di file univoco, ai nomi dei nuovi file viene automaticamente aggiunto un numero. Se si sceglie **Nessuna** il report non verrà recapitato se nel percorso di destinazione esiste già un file con lo stesso nome.  
  
9. In **Estensione file** selezionare **True** per aggiungere un'estensione del nome di file che corrisponde al formato di file dell'applicazione oppure False per creare il file senza estensione.  
  
10. In **Nome utente** e **Password** immettere le credenziali di un account dotato di autorizzazioni di scrittura per la cartella condivisa.  
  
11. In **Evento di recapito** specificare una pianificazione o un evento che determina l'esecuzione della sottoscrizione. È possibile creare una pianificazione personalizzata, selezionare una pianificazione condivisa, se disponibile, oppure eseguire la sottoscrizione ogni volta che vengono aggiornati i dati per un report eseguito con i dati di uno snapshot. Per altre informazioni sulle pianificazione e l'elaborazione dei dati, vedere [Impostare le opzioni di elaborazione &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
12. Se si sta creando una sottoscrizione per un report con parametri, specificare in **Parametri** i valori da utilizzare con il report durante l'elaborazione della sottoscrizione. Per altre informazioni sui parametri, vedere [Impostare i parametri per un report pubblicato &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-report-server-e-mail-delivery"></a><a name="bkmk_subscription_for_email"></a> Per creare una sottoscrizione per il recapito tramite posta elettronica del server di report  
  
1.  Passare a una raccolta di SharePoint contenente il report.  
  
2.  Fare clic sulla freccia GIÙ accanto al report, quindi selezionare **Gestisci sottoscrizioni**.  
  
3.  Fare clic su **Aggiungi sottoscrizione**.  
  
4.  In **Estensione per il recapito** selezionare **Posta elettronica**.  
  
5.  In **Opzioni recapito** specificare l'indirizzo di posta elettronica a cui inviare il report.  
  
6.  Facoltativamente è possibile modificare la riga Oggetto. La riga Oggetto utilizza alcuni parametri predefiniti che acquisiscono il nome del report, oltre alla data e all'ora dell'elaborazione. Tali parametri sono gli unici parametri predefiniti che è possibile utilizzare. I parametri sono segnaposto che personalizzano il testo visualizzato nella riga Oggetto, ma possono essere sostituiti con testo statico.  
  
7.  Scegliere **Includi collegamento al report** se si desidera includere l'URL di un report nel corpo del messaggio.  
  
8.  In **Contenuto report** specificare se si desidera includere il report effettivo nel corpo del messaggio.  
  
     Il formato di rendering e il browser determinano se il report verrà incorporato o allegato. Se il browser supporta HTML 4.0 e MHTML e si sceglie il formato di rendering archivio Web, il report verrà incorporato nel messaggio. Con tutti gli altri formati di rendering (CSV, PDF e così via), il report verrà recapitato come allegato. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non esegue la verifica delle dimensioni dell'allegato né di quelle del messaggio prima dell'invio del report. Se l'allegato o il messaggio supera il limite massimo consentito dal server di posta elettronica, il report non verrà recapitato. Se il report è di grandi dimensioni, è consigliabile selezionare una delle altre opzioni di recapito, ad esempio la notifica o l'invio dell'URL.  
  
9. In **Evento di recapito** specificare una pianificazione o un evento che determina l'esecuzione della sottoscrizione. È possibile creare una pianificazione personalizzata, selezionare una pianificazione condivisa, se disponibile, oppure eseguire la sottoscrizione ogni volta che vengono aggiornati i dati per un report eseguito con i dati di uno snapshot. Per altre informazioni sulle pianificazione e l'elaborazione dei dati, vedere [Impostare le opzioni di elaborazione &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Se si sta creando una sottoscrizione per un report con parametri, specificare in **Parametri** i valori da utilizzare con il report durante l'elaborazione della sottoscrizione. Per altre informazioni sui parametri, vedere [Impostare i parametri per un report pubblicato &#40;Reporting Services in modalità integrata SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-view-or-modify-a-subscription"></a><a name="bkmk_to_modify_subscription"></a> Per visualizzare o modificare una sottoscrizione  
  
1.  Passare a una raccolta di SharePoint contenente il report.  
  
2.  Fare clic sulla freccia GIÙ accanto al report, quindi fare clic su **Gestisci sottoscrizioni**.  
  
3.  Ogni sottoscrizione è identificata dal tipo di recapito. Fare clic sul tipo di sottoscrizione per visualizzare e modificare le proprietà esistenti.  
  
###  <a name="to-delete-a-subscription"></a><a name="bkmk_to_delete_subscription"></a> Per eliminare una sottoscrizione  
  
1.  Passare a una raccolta di SharePoint contenente il report.  
  
2.  Fare clic sulla freccia GIÙ accanto al report, quindi fare clic su **Gestisci sottoscrizioni**.  
  
3.  Fare clic sulla casella di controllo accanto alla sottoscrizione, quindi scegliere **Elimina**.  
  
## <a name="see-also"></a>Vedere anche  
 [Sottoscrizioni e recapito &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Recapito tramite posta elettronica in Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   
 [Recapito tramite condivisione file in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Recapito tramite la raccolta di SharePoint in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md)   
 [Configurare un server di report per il recapito tramite posta elettronica (Gestione configurazione del server di report)](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
  
  
