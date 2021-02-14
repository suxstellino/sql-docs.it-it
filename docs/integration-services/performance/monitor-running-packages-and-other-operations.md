---
description: Esecuzione di pacchetti e altre operazioni di monitoraggio
title: Monitorare l'esecuzione dei pacchetti e altre operazioni | Microsoft Docs
ms.custom: supportability
ms.date: 06/04/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isoperations.executions.f1
- sql13.ssis.ssms.isoperations.general.f1
ms.assetid: cbbcd79f-ab9b-46ec-84cb-4821c1d16b99
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2716b85dde5aaa7a7e2df15e87004c838b8137ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353024"
---
# <a name="monitor-running-packages-and-other-operations"></a>Esecuzione di pacchetti e altre operazioni di monitoraggio

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile monitorare esecuzioni di pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , convalide di progetto e altre operazioni di usando uno o più strumenti tra quelli indicati di seguito. Alcuni strumenti, tra cui le scelte dei dati, sono disponibili solo per i progetti distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Log  
  
     Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
-   Report  
  
     Per altre informazioni, vedere [report per il server Integration Services](#reports).  
  
-   Viste  
  
     Per altre informazioni, vedere [viste &#40;Catalogo di Integration Services&#41;](../../integration-services/system-views/views-integration-services-catalog.md).  
  
-   Contatori delle prestazioni  
  
     Per altre informazioni, vedere [i contatori delle prestazioni](../../integration-services/performance/performance-counters.md).  
  
-   Scelte dei dati  

> [!NOTE]
> In questo articolo viene illustrato come monitorare i pacchetti SSIS in esecuzione a livello generale e come monitorare i pacchetti in esecuzione in locale. È anche possibile eseguire e monitorare i pacchetti SSIS nel database SQL di Azure. Per altre informazioni, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Benché sia possibile anche eseguire pacchetti SSIS in Linux, in Linux non vengono offerti strumenti di monitoraggio. Per altre informazioni, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="operation-types"></a>Tipi di operazione  
 Nel catalogo **SSISDB** vengono monitorati numerosi tipi diversi di operazioni nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . A ogni operazione possono essere associati più messaggi. Ogni messaggio può essere classificato in uno dei molti tipi diversi. Ad esempio, un messaggio può essere informativo, di avviso o di errore. Per l'elenco completo dei tipi di messaggi, vedere la documentazione relativa alla vista Transact-SQL [catalog.operation_messages &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Per l'elenco completo dei tipi di operazioni, vedere [catalog.operations &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  
  
 Nove diversi tipi di stato consentono di indicare lo stato di un'operazione. Per l'elenco completo dei tipi di stato, vedere la vista [catalog.operations &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  

## <a name="active-operations-dialog-box"></a><a name="active_ops"></a> Finestra di dialogo Operazioni attive
  Utilizzare la finestra di dialogo **Operazioni attive** per visualizzare lo stato delle operazioni di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] attualmente in esecuzione nel server di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ad esempio distribuzione, convalida ed esecuzione dei pacchetti. Questi dati vengono archiviati nel catalogo SSISDB.  
  
 Per altre informazioni sulle viste [!INCLUDE[tsql](../../includes/tsql-md.md)] correlate, vedere[catalog.operations &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md), [catalog.validations &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) e [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
###  <a name="open-the-active-operations-dialog-box"></a><a name="open_dialog"></a> Apertura della finestra di dialogo Operazioni attive  
  
1.  Aprire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Connettersi al motore di database di Microsoft SQL Server.  
  
3.  In Esplora oggetti espandere il nodo **Integration Services** , fare clic con il pulsante destro del mouse su **SSISDB**, quindi fare clic su **Operazioni attive**.  
  
### <a name="configure-the-options"></a>Configurare le opzioni  
  
 **Tipo**  
 Consente di specificare il tipo di operazione. Di seguito sono riportati i valori possibili per il campo **Tipo** e i valori corrispondenti nella colonna operations_type della vista **catalog.operations** Transact-SQL.  
  
|Descrizione del campo Tipo|Valore operations_type|  
|-|-|  
|Inizializzazione di Integration Services|1|  
|Pulizia di operazioni (processo di SQL Agent)|2|  
|Pulizia delle versioni del progetto (processo di SQL Agent)|3|  
|Distribuzione del progetto|101|  
|Ripristino del progetto|106|  
|Creazione e avvio dell'esecuzione del pacchetto|200|  
|Arresto dell'operazione (arresto di una convalida o di un'esecuzione)|202|  
|Convalida del progetto|300|  
|Convalida del pacchetto|301|  
|Configurazione del catalogo|1000|  
  
 **Stop**  
 Fare clic per arrestare un'operazione in esecuzione.  

## <a name="viewing-and-stopping-packages-running-on-the-integration-services-server"></a>Visualizzazione e arresto dell'esecuzione dei pacchetti nel server Integration Services
  Nel database di **SSISDB** la cronologia di esecuzione viene archiviata in tabelle interne che non sono visibili agli utenti. Tuttavia, nel database vengono esposte le informazioni necessarie tramite viste pubbliche su cui è possibile eseguire una query. Inoltre, sono disponibili stored procedure che è possibile chiamare per eseguire attività comuni correlate ai pacchetti.  
  
 In genere, gli oggetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vengono gestiti nel server in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Tuttavia, è anche possibile eseguire query sulle viste del database e chiamare direttamente le stored procedure oppure scrivere codice personalizzato con cui chiamare l'API gestita. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e l'API gestita eseguono query sulle viste e chiamano le stored procedure per eseguire molte delle relative attività. È ad esempio possibile visualizzare l'elenco di pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] attualmente in esecuzione nel server e, se necessario richiederne l'arresto.  
  
### <a name="viewing-the-list-of-running-packages"></a>Visualizzazione dell'elenco di pacchetti in esecuzione  
 È possibile visualizzare l'elenco di pacchetti attualmente in esecuzione nel server nella finestra di dialogo **Operazioni attive** . Per altre informazioni, vedere [Active Operations Dialog Box](#active_ops).  
  
 Per informazioni su altri metodi utilizzabili per visualizzare l'elenco di pacchetti in esecuzione, vedere gli argomenti seguenti.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] accesso  
 Per visualizzare l'elenco di pacchetti in esecuzione nel server, eseguire una query sulla vista [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md) per i pacchetti con stato 2.  
  
 Accesso a livello di codice tramite l'API gestita  
 Vedere lo spazio dei nomi <xref:Microsoft.SqlServer.Management.IntegrationServices> e le relative classi.  
  
### <a name="stopping-a-running-package"></a>Arresto di un pacchetto in esecuzione  
 È possibile richiedere l'arresto di un pacchetto in esecuzione nella finestra di dialogo **Operazioni attive** . Per altre informazioni, vedere [Active Operations Dialog Box](#active_ops).  
  
 Per informazioni su altri metodi utilizzabili per arrestare un pacchetto in esecuzione, vedere gli argomenti seguenti.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] accesso  
 Per arrestare un pacchetto in esecuzione nel server, chiamare la stored procedure [catalog.stop_operation &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Accesso a livello di codice tramite l'API gestita  
 Vedere lo spazio dei nomi <xref:Microsoft.SqlServer.Management.IntegrationServices> e le relative classi.  
  
### <a name="viewing-the-history-of-packages-that-have-run"></a>Visualizzazione della cronologia dei pacchetti eseguiti  
 Per visualizzare la cronologia di pacchetti eseguiti in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], usare il report **Tutte le esecuzioni** . Per altre informazioni sul report **Tutte le esecuzioni** e gli altri report standard, vedere [Visualizzare i report per il server Integration Services](#reports).  
  
 Per informazioni su altri metodi utilizzabili per visualizzare la cronologia di pacchetti in esecuzione, vedere gli argomenti seguenti.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] accesso  
 Per visualizzare le informazioni sui pacchetti eseguiti, eseguire una query sulla vista [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).  
  
 Accesso a livello di codice tramite l'API gestita  
 Vedere lo spazio dei nomi <xref:Microsoft.SqlServer.Management.IntegrationServices> e le relative classi.  

## <a name="reports-for-the-integration-services-server"></a><a name="reports"></a> report per il server Integration Services
  Nella versione corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sono disponibili report standard in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per monitorare i progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I report consentono di visualizzare lo stato e la cronologia dei pacchetti e, se necessario, identificare la causa di eventuali errori.  
  
 All'inizio di ogni pagina dei report sono disponibili l'icona che consente di tornare alla pagina precedentemente visualizzata, l'icona tramite cui viene eseguito l'aggiornamento delle informazioni visualizzate nella pagina e l'icona che consente all'utente di stampare la pagina corrente.  
  
 Per informazioni su come distribuire i pacchetti al server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
### <a name="integration-services-dashboard"></a>Dashboard Integration Services  
 Nel report **Dashboard Integration Services** è disponibile una panoramica di tutte le esecuzioni dei pacchetti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ogni pacchetto eseguito nel server, il dashboard consente all'utente di effettuare un ingrandimento per trovare dettagli specifici sugli errori di esecuzione del pacchetto che potrebbero essersi verificati.  
  
 Nel report sono mostrate le sezioni di informazioni seguenti.  
  
|Sezione|Descrizione|  
|-------------|-----------------|  
|**Informazioni sulle esecuzioni**|Viene mostrato il numero di esecuzioni che si trovano in stati diversi (operazione non riuscita, in esecuzione, operazione riuscita e altri) nelle ultime 24 ore.|  
|**Informazioni sui pacchetti**|Viene mostrato il numero totale di pacchetti eseguiti nelle ultime 24 ore.|  
|**Informazioni di connessione**|Vengono mostrate le connessioni utilizzate nelle esecuzioni non completate correttamente durante le ultime 24 ore.|  
|**Informazioni dettagliate sui pacchetti**|Vengono mostrati i dettagli delle esecuzioni completate durante le ultime 24 ore. In questa sezione viene ad esempio illustrato il numero di esecuzioni non completate rispetto al numero totale di esecuzioni, la durata di un'esecuzione (in secondi) e la durata media delle esecuzioni negli ultimi tre mesi.<br /><br /> È possibile visualizzare altre informazioni per un pacchetto facendo clic su **Panoramica**, **All Messages**(Tutti i messaggi) e **Prestazioni di esecuzione**.<br /><br /> Nel report **Prestazioni di esecuzione** viene visualizzata la durata dell'ultima istanza di esecuzione, nonché l'ora di inizio e di fine e l'ambiente applicato.<br /><br /> Nel grafico e nella tabella associata inclusi nel report **Prestazioni di esecuzione** viene indicata la durata delle ultime 10 esecuzioni completate del pacchetto. Nella tabella viene inoltre illustrata la durata media di esecuzione nell'arco di tre mesi. In fase di esecuzione potrebbero essere stati applicati ambienti e valori letterali diversi per queste 10 esecuzioni completate del pacchetto.<br /><br /> Infine, nel report **Prestazioni di esecuzione** vengono visualizzati il tempo di attività e il tempo totale per i componenti flusso di dati del pacchetto. Il tempo attivo si riferisce alla quantità totale di tempo di esecuzione del componente in tutte le fasi e la durata totale equivale al tempo totale trascorso per un componente. Nel report vengono visualizzate queste informazioni per i componenti del pacchetto solo se il livello di registrazione dell'ultima esecuzione del pacchetto è stato impostato su Prestazioni o Dettagliato.<br /><br /> Nel report **Panoramica** viene mostrato lo stato delle attività del pacchetto. Nel report **Messaggi** vengono visualizzati i messaggi di evento e i messaggi di errore per il pacchetto e le attività, ad esempio per segnalare l'ora di inizio e di fine e il numero di righe scritte.<br /><br /> È anche possibile fare clic su **Visualizzazione messaggi** nel report **Panoramica** per passare al report **Messaggi** . È anche possibile fare clic su **Visualizza panoramica** nel report **Messaggi** per passare al report **Panoramica** .|  
  
 È possibile filtrare la tabella visualizzata in qualsiasi pagina facendo clic su **Filtro** e selezionando i criteri nella finestra di dialogo **Impostazioni filtro** . I criteri di filtro disponibili dipendono dai dati visualizzati. È possibile modificare l'ordinamento del report facendo clic sulla relativa icona nella finestra di dialogo **Impostazioni filtro** .  
  
### <a name="all-executions-report"></a>Report Tutte le esecuzioni  
 Nel report **All Executions** (Tutte le esecuzioni) viene visualizzato un riepilogo di tutte le esecuzioni di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] effettuate nel server. Possono essere presenti più esecuzioni del pacchetto di esempio. A differenza del report **Dashboard Integration Services** , è possibile configurare il report **All Executions** (Tutte le esecuzioni) per visualizzare le esecuzioni avviate durante un intervallo di date. Le date possono estendersi per più giorni, mesi o anni.  
  
 Nel report sono mostrate le sezioni di informazioni seguenti.  
  
|Sezione|Descrizione|  
|-------------|-----------------|  
|Filtro|Mostra il filtro corrente applicato al report, ad esempio l'intervallo Ora inizio.|  
|Informazioni sulle esecuzioni|Indica l'ora di inizio, l'ora di fine e la durata di ogni esecuzione del pacchetto. È possibile visualizzare un elenco di valori dei parametri utilizzati con l'esecuzione di un pacchetto, ad esempio i valori passati a un pacchetto figlio utilizzando l'attività Esegui pacchetto. Per visualizzare l'elenco dei parametri, fare clic su Panoramica.|  
  
 Per altre informazioni sull'utilizzo dell'attività Esegui pacchetto per rendere disponibili valori a un pacchetto figlio, vedere [Attività Esegui pacchetto](../../integration-services/control-flow/execute-package-task.md).  
  
 Per altre informazioni sui parametri, vedere [Pacchetto di Integration Services (SSIS) e i parametri del progetto](../../integration-services/integration-services-ssis-package-and-project-parameters.md).  
  
### <a name="all-connections"></a>Tutte le connessioni  
 Nel report **All Connections** (Tutte le connessioni) sono incluse le informazioni seguenti, relative alle connessioni non riuscite, per le esecuzioni che si sono verificate nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Nel report sono mostrate le sezioni di informazioni seguenti.  
  
|Sezione|Descrizione|  
|-------------|-----------------|  
|Filtro|Mostra il filtro corrente applicato al report, ad esempio le connessioni con una stringa specificata e il valore di **Intervallo di ore ultimo errore** .<br /><br /> Impostare un valore per **Intervallo di ore ultimo errore** per visualizzare solo gli errori di connessione che si sono verificati durante un intervallo di date. L'intervallo può estendersi per più giorni, mesi o anni.|  
|Dettagli|Mostra la stringa di connessione, il numero di esecuzioni durante le quali una connessione non è riuscita e la data in cui l'ultima connessione non è riuscita.|  
  
### <a name="all-operations-report"></a>Report Tutte le operazioni  
 Nel report **All Operations** (Tutte le operazioni) viene visualizzato un riepilogo di tutte le operazioni di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] eseguite nel server, incluse la distribuzione, la convalida e l'esecuzione dei pacchetti, nonché altre operazioni amministrative. Come per il Dashboard Integration Services, è possibile applicare un filtro alla tabella per limitare le informazioni visualizzate.  
  
### <a name="all-validations-report"></a>Report Tutte le convalide  
 Nel report **Tutte le convalide** viene visualizzato un riepilogo di tutte le convalide di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] effettuate nel server. Nel riepilogo vengono visualizzate informazioni per ogni convalida, ad esempio, stato, ora di inizio e ora di fine. In ogni voce del riepilogo è incluso un collegamento ai messaggi generati durante la convalida. Come per il Dashboard Integration Services, è possibile applicare un filtro alla tabella per limitare le informazioni visualizzate.  
  
### <a name="custom-reports"></a>Report personalizzati  
 È possibile aggiungere un report personalizzato (file con estensione rdl) al nodo del catalogo **SSISDB** nel nodo **Cataloghi di Integration Services** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Prima di aggiungere il report, verificare che sia in uso una convenzione di denominazione di terze parti per assegnare nomi completi agli oggetti a cui si fa riferimento, ad esempio una tabella di origine. In caso contrario, in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verrà visualizzato un errore. La convenzione di denominazione è \<database>.\<owner>.\<object>. ad esempio SSISDB.internal.executions.  
  
> [!NOTE]  
>  Quando si aggiungono report personalizzati al nodo **SSISDB** nel nodo **Database** , il prefisso SSISDB non è necessario.  
  
 Per istruzioni su come creare e aggiungere un report personalizzato, vedere [Aggiunta di un report personalizzato a Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md).  

## <a name="view-reports-for-the-integration-services-server"></a>Visualizzare i report per il server Integration Services
  Nella versione corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sono disponibili report standard in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per monitorare i progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  Per altre informazioni sui report, vedere [Report per il server Integration Services](#reports).  
  
### <a name="to-view-reports-for-the-integration-services-server"></a>Per visualizzare i report per il server Integration Services  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]espandere il nodo **Cataloghi di Integration Services** in Esplora oggetti.  
  
2.  Fare clic con il pulsante destro del mouse su **SSISDB**, scegliere **Report** e quindi fare clic su **Report standard**.  
  
3.  Fare clic su una delle opzioni seguenti per visualizzare un report.  
  
    -   **Dashboard Integration Services**  
  
    -   **Tutte le esecuzioni**  
  
    -   **Tutte le convalide**  
  
    -   **Tutte le operazioni**  
  
    -   **Tutte le connessioni**  

## <a name="see-also"></a>Vedere anche  
 [Esecuzione di progetti e pacchetti](../packages/deploy-integration-services-ssis-projects-and-packages.md)   
 [Report per la risoluzione dei problemi relativi all'esecuzione dei pacchetti](../troubleshooting/troubleshooting-reports-for-package-execution.md)  
