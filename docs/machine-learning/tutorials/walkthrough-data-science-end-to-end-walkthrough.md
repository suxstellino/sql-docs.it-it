---
title: 'Esercitazione su R: Sviluppare un modello in SQL'
description: Informazioni su come creare una soluzione end-to-end per la modellazione predittiva basata sul supporto delle funzionalità R in SQL Server 2016 o SQL Server 2017.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/11/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: d9bfe9ec4f81ca61bacbb4dc3526ea5b7a1a87f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272962"
---
# <a name="tutorial-sql-development-for-r-data-scientists"></a>Esercitazione: Sviluppo SQL per data scientist R
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Questa esercitazione per data scientist illustra come creare una soluzione end-to-end per la modellazione predittiva basata sul supporto delle funzionalità R in SQL Server 2016 o SQL Server 2017. In questa esercitazione viene usato il database [NYCTaxi_sample](demo-data-nyctaxi-in-sql.md) in SQL Server. 

Una combinazione di codice R, dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e funzioni SQL personalizzate viene usata per creare un modello di classificazione che indica la probabilità che l'autista riceva una mancia per una determinata corsa. Inoltre, il modello R viene distribuito in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i dati del server vengono usati per generare punteggi basati sul modello.

Questo esempio può essere esteso a tutti i tipi di problemi della vita reale, ad esempio alla previsione delle risposte dei clienti alle campagne di vendita o alla stima delle spese o della partecipazione a eventi. Poiché il modello può essere richiamato da una stored procedure, è possibile incorporarlo facilmente in un'applicazione.

Poiché la procedura dettagliata è pensata per far conoscere [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] agli sviluppatori R, R viene usato il più possibile. Ciò non significa però che R sia necessariamente lo strumento migliore per ogni attività. In molti casi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe offrire prestazioni migliori, in particolare in attività quali l'aggregazione dei dati e la progettazione delle funzionalità.  Tali attività possono trarre vantaggio dalle nuove funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio gli indici columnstore con ottimizzazione per la memoria. Le ottimizzazioni possibili vengono evidenziate man mano.

## <a name="prerequisites"></a>Prerequisiti

+ [Machine Learning Services per SQL Server con integrazione di R](../install/sql-machine-learning-services-windows-install.md#verify-installation) o [R Services per SQL Server 2016](../install/sql-r-services-windows-install.md)

+ [Autorizzazioni per il database](../security/user-permission.md) e un account di accesso per un database di SQL Server

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ [Database demo NYC Taxi](demo-data-nyctaxi-in-sql.md)

+ Un ambiente IDE R come RStudio oppure lo strumento RGUI predefinito incluso con R

È consigliabile eseguire questa procedura dettagliata in una workstation client. È necessario essere in grado di connettersi, nella stessa rete, a un computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con SQL Server e il linguaggio R abilitato. Per istruzioni sulla configurazione della workstation, vedere [Configurare un client di data science per lo sviluppo in R](../r/set-up-a-data-science-client.md).

In alternativa, è possibile eseguire la procedura dettagliata in un computer che dispone sia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che di un ambiente di sviluppo R, ma questa configurazione non è consigliata per un ambiente di produzione. Se è necessario collocare client e server nello stesso computer, assicurarsi di installare un secondo set di librerie Microsoft R per l'invio di script R da un client "remoto". Non usare le librerie R installate nella cartella Programmi dell'istanza di SQL Server. In particolare, se si usa un solo computer, per supportare le operazioni client e server è necessaria la libreria RevoScaleR in entrambi i percorsi.

+ C:\Programmi\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Programmi\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

> [!NOTE]
> Se si usa [Machine Learning Server](/machine-learning-server/) o [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/) anziché il cliente R, il percorso di RevoScaleR è C:\Programmi\Microsoft\ML Server\R_SERVER\library\RevoScaleR

<a name="add-packages"></a>

## <a name="additional-r-packages"></a>Pacchetti R aggiuntivi

Questa procedura dettagliata richiede alcune librerie R che non sono installate per impostazione predefinita come parte di [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. È necessario installare tali pacchetti sia nel client nel quale si svilupperà la soluzione, sia nel computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel quale si distribuirà la soluzione stessa.

### <a name="on-a-client-workstation"></a>In una workstation client

Nell'ambiente R copiare le righe seguenti ed eseguire il codice in una finestra della console (Rgui o IDE). Alcuni pacchetti installano anche i pacchetti necessari. In tutto vengono installati circa 32 pacchetti. Per completare questo passaggio è necessaria una connessione Internet.
    
  ```R
  # Install required R libraries, if they are not already installed.
  if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
  if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
  if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
  if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
  ```

### <a name="on-the-server"></a>Nel server

Sono disponibili diverse opzioni per l'installazione di pacchetti in SQL Server. Ad esempio, SQL Server fornisce una [funzionalità di gestione dei pacchetti R](../package-management/install-additional-r-packages-on-sql-server.md) che consente agli amministratori di database di creare un repository di pacchetti e assegnare all'utente i diritti per installare i propri pacchetti. Tuttavia, se si è un amministratore del computer, è possibile installare nuovi pacchetti usando R, facendo attenzione a installarli nella libreria corretta.

> [!NOTE]
> Nel server **non** installare in una libreria utente, anche se viene proposto a schermo. Se si installa in una libreria utente, l'istanza di SQL Server non sarà in grado di trovare o eseguire i pacchetti. Per altre informazioni, vedere [Installare nuovi pacchetti in SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

1. Nel computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aprire RGui.exe **come amministratore**.  Se R Services per SQL Server è stato installato con le impostazioni predefinite, Rgui.exe è disponibile in C:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64.

2. Al prompt di R, eseguire i comandi R seguenti:
  
  ```R
  install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  ```
  In questo esempio la funzione grep di R viene usata per cercare il vettore di percorsi disponibili e trovare quello che include "Program Files". Per altre informazioni, vedere [https://www.rdocumentation.org/packages/base/functions/grep](https://www.rdocumentation.org/packages/base/functions/grep).

  Se si ritiene che i pacchetti siano già installati, controllare l'elenco dei pacchetti installati eseguendo `installed.packages()`.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Esplorare e riepilogare i dati](walkthrough-view-and-summarize-data-using-r.md)