---
title: Impostazioni di posta elettronica in modalità nativa SSRS (Gestione configurazione) | Microsoft Docs
description: SQL Server Reporting Services include un'estensione per il recapito tramite posta elettronica che consente di distribuire report tramite questa modalità.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.emailsettings.F1
helpviewer_keywords:
- SQL11.rsconfigtool.emailsettings.F1
ms.assetid: cdad1529-bfa6-41fb-9863-d9ff1b802577
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31ab6390286c748d8f9cb8f6b98b80c9c53818a1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596934"
---
# <a name="e-mail-settings---reporting-services-native-mode-configuration-manager"></a>Impostazioni posta elettronica - Modalità nativa di Reporting Services (Gestione configurazione)
SQL Server Reporting Services include un'estensione per il recapito tramite posta elettronica che consente di distribuire report tramite questa modalità. A seconda di come viene definita la sottoscrizione tramite posta elettronica, un recapito può essere costituito da una notifica, un collegamento, un allegato o un report incorporato. L'estensione per il recapito tramite posta elettronica può essere utilizzata con la tecnologia del server di posta elettronica esistente. Il server di posta elettronica deve essere un server SMTP o un server di inoltro. Il server di report si connette a un server SMTP tramite librerie Collaboration Data Objects, o CDO, (cdosys.dll) fornite dal sistema operativo.

Per impostazione predefinita, l'estensione per il recapito tramite posta elettronica del server di report non è configurata. Per configurare al minimo l'estensione, è necessario usare Gestione configurazione del server di report. Per impostare le proprietà avanzate, è necessario modificare il file RSReportServer.config. Se non è possibile configurare il server di report per utilizzare questa estensione, è possibile invece recapitare i report in una cartella condivisa. Per altre informazioni, vedere Recapito tramite condivisione file in Reporting Services.

## <a name="configuration-requirements"></a>Requisiti di configurazione

- La funzionalità di recapito tramite posta elettronica del server di report viene implementata in oggetti CDO (Collaboration Data Objects) e per essa è richiesto un server SMTP (Simple Mail Transfer Protocol) locale o remoto o un server d'inoltro SMTP. SMTP non è supportato in tutti i sistemi operativi Windows. Se si utilizza l'edizione basata su Itanium di Windows Server 2008, SMTP non è supportato. Per ulteriori informazioni sulle opzioni di configurazione disponibili tramite CDO, vedere la pagina relativa alla [coclasse Configuration](/previous-versions/exchange-server/exchange-10/ms526939(v=exchg.10)) nel sito Web MSDN.

L'account di autenticazione configurato deve disporre dell'autorizzazione necessaria per inviare messaggi di posta elettronica nel server SMTP.

- L'estensione per il recapito tramite posta elettronica utilizza la codifica UTF-8 negli allegati di posta elettronica. Non è possibile modificare la codifica. L'estensione per il rendering HTML supporta solo la codifica UTF-8.

> [!NOTE] 
> L'estensione predefinita per il recapito tramite posta elettronica non supporta la firma digitale e la crittografia dei messaggi in uscita.

## <a name="setting-configuration-options-for-e-mail-delivery"></a>Impostazione delle opzioni di configurazione per il recapito tramite posta elettronica
Prima di poter utilizzare il recapito tramite posta elettronica di Server report, è necessario impostare valori di configurazione che offrano informazioni sul server SMTP da utilizzare.

Per configurare un server di report per il recapito tramite posta elettronica, eseguire le operazioni seguenti:

- Usare Gestione configurazione del server di report se si specifica soltanto un server SMTP e un account utente con autorizzazione a inviare posta elettronica. Si tratta delle impostazioni minime necessarie per la configurazione dell'estensione per il recapito tramite posta elettronica di Server report.

- Utilizzare un editor di testo per specificare impostazioni aggiuntive nel file RSreportserver.config (facoltativo). Questo file contiene tutte le impostazioni di configurazione per il recapito tramite posta elettronica del server di report. È necessario specificare impostazioni aggiuntive in questi file se si utilizza un server SMTP locale o se il recapito tramite posta elettronica è limitato a host specifici. Per altre informazioni sulla ricerca e la modifica dei file di configurazione, vedere [Modificare un file di configurazione di Reporting Services (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).

> [!NOTE] 
> Le impostazioni della posta elettronica del server di report sono basate su CDO. Per ulteriori informazioni su impostazioni specifiche, fare riferimento alla documentazione di CDO.

## <a name="configure-report-server-e-mail-using-the-report-server-configuration-manager"></a><a name="rsconfigman"/>Configurare la funzionalità di posta elettronica del server di report tramite Gestione configurazione del server di report

1. Avviare Gestione configurazione del server di report e connettersi all'istanza del server di report.

2. In **Indirizzo mittente**, immettere l'indirizzo di posta elettronica da usare nel campo **Da:** di un messaggio di posta elettronica generato. 

     È necessario specificare un account utente che abbia l'autorizzazione per l'invio di posta elettronica dal server SMTP. Il valore immesso per l' **Indirizzo mittente** viene salvato nel campo `<From>` del file rsreportserver.config.  

3.  In **Server SMTP**, specificare il server o il gateway SMTP da usare. 

     Questo valore può corrispondere a un indirizzo IP, un nome NetBIOS di un computer dell'Intranet aziendale o un nome di dominio completo. Il valore immesso per il **Server SMTP** viene salvato nel campo `<SMTPServer>` del file rsreportserver.config.

4. Usare l'elenco a discesa **Autenticazione** per specificare come eseguire l'autenticazione al server SMTP. Questa 

     - **Non richiesta** significa che verrà effettuata la connessione in modo anonimo al server di posta elettronica che è stato specificato.
     
          Selezionando questa opzione `<SendUsing>` verrà impostato su un valore di **2** e `<SMTPAuthenticate>` su un valore di **0** nel file rsreportserver.config.
     
     - **Nome utente e password (di base)** consente di specificare un nome utente e una password per connettersi al server di posta elettronica. È anche possibile selezionare **Utilizza connessione protetta** per accedere al server di posta elettronica tramite una connessione crittografata.
     
          Selezionando questa opzione `<SendUsing>` verrà impostato su un valore di **2** e `<SMTPAuthenticate>` su un valore di **1** nel file rsreportserver.config. Se si seleziona **Usa connessione sicura**, `SMTPUseSSL` verrà impostato su **True**. **Nome utente** verrà impostato in `<SendUserName>` come valore crittografato. **Password** verrà impostato in `<SendPassword>` come valore crittografato.
     
     - **Account servizio server di report (NTLM)** userà l'account di servizio specificato per il server di report. Se si usa l'account del servizio del server di report per l'autenticazione, verificare che l'account del servizio disponga delle autorizzazioni **Invia come** per il server SMTP.
     
          Selezionando questa opzione `<SendUsing>` verrà impostato su un valore di **2** e `<SMTPAuthenticate>` su un valore di **2** nel file rsreportserver.config.

5. Selezionare **Applica**.

6. Nel file rsreportserver.config è anche possibile impostare campi aggiuntivi per la configurazione della posta elettronica.

## <a name="example-report-server-e-mail-configuration"></a>Esempio di configurazione della posta elettronica del server di report
Nell'esempio seguente vengono illustrate le impostazioni nel file RSreportserver.config per un server SMTP remoto. Per informazioni sulle descrizioni delle impostazioni e i valori validi, vedere [File di configurazione Rsreportserver.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

```
<RSEmailDPConfiguration>
     <SMTPServer>mySMTPServer.Adventure-Works.com</SMTPServer>
     <SMTPServerPort></SMTPServerPort>
     <SMTPAccountName></SMTPAccountName>
     <SMTPConnectionTimeout></SMTPConnectionTimeout>
     <SMTPServerPickupDirectory></SMTPServerPickupDirectory>
     <SMTPUseSSL>False</SMTPUseSSL>
     <SendUsing>2</SendUsing>
     <SMTPAuthenticate>2</SMTPAuthenticate>
     <From>my-rs-email-account@Adventure-Works.com</From>
     <EmbeddedRenderFormats>
          <RenderingExtension>MHTML</RenderingExtension>
     </EmbeddedRenderFormats>
     <PrivilegedUserRenderFormats></PrivilegedUserRenderFormats>
     <ExcludedRenderFormats>
          <RenderingExtension>HTMLOWC</RenderingExtension>
          <RenderingExtension>NULL</RenderingExtension>
          <RenderingExtension>RGDI</RenderingExtension>
     </ExcludedRenderFormats>
     <SendEmailToUserAlias>True</SendEmailToUserAlias>
     <DefaultHostName></DefaultHostName>
     <PermittedHosts>
          <HostName>Adventure-Works.com</HostName>
          <HostName>hotmail.com</HostName>
     </PermittedHosts>
     <SendUserName></SendUserName>
     <SendPassword></SendPassword>
</RSEmailDPConfiguration>
```
## <a name="configuration-options-for-setting-the-to-field-in-a-message"></a>Configurazione delle opzioni per l'impostazione del campo A: in un messaggio
Le sottoscrizioni definite dall'utente create in base alle autorizzazioni concesse dall'attività Gestione di sottoscrizioni individuali contengono un nome utente preimpostato che si basa sull'account utente di dominio. Quando l'utente crea la sottoscrizione, l'indirizzo del nome del destinatario incluso nel campo **A:** viene immesso automaticamente in base all'account utente di dominio della persona che crea la sottoscrizione.

Se si utilizza un server SMTP o un server d'inoltro che utilizza account di posta elettronica diversi dall'account utente di dominio, il recapito del report non riuscirà quando il server SMTP tenterà di recapitare il report a tale utente.

Per ovviare a questo problema, è possibile modificare le impostazioni di configurazione che consentono agli utenti di immettere un nome nel campo **A:**

1. Aprire RSReportServer.config con un editor di testo.

2. Impostare `<SendEmailToUserAlias>` su **False**.

3. Impostare `<DefaultHostName>` sul nome DNS (Domain Name System) o sull'indirizzo IP del server SMTP o del server d'inoltro.

4. Salvare il file.

## <a name="configuration-options-for-remote-smtp-service"></a>Opzioni di configurazione per il servizio SMTP remoto
La connessione tra il server di report e un server SMTP o un server d'inoltro viene determinata tramite le impostazioni di configurazione seguenti:

- `<SendUsing>` specifica un metodo per l'invio di messaggi. È possibile scegliere tra un servizio SMTP di rete o una directory di prelievo del servizio SMTP locale. Per utilizzare un servizio SMTP remoto, questo valore deve essere impostato su **2** nel file RSReportServer.config.
- `<SMTPServer>` specifica il server SMTP remoto o il server d'inoltro. Questo valore è obbligatorio se si utilizza un server SMTP remoto o un server d'inoltro.
- `<From>` imposta il valore che viene visualizzato nella riga **Da:** di un messaggio di posta elettronica. Questo valore è obbligatorio se si utilizza un server SMTP remoto o un server d'inoltro.

Tra gli altri valori utilizzati per il servizio SMTP remoto sono inclusi quelli indicati di seguito. Si noti che non è necessario specificare tali valori, a meno che non si desideri ignorare i valori predefiniti.

- `<SMTPServerPort>` è configurato per la porta 25 per impostazione predefinita.
- `<SMTPAuthenticate>` specifica il modo in cui il server di report si connette a un server SMTP remoto. Il valore predefinito è **0** , ovvero nessuna autenticazione. In questo caso, la connessione viene stabilita tramite l'accesso anonimo. In base alla configurazione del dominio, potrebbe essere necessario che il server di report e il server SMTP siano membri dello stesso dominio.
- Per inviare messaggi di posta elettronica a liste di distribuzione limitate, ad esempio liste di distribuzione in cui si accettano i messaggi in arrivo solo da account autenticati, impostare `<SMTPAuthenticate>` su **1** o **2**. Se si imposta su **1**, è necessario impostare anche `<SendUserName>` e `<SendPassword>`. È consigliabile eseguire questa operazione tramite Gestione configurazione del server di report in modo da crittografare i valori per `<SendUserName>` e `<SendPassword>`.

### <a name="to-configure-a-remote-smtp-service-for-the-report-server"></a>Per configurare un servizio SMTP remoto per il server di report

> [!NOTE] 
> È consigliabile configurare il server di posta elettronica tramite Gestione configurazione del server di report.

1. Verificare che il servizio Windows ReportServer disponga delle autorizzazioni **Send As** sul server SMTP.

2. Aprire il file RSReportServer.config in un editor di testo.

3. Verificare che `<UrlRoot>` sia impostato sull'indirizzo URL del server di report. Questo valore viene impostato quando si configura il server di report e quindi dovrebbe essere già inserito. In caso contrario, digitare l'indirizzo URL del server di report.

4. Nella sezione relativa al recapito individuare `<RSEmailDPConfiguration>`.
     
5. In `<SMTPServer>`digitare il nome del server SMTP. Questo valore può corrispondere a un indirizzo IP, un nome UNC di un computer dell'Intranet aziendale o un nome di dominio completo.

6. Impostare `<SendUsing>` su un valore di **2** per usare l'account del servizio per il server di report. Impostare `<SendUsing>` su un valore di **1** per l'autenticazione di base. Se si imposta su **1**, è necessario fornire anche un valore per `<SendUserName>` e `<SendPassword>`. Se si vuole che tali valori siano crittografati, impostare l'autenticazione in Gestione configurazione del server di report.

7. Impostare `<SMTPAuthenticate>` su un valore di **1** se si imposta `<SendUsing>` su 1 o 2.

7. Impostare `<From>`. È necessario specificare un account utente che abbia l'autorizzazione per l'invio di posta elettronica dal server SMTP.

8. Salvare il file.

     Il server di report utilizzerà automaticamente le nuove impostazioni e non sarà necessario riavviare il servizio. È possibile specificare impostazioni SMTP aggiuntive per configurare ulteriormente la modalità di utilizzo del server SMTP per il recapito tramite posta elettronica del server di report.

## <a name="configuration-options-for-local-smtp-service"></a>Opzioni di configurazione per il servizio SMTP locale
La configurazione di un servizio SMTP locale è utile se si desidera testare o risolvere i problemi di recapito tramite posta elettronica del server di report. Il servizio SMTP locale non è attivato per impostazione predefinita.

La connessione tra il server di report e un server SMTP locale o un server d'inoltro viene determinata tramite le impostazioni di configurazione seguenti:

- **SendUsing** è impostato su **1**.
- **SMTPServerPickupDirectory** è impostato su una cartella nell'unità locale.

  > [!NOTE] 
  > Accertarsi che SMTPServer non sia impostato se si usa un server SMTP locale.

- **From** imposta il valore che viene visualizzato nella riga **Da:** di un messaggio di posta elettronica. Questo valore è obbligatorio.

### <a name="to-configure-a-local-smtp-service-for-the-report-server"></a>Per configurare un servizio SMTP locale per il server di report

1. Nel Pannello di controllo selezionare **Attivazione o disattivazione delle funzionalità Windows** per avviare l' **Aggiunta guidata ruoli e funzionalità**.

2. Selezionare **Installazione basata su ruoli o basata su funzionalità** e quindi selezionare **Avanti**.

3. Selezionare il server per installare Internet Information Server (IIS), quindi selezionare **Avanti**.

4. Nella pagina **Ruoli server** selezionare *Avanti*.
     
5. Nella pagina *Funzionalità* selezionare **Server SMTP** e quindi selezionare **Avanti**.

     Se viene richiesto di aggiungere le funzionalità necessarie per il server SMTP, selezionare **Aggiungi funzionalità**.

6. Nella pagina **Ruolo server Web (IIS)** selezionare *Avanti* .

7. Nella pagina **Servizi ruolo** selezionare *Avanti* .

8. Nella pagina di **conferma** selezionare **Installa** .

9. Verificare che il servizio di Windows **Simple Mail Transfer Protocol (SMTP)** sia in esecuzione nella console Servizi.

     Per configurare il server SMTP locale, è necessario usare Gestione IIS 6.0 negli strumenti di amministrazione.

10. Aprire il file RSReportServer.config in un editor di testo.

11. Verificare che `<UrlRoot>` sia impostato sull'indirizzo URL del server di report. Questo valore viene impostato quando si configura il server di report e quindi dovrebbe essere già inserito. Se non è impostato, digitare l'indirizzo URL del servizio Web per il server di report.

12. Nella sezione relativa al recapito individuare `<RSEmailDPConfiguration>`.
     
13. Assicurarsi che `<SMTPServer>` sia presente, ma vuoto.
     
14. Impostare `<SendUsing>` su 1.
     
14. Impostare `<SMTPAuthenticate>` su 0.
     
15. Impostare `<SMTPServerPickupDirectory>` nella cartella di prelievo del servizio SMTP.
     
     Il percorso predefinito sarà *C:\inetpub\mailroot\Pickup*.
     
16. Impostare `<From>`. Imposta il valore che viene visualizzato nella riga **Da:** di un messaggio di posta elettronica.
     
17. Salvare il file.
  
## <a name="see-also"></a>Vedere anche  
[Gestione configurazione server di report (modalità nativa)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
[Modify a Reporting Services Configuration File (rsreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  
[File di configurazione Rsreportserver.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)
  
