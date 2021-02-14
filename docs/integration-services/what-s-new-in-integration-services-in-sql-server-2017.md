---
description: Novità&#39; di Integration Services in SQL Server 2017
title: Novità&#39; di Integration Services in SQL Server 2017 | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e26d7884-e772-46fa-bfdc-38567fe976a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b955aa61a4e196fc4c419a9b34e186590006df42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345672"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2017"></a>Novità&#39; di Integration Services in SQL Server 2017

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questo argomento descrive le funzionalità che sono state aggiunte o aggiornate in [!INCLUDE[ssSQLv14_md](../includes/sssql17-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].

> [!NOTE]
> SQL Server 2017 include anche le funzionalità di SQL Server 2016 e quelle introdotte negli aggiornamenti di SQL Server 2016. Per informazioni sulle nuove funzionalità di SSIS in SQL Server 2016, vedere [Novità di Integration Services in SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="highlights-of-this-release"></a>Elementi da evidenziare in questa versione

Di seguito sono elencate le principali nuove funzionalità di Integration Services in SQL Server 2017.

-   **Aumentare il numero di istanze**. Consente di distribuire più facilmente l'esecuzione di pacchetti SSIS su più computer del ruolo di lavoro e gestire le esecuzioni e i ruoli di lavoro da un singolo computer master. Per altre informazioni, vedere [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md).

-   **Integration Services in Linux**. Consente di eseguire pacchetti SSIS in computer Linux. Per altre informazioni, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

-   **Miglioramenti di connettività**. È possibile connettersi ai feed OData di Microsoft Dynamics AX Online e Microsoft Dynamics CRM Online grazie ai componenti OData aggiornati. 

## <a name="new-in-azure-data-factory"></a>Novità in Azure Data Factory

Con l'anteprima pubblica di Azure Data Factory versione 2 del mese di settembre 2017, è ora possibile eseguire le operazioni seguenti:
-   Distribuire i pacchetti al database del catalogo SSIS (SSISDB) nel database SQL di Azure.
-   Eseguire i pacchetti distribuiti in Azure in SSIS Integration Runtime di Azure, un componente di Azure Data Factory versione 2.

Per altre informazioni, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Per queste nuove funzionalità è necessario SQL Server Data Tools (SSDT) versione 17.2 o successiva. Non è invece richiesto SQL Server 2017 o SQL Server 2016. Quando i pacchetti vengono distribuiti in Azure, la distribuzione guidata aggiorna sempre i pacchetti al formato del pacchetto più recente.

## <a name="new-in-the-azure-feature-pack"></a>Novità nel Feature Pack di Azure

Oltre ai miglioramenti di connettività in SQL Server, in Integration Services Feature Pack per Azure è stato aumentato il supporto per Azure Data Lake Store. Per altre informazioni, vedere il post di blog [New Azure Feature Pack Release Strengthening ADLS Connectivity](https://blogs.msdn.microsoft.com/ssis/2017/08/29/new-azure-feature-pack-release-strengthening-adls-connectivity/) (Connettività ADSL potenziata con la nuova versione del Feature Pack di Azure). Vedere anche [Feature Pack di Integration Services (SSIS) per Azure](azure-feature-pack-for-integration-services-ssis.md).

## <a name="new-in-sql-server-data-tools-ssdt"></a>Novità di SQL Server Data Tools (SSDT)

È ora possibile sviluppare progetti SSIS e i pacchetti destinati alle versioni di SQL comprese tra 2012 e 2017 in Visual Studio 2017 o in Visual Studio 2015. Per altre informazioni, vedere [Scaricare SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

## <a name="new-in-ssis-in-sql-server-2017-rc1"></a>Novità di SSIS in SQL Server 2017 RC1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Funzionalità nuove e modificate in Scale Out per SSIS

-   Il master di scalabilità orizzontale supporta ora la disponibilità elevata. È possibile abilitare Always On per SSISDB e configurare Windows Server Failover Clustering per il server che ospita il servizio Scale Out Master. Applicando questa modifica a Scale Out Master, è possibile evitare un singolo punto di errore e garantire una disponibilità elevata per l'intera distribuzione di Scale Out.
-   La gestione del failover dei log di esecuzione dai ruoli di lavoro di scalabilità orizzontale è stata migliorata. I log di esecuzione vengono resi persistenti su disco locale in caso di arresto improvviso di Scale Out Worker. Al riavvio del ruolo di lavoro, i log persistenti saranno ricaricati e salvati in SSISDB.
-   Il parametro *runincluster* della stored procedure **[catalog].[create_execution]** è stato rinominato in *runinscaleout* per coerenza e leggibilità. Questa modifica del nome del parametro ha le conseguenze seguenti:
    -   Se esistono script che eseguono pacchetti in Scale Out, è necessario modificare il nome del parametro da *runincluster* a *runinscaleout* affinché gli script possano essere usati nella versione RC1.
    -   In SQL Server Management Studio (SSMS) 17.1 e nelle versioni precedenti non è possibile eseguire i pacchetti in Scale Out nella versione RC1. Messaggio di errore: " *@runincluster* non è un parametro per la procedura **create_execution**." Questo problema viene risolto nella versione 17.2 di SSMS. La versione 17.2 e successiva di SSMS supportano il nuovo nome del parametro ed eseguono i pacchetti in Scale Out. Finché la versione 17.2 di SSMS sarà disponibile, è possibile usare come soluzione alternativa la versione esistente di SSMS per generare lo script che esegue i pacchetti, modificare il nome del parametro *runincluster* in *runinscaleout* nello script ed eseguire lo script.
-   Il catalogo SSIS include una nuova proprietà globale che consente di specificare la modalità predefinita per l'esecuzione dei pacchetti SSIS. Questa nuova proprietà si applica quando si chiama la stored procedure **[catalog]. [ create_execution]** con il parametro *runinscaleout* impostato su Null. Questa modalità si applica anche ai processi di SQL Agent SSIS. È possibile impostare la nuova proprietà globale nella finestra di dialogo Proprietà per il nodo SSISDB in SSMS oppure con il comando seguente:
    ```sql
    EXEC [catalog].[configure_catalog] @property_name=N'DEFAULT_EXECUTION_MODE', @property_value=1
    ```

## <a name="new-in-ssis-in-sql-server-2017-ctp-21"></a>Novità di SSIS in SQL Server 2017 CTP 2.1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Funzionalità nuove e modificate in Scale Out per SSIS

-   È ora possibile usare il parametro **Use32BitRuntime** quando si attiva l'esecuzione in Scale Out.
-   Le prestazioni di registrazione in SSISDB dei pacchetti eseguiti in Scale Out sono state ottimizzate. I log Messaggio evento e Contesto messaggio vengono ora scritti in SSISDB in modalità batch anziché singolarmente. Di seguito alcune note aggiuntive su questo miglioramento:        
    - Attualmente alcuni report della versione corrente di SQL Server Management Studio (SSMS) non visualizzano questi registri per le esecuzioni in Scale Out. Saranno supportati nella prossima versione di SSMS. Sono interessati i report *Tutte le connessioni* e *Contesto errore* nonché la sezione *Informazioni di connessione* nel dashboard di Integration Service.
    - È stata aggiunta una nuova colonna **event_message_guid**. È possibile usare questa colonna per unire le viste [catalog].[event_message_context] e [catalog].[event_messages] anziché usare **event_message_id** quando si esegue una query di questi log di esecuzione in Scale Out.
-   Per ottenere l'applicazione di gestione per SSIS Scale Out, [scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 17,1 o versione successiva.

## <a name="new-in-ssis-in-sql-server-2017-ctp-20"></a>Novità di SSIS in SQL Server 2017 CTP 2.0

In SQL Server 2017 CTP 2.0 non sono presenti nuove funzionalità di SSIS.

## <a name="new-in-ssis-in-sql-server-2017-ctp-14"></a>Novità di SSIS in SQL Server 2017 CTP 1.4

In SQL Server 2017 CTP 1.4 non sono presenti nuove funzionalità di SSIS.

## <a name="new-in-ssis-in-sql-server-2017-ctp-13"></a>Novità di SSIS in SQL Server 2017 CTP 1.3

In SQL Server 2017 CTP 1.3 non sono presenti nuove funzionalità di SSIS.

## <a name="new-in-ssis-in-sql-server-2017-ctp-12"></a>Novità di SSIS in SQL Server 2017 CTP 1.2

In SQL Server 2017 CTP 1.2 non sono presenti nuove funzionalità di SSIS.

## <a name="new-in-ssis-in-sql-server-2017-ctp-11"></a>Novità di SSIS in SQL Server 2017 CTP 1.1

In SQL Server 2017 CTP 1.1 non sono presenti nuove funzionalità di SSIS.

## <a name="new-in-ssis-in-sql-server-2017-ctp-10"></a>Novità di SSIS in SQL Server 2017 CTP 1.0

### <a name="scale-out-for-ssis"></a>Scalabilità orizzontale per SSIS

La funzionalità di scalabilità orizzontale semplifica l'esecuzione di [!INCLUDE[ssIS_md](../includes/ssis-md.md)] su più computer. 
   
Dopo l'installazione del master e dei ruoli di lavoro di scalabilità orizzontale, il pacchetto può essere distribuito per l'esecuzione automatica in diversi ruoli di lavoro. Se l'esecuzione viene terminata in modo imprevisto, viene ritentata automaticamente. Inoltre, tutte le esecuzioni e i ruoli di lavoro possono essere gestiti centralmente tramite Master.
   
Per altre informazioni, vedere [Scalabilità orizzontale di Integration Services](../integration-services/scale-out/integration-services-ssis-scale-out.md).
   
### <a name="support-for-microsoft-dynamics-online-resources"></a>Supporto per le risorse di Microsoft Dynamics Online

L'origine OData e la gestione connessione OData supportano ora la connessione ai feed OData di Microsoft Dynamics AX Online e Microsoft Dynamics CRM Online.