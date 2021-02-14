---
title: Scenari d'uso dell'archivio query | Microsoft Docs
description: Informazioni su come è possibile usare Query Store per tenere traccia e garantire prestazioni prevedibili del carico di lavoro. Prendere in considerazione diversi esempi in SQL Server.
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, usage scenarios
ms.assetid: f5309285-ce93-472c-944b-9014dc8f001d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c65196b3195b8963b6d7c0cf97720b2c4f0d3212
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347699"
---
# <a name="query-store-usage-scenarios"></a>Scenari di utilizzo dell'Archivio query
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Archivio query può essere usato in diversi scenari in cui è fondamentale rilevare e garantire prestazioni prevedibili del carico di lavoro. Di seguito sono riportati alcuni esempi:  
  
-   Individuare e risolvere le query con regressioni nella scelta del piano  
-   Identificare e ottimizzare le query che hanno il maggior consumo di risorse  
-   Test A/B  
-   Mantenere la stabilità delle prestazioni durante l'aggiornamento alla nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
-   Identificare e migliorare i carichi di lavoro ad hoc  
  
## <a name="pinpoint-and-fix-queries-with-plan-choice-regressions"></a>Individuare e risolvere le query con regressioni nella scelta del piano  
 Durante l'esecuzione di query normali, Query Optimizer può scegliere un piano diverso se alcuni input importanti vengono modificati, ad esempio se viene modificata la cardinalità dei dati, se vengono creati, modificati o eliminati indici, se vengono aggiornate le statistiche e così via. In generale, il nuovo piano è migliore o uguale a quello usato in precedenza. Tuttavia, a volte il nuovo piano risulta decisamente peggiore. In questi casi si parla di regressione nella scelta del piano. Prima di Query Store, identificare e risolvere questo problema risultava difficile perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non offriva un archivio dati predefinito che gli utenti potessero esaminare per i piani di esecuzione usati nel corso del tempo.  
  
 Con Query Store, è possibile eseguire rapidamente le operazioni seguenti:  
  
-   Identificare tutte le query le cui metriche di esecuzione siano peggiorate nel periodo di tempo di interesse (ultima ora, giorno, settimana e così via). Usare **Query regredite** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per velocizzare l'analisi.  
  
-   Tra le query regredite è semplice trovare quelle con più piani e che hanno subito un peggioramento a causa di una scelta errata di un piano. Usare il riquadro **Riepilogo piano** in **Query regredite** per visualizzare tutti i piani per una query regredita e le relative prestazioni della query nel tempo.  
  
-   Forzare il piano precedente dalla cronologia se ha dimostrato di essere più efficace. Usare il pulsante **Forza piano** in **Query regredite** per forzare un piano selezionato per la query.  
  
 ![Screenshot di Query Store che mostra un riepilogo del piano.](../../relational-databases/performance/media/query-store-usage-1.png "utilizzo archivio query 1")  
  
 Per una descrizione dettagliata dello scenario fare riferimento al blog [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/) (Query Store: la scatola nera del database).  
  
## <a name="identify-and-tune-top-resource-consuming-queries"></a>Identificare e ottimizzare le query che hanno il maggior consumo di risorse  
 Anche se il carico di lavoro può generare migliaia di query, nella pratica la gran parte delle risorse di sistema viene usata solo da poche query che, di conseguenza, richiedono attenzione. Tra le prime query per consumo di risorse si rilevano in genere quelle regredite o quelle che possono essere migliorate con un'ottimizzazione aggiuntiva.  
  
 Il modo più facile per iniziare l'esplorazione consiste nell'aprire **Prime query per consumo di risorse** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. L'interfaccia utente è suddivisa in tre riquadri: un istogramma che rappresenta le prime query per consumo di risorse (sinistra), un riepilogo del piano per la query selezionata (destra) e un piano di query visivo per il piano selezionato (in basso). Fare clic sul pulsante **Configura** per controllare il numero di query da analizzare e l'intervallo di tempo di interesse. È anche possibile scegliere tra diverse dimensioni di consumo delle risorse (durata, CPU, memoria, operazioni I/O, numero di esecuzione) e la baseline (Media, Min, Max, Totale, Deviazione standard).  
  
 ![Screenshot di Query Store che mostra che è possibile identificare e ottimizzare le query che consumano più risorse.](../../relational-databases/performance/media/query-store-usage-2.png "utilizzo archivio query 2")  
  
 Esaminare il riepilogo del piano a destra per analizzare la cronologia di esecuzione e avere informazioni sui diversi piani e sulle statistiche di runtime. Usare il riquadro inferiore per esaminare i diversi piani o per eseguire un confronto visivo con il rendering in modalità affiancata (usare il pulsante Confronta).  
  
Quando si identifica una query con prestazioni non ottimali, l'azione correttiva dipende dalla natura del problema.  
  
1.  Se la query è stata eseguita con più piani e l'ultimo piano è significativamente peggiore del piano precedente, è possibile usare il meccanismo di utilizzo forzato del piano per essere certi che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] userà il piano ottimale per le esecuzioni successive  
  
2.  Verificare se Query Optimizer rileva indici mancanti nel piano XML. In caso affermativo, creare l'indice mancante e usare Archivio query per valutare le prestazioni delle query dopo la creazione dell'indice  
  
3.  Assicurarsi che le statistiche siano aggiornate per le tabelle sottostanti usate dalla query.  
  
4.  Verificare che gli indici usati dalla query siano deframmentati.  
  
5.  Valutare l'opportunità di riscrivere la query con costo elevato. Ad esempio, sfruttare i vantaggi associati alla parametrizzazione della query e ridurre l'utilizzo di SQL dinamico. Implementare la logica ottimale durante la lettura dei dati (applicare il filtro dei dati sul lato database, non sul lato applicazione).  

## <a name="ab-testing"></a>Test A/B  
 Usare Archivio query per confrontare le prestazioni del carico di lavoro prima e dopo la modifica dell'applicazione che si intende introdurre. L'elenco seguente contiene alcuni esempi in cui è possibile usare Archivio query per valutare l'impatto della modifica dell'ambiente o dell'applicazione sulle prestazioni del carico di lavoro:  
  
-   Implementazione della nuova versione dell'applicazione.  
  
-   Aggiunta di nuovo hardware al server.  
  
-   Creazione di indici mancanti nelle tabelle a cui fanno riferimento query con costo elevato.  
  
-   Applicazione di un criterio di filtro per la sicurezza a livello di riga. Per altre informazioni, vedere [Optimizing Row Level Security with Query Store](/archive/blogs/sqlsecurity/optimizing-rls-performance-with-the-query-store) (Ottimizzazione della sicurezza a livello di riga con Query Store).  
  
-   Aggiunta del controllo temporale delle versioni di sistema alle tabelle che vengono modificate di frequente dalle applicazioni OLTP.  
  
In qualsiasi scenario, applicare il flusso di lavoro seguente:  
  
1.  Eseguire il carico di lavoro con Archivio query prima della modifica pianificata per generare dati di base delle prestazioni.  
  
2.  Applicare la modifica dell'applicazione in un determinato momento controllato.  
  
3.  Continuare l'esecuzione del carico di lavoro per il tempo necessario a generare un'immagine delle prestazioni del sistema dopo la modifica  
  
4.  Confrontare i risultati di 1 e 3.  
  
    1.  Aprire **Overall Database Consumption** (Consumo database globale) per determinare l'impatto sull'intero database.  
  
    2.  Aprire **Prime query per consumo di risorse** (o eseguire un'analisi con [!INCLUDE[tsql](../../includes/tsql-md.md)]) per analizzare l'impatto della modifica nelle query più importanti.  
  
5.  Decidere se mantenere la modifica o eseguire operazioni di rollback se le nuove prestazioni sono inaccettabili.  
  
La figura seguente mostra l'analisi di Archivio query (passaggio 4) in caso di creazione dell'indice mancante. Aprire il riquadro **Prime query per consumo di risorse** /Riepilogo piano per ottenere la visualizzazione per la query che dovrebbe essere interessata dalla creazione dell'indice:  
  
![Screenshot che mostra l'analisi di Query Store (passaggio 4) in caso di creazione dell'indice mancante.](../../relational-databases/performance/media/query-store-usage-3.png "utilizzo archivio query 3")  
  
È anche possibile confrontare i piani prima e dopo la creazione dell'indice con il rendering in modalità affiancata, usando l'opzione della barra degli strumenti Confronta i piani per la query selezionata in una finestra separata, contrassegnata con un quadrato rosso nella barra degli strumenti.  
  
![Screenshot che mostra Query Store e l'opzione della barra degli strumenti Confronta i piani per la query selezionata in una finestra separata.](../../relational-databases/performance/media/query-store-usage-4.png "utilizzo archivio query 4")  
  
Nel piano prima della creazione dell'indice (plan_id = 1, sopra) manca l'hint per l'indice e si può vedere che Clustered Index Scan è l'operatore con il costo più elevato nella query (rettangolo rosso).  
  
Nel piano dopo la creazione dell'indice mancante (plan_id = 15, sotto) ora è presente Index Seek (Nonclustered) che consente di ridurre il costo complessivo della query e di migliorare le prestazioni (rettangolo verde).  
  
In base all'analisi è consigliabile mantenere l'indice visto che le prestazioni delle query sono state migliorate.  
  
## <a name="keep-performance-stability-during-the-upgrade-to-newer-ssnoversion"></a><a name="CEUpgrade"></a> Mantenere la stabilità delle prestazioni durante l'aggiornamento alla nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Prima di [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], gli utenti erano esposti al rischio di regressione delle prestazioni durante l'aggiornamento alla versione più recente della piattaforma. Il motivo era che la versione più recente di Query Optimizer si attivava subito dopo l'installazione dei nuovi bit.  
  
A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] tutte le modifiche di Query Optimizer sono associate al [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) più recente, quindi i piani non vengono modificati esattamente al momento di aggiornamento, ma quando un utente modifica `COMPATIBILITY_LEVEL` in un livello più recente. Questa funzionalità, in combinazione con Archivio query, offre un alto livello di controllo sulle prestazioni delle query nel processo di aggiornamento. Il flusso di lavoro di aggiornamento consigliato è illustrato nella figura seguente:  
  
![Diagramma che illustra il flusso di lavoro di aggiornamento consigliato.](../../relational-databases/performance/media/query-store-usage-5.png "utilizzo archivio query 5")  
  
1.  Aggiornare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza modificare il livello di compatibilità del database. In questo modo non si espongono le modifiche più recenti di Query Optimizer, ma è possibile usare le funzionalità più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tra cui Query Store.  
  
2.  Abilitare Query Store. Per altre informazioni su questo argomento, vedere [Adattare Query Store al proprio carico di lavoro](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure).

3.  Consentire a Query Store di acquisire query e piani e stabilire una baseline delle prestazioni del processo con il livello di compatibilità del database sorgente/precedente. Non proseguire con i passaggi successivi finché tutti i piani sono stati acquisiti e la baseline è diventata stabile. Tale operazione può avere la durata di un ciclo aziendale normale per un carico di lavoro di produzione.  
  
4.  Passare al livello di compatibilità più recente: esporre il carico di lavoro alle modifiche più recenti di Query Optimizer e consentire la possibilità di creare nuovi piani.  
  
5.  Usare Query Store per le correzioni di analisi e regressioni: nella maggior parte dei casi, le nuove modifiche di Query Optimizer genereranno piani migliori. Query Store offre comunque un modo semplice per identificare le regressioni di scelta del piano e risolverle usando un meccanismo che forza il piano. A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)],quando si usa la funzionalità di [correzione automatica del piano](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) questo passaggio è automatico.  

    a.  Per i casi in cui sono presenti regressioni, forzare il piano considerato migliore in precedenza in Query Store.  
  
    b.  Se non è possibile forzare i piani di query o se le prestazioni non sono ancora sufficienti, è consigliabile ripristinare l'impostazione precedente del [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) e contattare il supporto tecnico Microsoft.  
    
> [!TIP]
> Usare l'attività *Aggiorna database* di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per aggiornare il [livello di compatibilità](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) del database. Per informazioni dettagliate, vedere [Aggiornamento di database mediante l'Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md).
  
## <a name="identify-and-improve-ad-hoc-workloads"></a>Identificare e migliorare i carichi di lavoro ad hoc  
Alcuni carichi di lavoro non includono query dominanti che è possibile ottimizzare per migliorare le prestazioni complessive dell'applicazione. In genere, questi carichi di lavoro sono caratterizzati da un numero relativamente elevato di query diverse, ognuna delle quali utilizza una parte delle risorse di sistema. Essendo univoche, queste query vengono eseguite raramente, (di solito una sola volta, quindi si consiglia di assegnare un nome ad hoc), di conseguenza il consumo di runtime non è critico. D'altra parte, dato che l'applicazione genera continuamente nuove query, una parte significativa delle risorse di sistema viene impiegata per la compilazione di query, il che non rappresenta uno scenario ottimale. Questa situazione non è ideale per Archivio query perché un numero elevato di query e piani ne occupa lo spazio riservato e causa probabilmente il passaggio in tempi molto rapidi alla modalità di sola lettura di Archivio query. Se è attivato **Modalità di pulizia basata sulle dimensioni** , un'opzione [fortemente consigliata](best-practice-with-the-query-store.md) per mantenere Archivio query sempre attivo e in esecuzione, il processo in background esegue costantemente la pulizia delle strutture di Archivio query assorbendo molto spesso notevoli risorse di sistema.  
  
 La visualizzazione **Prime query per consumo di risorse** fornisce la prima indicazione relativa alla natura ad hoc del carico di lavoro:  
  
![Screenshot della visualizzazione Prime query per consumo di risorse che mostra che la maggior parte delle prime query per consumo di risorse viene eseguita una sola volta.](../../relational-databases/performance/media/query-store-usage-6.png "utilizzo archivio query 6")  
  
Usare la metrica **Conteggio esecuzioni** per verificare se le prime query sono ad hoc (questa operazione richiede l'esecuzione di Archivio query con `QUERY_CAPTURE_MODE = ALL`). Dal diagramma precedente, è possibile vedere che il 90% delle **Prime query per consumo di risorse** viene eseguito una sola volta.  
  
In alternativa, è possibile eseguire script [!INCLUDE[tsql](../../includes/tsql-md.md)] per ottenere il numero totale di testi della query, query e piani nel sistema e determinarne le differenze confrontando query_hash e plan_hash:  
  
```sql  
--Do cardinality analysis when suspect on ad hoc workloads
SELECT COUNT(*) AS CountQueryTextRows FROM sys.query_store_query_text;  
SELECT COUNT(*) AS CountQueryRows FROM sys.query_store_query;  
SELECT COUNT(DISTINCT query_hash) AS CountDifferentQueryRows FROM  sys.query_store_query;  
SELECT COUNT(*) AS CountPlanRows FROM sys.query_store_plan;  
SELECT COUNT(DISTINCT query_plan_hash) AS  CountDifferentPlanRows FROM  sys.query_store_plan;  
```  
  
Si tratta di un potenziale risultato che può essere prodotto nel caso di carichi di lavoro con query ad hoc:  
  
![Screenshot del potenziale risultato che è possibile ottenere nel caso di carichi di lavoro con query ad hoc.](../../relational-databases/performance/media/query-store-usage-7.png "utilizzo archivio query 7")  
  
Il risultato della query mostra che, nonostante il numero elevato di query e piani in Query Store, query_hash e query_plan_hash in realtà non sono diversi. Un rapporto molto maggiore di 1 tra i testi delle query univoci e gli hash delle query univoci indica che il carico di lavoro è un buon candidato per la parametrizzazione perché l'unica differenza tra le query è la costante letterale (parametro) fornita come parte del testo della query.  
  
In genere, questa situazione si verifica quando l'applicazione genera query, invece di richiamare stored procedure o query con parametri, oppure quando si basa su framework di mapping relazionale a oggetti che generano query per impostazione predefinita.  
  
Se si ha il controllo del codice dell'applicazione, è possibile valutare l'opportunità di riscrivere il livello di accesso ai dati per usare stored procedure o query con parametri. Tuttavia, questa situazione può essere notevolmente migliorata anche senza modificare l'applicazione, forzando la parametrizzazione delle query per l'intero database, ovvero per tutte le query, oppure per i singoli modelli di query con lo stesso valore query_hash.  
  
L'approccio che prevede l'uso di singoli modelli di query richiede la creazione di guide di piano:  
  
```sql  
--Apply plan guide for the selected query template 
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'<your query text goes here>',  
    @stmt OUTPUT,   
    @params OUTPUT;  
  
EXEC sp_create_plan_guide   
    N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION (PARAMETERIZATION FORCED)';  
```  
  
La soluzione che include le guide di piano è più precisa, ma richiede più lavoro.  
  
Se tutte o la maggior parte delle query sono idonee alla parametrizzazione automatica, valutare la modifica di `FORCED PARAMETERIZATION` per l'intero database:  
  
```sql  
--Apply forced parameterization for entire database  
ALTER DATABASE <database name> SET PARAMETERIZATION FORCED;  
```  

> [!NOTE]
> Per altre informazioni su questo argomento, vedere le [linee guida per l'utilizzo della parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

Dopo aver applicato uno di questi passaggi, in **Prime query per consumo di risorse** viene visualizzata un'altra immagine del carico di lavoro.  
  
![Screenshot della visualizzazione Prime query per consumo di risorse che mostra un'immagine diversa del carico di lavoro.](../../relational-databases/performance/media/query-store-usage-8.png "utilizzo archivio query 8")  
  
In alcuni casi, l'applicazione può generare un numero elevato di query diverse, che non sono adatte alla parametrizzazione automatica. In questo caso, viene visualizzato un numero elevato di query nel sistema, ma il rapporto tra le query univoche e il valore univoco `query_hash` è probabilmente prossimo a 1.  
  
In questo caso, è opportuno abilitare l'opzione server [**Ottimizza per carichi di lavoro ad hoc**](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) per evitare di usare in modo non efficiente cache per query che probabilmente non verranno più eseguite. Per impedire l'acquisizione di tali query in Archivio query, impostare `QUERY_CAPTURE_MODE` su `AUTO`.  
  
```sql  
EXEC sys.sp_configure N'show advanced options', N'1' RECONFIGURE WITH OVERRIDE
GO
EXEC sys.sp_configure N'optimize for ad hoc workloads', N'1'
GO
RECONFIGURE WITH OVERRIDE
GO 
  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE CLEAR;  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE = ON   
    (OPERATION_MODE = READ_WRITE, QUERY_CAPTURE_MODE = AUTO);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md)         
 [Aggiornamento di database mediante l'Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
