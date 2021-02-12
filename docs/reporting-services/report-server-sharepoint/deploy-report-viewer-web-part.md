---
title: Distribuire la web part Visualizzatore report di SQL Server Reporting Services in un sito di SharePoint | Microsoft Docs
description: Per SQL Server Reporting Services, è possibile aggiungere manualmente la web part personalizzata Visualizzatore report a un prodotto SharePoint.
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b8638e0c9a958e8f5dd6b6039540e8492555b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074832"
---
# <a name="deploy-the-sql-server-reporting-services-report-viewer-web-part-on-a-sharepoint-site"></a>Distribuire la web part Visualizzatore report di SQL Server Reporting Services in un sito di SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

La web part Visualizzatore report è una web part personalizzata che può essere usata per visualizzare i report di SQL Server Reporting Services (modalità nativa) nel sito di SharePoint. La web part può essere usata per visualizzare, stampare ed esportare report in un server di report. La web part Visualizzatore report è associata ai file di definizione dei report (con estensione rdl) elaborati da un server di report di SQL Server Reporting Services o da un server di report di Microsoft Power BI. Questa web part Visualizzatore report non può essere usata con i report di Power BI ospitati nel server di report di Microsoft Power BI.

Usare le istruzioni seguenti per distribuire manualmente il pacchetto di soluzioni che consente di aggiungere la web part Visualizzatore report a un ambiente SharePoint Server 2013, SharePoint Server 2016 o SharePoint Server 2019. La distribuzione della soluzione è un passaggio obbligatorio per la configurazione della web part.

**La web part Visualizzatore report è un pacchetto di soluzione autonomo e non è associato alla modalità integrata SharePoint per SQL Server Reporting Services.**

## <a name="requirements"></a>Requisiti

> [!IMPORTANT]
> A partire dalla versione "15.X.X.X" è possibile installare ReportViewerWebPart side-by-side con le applicazioni di servizio condiviso in modalità integrata SharePoint per Reporting Services esistenti.
> Con questo aggiornamento della soluzione WSP sono stati introdotti nuovi file. La soluzione precedente deve quindi essere ritirata e il nuovo file WSP ridistribuito usando i cmdlet Uninstall-SPSolution e Install-SPSolution, rispettivamente.
>

**Versioni supportate di SharePoint Server:**
* SharePoint Server 2019
* SharePoint Server 2016
* SharePoint Server 2013

**Versioni supportate di Reporting Services:**  
* Microsoft SQL Server 2008 Reporting Services (modalità nativa) e versioni successive.
* Server di report di Power BI

## <a name="download-the-report-viewer-web-part-solution-package"></a>Scaricare il pacchetto della soluzione web part Visualizzatore di report

La web part Visualizzatore di report è disponibile nell'Area download Microsoft.

[Scaricare il pacchetto della soluzione web part Visualizzatore di report](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="deploy-the-farm-solution"></a>Distribuire la soluzione farm

Questa sezione descrive come distribuire il pacchetto della soluzione alla farm di SharePoint. È necessario eseguire questa attività una sola volta.

1. In un server SharePoint aprire una shell di gestione di SharePoint usando l'opzione **Esegui come amministratore**.

2. Eseguire [Add-SPSolution](/powershell/module/sharepoint-server/Add-SPSolution) per aggiungere la soluzione farm.

    ```
    Add-SPSolution -LiteralPath "{path to file}\ReportViewerWebPart.wsp"
    ```

    Il cmdlet restituisce il nome e l'ID della soluzione e Deployed=False. Nel passaggio successivo la soluzione verrà distribuita.

3. Eseguire il cmdlet [Install-SPSolution](/powershell/module/sharepoint-server/Install-SPSolution) per distribuire la soluzione farm.

    **SharePoint 2013**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -CompatibilityLevel "14,15" -GACDeployment -WebApplication {URL to web application}
    ```

    **SharePoint Server 2016 e 2019**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -GACDeployment -WebApplication {URL to web application}
    ```

## <a name="activate-feature"></a>Attivare la funzionalità

1. Nel sito di SharePoint selezionare l'icona dell'**ingranaggio** in alto a sinistra e selezionare **Impostazioni sito**.

    ![Impostazioni sito dall'icona dell'ingranaggio.](media/sharepoint-site-settings.png)

    Per impostazione predefinita, l'accesso alle applicazioni Web SharePoint viene effettuato tramite la porta 80. Ciò significa che spesso è possibile accedere a un sito di SharePoint immettendo *https://<computer name>* per aprire la raccolta siti radice.

3. In **Amministrazione raccolta siti** selezionare **Caratteristiche raccolta siti**.

4. Scorrere la pagina verso il basso fino a trovare la funzionalità **Web part Visualizzatore report**.

5. Selezionare **Attiva**.

    ![Attivare la funzionalità Web part Visualizzatore report](media/web-part-activiate-feature.png)

6. Ripetere l'operazione per le raccolte siti aggiuntive aprendo ogni sito e facendo clic su Azioni sito.

Facoltativamente è possibile usare PowerShell per abilitare questa funzionalità in tutti i siti usando il cmdlet [Enable-SPFeature](/powershell/module/sharepoint-server/Enable-SPFeature).

```
Get-SPWebApplication "<web application url>" | Get-SPSite -Limit ALL | 
        ForEach-Object {
            Write-Host "Enabling feature for $($_.URL)"
            Enable-SPFeature -identity "ReportViewerWebPart" -URL $_.URL -ErrorAction Continue
        }
```

## <a name="remove-the-solution"></a>Rimuovere la soluzione

Anche se Amministrazione centrale SharePoint consente il ritiro della soluzione, non è necessario ritirare il file **ReportViewerWebPart.wsp** a meno che non si stia eseguendo sistematicamente la risoluzione dei problemi relativi a un'installazione o alla distribuzione di una patch.

1. In **Impostazioni sistema** di Amministrazione centrale SharePoint fare clic su **Gestisci soluzioni farm**.

2. Selezionare **ReportViewerWebPart.wsp**.

3. Selezionare Ritira soluzione.

### <a name="remove-the-web-part-from-site-settings"></a>Rimuovere la web part da Impostazioni sito

Il ritiro della soluzione non rimuove la web part Visualizzatore report dall'elenco di web part del sito di SharePoint. Per rimuovere la web part Visualizzatore report seguire questa procedura.

1. Nel sito di SharePoint selezionare l'icona dell'**ingranaggio** in alto a sinistra e selezionare **Impostazioni sito**.

    ![Impostazioni sito dall'icona dell'ingranaggio.](media/sharepoint-site-settings.png)

    Per impostazione predefinita, l'accesso alle applicazioni Web SharePoint viene effettuato tramite la porta 80. Ciò significa che spesso è possibile accedere a un sito di SharePoint immettendo *https://<computer name>* per aprire la raccolta siti radice.

2. In **Raccolte Designer Web** selezionare **Web part**.

3. Selezionare l'**icona di modifica** accanto a **ReportViewerNativeMode.dwp**. Il file potrebbe non essere elencato nella prima pagina dei risultati.

4. Selezionare **Elimina elemento**.

    ![Modificare ed eliminare la web part Visualizzatore report (modalità nativa)](media/report-viewer-native-mode-edit-delete.png)

È possibile provare a eliminare la web part tramite PowerShell, ma per questa operazione non è disponibile un comando diretto. Per un esempio di script, vedere [How to delete web parts from the web part Gallery](https://gallery.technet.microsoft.com/office/How-to-delete-Web-Parts-1132701f) (Come eliminare web part dalla Raccolta web part).

## <a name="supported-languages"></a>Lingue supportate

La web part supporta le lingue seguenti:

* Inglese (en)
* Tedesco (de)
* Spagnolo (sp)
* Francese (fr)
* Italiano (it)
* Giapponese (ja)
* Coreano (ko)
* Portoghese (pt)
* Russo (ru)
* Cinese (semplificato - zh-HANS e zh-CHS)
* Cinese (tradizionale - zh-HANT e zh-CHT)

## <a name="troubleshoot"></a>Risolvere problemi

* Errore durante la disinstallazione di SSRS se è configurata la modalità integrata SharePoint:

    Install-SPRSService : [A] Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService cannot be cast to [B]Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService. Type A originates from 'Microsoft.ReportingServices.SharePoint.SharedService,Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' in the context 'Default' at location 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'. Type B originates from 'Microsoft.ReportingServices.SharePoint.SharedService,Version=12.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' in the context 'Default' at location 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'.
    
    Soluzione:
    1. Rimuovere la web part Visualizzatore report
    2. Disinstallare SSRS
    3. Reinstallare la web part Visualizzatore report

* Errore durante il tentativo di aggiornamento di SharePoint se è configurata la modalità integrata SharePoint:

    Could not load file or assembly 'Microsoft.ReportingServices.Alerting.ServiceContract, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' or one of its dependencies. Non è possibile trovare il file specificato. 00000000-0000-0000-0000-000000000000
    
    Soluzione:
    1. Rimuovere la web part Visualizzatore report
    2. Disinstallare SSRS
    3. Reinstallare la web part Visualizzatore report

## <a name="next-steps"></a>Passaggi successivi

Dopo aver completato la distribuzione e l'attivazione della web part è possibile aggiungerla a una pagina di SharePoint. Per altre informazioni, vedere [Add Report Viewer web part to a SharePoint page](add-report-viewer-web-part-to-page.md) (Aggiungere una web part Visualizzatore report a una pagina di SharePoint).

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)