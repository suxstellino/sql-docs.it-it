---
title: Abilitare gli eventi di Reporting Services per il log di traccia di SharePoint (ULS) | Microsoft Docs
description: Informazioni su come attivare la scrittura di eventi di Reporting Services nel log di traccia ULS di SharePoint per i server di Reporting Services in modalità SharePoint.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 81110ef6-4289-405c-a931-e7e9f49e69ba
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 73a806d2f86cc155c0aa9d70c4fa0f9e01aa2b59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023346"
---
# <a name="turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls"></a>Abilitare gli eventi di Reporting Services per il log di traccia di SharePoint (ULS)

  A partire da [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], tramite i server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint è possibile scrivere gli eventi di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nel log di traccia del Servizio di registrazione unificato (ULS, Unified Logging Service) di SharePoint. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sono disponibili nella pagina di monitoraggio di Amministrazione centrale SharePoint.  
  
 Contenuto dell'argomento:  
  
-   [Indicazioni generali per il log ULS](#bkmk_general)  
  
-   [Per abilitare e disabilitare gli eventi di Reporting Services nella categoria Reporting Services](#bkmk_turnon)  
  
-   [Configurazione consigliata](#bkmk_recommended)  
  
-   [Lettura delle voci di log](#bkmk_readentries)  
  
-   [Elenco di eventi di SQL Server Reporting Services](#bkmk_list)  
  
-   [Visualizzare un file di log con PowerShell](#bkmk_powershell)  
  
-   [Percorso del log di traccia](#bkmk_trace)  
  
##  <a name="general-uls-log-recommendations"></a><a name="bkmk_general"></a> Indicazioni generali per il log ULS  
 Nella tabella seguente sono elencate le categorie e i livelli degli eventi consigliati per il monitoraggio di un ambiente [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Quando un evento viene registrato, in ogni voce sono inclusi l'ora di registrazione dell'evento, il nome del processo e l'ID del thread.  
  
|Category|Level|Descrizione|  
|--------------|-----------|-----------------|  
|Database|Dettagliato|Registra eventi che comportano l'accesso al database.|  
|Generale|Dettagliato|Registra eventi che comportano l'accesso agli elementi seguenti:<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pagine Web<br /><br /> Gestore HTTP Visualizzatore report<br /><br /> Accesso al report (file con estensione rdl)<br /><br /> Origini dati (file con estensione rsds)<br /><br /> URL nel sito di SharePoint (file con estensione smdl)|  
|Office Server General|Eccezione|Registra errori di accesso.|  
|Topologia|Dettagliato|Registra informazioni sull'utente corrente|  
|web part|Dettagliato|Consente di registrare eventi che comportano l'accesso alla web part Visualizzatore report.|  
  
##  <a name="to-turn-on-and-off-reporting-services-events-in-the-reporting-services-category"></a><a name="bkmk_turnon"></a> Per abilitare e disabilitare gli eventi di Reporting Services nella categoria Reporting Services  
  
1.  Da Amministrazione centrale SharePoint  
  
2.  Fare clic su **Monitoraggio**.  
  
3.  Fare clic su **Configura registrazione diagnostica** nel gruppo **Report** .  
  
4.  Individuare **SQL Server Reporting Services** nell'elenco delle categorie.  
  
5.  Fare clic sul segno più (+) per espandere le sottocategorie in **SQL Server Reporting Services**.  
  
6.  Selezionare le sottocategorie da aggiungere al log di traccia.  
  
7.  Nella parte inferiore dell'elenco di categorie selezionare un livello di evento per **Evento meno critico da includere nel registro di traccia**. Selezionare **Nessuno** per disabilitare la traccia.  
  
> [!NOTE]  
>  L'opzione **Evento meno critico da includere nel registro eventi** non è supportata da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. L'opzione verrà ignorata.  
  
##  <a name="recommended-configuration"></a><a name="bkmk_recommended"></a> Configurazione consigliata  
 Le opzioni di registrazione seguenti sono consigliate come configurazione standard:  
  
-   **Redirector HTTP**  
  
-   **Proxy client SOAP**  
  
-   Se si verificano dei problemi durante la configurazione, aggiungere **Pagine di configurazione**.  
  
 È possibile rivedere tutte le impostazioni del registro diagnostico della farm correnti con il cmdlet di PowerShell seguente:  
  
```  
Get-SPDiagnosticConfig  
```  
  
##  <a name="reading-the-logs-entries"></a><a name="bkmk_readentries"></a> Lettura delle voci di log  
 Le voci di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nel log vengono formattate come indicato di seguito.  
  
1.  **Prodotto:SQL Server Reporting Services**  
  
2.  **Categoria:** per gli eventi correlati al server viene aggiunta al nome l'indicazione "server di report". Ad esempio, "Runtime avvisi server di report". Questi eventi vengono registrati anche nei file di log del server di report.  
  
3.  **Categoria:** gli eventi correlati a o comunicati da un componente front-end Web non contengono l'indicazione "server di report". Ad esempio, "Proxy applicazione del servizio" Runtime avvisi server di report". Le voci relative al front-end Web contengono un valore CorrelationID, mentre le voci relative al server no.  
  
##  <a name="list-of-sql-server-reporting-services-events"></a><a name="bkmk_list"></a> Elenco di eventi di SQL Server Reporting Services  
 Nella tabella seguente sono elencati gli eventi nella categoria SQL Server Reporting Services:  
  
|Nome area|Descrizione o voci di esempio|  
|---------------|-----------------------------------|  
|Pagine di configurazione||  
|Redirector HTTP||  
|Elaborazione in modalità locale||  
|Rendering in modalità locale||  
|Proxy client SOAP||  
|Pagine dell'interfaccia utente||  
|Power View|Voci di log scritte nell'API **LogClientTraceEvents** . Queste voci vengono originate dalle applicazioni client, incluso Power View, una funzionalità del componente aggiuntivo di SQL Server Reporting Services.<br /><br /> Tutte le voci di log dell'API LogClientTraceEvents saranno registrate nella **categoria** di "SQL Server Reporting Services" e nell'**area** di "Power View".<br /><br /> Il contenuto di voci registrate con l'area di "Power View" è determinato dall'applicazione client.|  
|Runtime avvisi server di report||  
|Strumento gestione dominio applicazione del server di report||  
|Risposta nel buffer del server di report||  
|Cache del server di report||  
|Catalogo del server di report||  
|Blocco del server di report||  
|Pulizia server di report||  
|Gestione configurazione server di report|Voci di esempio:<br /><br /> Url interno del server di report MediumUsing `https://localhost:80/ReportServer`.<br /><br /> UnexpectedMissing o impostazione ExtendedProtectionLevel non valida|  
|Crittografia server di report||  
|Estensione per i dati del server di report||  
|Polling DB server di report||  
|Impostazione predefinita per server di report||  
|Report Server Email Extension||  
|Renderer di Excel per server di report||  
|Factory dell'estensione per server di report||  
|Runtime HTTP del server di report||  
|Renderer di immagini per il server di report||  
|Monitoraggio memoria del server di report||  
|Notifica del server di report||  
|Elaborazione del server di report||  
|Provider del server di report||  
|Rendering del server di report||  
|Anteprima report del server di report||  
|Utilità risorse server di report|Voci di esempio:<br /><br /> Servizi MediumReporting di avvio SKU: Versione di valutazione<br /><br /> Copia di MediumEvaluation: scadenza tra 180 giorni|  
|Processi in esecuzione del server di report||  
|Richieste in esecuzione del server di report||  
|Pianificazione del server di report||  
|Sicurezza del server di report||  
|Controller servizio server di report||  
|Sessione server di report||  
|Sottoscrizione del server di report||  
|Runtime WCF del server di report||  
|Server Web del server di report||  
|Proxy applicazione del servizio||  
|Servizio condiviso|Voci di esempio:<br /><br /> MediumUpdating ReportingWebServiceApplication<br /><br /> Accesso MediumGranting ai database di contenuto.<br /><br /> Istanze MediumProvisioning per ReportingWebServiceApplication<br /><br /> Modifica dell'account del servizio MediumProcessing per ReportingWebServiceApplication<br /><br /> Autorizzazioni per database MediumSetting.|  
  
##  <a name="view-a-log-file-with-powershell"></a><a name="bkmk_powershell"></a> Visualizzare un file di log con PowerShell  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell")È possibile usare PowerShell per restituire un elenco di eventi correlati a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da un file di log ULS. Digitare il comando seguente dalla shell di gestione SharePoint 2010 per ottenere un elenco filtrato di righe del file di log ULS, UESQL11SPOINT-20110606-1530.log, contenenti "**sql server reporting services**":  
  
```  
Get-content -path "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\LOGS\UESQL11SPOINT-20110606-1530.log" | select-string "sql server reporting services"  
```  
  
 Sono disponibili anche strumenti scaricabili che consentono di leggere i log ULS, ad esempio il [visualizzatore log di SharePoint](https://github.com/hasankhan/SharePointLogViewer), disponibile in GitHub. 
  
 Per altre informazioni sull'uso di PowerShell per visualizzare dati del log, vedere [Visualizzare i log diagnostici (SharePoint Server 2010)](/SharePoint/administration/view-diagnostic-logs)  
  
##  <a name="trace-log-location"></a><a name="bkmk_trace"></a> Percorso del log di traccia  
 I file dei log di traccia si trovano in genere nella cartella **c:\Programmi\Common files\Microsoft Shared\Web Server Extensions\14\logs** , ma è possibile verificare o modificare il percorso dalla pagina **Registrazione diagnostica** in Amministrazione centrale SharePoint.  
  
 Per altre informazioni e istruzioni per la configurazione della registrazione diagnostica in un server SharePoint in Amministrazione centrale SharePoint 2010, vedere [Configurare le impostazioni della registrazione diagnostica (Windows SharePoint Services)](/previous-versions/office/sharepoint-2007-products-and-technologies/cc288649(v=office.12)).  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)