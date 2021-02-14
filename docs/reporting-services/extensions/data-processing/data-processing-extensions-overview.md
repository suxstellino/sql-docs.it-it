---
title: Panoramica delle estensioni per l'elaborazione dati | Microsoft Docs
description: Informazioni sulle estensioni per l'elaborazione dati incluse in Reporting Services e su come aggiungere l'elaborazione dati personalizzata al server di report.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], about extensions
ms.assetid: 1d652605-9313-4c75-98b4-ba4dcbbb222d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 61c30aa9214b87e9acf74dc12d71dd9e8e6d6fa5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061186"
---
# <a name="data-processing-extensions-overview"></a>Cenni preliminari sulle estensioni per l'elaborazione dati
  Le estensioni per l'elaborazione dati in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] consentono di eseguire la connessione a un'origine dati e di recuperare i dati. Fungono inoltre da ponte tra un'origine dati e un set di dati. Le estensioni per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sono modellate in base a un subset delle interfacce dei provider di dati [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
 Nella tabella seguente sono elencate le estensioni per l'elaborazione dati incluse in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Estensione per l'elaborazione dati|Descrizione|  
|-------------------------------|-----------------|  
|Estensione per l'elaborazione dati per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Utilizza il provider di dati .NET Framework per SQL Server per la connessione a e il recupero dei dati da [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)].|  
|Estensione per l'elaborazione dati per OLE DB|Usa il provider di dati .NET Framework per OLE DB. Con questa estensione, il server di report può eseguire una query su qualsiasi origine dati che dispone di un provider OLE DB.|  
|Estensione per l'elaborazione dati per Oracle|Usa il provider di dati .NET Framework per Oracle. Con questa estensione, il server di report può accedere alle origini dati Oracle tramite software di connettività client Oracle.|  
|Estensione per l'elaborazione dati per ODBC|Usa il provider di dati .NET Framework per ODBC. Con questa estensione, il server di report può accedere ai dati in qualsiasi database per il quale è disponibile un driver ODBC.|  
  
 È possibile utilizzare le API di elaborazione dati di [!INCLUDE[ssRS](../../../includes/ssrs.md)] per aggiungere funzionalità personalizzate di elaborazione dati al server di report.  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] offre supporto predefinito per i provider di dati in [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Se è già stato implementato un provider di dati completo, non è necessario implementare un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. È tuttavia consigliabile considerare di estendere il provider di dati per includere le funzionalità specifiche di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2005, tra cui credenziali di connessione protette e aggregazioni sul lato server.  
  
 Ognuna delle estensioni per l'elaborazione dati incluse in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] utilizza un set comune di interfacce. Questo garantisce che ogni estensione implementi funzionalità simili.  
  
 È possibile sviluppare estensioni per l'elaborazione dati per le proprie origini dati oppure è possibile utilizzare le interfacce per aggiungere un ulteriore livello di elaborazione dati alle infrastrutture di database comuni. È possibile distribuire le estensioni per l'elaborazione dati personalizzate per consentire un'agevole integrazione dei dati nei server di report esistenti nell'organizzazione. È inoltre possibile utilizzarle come parte di una famiglia di prodotti di creazione di report personalizzati forniti agli utenti.  
  
 ![Architettura dell'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/media/bk-dataprocess-extensions.gif "Architettura dell'estensione per l'elaborazione dati")  
Architettura delle estensioni per l'elaborazione dati di Reporting Services  
  
 I vantaggi dell'implementazione di un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] personalizzata includono:  
  
-   Architettura di accesso ai dati semplificata, che spesso offre maggiore semplicità di gestione e prestazioni migliorate.  
  
-   Possibilità di esporre direttamente le funzionalità specifiche dell'estensione agli utenti.  
  
-   Interfaccia specifica per consentire agli utenti di accedere all'origine dati da [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
## <a name="data-extension-process-flow"></a>Flusso di processo dell'estensione per i dati  
 Prima di sviluppare un'estensione per i dati personalizzata, è necessario capire in che modo le estensioni per i dati vengono utilizzate dal server di report per elaborare i dati. È inoltre necessario comprendere i costruttori e i metodi chiamati dal server di report.  
  
 ![Flusso di processo per l'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/media/bk-ext-01.gif "Flusso di processo per l'estensione per l'elaborazione dati")  
Flusso di processo dettagliato di un'estensione per i dati chiamata dal server di report  
  
 Nella figura è illustrata la sequenza di eventi seguente:  
  
1.  Il server di report crea un oggetto connessione e passa la stringa di connessione e le credenziali associate al report.  
  
2.  Il testo del comando del report viene utilizzato per creare un oggetto comando. Nel processo, l'estensione per l'elaborazione dati può includere codice che consente di analizzare il testo del comando e creare i parametri per il comando.  
  
3.  Dopo che l'oggetto comando e i parametri sono stati elaborati, viene generato un lettore di dati che restituisce un set di risultati e consente al server di report di associare i dati del report al layout del report.  
  
## <a name="developer-requirements"></a>Requisiti per lo sviluppatore  
 Per lo sviluppo di un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è necessario disporre di quanto segue:  
  
-   Un computer di distribuzione in cui sia installato Progettazione report o un server di report.  
  
-   Un computer di sviluppo in cui sia installato [!INCLUDE[vsprvsext](../../../includes/vsprvsext-md.md)] o versioni successive o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] Software Development Kit (SDK).  
  
-   Conoscenza approfondita delle caratteristiche e delle funzionalità di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
-   Conoscenza approfondita dell'architettura di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vstecado](../../../includes/vstecado-md.md)], dei provider di dati [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], degli oggetti DataSet ADO.NET e delle interfacce [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] comuni.  
  
-   Esperienza di sviluppo in un linguaggio [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], ad esempio [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
