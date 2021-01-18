---
title: Vista ExecutionLog ed ExecutionLog3 del server di report | Microsoft Docs
description: Informazioni sul log di esecuzione del server di report in Reporting Services, che contiene informazioni sui report eseguiti in server in modalità nativa o in una farm di SharePoint.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], execution
- execution logs [Reporting Services]
ms.assetid: a7ead67d-1404-4e67-97e7-4c7b0d942070
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 28d083e053e31c1fcce26e233ba22211e211a993
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125588"
---
# <a name="report-server-executionlog-and-the-executionlog3-view"></a>Vista ExecutionLog ed ExecutionLog3 del server di report
  Il log di esecuzione del server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]include informazioni sui report eseguiti in uno o più server in una distribuzione con scalabilità orizzontale in modalità nativa o in una farm di SharePoint. Il log consente di conoscere la frequenza con cui un report viene richiesto, i formati di output più usati e i millisecondi dedicati a ogni fase dell'elaborazione. Nel log, inoltre, sono contenute informazioni sul tempo impiegato per l'esecuzione di una query del set di dati di un report e su quello speso per l'elaborazione dei dati. Se si è un amministratore del server di report, è possibile esaminare le informazioni sul log, identificare le attività con esecuzione prolungata e inviare suggerimenti agli autori del report sulle aree del report, set di dati o elaborazione, che potrebbero essere migliorate.  
  
 Nei server di report configurati per la modalità SharePoint possono essere usati anche i log ULS di SharePoint. Per altre informazioni, vedere [Abilitare gli eventi di Reporting Services per il log di traccia di SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
##  <a name="viewing-log-information"></a><a name="bkmk_top"></a> Visualizzazione delle informazioni sul log di esecuzione  
 Nel server di report vengono registrati i dati sull'esecuzione dei report in una tabella interna del database. Le informazioni della tabella sono disponibili dalle viste SQL Server.  
  
 Il log di esecuzione del report viene archiviato nel database del server di report denominato **ReportServer** per impostazione predefinita. Nelle viste SQL sono incluse le informazioni sul log di esecuzione. Le viste "2" e "3" sono state aggiunte in versioni più recenti e contengono nuovi campi oppure campi con nomi più descrittivi rispetto alle versioni precedenti. Le viste precedenti rimangono nel prodotto, così non vengono influenzate le applicazioni personalizzate basata su di esse. Se non si dispone di una dipendenza da una vista precedente, ad esempio ExecutionLog, si consiglia di usare la vista più recente, ExecutionLog **3**.  
  
 In questo argomento  
  
-   [Impostazioni di configurazione per un server di report in modalità SharePoint](#bkmk_sharepoint)  
  
-   [Impostazioni di configurazione per un server di report in modalità nativa](#bkmk_native)  
  
-   [Campi del log (ExecutionLog3)](#bkmk_executionlog3)  
  
-   [Campo AdditionalInfo](#bkmk_additionalinfo)  
  
-   [Campi del log (ExecutionLog2)](#bkmk_executionlog2)  
  
-   [Campi del log (ExecutionLog)](#bkmk_executionlog)  
  
##  <a name="configuration-settings-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepoint"></a> Impostazioni di configurazione per un server di report in modalità SharePoint  
 È possibile abilitare o disabilitare la registrazione per l'esecuzione del report dalle impostazioni di sistema di un'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Per impostazione predefinita, le voci di log vengono mantenute per 60 giorni. Ogni giorno, alle 14.00, vengono rimosse le voci antecedenti . In un'installazione datata saranno disponibili solo 60 giorni di informazioni in qualsiasi momento.  
  
 Non è possibile impostare limiti sul numero di righe o sul tipo di voci registrate.  
  
 **Per abilitare la registrazione per l'esecuzione:**  
  
1.  Nel gruppo **Gestione applicazioni** di Amministrazione centrale SharePoint fare clic su **Gestisci applicazioni di servizio** .  
  
2.  Fare clic sul nome dell'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da configurare.  
  
3.  Fare clic su **Impostazioni sistema**.  
  
4.  Selezionare **Abilita registrazione di esecuzione** nella sezione **Registrazione** .  
  
5.  Fare clic su **OK**.  
  
 **Per abilitare la registrazione dettagliata:**  
  
 La registrazione deve essere abilitata come descritto nei passaggi precedenti e successivamente completare le operazioni seguenti:  
  
1.  Nella pagina **Impostazioni sistema** dell'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] trovare la sezione **Definito dall'utente** .  
  
2.  Impostare **ExecutionLogLevel** su **verbose**(dettagliato). Si tratta di un campo di immissione testo e i due valori possibili sono **verbose** (dettagliato) e **normal**(normale).  
  
##  <a name="configuration-settings-for-a-native-mode-report-server"></a><a name="bkmk_native"></a> Impostazioni di configurazione per un server di report in modalità nativa  
 È possibile abilitare o disabilitare la registrazione per l'esecuzione del report dalla pagina Proprietà server in SQL Server Management Studio. **EnableExecutionLogging** è la proprietà avanzata.  
  
 Per impostazione predefinita, le voci di log vengono mantenute per 60 giorni. Ogni giorno, alle 14.00, vengono rimosse le voci antecedenti . In un'installazione datata saranno disponibili solo 60 giorni di informazioni in qualsiasi momento.  
  
 Non è possibile impostare limiti sul numero di righe o sul tipo di voci registrate.  
  
 **Per abilitare la registrazione per l'esecuzione:**  
  
1.  Avviare SQL Server Management Studio con privilegi amministrativi. Ad esempio, fare clic con il pulsante destro del mouse sull'icona di Management Studio e scegliere "Esegui come amministratore".  
  
2.  Connettersi al server di report desiderato.  
  
3.  Fare clic con il pulsante destro del mouse sul nome del server e scegliere **Proprietà**. Se l'opzione Proprietà è disabilitata, verificare che SQL Server Management Studio venga eseguito con i privilegi amministrativi.  
  
4.  Fare clic sulla pagina **Registrazione** .  
  
5.  Selezionare **Abilita la registrazione per l'esecuzione di report**.  
  
 **Per abilitare la registrazione dettagliata:**  
  
 La registrazione deve essere abilitata come descritto nei passaggi precedenti e successivamente completare le operazioni seguenti:  
  
1.  Scegliere **Avanzate** nella finestra di dialogo **Proprietà server** .  
  
2.  Nella sezione **Definito dall'utente** impostare **ExecutionLogLevel** su **verbose**(dettagliato). Si tratta di un campo di immissione testo e i due valori possibili sono **verbose** (dettagliato) e **normal**(normale).  
  
##  <a name="log-fields-executionlog3"></a><a name="bkmk_executionlog3"></a> Campi del log (ExecutionLog3)  
 In questa vista è stato aggiunto un nodo di diagnostica delle prestazioni all'interno della colonna **AdditionalInfo** basata su XML. La colonna AdditionalInfo contiene una struttura XML di campi aggiuntivi di informazioni in una relazione uno-a-molti. Di seguito è riportato un esempio di istruzione Transact SQL per recuperare righe dalla vista ExecutionLog3. Nell'esempio si presuppone che il database del server di report sia denominato **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog3 order by TimeStart DESC  
```  
  
 Nella tabella seguente vengono descritti i dati acquisiti nel log di esecuzione del report:  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|InstanceName|Nome dell'istanza del server di report tramite cui è stata gestita la richiesta. Se nell'ambiente è disponibile più di un server di report, è possibile analizzare la distribuzione di InstanceName per monitorare e determinare se tramite il servizio di bilanciamento del carico di rete vengono distribuite richieste attraverso i server di report come previsto.|  
|ItemPath|Percorso in cui viene archiviato un report o un elemento del report.|  
|UserName|Identificatore dell'utente.|  
|ExecutionID|L'identificatore interno associato a una richiesta. Le richieste nelle sessioni dello stesso utente condividono lo stesso ID esecuzione.|  
|RequestType|I valori possibili sono:<br /><br /> Interattività<br /><br /> Subscription<br /><br /> <br /><br /> L'analisi dei dati del log filtrati in base RequestType=Subscription e ordinati per TimeStart può rivelare periodi di utilizzo eccessivo della sottoscrizione ed è pertanto necessario modificare alcune delle sottoscrizioni del report a un'ora diversa.|  
|Format|Formato di rendering.|  
|Parametri|Valori dei parametri usati per l'esecuzione del report.|  
|ItemAction|Valori possibili:<br /><br /> Rendering<br /><br /> Ordina<br /><br /> BookMarkNavigation<br /><br /> DocumentNavigation<br /><br /> GetDocumentMap<br /><br /> Findstring<br /><br /> Execute<br /><br /> RenderEdit|  
|TimeStart|Ora di inizio e ora dell'arresto, che indicano la durata dell'elaborazione del report.|  
|TimeEnd||  
|TimeDataRetrieval|Numero di millisecondi impiegati per il recupero dei dati.|  
|TimeProcessing|Numero di millisecondi impiegati per l'elaborazione del report.|  
|TimeRendering|Numero di millisecondi impiegati per il rendering del report.|  
|Source (Sorgente)|Origine dell'esecuzione del report. Valori possibili:<br /><br /> Live<br /><br /> Cache: indica un'esecuzione memorizzata nella cache, ad esempio le query del set di dati che non vengono eseguite in tempo reale.<br /><br /> Snapshot<br /><br /> Cronologia<br /><br /> AdHoc: indica un report drill-through basato su un modello di report generato dinamicamente o un report di Generatore report visualizzato in anteprima in un client che usa il server di report per l'elaborazione e l'esecuzione del rendering.<br /><br /> Session: indica una richiesta di completamento in una sessione già stabilita.  Ad esempio la richiesta iniziale è di visualizzare la pagina 1 e la richiesta di completamento è di esportare in Excel con lo stato della sessione corrente.<br /><br /> Rdce:  indica un'estensione RDCE (Report Definition Customization Extension). Un'estensione personalizzata RDCE consente di personalizzare in modo dinamico la definizione di un report prima che venga passata al motore di elaborazione all'esecuzione del report.|  
|Stato|Stato (rsSuccess oppure un codice di errore; in caso di più errori, viene registrato solo il primo).|  
|ByteCount|Dimensione dei report visualizzabili, in byte.|  
|RowCount|Numero di righe restituite dalle query.|  
|AdditionalInfo|Contenitore di proprietà XML in cui sono incluse informazioni aggiuntive sull'esecuzione. Il contenuto può essere diverso per ogni riga.|  
  
##  <a name="the-additionalinfo-field"></a><a name="bkmk_additionalinfo"></a> Campo AdditionalInfo  
 Il campo AdditionalInfo è un contenitore o struttura di proprietà XML contenente informazioni aggiuntive sull'esecuzione. Il contenuto può essere diverso per ogni riga del log.  
  
 Gli esempi seguenti mostrano il contenuto del campo AddtionalInfo per la registrazione standard e dettagliata:  
  
 **Esempio di registrazione standard di AddtionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>147</ConnectionOpenTime>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>642</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>63</ExecuteReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>157</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>60</ExecuteReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 **Esempio di registrazione dettagliata di AddtionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>127</ConnectionOpenTime>  
      <DataSource>  
        <Name>DataSource1</Name>  
        <DataExtension>SQL</DataExtension>  
      </DataSource>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>33</ExecuteReaderTime>  
          <DataReaderMappingTime>30</DataReaderMappingTime>  
          <DisposeDataReaderTime>1</DisposeDataReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>1</ExecuteReaderTime>  
          <DataReaderMappingTime>0</DataReaderMappingTime>  
          <DisposeDataReaderTime>0</DisposeDataReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 Di seguito sono riportati alcuni dei valori visualizzati nel campo AdditionalInfo:  
  
-   **ProcessingEngine**  
  
     1=SQL Server 2005, 2=Nuovo motore di elaborazione su richiesta. Se nella maggior parte dei report viene ancora mostrato il valore 1, è possibile esaminare come riprogettare questi report in modo che in essi venga utilizzato il motore di elaborazione su richiesta più nuovo e più efficiente.  
  
     `<ProcessingEngine>2</ProcessingEngine>`  
  
-   **ScalabilityTime**  
  
     Numero di millisecondi impiegati per l'esecuzione delle operazioni correlate alla scala nel motore di elaborazione. Un valore 0 indica che non è stato impiegato ulteriore tempo per operazioni di scala. Questo valore indica inoltre che la richiesta non ha determinato un utilizzo eccessivo della memoria.  
  
    ```  
    <ScalabilityTime>  
        <Processing>0</Processing>  
    </ScalabilityTime>  
    ```  
  
-   **EstimatedMemoryUsageKB**  
  
     Stima della quantità massima di memoria, in KB, usata da ogni componente durante una particolare richiesta.  
  
    ```  
    <EstimatedMemoryUsageKB>  
        <Processing>38</Processing>  
    </EstimatedMemoryUsageKB>  
    ```  
  
-   **DataExtension**  
  
     Tipi di estensioni o di origini dei dati usate nel report. Il numero è quello delle occorrenze dell'origine dati specificata.  
  
    ```  
    <DataExtension>  
       <DAX>2</DAX>  
    </DataExtension>  
    ```  
  
-   **ExternalImages**  
  
     Il valore è espresso in millisecondi. Queste informazioni possono essere usate nella diagnosi dei problemi di prestazioni. Il tempo necessario a recuperare le immagini da un webserver esterno può rallentare l'esecuzione del report complessiva.  
  
    ```  
    <ExternalImages>  
        <Count>3</Count>  
        <ByteCount>9268</ByteCount>  
        <ResourceFetchTime>9</ResourceFetchTime>  
    </ExternalImages>  
    ```  
  
-   **Connessioni**  
  
     Struttura multilivello  
  
    ```  
    <Connections>  
        <Connection>  
          <ConnectionOpenTime>127</ConnectionOpenTime>  
          <DataSource>  
            <Name>DataSource1</Name>  
            <DataExtension>SQL</DataExtension>  
          </DataSource>  
          <DataSets>  
            <DataSet>  
              <Name>DataSet1</Name>  
              <RowsRead>16</RowsRead>  
              <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>33</ExecuteReaderTime>  
              <DataReaderMappingTime>30</DataReaderMappingTime>  
              <DisposeDataReaderTime>1</DisposeDataReaderTime>  
            </DataSet>  
            <DataSet>  
              <Name>DataSet2</Name>  
              <RowsRead>3</RowsRead>  
              <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>1</ExecuteReaderTime>  
              <DataReaderMappingTime>0</DataReaderMappingTime>  
              <DisposeDataReaderTime>0</DisposeDataReaderTime>  
            </DataSet>  
          </DataSets>  
        </Connection>  
    </Connections>  
  
    ```  
  
##  <a name="log-fields-executionlog2"></a><a name="bkmk_executionlog2"></a> Campi del log (ExecutionLog2)  
 In questa vista sono stati aggiunti alcuni campi nuovi e ne sono stati rinominati alcuni altri. Di seguito è riportato un esempio di istruzione Transact SQL per recuperare righe dalla vista ExecutionLog2. Nell'esempio si presuppone che il database del server di report sia denominato **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog2 order by TimeStart DESC  
```  
  
 Nella tabella seguente vengono descritti i dati acquisiti nel log di esecuzione del report:  
  
|Colonna|Descrizione|  
|------------|------------------------------------------------------------|  
|InstanceName|Nome dell'istanza del server di report tramite cui è stata gestita la richiesta.|  
|ReportPath|Struttura del percorso del report. Per un report salvato nella cartella radice "test", il valore di ReportPath è "/test".<br /><br /> Per un report denominato "test" salvato nella cartella "samples", il valore di ReportPath sarà "/Samples/test/".|  
|UserName|Identificatore dell'utente.|  
|ExecutionID||  
|RequestType|Tipo di richiesta (utente o sistema).|  
|Format|Formato di rendering.|  
|Parametri|Valori dei parametri usati per l'esecuzione del report.|  
|ReportAction|Valori possibili: Render, Sort, BookMarkNavigation, DocumentNavigation, GetDocumentMap, Findstring|  
|TimeStart|Ora di inizio che indica la durata dell'elaborazione del report.|
|TimeEnd|Ora di fine che indica la durata dell'elaborazione del report.|
|TimeDataRetrieval|Numero di millisecondi impiegati per il recupero dei dati.|
|TimeProcessing|Numero di millisecondi impiegati per l'elaborazione del report.|
|TimeRendering|Numero di millisecondi impiegati per il rendering del report.|
|Source (Sorgente)|Origine dell'esecuzione del report (1=Live, 2=Cache, 3=Snapshot, 4=History).|  
|Stato|Stato (rsSuccess oppure un codice di errore; in caso di più errori, viene registrato solo il primo).|  
|ByteCount|Dimensione dei report visualizzabili, in byte.|  
|RowCount|Numero di righe restituite dalle query.|  
|AdditionalInfo|Contenitore di proprietà XML in cui sono incluse informazioni aggiuntive sull'esecuzione.|  
  
##  <a name="log-fields-executionlog"></a><a name="bkmk_executionlog"></a> Campi del log (ExecutionLog)  
 Di seguito è riportato un esempio di istruzione Transact SQL per recuperare righe dalla vista ExecutionLog. Nell'esempio si presuppone che il database del server di report sia denominato **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog order by TimeStart DESC  
  
```  
  
 Nella tabella seguente vengono descritti i dati acquisiti nel log di esecuzione del report:  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|InstanceName|Nome dell'istanza del server di report tramite cui è stata gestita la richiesta.|  
|ReportID|Identificatore del report.|  
|UserName|Identificatore dell'utente.|  
|RequestType|Valori possibili:<br /><br /> True = Richiesta di sottoscrizione<br /><br /> False= Richiesta interattiva|  
|Format|Formato di rendering.|  
|Parametri|Valori dei parametri usati per l'esecuzione del report.|  
|TimeStart|Ora di inizio e ora dell'arresto, che indicano la durata dell'elaborazione del report.|  
|TimeEnd||  
|TimeDataRetrieval|Numero di millisecondi dedicati al recupero dei dati, all'elaborazione del report e al rendering del report.|  
|TimeProcessing||  
|TimeRendering||  
|Source (Sorgente)|Origine dell'esecuzione del report. Valori possibili: 1=Live, 2=Cache, 3=Snapshot, 4=History, 5=Adhoc, 6=Session, 7=RDCE.|  
|Stato|Valori possibili: rsSuccess, rsProcessingAborted o un codice di errore. Se si verificano più errori, viene registrato solo il primo.|  
|ByteCount|Dimensione dei report visualizzabili, in byte.|  
|RowCount|Numero di righe restituite dalle query.|  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare gli eventi di Reporting Services per il log di traccia di SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)   
 [File di log e origini di Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Guida di riferimento a errori ed eventi &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
