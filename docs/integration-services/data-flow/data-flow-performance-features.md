---
description: Funzionalità delle prestazioni del flusso di dati
title: Funzionalità delle prestazioni del flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- Integration Services packages, performance
- performance [Integration Services]
- data flow [Integration Services], troubleshooting
- SQL Server Integration Services packages, performance
- loading data
- control flow [Integration Services], troubleshooting
- SSIS packages, performance
- packages [Integration Services], performance
- queries [Integration Services], troubleshooting
- sorting data [Integration Services]
- aggregations [Integration Services]
ms.assetid: c4bbefa6-172b-4547-99a1-a0b38e3e2b05
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 600ab1bce27f4fa53c2e25ae7562a870f2ccf6a6
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536355"
---
# <a name="data-flow-performance-features"></a>Funzionalità delle prestazioni del flusso di dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In questo argomento sono inclusi alcuni suggerimenti sulla progettazione di pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per evitare problemi di prestazioni comuni. Sono inoltre fornite informazioni sugli strumenti e sulle funzionalità che è possibile usare per risolvere i problemi relativi alle prestazioni dei pacchetti.  
  
## <a name="configuring-the-data-flow"></a>Configurazione del flusso di dati  
 Per configurare l'attività Flusso di dati per ottenere prestazioni migliori, è possibile configurare le proprietà dell'attività, modificare le dimensioni del buffer e configurare il pacchetto per l'esecuzione parallela.  
  
### <a name="configure-the-properties-of-the-data-flow-task"></a>Configurazione delle proprietà dell'attività Flusso di dati  
  
> [!NOTE]  
>  Le proprietà trattate in questa sezione devono essere impostate separatamente per ogni attività Flusso di dati in un pacchetto.  
  
 È possibile configurare le proprietà seguenti dell'attività Flusso di dati, che influenzano le prestazioni:  
  
-   Specificare il percorso per l'archiviazione temporanea dei dati del buffer, impostando la proprietà BufferTempStoragePath e delle colonne contenenti dati BLOB (Binary Large Object), impostando la proprietà BLOBTempStoragePath. Per impostazione predefinita, le proprietà contengono i valori delle variabili di ambiente TEMP e TMP. Potrebbe essere necessario specificare altre cartelle per inserire i file temporanei in un'unità disco rigido diversa o più potente o distribuirli in più unità. È possibile specificare più directory delimitando i relativi nomi con punti e virgola (;).  
  
-   Definire le dimensioni predefinite del buffer usato dall'attività, impostando la proprietà DefaultBufferSize e il numero massimo di righe in ogni buffer, impostando la proprietà DefaultBufferMaxRows. Impostare la proprietà AutoAdjustBufferSize per indicare se le dimensioni predefinite del buffer vengono calcolate automaticamente dal valore della proprietà DefaultBufferMaxRows. Le dimensioni predefinite del buffer sono di 10 MB e quelle massime di 2^31-1 byte. Il numero di righe massimo predefinito è 10.000.  
  
-   Definire il numero di thread che l'attività può usare durante l'esecuzione, impostando la proprietà EngineThreads. Questa proprietà indica al motore flusso di dati il numero di thread da utilizzare. Il valore predefinito è 10. Il valore minimo è 3. Il motore, tuttavia, non usa più thread di quelli necessari, indipendentemente dal valore di questa proprietà. Il motore può inoltre usare un numero di thread maggiore di quello specificato da questa proprietà, se necessario per evitare problemi di concorrenza.  
  
-   Indicare se l'attività Flusso di dati viene eseguita in modalità ottimizzata, impostando la proprietà RunInOptimizedMode. La modalità ottimizzata consente di migliorare le prestazioni rimuovendo dal flusso di dati le colonne, gli output e i componenti inutilizzati.  
  
    > [!NOTE]  
    >  Per indicare che l'attività Flusso di dati viene eseguita in modalità ottimizzata durante il debug, è possibile impostare una proprietà con lo stesso nome, ovvero RunInOptimizedMode, a livello di progetto in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Questa proprietà ha la precedenza sulla proprietà RunInOptimizedMode delle attività Flusso di dati in fase di progettazione.  
  
### <a name="adjust-the-sizing-of-buffers"></a>Modificare il ridimensionamento dei buffer  
 Il motore flusso di dati inizia l'attività di ridimensionamento dei buffer calcolando le dimensioni stimate di una singola riga di dati. Queste dimensioni vengono quindi moltiplicate per il valore di DefaultBufferMaxRows, per ottenere un valore preliminare per le dimensioni del buffer.  
  
-   Se AutoAdjustBufferSize è impostata su true, il motore flusso di dati usa il valore calcolato come dimensioni del buffer e il valore di DefaultBufferSize viene ignorato.  
  
-   Se AutoAdjustBufferSize è impostato su false, il motore flusso di dati usa le regole seguenti per determinare le dimensioni del buffer.  
  
    -   Se il risultato è superiore al valore di DefaultBufferSize, il numero di righe viene ridotto.  
  
    -   Se il risultato è inferiore alle dimensioni minime del buffer calcolate internamente, il numero di righe viene aumentato.  
  
    -   Se il risultato è compreso tra le dimensioni minime del buffer e il valore della proprietà DefaultBufferSize, il buffer viene ridimensionato per ottenere il valore più vicino possibile alle dimensioni stimate della riga per il valore di DefaultBufferMaxRows.  
  
 Quando si inizia a testare le prestazioni delle attività Flusso di dati, usare i valori predefiniti per DefaultBufferSize e DefaultBufferMaxRows. Abilitare la registrazione nell'attività Flusso di dati e selezionare l'evento BufferSizeTuning per verificare il numero di righe presenti in ogni buffer.  
  
 Prima di iniziare a modificare il ridimensionamento dei buffer, è possibile ridurre le dimensioni di ogni riga di dati rimuovendo le colonne non necessarie e configurando i tipi di dati in modo appropriato.  
  
 Per determinare il numero e le dimensioni ottimali dei buffer, provare a usare diversi valori di DefaultBufferSize e DefaultBufferMaxRows monitorando le prestazioni e analizzando le informazioni riportate dall'evento BufferSizeTuning.  
  
 Evitare di aumentare le dimensioni del buffer fino al punto in cui inizia a verificarsi il paging su disco. Il paging su disco influisce negativamente sulle prestazioni più di quanto non faccia la mancata ottimizzazione delle dimensioni del buffer. Per determinare il verificarsi o meno del paging, monitorare il contatore delle prestazioni "Buffer con spooling" nello snap-in Prestazioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC).  
  
### <a name="configure-the-package-for-parallel-execution"></a>Configurare il pacchetto per l'esecuzione parallela  
 L'esecuzione parallela migliora le prestazioni nei computer dotati di più processori fisici o logici. Per supportare l'esecuzione parallela di diverse attività nel pacchetto, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa due proprietà: **MaxConcurrentExecutables** e **EngineThreads**.  
  
#### <a name="the-maxconcurrentexcecutables-property"></a>Proprietà MaxConcurrentExcecutables  
 La proprietà **MaxConcurrentExecutables** è una proprietà del pacchetto stesso. Questa proprietà definisce il numero massimo di attività che è possibile eseguire simultaneamente. Il valore predefinito è -1, a indicare il numero di processori fisici o logici più 2.  
  
 Per comprendere il funzionamento di questa proprietà, considerare un pacchetto di esempio contenente tre attività Flusso di dati. Se si imposta **MaxConcurrentExecutables** su 3, le tre attività Flusso di dati potranno essere eseguite tutte simultaneamente. Si supponga, tuttavia, che ogni attività Flusso di dati contenga 10 alberi di esecuzione dall'origine alla destinazione. L'impostazione di **MaxConcurrentExecutables** su 3 non garantisce l'esecuzione in parallelo degli alberi di esecuzione in ogni attività Flusso di dati.  
  
#### <a name="the-enginethreads-property"></a>Proprietà EngineThreads  
 La proprietà **EngineThreads** è una proprietà di tutte le attività Flusso di dati. Questa proprietà definisce il numero di thread che il motore del flusso di dati può creare ed eseguire in parallelo. La proprietà **EngineThreads** si applica ugualmente sia ai thread di origine creati dal motore del flusso di dati per le origini sia ai thread di lavoro creati dal motore per trasformazioni e destinazioni. L'impostazione di **EngineThreads** su 10, pertanto, indica che il motore può creare fino a dieci thread di origine e fino a dieci thread di lavoro.  
  
 Per comprendere il funzionamento di questa proprietà, considerare il pacchetto di esempio contenente tre attività Flusso di dati. Ogni attività Flusso di dati contiene dieci alberi di esecuzione dall'origine alla destinazione. Se si imposta EngineThreads su 10 in ogni attività Flusso di dati, è virtualmente possibile eseguire tutti i 30 alberi di esecuzione simultaneamente.  
  
> [!NOTE]  
>  Una trattazione del threading esula dall'ambito di questo argomento. La regola generale, tuttavia, consiste nell'evitare l'esecuzione in parallelo di un numero di thread maggiore del numero di processori disponibili. L'esecuzione di un numero di thread maggiore del numero di processori disponibili può influire negativamente sulle prestazioni a causa del frequente scambio del contesto tra thread.  
  
## <a name="configuring-individual-data-flow-components"></a>Configurazione di singoli componenti del flusso di dati  
 Per configurare singoli componenti del flusso di dati ai fini delle prestazioni, è consigliabile seguire alcune linee guida generali. Vi sono inoltre linee guida specifiche per ogni tipo di componente del flusso di dati, ovvero origine, trasformazione e destinazione.  
  
### <a name="general-guidelines"></a>Linee guida generali  
 Indipendentemente dal componente del flusso di dati, è consigliabile seguire due linee guida generali per migliorare le prestazioni: ottimizzare le query ed evitare stringhe non necessarie.  
  
#### <a name="optimize-queries"></a>Ottimizzazione delle query  
 Numerosi componenti del flusso di dati usano query in operazioni di estrazione dei dati dalle origini o in operazioni di ricerca per la creazione di tabelle di riferimento. La query predefinita usa la sintassi SELECT * FROM \<tableName>. con cui vengono restituite tutte le colonne della tabella di origine. La disponibilità di tutte le colonne in fase di progettazione consente di scegliere qualsiasi colonna come colonna di ricerca, colonna pass-through o colonna di origine. Dopo avere selezionato le colonne da usare, è tuttavia necessario modificare la query in modo che includa solo le colonne selezionate. La rimozione delle colonne superflue garantisce una maggiore efficienza del flusso di dati in un pacchetto, in quanto un minor numero di colonne comporta la creazione di una riga con dimensioni inferiori. Una riga con dimensioni inferiori fa sì che sia possibile inserire più righe in un buffer e che l'elaborazione di tutte le righe nel set di dati risulti meno complessa.  
  
 Per creare una query, è possibile digitarla o usare Generatore query.  
  
> [!NOTE]  
>  Quando si esegue un pacchetto in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], nella scheda Stato di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] vengono visualizzati alcuni avvisi. Tali avvisi includono l'identificazione di qualsiasi colonna di dati resa disponibile per il flusso di dati da un'origine, ma che non viene usata successivamente dai componenti del flusso di dati a valle. È possibile usare la proprietà **RunInOptimizedMode** per rimuovere queste colonne automaticamente.  
  
#### <a name="avoid-unnecessary-sorting"></a>Eliminazione di operazioni di ordinamento superflue  
 L'ordinamento è di per sé un'operazione lenta. Evitando operazioni di ordinamento non necessarie è pertanto possibile migliorare le prestazioni del flusso di dati di un pacchetto.  
  
 Talvolta i dati di origine sono già ordinati prima di essere usati da un componente a valle. Questo preordinamento può verificarsi quando la query SELECT usa una clausola ORDER BY o quando i dati vengono inseriti nell'origine come già ordinati. Per tali dati di origine preordinati, è possibile fornire un hint indicante che i dati sono ordinati e pertanto evitare l'uso di una trasformazione Ordinamento per rispondere ai requisiti di ordinamento di determinate trasformazioni a valle. Le trasformazioni Unione e Merge Join, ad esempio, richiedono input ordinati. Per fornire un hint indicante che i dati sono ordinati, è necessario eseguire le attività seguenti:  
  
-   Impostare la proprietà **IsSorted** nell'output di un componente del flusso di dati a monte su **True**.  
  
-   Specificare le colonne chiave di ordinamento in cui i dati sono ordinati.  
  
 Per altre informazioni, vedere [Ordinamento dei dati per le trasformazioni Unione e Merge Join](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).  
  
 Se è necessario ordinare i dati del flusso di dati, è possibile migliorare le prestazioni progettando il flusso di dati in modo che venga eseguito il minor numero possibile di operazioni di ordinamento. Il flusso di dati, ad esempio, usa una trasformazione Multicast per copiare il set di dati. Ordinare il set di dati una volta prima dell'esecuzione della trasformazione Multicast anziché ordinare più output in seguito alla trasformazione.  
  
 Per altre informazioni, vedere [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md), [Merge Transformation](../../integration-services/data-flow/transformations/merge-transformation.md), [Merge Join Transformation](../../integration-services/data-flow/transformations/merge-join-transformation.md)e [Multicast Transformation](../../integration-services/data-flow/transformations/multicast-transformation.md).  
  
### <a name="sources"></a>Origini  
  
#### <a name="ole-db-source"></a>Origine OLE DB  
 Quando si usa un'origine OLE DB per recuperare dati da una vista, selezionare "Comando SQL" come modalità di accesso ai dati e immette un'istruzione SELECT. L'accesso a dati tramite un'istruzione SELECT è più efficace rispetto alla selezione di "Tabella o vista" come modalità di accesso ai dati.  
  
### <a name="transformations"></a>Trasformazioni  
 Usare i suggerimenti inclusi in questa sezione per migliorare la prestazione delle trasformazioni Aggregazione, Ricerca fuzzy, Raggruppamento fuzzy, Ricerca, Merge Join e Dimensione a modifica lenta.  
  
#### <a name="aggregate-transformation"></a>Trasformazione Aggregazione  
 La trasformazione Aggregazione include le proprietà **Keys**, **KeysScale**, **CountDistinctKeys** e **CountDistinctScale** . Queste proprietà migliorano le prestazioni in quanto consentono alla trasformazione di preallocare la quantità di memoria necessaria per i dati memorizzati nella cache. Se si conosce il numero esatto o approssimativo di gruppi che dovrebbero risultato da un'operazione **Group by** , impostare rispettivamente le proprietà **Keys** e **KeysScale** . Se si conosce il numero esatto o approssimativo di valori distinct che dovrebbero risultare da un'operazione **Distinct Count** , impostare rispettivamente le proprietà **CountDistinctKeys** e **CountDistinctScale** .  
  
 Se in un flusso di dati è necessario creare più aggregazioni, valutare l'opportunità di creare più aggregazioni che usano una singola trasformazione Aggregazione anziché creare più trasformazioni. Questo approccio consente prestazioni migliori quando un'aggregazione è un subset di un'altra aggregazione, in quanto la trasformazione può ottimizzare l'archiviazione interna ed eseguire l'analisi dei dati in ingresso una sola volta. Nel caso, ad esempio, di un'aggregazione che usa la clausola GROUP BY e l'aggregazione AVG, è possibile migliorare le prestazioni combinando clausola e aggregazione in una sola trasformazione. L'esecuzione di più aggregazioni all'interno di una trasformazione Aggregazione, tuttavia, comporta la serializzazione delle operazioni di aggregazione e può pertanto influire sulle prestazioni quando è necessario calcolare più aggregazioni indipendentemente.  
  
#### <a name="fuzzy-lookup-and-fuzzy-grouping-transformations"></a>Trasformazioni Ricerca fuzzy e Raggruppamento fuzzy  
 Per informazioni sull'ottimizzazione della prestazione di tali trasformazioni, vedere il white paper relativo a [Ricerca fuzzy e Raggruppamento fuzzy in SQL Server Integration Services 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90)).  
  
#### <a name="lookup-transformation"></a>Trasformazione Ricerca  
 È possibile ridurre al minimo le dimensioni dei dati di riferimento nella memoria immettendo un'istruzione SELECT per la ricerca delle sole colonne necessarie. Questa opzione garantisce prestazioni migliori rispetto alla selezione di un'intera tabella o vista, che restituisce invece una quantità elevata di dati non necessari.  
  
#### <a name="merge-join-transformation"></a>Merge join - trasformazione  
 Non è più necessario configurare il valore della proprietà **MaxBuffersPerInput** , in quanto Microsoft ha apportato modifiche che riducono il rischio di utilizzo di una quantità eccessiva di memoria da parte della trasformazione Merge join. Questo problema si verificava in genere quando tramite i diversi input della trasformazione Merge Join venivano prodotti dati con frequenze irregolari.  
  
#### <a name="slowly-changing-dimension-transformation"></a>Dimensione a modifica lenta - trasformazione  
 La Configurazione guidata dimensioni a modifica lenta e la trasformazione Dimensione a modifica lenta sono strumenti di uso generale in grado di rispondere alle esigenze della maggior parte degli utenti. Il flusso di dati generato dalla procedura guidata, tuttavia, non è ottimizzato per le prestazioni.  
  
 I componenti più lenti nella trasformazione Dimensione a modifica lenta sono in genere le trasformazioni Comando OLE DB che eseguono istruzioni UPDATE su una singola riga per volta. Il modo più efficace per migliorare le prestazioni della trasformazione Dimensione a modifica lenta consiste pertanto nel sostituire le trasformazioni Comando OLE DB. È possibile sostituire tali trasformazioni con componenti di destinazione che salvano tutte le righe da aggiornare in una tabella di staging. È quindi possibile aggiungere un'attività Esegui SQL per l'esecuzione di un singola istruzione UPDATE di Transact-SQL basata su set su tutte le righe contemporaneamente.  
  
 Gli utenti avanzati possono progettare un flusso di dati personalizzato per l'elaborazione delle dimensioni a modifica lenta ottimizzata per dimensioni estese. Per una descrizione e un esempio di questo approccio, vedere la sezione relativa allo scenario con dimensione univoca nel white paper [Project REAL: Business Intelligence ETL Design Practices](https://www.microsoft.com/download/details.aspx?id=14582).  
  
### <a name="destinations"></a>Destinations  
 Per ottenere prestazioni migliori con le destinazioni, valutare l'opportunità di usare una destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di testarne le prestazioni.  
  
#### <a name="sql-server-destination"></a>SQL Server - destinazione  
 Quando un pacchetto carica dati in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nello stesso computer, usare una destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tale destinazione è ottimizzata per caricamenti bulk ad alta velocità.  
  
#### <a name="testing-the-performance-of-destinations"></a>Test delle prestazioni delle destinazioni  
 In alcuni casi il salvataggio di dati nelle destinazioni potrebbe richiedere tempi più lunghi di quelli previsti. Per stabilire se ciò è dovuto a un'elaborazione lenta dei dati nella destinazione, è possibile sostituire temporaneamente la destinazione con una trasformazione Conteggio righe. Se la velocità effettiva risulta notevolmente migliorata, è probabile che la causa delle prestazioni lente sia la destinazione in cui vengono caricati i dati.  
  
### <a name="review-the-information-on-the-progress-tab"></a>Analisi delle informazioni nella scheda Stato  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] , in Progettazione [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Nella scheda **Stato** sono elencati i contenitori e le attività in ordine di esecuzione, nonché l'ora di inizio e di fine, gli avvisi e i messaggi di errore per ogni contenitore e attività, inclusi quelli relativi al pacchetto stesso. Sono inoltre elencati i componenti del flusso di dati in ordine di esecuzione, nonché informazioni sullo stato, visualizzato in forma di percentuale di completamento, e il numero di righe elaborate.  
  
 Per abilitare o disabilitare la visualizzazione di messaggi nella scheda **Stato** , attivare o disattivare l'opzione **Debug report di stato** del menu **SSIS** . La disabilitazione del report di stato consente di migliorare le prestazioni durante l'esecuzione di un pacchetto complesso in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
## <a name="related-tasks"></a>Attività correlate  
  
-   [Ordinare i dati per le trasformazioni Unione e Merge Join](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-content"></a>Contenuto correlato  
 **Articoli e post di Blog**  
  
-   Articolo tecnico [SQL Server 2005 Integration Services: una strategia per ottimizzare le prestazioni](/previous-versions/sql/sql-server-2005/administrator/cc966530(v=technet.10)), su technet.microsoft.com  
  
-   Articolo tecnico relativo alle [tecniche di ottimizzazione delle prestazione in Integration Services](/previous-versions/sql/sql-server-2005/administrator/cc966529(v=technet.10)), disponibile su technet.microsoft.com  
  
-   Articolo tecnico sull'[aumento della velocità effettiva delle pipeline suddividendo le trasformazioni sincrone in più attività](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20BI%20and%20Analytics.pdf) nel documento _SQLCAT's Guide to BI and Analytics_ (Guida alla business intelligence e all'analisi di SQLCAT)
  
-   Articolo tecnico relativo alla [guida alle prestazioni del caricamento dati](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))sul sito msdn.microsoft.com.  
  
-   Articolo tecnico relativo al [caricamento di 1 TB in 30 minuti con SSIS](/previous-versions/sql/sql-server-2008/dd537533(v=sql.100)), disponibile su msdn.microsoft.com  
  
-   Articolo tecnico relativo alle [10 principali procedure consigliate per SQL Server Integration Services](https://techcommunity.microsoft.com/t5/datacat/top-10-sql-server-integration-services-best-practices/ba-p/305163), disponibile su sqlcat.com  
  
-   Articolo tecnico ed esempio relativo al [server di distribuzione di dati bilanciati di SSIS](https://www.sqlshack.com/ssis-balanced-data-distributor-overview/), disponibile su sqlcat.com  
  
-   Post del blog relativo alla [risoluzione dei problemi di prestazioni dei pacchetti SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553)su blogs.msdn.com  
  
 **Video**  
  
-   Serie di video sulla [progettazione e il perfezionamento delle prestazioni dei pacchetti SSIS nell'organizzazione (Serie di video su SQL)](https://go.microsoft.com/fwlink/?LinkId=400878)  
  
-   Video sull' [ottimizzazione del flusso di dati del pacchetto SSIS nell'organizzazione (video di SQL Server)](/previous-versions/ff686901(v=msdn.10)), su technet.microsoft.com  
  
-   Video sulle [informazioni sui buffer del flusso di dati SSIS (video di SQL Server)](/previous-versions/ff686905(v=msdn.10)), su technet.microsoft.com  
  
-   Video, [Modelli di progettazione per prestazioni del servizio di integrazione di Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=233698&clcid=0x409)su channel9.msdn.com.  
  
-   Presentazione relativa all' [utilizzo dei miglioramenti al motore del flusso di dati di SQL Server 2008 SSIS da parte di Microsoft IT](https://channel9.msdn.com/Shows/TechNet+Radio/TechNet-Radio-How-Microsoft-IT-Leverages-SQL-Server-2008-SSIS-Dataflow-Engine-Enhancements), su sqlcat.com.  
  
-   Video relativo al [server di distribuzione di dati bilanciati](/previous-versions/dn912438(v=msdn.10))sul sito technet.microsoft.com.  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi relativi agli strumenti per lo sviluppo dei pacchetti](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)   
 [Strumenti per la risoluzione dei problemi relativi all'esecuzione dei pacchetti](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
