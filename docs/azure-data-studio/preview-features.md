---
title: Funzionalità di anteprima in Azure Data Studio
description: Informazioni sulle funzionalità di anteprima in Azure Data Studio e istruzioni su come abilitarle e usarle.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: hannahqin
ms.author: hanqin
ms.reviewer: maghan
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: bccf25686c397266a4cee3c44819b08ee301569b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040127"
---
# <a name="preview-features-in-azure-data-studio"></a>Funzionalità di anteprima in Azure Data Studio

In Azure Data Studio, le nuove funzionalità e i miglioramenti vengono spesso rilasciati come funzionalità di anteprima prima di essere resi disponibili a livello generale (GA). La quantità di tempo in cui una funzionalità rimane in anteprima può variare in base al feedback degli utenti, ai controlli qualitativi e ai piani d'azione a lungo termine. Abilitando le funzionalità di anteprima, si ottiene l'accesso completo alle funzionalità di Azure Data Studio e si ha la possibilità di fornire feedback tempestivo.

## <a name="how-do-i-enable-preview-features"></a>Come abilitare le funzionalità di anteprima?

### <a name="on-first-launch"></a>Al primo avvio

Se si è un nuovo utente, è possibile acconsentire esplicitamente ad abilitare le funzionalità di anteprima quando si avvia Azure Data Studio per la prima volta. All'avvio, viene visualizzata una notifica di tipo avviso popup nell'angolo in basso a destra della schermata che consente di abilitare o disabilitare le funzionalità di anteprima. Selezionare **Sì (scelta consigliata)** per abilitare le funzionalità di anteprima.

![Notifica di tipo avviso popup per la funzionalità di anteprima al primo avvio](./media/getting-started/preview-toast-notification.png)

### <a name="in-settings"></a>Nelle impostazioni

È possibile abilitare o disabilitare le funzionalità di anteprima in qualsiasi momento nelle impostazioni.

1. Selezionare l'icona a forma di **ingranaggio** nell'angolo in basso a sinistra e quindi scegliere **Impostazioni** dal menu di scelta rapida. Verrà aperta la scheda Impostazioni.

   ![Icona dell'ingranaggio per accedere alle impostazioni in ADS](./media/settings/open-settings-menu.png)

2. Digitare "abilita funzionalità di anteprima" nella barra di ricerca.

3. Per abilitare le funzionalità di anteprima, selezionare la casella di controllo **Abilita le funzionalità di anteprima non rilasciate** in **Workbench: Abilita funzionalità di anteprima**. Per disabilitare le funzionalità di anteprima, deselezionare la casella di controllo.

   ![Abilitare l'impostazione delle funzionalità di anteprima in ADS](./media/settings/preview-features-settings.png)

## <a name="list-of-preview-features-in-azure-data-studio"></a>Elenco delle funzionalità di anteprima in Azure Data Studio

### <a name="general-features-in-preview"></a>Funzionalità generali in anteprima

* Integrazione del portale di Azure
* Backup/ripristino
* Deployments
    * SQL Edge
    * Cluster Big Data di SQL Server
    * Immagine del contenitore di SQL Server
    * SQL Server in Windows
* Tour delle funzionalità
*  Modalità SQLCMD
* Nuova pagina iniziale

### <a name="notebook-features-in-preview"></a>Funzionalità dei notebook in anteprima

* Supporto interattivo Dotnet
* Barra degli strumenti Markdown
*  Nuova barra degli strumenti Notebook
* Kernel dei notebook
    * Kusto
    * PowerShell
    * PySpark
    * Python
    * Spark | Scala
    * Spark | R
    * SQL
* Apertura di notebook dal browser
* Notebook bloccati
* Procedura guidata delle dipendenze Python

### <a name="first-party-extensions-in-preview"></a>Estensioni proprietarie in anteprima

* Admin Pack per SQL Server
* Informazioni dettagliate di Azure Synapse Analytics
* Server di gestione centrale
* Estensione Database Administration Tool per Windows
* Kusto
* Language Pack
* PostgreSQL
* PowerShell
* Cronologia query
* SandDance per Azure Data Studio
* Report del server
* SQL Assessment
* SQL Server Agent
* SQL Server Profiler
* Machine Learning
* Managed Instance Dashboard
* Visual Studio IntelliCode
* whoisactive

## <a name="next-steps"></a>Passaggi successivi

* [Azure Data Studio](what-is-azure-data-studio.md)
