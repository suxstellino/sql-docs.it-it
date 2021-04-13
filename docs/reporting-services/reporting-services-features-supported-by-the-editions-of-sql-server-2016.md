---
title: Funzionalità supportate dalle diverse edizioni - SQL Server Reporting Services | Microsoft Docs
description: Informazioni sulle funzionalità di SQL Server Reporting Services (SSRS) supportate dalle diverse edizioni di SQL Server.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 12/16/2019
ms.openlocfilehash: a818db1a2bc8626f19bd3f98abdbb721ccbd74b1
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072399"
---
# <a name="sql-server-reporting-services-features-supported-by-editions"></a>Funzionalità di SQL Server Reporting Services supportate nelle diverse edizioni

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Questo argomento illustra le funzionalità di SQL Server Reporting Services (SSRS) supportate dalle diverse edizioni di SQL Server. SQL Server Evaluation Edition è disponibile per un periodo di valutazione di 180 giorni.  

## <a name="related-links"></a>Collegamenti correlati
  
 - [Note sulla versione per SQL Server Reporting Services (SSRS)](release-notes-reporting-services.md). 
 - [Novità di SQL Server Reporting Services (SSRS)](~/reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).
 - [Funzionalità supportate dalle edizioni di SQL Server](~/sql-server/editions-and-components-of-sql-server-version-15.md)

##  <a name="sql-server-reporting-services"></a><a name="SSRS"></a> SQL Server Reporting Services  

Per le funzionalità supportate dalle edizioni Evaluation e Developer, vedere la colonna dell'edizione SQL Server Enterprise nella tabella seguente.

|Nome funzionalità|Enterprise|Standard|Web|Express with Advanced Services|Developer|  
|------|---------|---------------|-----------|-------|---------|  
| Report di Power BI e cartelle di lavoro di Excel | Sì, con Software Assurance | | | | Sì |
|Report per dispositivi mobili e analisi|Sì||||Sì|  
|Edizione supportata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per il database del catalogo|Standard o versione successiva|Standard o versione successiva|Web|Express|Standard o versione successiva|  
|Edizione supportata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per le origini dati|Tutte le edizioni di   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Tutte le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Web|Express|Tutte le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|  
|Server di report|Sì|Sì|Sì|Sì|Sì|  
|Progettazione report|Sì|Sì|Sì|Sì|Sì|  
|Portale Web di Progettazione report|Sì|Sì|Sì|Sì|Sì|  
|Sicurezza basata sui ruoli|Sì|Sì|Sì|Sì|Sì|  
|Esportazione in Excel, PowerPoint, Word, PDF e immagini|Sì|Sì|Sì|Sì|Sì|  
|Misuratori e grafici migliorati|Sì|Sì|Sì|Sì|Sì|  
|Aggiungere elementi di un report a dashboard di [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]|Sì|Sì|Sì|Sì|Sì|  
|Autenticazione personalizzata|Sì|Sì|Sì||Sì|  
|Dati del report come feed di dati|Sì|Sì|Sì|Sì|Sì|  
|Supporto modelli|Sì|Sì|Sì||Sì|  
|Creare ruoli personalizzati per la sicurezza basata sui ruoli|Sì|Sì|||Sì|  
|Sicurezza degli elementi del modello|Sì|Sì|||Sì|  
|Click-through illimitato|Sì|Sì|||Sì|  
|Libreria dei componenti condivisi|Sì|Sì|||Sì|  
|Sottoscrizioni e pianificazione posta elettronica e condivisione file|Sì|Sì|||Sì|  
|Cronologia, snapshot di esecuzione e memorizzazione nella cache dei report|Sì|Sì|||Sì|  
|Integrazione con SharePoint<sup>2</sup>|Sì|Sì|||Sì|  
|Supporto origini dati remote e non SQL<sup>1</sup>|Sì|Sì|||Sì|  
|Estensibilità RDCE per il rendering, il recapito e le origini dati|Sì|Sì|||Sì|  
|Personalizzazione|Sì||||Sì|  
|Sottoscrizione guidata dai dati dei report|Sì||||Sì|  
|Distribuzione con scalabilità orizzontale (Web farm)|Sì||||Sì|  
|Avvisi<sup>2</sup> (SSRS 2016) |Sì||||Sì|  
|Power View<sup>2</sup> (SSRS 2016) |Sì||||Sì| 
|Commenti<sup>3</sup> |Sì|Sì|Sì|Sì|Sì|  

 <sup>1</sup> Per altre informazioni sulle origini dati supportate in SQL Server Reporting Services (SSRS), vedere [Origini dei dati supportate da Reporting Services &#40;SSRS&#41;](../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
 <sup>2</sup> Richiede SQL Server 2016 Reporting Services in modalità SharePoint. Per altre informazioni, vedere [Installare la modalità SharePoint di Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md). A partire da SQL Server 2017 Reporting Services, l'integrazione con SharePoint non è più disponibile. 

<sup>3</sup> Solo nel Server di report di Power BI e in SQL Server 2017 Reporting Services e versioni successive.

> [!NOTE]
> SQL Server Express e SQL Server Express with Tools non supportano SQL Server Reporting Services.
  
## <a name="edition-requirements-for-the-report-server-database"></a>Requisiti relativi all'edizione per il database del server di report
 Quando si crea un database del server di report, non tutte le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possono essere usate per ospitare il database. Nella tabella seguente sono illustrate le edizioni del [!INCLUDE[ssDE](../includes/ssde-md.md)] che è possibile usare per le edizioni specifiche di SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)].  
  
|Edizione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Reporting Services|Edizione dell'istanza del motore di database da usare per ospitare il database.|  
|----------------------------------------------------------------------|---------------------------------------------------------------------------|  
|Enterprise|Edizioni Enterprise o Standard (locali o remote)|  
|Standard|Edizioni Enterprise o Standard (locali o remote)|  
|Web|Web Edition (solo locale)|  
|Express with Advanced Services|Express with Advanced Services (solo locale)|  
|Versione di valutazione|Versione di valutazione|  
  
##  <a name="business-intelligence-clients"></a><a name="BIC"></a> Client di business intelligence  
Le applicazioni client software seguenti sono disponibili nell'Area download Microsoft. Consentono di creare documenti di business intelligence eseguiti in un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Quando questi documenti vengono ospitati in un ambiente server, usare un'edizione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supportata per tale tipo di documento. Nella tabella seguente viene indicata l'edizione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contenente le funzionalità del server richieste per ospitare i documenti creati in queste applicazioni client.  
  
|Nome dello strumento|Enterprise|Standard|Web|Express with Advanced Services|Developer|  
|---------------|----------------|--------------|------------------------|-------------|---------------| 
| Power BI Desktop ottimizzato per il server di report di Power BI, **PBIX** | Sì, con Software Assurance | | | | Sì |
|[!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)], **.rdl** e **.rds**|Sì|Sì|Sì|Sì|Sì|  
|[!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)], **.rsmobile**|Sì||||Sì|  
|App di Power BI per dispositivi mobili (iOS, Windows 10, Android), **.rsmobile**|Sì||||Sì|  
  
> [!NOTE]  
> * Nella tabella precedente vengono identificate le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] necessarie per abilitare questi strumenti client. Tuttavia, questi strumenti possono accedere ai dati ospitati in qualsiasi edizione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
> * [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] è il punto singolo per la creazione di report per dispositivi mobili. Connettersi a un server SSRS per accedere alle origini dati e creare report. Pubblicare quindi i report nel server SSRS per consentire l'accesso ad altri utenti dell'organizzazione, nel server o nei dispositivi mobili. È anche possibile usare [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] autonomo con origini dati locali.  
> * Se si usa [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] in locale, [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] nel cloud o entrambi come soluzione di recapito dei report, è necessaria una sola app per dispositivi mobili per accedere a dashboard e report sui dispositivi mobili. Le app di [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] sono disponibili per il download dall'App Store di Windows, iOS o Android.  

## <a name="next-steps"></a>Passaggi successivi

* Vedere [Funzionalità supportate dalle edizioni di SQL Server 2017](~/sql-server/editions-and-components-of-sql-server-2017.md). 

* [Pianificare un'installazione di SQL Server](../sql-server/install/planning-a-sql-server-installation.md).

* Altre domande? Usare il [forum di SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
