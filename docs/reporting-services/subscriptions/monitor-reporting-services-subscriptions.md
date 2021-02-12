---
title: Monitorare le sottoscrizioni di Reporting Services | Microsoft Docs
description: Informazioni su come usare l'interfaccia utente, PowerShell o i file di log per tenere traccia delle sottoscrizioni di Reporting Services. Le opzioni di monitoraggio dipendono dalla modalità del server di report in esecuzione.
ms.date: 06/12/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], inactive
- subscriptions [Reporting Services], status
- monitoring [Reporting Services], subscriptions
- status information [Reporting Services]
- inactive subscriptions [Reporting Services]
ms.assetid: 054c4a87-60bf-4556-9a8c-8b2d77a534e6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6631325809a468aed4e7c7fd00cf71aedecc7ba0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042059"
---
# <a name="monitor-reporting-services-subscriptions"></a>Monitorare le sottoscrizioni di Reporting Services
  È possibile monitorare le sottoscrizioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dall'interfaccia utente, da Windows PowerShell o dai file di log. Le opzioni disponibili per il monitoraggio dipendono dalla modalità del server di report in esecuzione.  
  
**[!INCLUDE[applies](../../includes/applies-md.md)]**

:::row:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modalità nativa  
    :::column-end:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modalità SharePoint  
    :::column-end:::
:::row-end:::

 **Contenuto dell'articolo:**  
  
-   [Interfaccia utente in modalità nativa](#bkmk_native_mode)  
  
-   [Modalità SharePoint](#bkmk_sharepoint_mode)  
  
-   [Usare PowerShell per monitorare le sottoscrizioni](#bkmk_use_powershell)  
  
-   [Gestione di sottoscrizioni inattive](#bkmk_manage_inactive)  
  
##  <a name="native-mode-user-interface"></a><a name="bkmk_native_mode"></a> Interfaccia utente in modalità nativa  
 I singoli utenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] possono monitorare lo stato di una sottoscrizione tramite la pagina **Sottoscrizioni personali** o la scheda **Sottoscrizioni** nel portale Web. Le pagine delle sottoscrizioni includono colonne che indicano la data di ultima esecuzione e lo stato di ogni sottoscrizione. I messaggi di stato vengono aggiornati in corrispondenza del momento in cui è stata pianificata la sottoscrizione. Se non si verifica mai l'evento che determina l'elaborazione della sottoscrizione (ad esempio se uno snapshot dell'esecuzione del report non viene mai aggiornato o se una pianificazione non viene mai eseguita), il messaggio di stato non verrà aggiornato.  
  
 La tabella seguente descrive i valori possibili per la colonna **Stato** .  
  
|Stato|Descrizione|  
|------------|-----------------|  
|Nuova sottoscrizione|Viene visualizzato quando viene creata una nuova sottoscrizione.|  
|Inactive|Viene visualizzato quando una sottoscrizione non può essere elaborata. Per altre informazioni, vedere "Gestione di sottoscrizioni inattive" più avanti in questo articolo.|  
|Operazione completata: \<*number*> elaborati di \<*number*> totali; \<*number*> errori.|Consente di visualizzare lo stato di esecuzione di una sottoscrizione guidata dai dati. Questo messaggio viene generato da Elaborazione pianificazione e recapito.|  
|\<*number*> elaborati|Numero di notifiche che Elaborazione pianificazione e recapito ha recapitato senza errori oppure che non tenterà più di recapitare. Quando viene completata l'operazione di recapito correlata a una sottoscrizione guidata dai dati, il numero di notifiche elaborate dovrebbe corrispondere al numero totale di notifiche generate.|  
|\<*number*> totali|Numero totale di notifiche generate per l'ultima operazione di recapito correlata alla sottoscrizione.|  
|\<*number*> errori|Numero di notifiche che Elaborazione pianificazione e recapito non ha potuto recapitare oppure che non tenterà più di recapitare.|  
|Errore durante l'invio della posta: il trasporto non è riuscito a connettersi al server.|Indica che il server di report non si è connesso al server di posta elettronica. Questo messaggio viene generato dall'estensione per il recapito tramite posta elettronica.|  
|Il file \<*filename*> è stato scritto in \<path>.|Indica l'avvenuto recapito al percorso di condivisione file. Questo messaggio viene generato dall'estensione per il recapito tramite condivisione file.|  
|Errore sconosciuto durante la scrittura del file.|Indica il mancato recapito al percorso di condivisione file. Questo messaggio viene generato dall'estensione per il recapito alla condivisione file.|  
|Errore durante la connessione alla cartella di destinazione, \<path>. Verificare che la cartella di destinazione o la condivisione file esista.|Indica che la cartella specificata non è stata trovata. Questo messaggio viene generato dall'estensione per il recapito alla condivisione file.|  
|Non è stato possibile scrivere il file \<filename> in \<path>. Nuovo tentativo in corso.|Indica il mancato aggiornamento del file a una versione più recente. Questo messaggio viene generato dall'estensione per il recapito alla condivisione file.|  
|Errore durante la scrittura del file \<filename>: \<message>|Indica il mancato recapito al percorso di condivisione file. Questo messaggio viene generato dall'estensione per il recapito alla condivisione file.|  
|\<custom status messages>|Messaggi di stato relativi all'avvenuto o mancato recapito, generati dalle estensioni per il recapito. Se si utilizza un'estensione per il recapito di terze parti o personalizzata, potrebbero essere visualizzati altri messaggi.|  
  
 Gli amministratori del server di report possono inoltre monitorare le sottoscrizioni standard in corso di elaborazione. Non è possibile monitorare le sottoscrizioni guidate dai dati. Per altre informazioni, vedere [Gestire un processo in esecuzione](../../reporting-services/subscriptions/manage-a-running-process.md).  
  
 Se non è stato possibile recapitare una sottoscrizione (ad esempio, se il server di posta elettronica non è disponibile), l'estensione per il recapito tenta nuovamente di eseguire il recapito. Il numero massimo di tentativi viene specificato tramite un'impostazione di configurazione. L'impostazione predefinita prevede che non vengano eseguiti nuovi tentativi nel caso il primo non riesca. In alcuni casi, se un report viene elaborato senza dati (ad esempio, se l'origine dei dati è offline), nel corpo del messaggio sarà presente un'indicazione di questo problema.  
  
### <a name="native-mode-log-files"></a>File di log in modalità nativa  
 Se si verifica un errore durante il recapito, viene inserita una voce specifica nel log di traccia del server di report.  
  
 Per determinare lo stato di recapito delle sottoscrizioni, gli amministratori del server di report possono esaminare i file **ReportServerService_*.log**. Per il recapito tramite posta elettronica, i file di registro del server di report includono una registrazione delle operazioni di elaborazione e di recapito ad account di posta elettronica specifici. Di seguito è riportato il percorso predefinito dei file di log:  
  
 `C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\LogFiles`  
  
 Di seguito è riportato un esempio del nome di un file di log:  
  
 `ReportServerService__05_21_2019_00_05_07.log`  
  
 Di seguito è riportato un esempio di un messaggio di errore nel file di log di traccia relativo alle sottoscrizioni:  
  
-   library!WindowsService_7!b60!05/20/2019-22:34:36 i INFO: Initializing EnableExecutionLogging to 'True'  as specified in Server system properties.emailextension!WindowsService_7!b60!05/20/2019-22:34:41 ERROR: **Error sending email**. Eccezione: System.Net.Mail.SmtpException: Il server SMTP richiede una connessione protetta oppure il client non è stato autenticato. Risposta del server: 5.7.1 Client was not authenticated   at System.Net.Mail.MailCommand.CheckResponse(SmtpStatusCode statusCode, String response)  
  
 Il file di log non indica se il report è stato aperto né se il recapito è effettivamente riuscito. Un'operazione di recapito è considerata riuscita quando non vengono generati errori da Elaborazione pianificazione e recapito e il server di report si è connesso al server di posta elettronica. Nel file di log non vengono registrati, ad esempio, gli errori di mancato recapito dei messaggi di posta elettronica nella cassetta postale degli utenti. Per altre informazioni sui file di log, vedere [File di log e origini di Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
##  <a name="sharepoint-mode"></a><a name="bkmk_sharepoint_mode"></a> Modalità SharePoint  
 È possibile monitorare lo stato di una sottoscrizione in modalità SharePoint dalla pagina **Gestisci sottoscrizioni** .  
  
1.  Selezionare la raccolta documenti contenente il report.  
  
2.  Aprire il menu di scelta rapida del report ( **...** ).  
  
3.  Selezionare l'opzione di menu espanso ( **...** ).  
  
4.  Selezionare **Gestisci sottoscrizioni**  
  
### <a name="sharepoint-uls-log-files"></a>File di log ULS di SharePoint  
 Le informazioni correlate alle sottoscrizioni vengono scritte nel log ULS di SharePoint. Per altre informazioni sulla configurazione di eventi di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per il log ULS, vedere [Abilitare gli eventi di Reporting Services per il log di traccia di SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md).  Di seguito è riportato un esempio di una voce di log ULS correlata alle sottoscrizioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
|Data|Process|Area|Category|Level|Correlation|Message|  
|-|-|-|-|-|-|-|  
|21/5/2019 14:34:06:15|App Pool: a0ba039332294f40bc4a81544afde01d|SQL Server Reporting Services|Report Server Email Extension|Unexpected|(vuoto)|**Error sending email.** Eccezione: System.Net.Mail.SmtpException: Mailbox unavailable. Risposta del server: 5.7.1 Client does not have permissions to send as this sender  at System.Net.Mail.DataStopCommand.CheckResponse(SmtpStatusCode statusCode, String serverResponse)  at System.Net.Mail.DataStopCommand.Send(SmtpConnection conn)  at System.Net.Mail.SmtpClient.Send(MailMessage message)  at Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider.Deliver(Notification notification)|  
  
##  <a name="use-powershell-to-monitor-subscriptions"></a><a name="bkmk_use_powershell"></a> Usare PowerShell per monitorare le sottoscrizioni  
 Per un esempio degli script di PowerShell che è possibile usare per controllare lo stato delle sottoscrizioni in modalità nativa o in modalità SharePoint, vedere [Gestire i proprietari di sottoscrizioni ed eseguire la sottoscrizione - PowerShell](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md).  
  
##  <a name="managing-inactive-subscriptions"></a><a name="bkmk_manage_inactive"></a> Gestione di sottoscrizioni inattive  
 Se una sottoscrizione diventa inattiva, è necessario eliminarla oppure riattivarla risolvendo i problemi che ne impediscono l'elaborazione. Le sottoscrizioni possono diventare inattive se si verificano condizioni che ne impediscono l'elaborazione. Alcune di queste condizioni sono indicate di seguito:  
  
-   Rimozione o disinstallazione dell'estensione per il recapito specificata nella sottoscrizione.  
  
-   Modifica delle impostazioni delle credenziali, passando da valori archiviati a valori integrati o su richiesta.  
  
-   Modifica del nome o del tipo di dati di un parametro nella definizione del report e successiva ripubblicazione del report. Se in una sottoscrizione è incluso un parametro non più valido, la sottoscrizione diventerà inattiva.  
  
-   Modifica della modalità di esecuzione di un report, ad esempio modifica di un report su richiesta, in modo che venga eseguito come snapshot dell'esecuzione del report. Per altre informazioni, vedere [Impostare proprietà di elaborazione dei report](../../reporting-services/report-server/set-report-processing-properties.md).  
  
 Quando una sottoscrizione è inattiva, un messaggio nella sottoscrizione indica questo stato. Il messaggio include informazioni sulle cause e sulla procedura che è necessario eseguire per riattivare la sottoscrizione.  
  
 Lo stato di inattività della sottoscrizione viene indicato nella sottoscrizione stessa quando questa viene eseguita dal server di report. Ad esempio, se una sottoscrizione è pianificata in modo da recapitare un report ogni venerdì alle 2.00 e l'estensione per il recapito che utilizza viene disinstallata lunedì alle 9.00, la sottoscrizione non indicherà lo stato di inattività fino alle 2.00 di venerdì.  
  
## <a name="see-also"></a>Vedere anche  
 [Creare e gestire sottoscrizioni per server di report in modalità nativa](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)   
 [Sottoscrizioni e recapito &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
  
