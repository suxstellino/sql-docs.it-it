---
title: Colonne e tipo di dati XML (SQL Server) | Microsoft Docs
description: Informazioni sui vantaggi e sulle limitazioni del tipo di dati XML per l'archiviazione dei dati XML in SQL Server.
ms.custom: ''
ms.date: 05/17/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 00db8f21-7d4b-4347-ae43-3a7c314d2fa1
author: rothja
ms.author: jroth
ms.openlocfilehash: e05a14e95020606b6474cc582ebc61235a6009c4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486741"
---
# <a name="xml-data-type-and-columns-sql-server"></a>Colonne e tipo di dati XML (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento vengono illustrati i vantaggi e le limitazioni del tipo di dati **xml** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]e viene spiegato come scegliere la modalità di archiviazione dei dati XML.  
  
## <a name="relational-or-xml-data-model"></a>Modello di dati relazionale o XML  
 Se i dati utilizzati sono altamente strutturati in base a uno schema noto, il modello relazionale costituisce senz'altro il metodo di archiviazione ottimale. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili tutte le funzionalità e gli strumenti necessari. Se invece i dati sono semistrutturati, non strutturati o dotati di una struttura sconosciuta, sarà necessario prestare particolare attenzione alla modellazione.  
  
 XML costituisce la soluzione ottimale quando si desidera un modello indipendente dalla piattaforma, per garantire la portabilità dei dati tramite markup strutturale e semantico. Costituisce inoltre una soluzione appropriata nelle circostanze seguenti:  
  
-   I dati sono di tipo sparse, non se ne conosce la struttura o si prevede che in futuro la struttura verrà modificata in modo significativo.  
  
-   I dati rappresentano una gerarchia di contenuto, anziché riferimenti tra entità, e potrebbero essere ricorsivi.  
  
-   L'ordine è implicito nei dati.  
  
-   Si desidera eseguire query sui dati o aggiornarne alcune parti, in base alla struttura.  
  
 Se nessuna di queste condizioni è soddisfatta, è consigliabile utilizzare il modello di dati relazionale. Se ad esempio i dati sono in formato XML ma l'applicazione usa il database solo per archiviarli e recuperarli, è sufficiente usare una colonna di tipo **[n]varchar(max)** . L'archiviazione dei dati in una colonna XML offre ulteriori vantaggi, ad esempio la possibilità di determinare automaticamente se i dati sono validi e in formato corretto e includono anche il supporto per l'esecuzione di aggiornamenti e query dettagliate sui dati XML.  
  
## <a name="reasons-for-storing-xml-data-in-sql-server"></a>Motivi per l'archiviazione di dati XML in SQL Server  
 Di seguito sono illustrati alcuni dei motivi per utilizzare le caratteristiche XML native di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , anziché gestire i dati XML nel file system:  
  
-   Si desidera condividere, modificare ed eseguire query sui dati XML in un modo efficiente e basato sulle transazioni. Per l'applicazione è importante disporre di un accesso ai dati estremamente dettagliato, ad esempio perché si desidera estrarre alcune sezioni da un documento XML oppure inserire una nuova sezione senza sostituire l'intero documento.  
  
-   Si utilizzano sia dati relazionali che dati XML e si desidera garantirne l'interoperabilità nell'ambito dell'applicazione.  
  
-   È necessario il supporto del linguaggio per le query e la modifica dei dati per le applicazioni tra domini.  
  
-   Si desidera che il server garantisca che i dati sono nel formato corretto e, facoltativamente, che i dati vengano convalidati in base a XML Schema specifici.  
  
-   Si desidera indicizzare i dati XML per una più efficiente elaborazione delle query e una maggiore scalabilità e per consentire l'utilizzo di un'avanzata utilità di ottimizzazione delle query.  
  
-   Si desidera accedere ai dati XML tramite SOAP, ADO.NET e OLE DB.  
  
-   Per la gestione dei dati XML si desidera utilizzare le funzionalità amministrative del server di database, ad esempio le funzionalità di backup, recupero e replica.  
  
 Se nessuna di queste condizioni è soddisfatta, è preferibile archiviare i dati in un formato diverso da XML usando un tipo di dati per oggetti di grandi dimensioni, ad esempio **[n]varchar(max)** o **varbinary(max)** .  
  
## <a name="xml-storage-options"></a>Opzioni di archiviazione per i dati XML  
 Le opzioni di archiviazione per i dati XML disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] includono le seguenti:  
  
-   Archiviazione nativa tramite il tipo di dati **xml** .  
  
     I dati vengono archiviati in una rappresentazione interna che mantiene il contenuto XML e che contiene informazioni sulla gerarchia di contenimento, l'ordine dei documenti e i valori dell'elemento e dell'attributo. In particolare, viene mantenuto il contenuto InfoSet dei dati XML. Per altre informazioni su InfoSet, vedere [http://www.w3.org/TR/xml-infoset](https://go.microsoft.com/fwlink/?LinkId=48843). Il contenuto InfoSet potrebbe non essere una copia identica del testo XML, perché non vengono mantenute le informazioni seguenti: spazi vuoti non significativi, ordine degli attributi, prefissi degli spazi dei nomi e dichiarazione XML.  
  
     Per il tipo di dati **xml** tipizzato, un tipo di dati **xml** associato a XML Schema, PSVI (Post-Schema Validation InfoSet) aggiunge all'InfoSet le informazioni sul tipo e viene codificato nella rappresentazione interna. Questo consente di migliorare in modo significativo la velocità di analisi. Per altre informazioni, vedere le specifiche relative allo schema XML W3C su [http://www.w3.org/TR/xmlschema-1](https://go.microsoft.com/fwlink/?LinkId=48881) e [http://www.w3.org/TR/xmlschema-2](https://go.microsoft.com/fwlink/?LinkId=4871).  
  
-   Mapping tra archiviazione XML e relazionale  
  
     Tramite uno schema con annotazioni (AXSD), il codice XML viene scomposto in colonne e archiviato in una o più tabelle. Questo consente di mantenere la conformità dei dati a livello relazionale, conservando la struttura gerarchica anche se l'ordine degli elementi viene ignorato. Lo schema non può essere ricorsivo.  
  
-   Archiviazione di oggetti di grandi dimensioni, **[n]varchar(max)** e **varbinary(max)**  
  
     Viene archiviata una copia identica dei dati. Questo è utile per le applicazioni specializzate per scopi particolari, ad esempio quelle per i documenti legali. Per la maggior parte delle applicazioni non è invece necessaria una copia esatta, ma è sufficiente il contenuto XML (conformità all'InfoSet).  
  
 È in genere necessario utilizzare una combinazione di questi approcci, ad esempio se si desidera archiviare i dati XML in una colonna con tipo di dati **xml** e promuovere le relative proprietà al livello di colonne relazionali, oppure se si vuole usare la tecnologia di mapping per archiviare le parti non ricorsive in colonne non XML e solo le parti ricorsive in colonne con tipo di dati **xml** .  
  
### <a name="choice-of-xml-technology"></a>Scelta della tecnologia XML  
 La scelta della tecnologia XML, ovvero tra XML nativo e visualizzazione XML, dipende in genere dai fattori seguenti:  
  
-   Opzioni di archiviazione  
  
     Per i dati XML potrebbe essere più appropriata l'archiviazione di oggetti di grandi dimensioni (ad esempio, il manuale di un prodotto) oppure l'archiviazione in colonne relazionali (ad esempio, una voce convertita in formato XML). Ogni opzione di archiviazione mantiene la conformità del documento a un livello diverso.  
  
-   Funzionalità di query  
  
     L'opzione di archiviazione più appropriata dipende dalla natura delle query e dal livello di dettaglio con cui si desidera eseguire query sui dati XML. Le query dettagliate sui dati XML, quale la valutazione del predicato sui nodi XML, sono supportate a livelli diversi dalle due opzioni di archiviazione.  
  
-   Indicizzazione dei dati XML  
  
     Tramite l'indicizzazione dei dati XML è possibile migliorare le prestazioni delle query XML. Le opzioni di indicizzazione disponibili dipendono dalle opzioni di archiviazione ed è necessario selezionare quella più appropriata per ottimizzare il carico di lavoro.  
  
-   Funzionalità per la modifica dei dati  
  
     Alcuni carichi di lavoro comportano una modifica estremamente dettagliata dei dati XML, che può ad esempio includere l'aggiunta di una nuova sezione all'interno di un documento, mentre altri, ad esempio il contenuto Web, non richiedono modifiche così specifiche. Il supporto del linguaggio per la modifica dei dati può essere molto importante per un'applicazione.  
  
-   Supporto degli schemi  
  
     I dati XML possono essere descritti da uno schema che può essere costituito o meno da un documento di XML Schema. Il supporto dell'XML associato a schema dipende dalla tecnologia XML.  
  
 A opzioni diverse corrispondono caratteristiche di prestazioni diverse.  
  
### <a name="native-xml-storage"></a>Archiviazione XML nativa  
 È possibile archiviare i dati XML in una colonna con tipo di dati **xml** nel server. Questa soluzione è particolarmente appropriata se:  
  
-   Si desidera un metodo diretto per archiviare i dati XML nel server e, al tempo stesso, mantenere l'ordine e la struttura dei documenti.  
  
-   Non è sempre disponibile uno schema per i dati XML.  
  
-   Si desidera modificare ed eseguire query sui dati XML.  
  
-   Si desidera indicizzare i dati XML per una più rapida elaborazione delle query.  
  
-   L'applicazione richiede viste del catalogo di sistema per l'amministrazione di dati e XML Schema.  
  
 L'archiviazione XML nativa è utile quando si utilizzano documenti XML con strutture diverse oppure conformi a schemi diversi o complessi, di cui è difficile eseguire il mapping a strutture relazionali.  
  
#### <a name="example-modeling-xml-data-using-the-xml-data-type"></a>Esempio: modellazione di dati XML tramite il tipo di dati XML  
 Si consideri un manuale di prodotto in formato XML, composto da un capitolo per ogni argomento e con più sezioni in ogni capitolo. Una sezione può contenere sottosezioni e, di conseguenza, l'elemento \<section> è ricorsivo. I manuali dei prodotti includono elevati volumi di contenuto eterogeneo, ad esempio diagrammi e materiale tecnico, e i dati sono semistrutturati. Gli utenti possono avere l'esigenza di ricercare gli argomenti di interesse in un contesto specifico, ad esempio la sezione dedicata agli indici cluster nel capitolo dedicato all'indicizzazione, e di eseguire query su quantità tecniche.  
  
 Una colonna con tipo di dati **xml** costituisce un modello di archiviazione particolarmente appropriato per i documenti XML, perché mantiene il contenuto InfoSet dei dati XML. È possibile indicizzare la colonna XML per migliorare le prestazioni delle query.  
  
#### <a name="example-retaining-exact-copies-of-xml-data"></a>Esempio: conservazione di copie esatte dei dati XML  
 Si supponga che, per legge, sia necessario conservare copie testuali esatte dei propri documenti XML, quali documenti firmati, documenti legali oppure ordini di transazioni azionarie. È consigliabile archiviare i documenti in una colonna con tipo di dati **[n]varchar(max)** .  
  
 Per le query, in fase di esecuzione è necessario convertire i dati nel tipo di dati **xml** ed eseguire query XQuery su di essi. La conversione in fase di esecuzione può essere tuttavia molto costosa, soprattutto se il documento è di grandi dimensioni. Se è necessario eseguire query di frequente, è possibile archiviare una seconda copia dei documenti in una colonna con tipo di dati **xml** e indicizzarla, mentre per restituire le copie esatte dei documenti si usa la colonna con tipo di dati **[n]varchar(max)** .  
  
 La colonna XML può essere una colonna calcolata, basata sulla colonna **[n]varchar(max)** . Non è tuttavia possibile creare un indice XML su una colonna calcolata di tipo XML, né su colonne di tipo **[n]varchar(max)** o **varbinary(max)** .  
  
### <a name="xml-view-technology"></a>Tecnologia di visualizzazione XML  
 Definendo un mapping tra elementi XML Schema e le tabelle in un database, si crea una visualizzazione XML dei dati persistenti. Tramite la visualizzazione XML, è possibile utilizzare il caricamento bulk XML per popolare le tabelle sottostanti. È possibile eseguire query sulla visualizzazione XML utilizzando XPath versione 1.0. Le query vengono convertite in query SQL sulle tabelle. Analogamente, anche gli aggiornamenti vengono propagati a tali tabelle.  
  
 Questa tecnologia è utile nelle situazioni seguenti:  
  
-   Si desidera disporre di un modello di programmazione incentrato su XML, che utilizza visualizzazioni XML basate sui dati relazionali esistenti.  
  
-   Si dispone di uno schema (XSD, XDR) per i dati XML fornito da un partner esterno.  
  
-   L'ordine dei dati non è importante, i dati della tabella su cui si esegue la query non sono ricorsivi oppure si conosce in anticipo la profondità di ricorsione massima.  
  
-   Si desidera modificare ed eseguire query sui dati tramite la visualizzazione XML utilizzando XPath versione 1.0.  
  
-   Si desidera eseguire il caricamento bulk dei dati XML e scomporli nelle tabelle sottostanti utilizzando la visualizzazione XML.  
  
 Questa tecnica viene utilizzata ad esempio per i dati relazionali esposti come XML per lo scambio di dati e i servizi Web e per i dati XML con schema fisso. Per ulteriori informazioni vedere [MSDN Online Library](https://go.microsoft.com/fwlink/?linkid=31174).  
  
#### <a name="example-modeling-data-using-an-annotated-xml-schema-axsd"></a>Esempio: modellazione di dati tramite un elemento XML Schema con annotazioni (AXSD)  
 Si supponga ad esempio di avere a disposizione dati relazionali relativi a clienti, ordini e voci, che si desidera gestire come XML. Definire una visualizzazione XML applicando uno schema AXSD ai dati relazionali. La visualizzazione XML consente di eseguire il caricamento bulk dei dati XML nelle tabelle, nonché di aggiornare ed eseguire query sui dati relazionali. Questo modello è utile quando è necessario scambiare dati contenenti markup XML con altre applicazioni, senza interrompere le applicazioni SQL.  
  
### <a name="hybrid-model"></a>Modello ibrido  
 La soluzione più appropriata per la modellazione dei dati è spesso costituita da una combinazione di dati relazionali e colonne con tipo di dati **xml** . È possibile archiviare parte dei dati XML in colonne relazionali e il resto, o l'intero valore XML, in una colonna XML. Questa soluzione può migliorare le prestazioni, perché offre un maggiore controllo sugli indici creati sulle colonne relazionali e sulle caratteristiche di blocco.  
  
 I valori da archiviare nelle colonne relazionali dipendono dal carico di lavoro. Se ad esempio per il recupero di tutti i valori XML si usa l'espressione del percorso, /Customer/@CustId, promuovendo il valore dell'attributo **CustId** in modo da ottenere una colonna relazionale e indicizzando tale colonna sarà possibile eseguire le query molto più rapidamente. Se tuttavia i dati XML sono estensivamente scomposti in colonne relazionali in modo non ridondante, il riassemblaggio potrebbe avere un costo significativo.  
  
 Se ad esempio per dati XML altamente strutturati il contenuto di una tabella viene convertito in XML, sarà possibile eseguire il mapping di tutti i valori a colonne relazionali ed eventualmente utilizzare la tecnologia della visualizzazione XML.  
  
## <a name="granularity-of-xml-data"></a>Granularità dei dati XML  
 La granularità dei dati XML archiviata in una colonna XML è molto importante per le operazioni di blocco e, a un livello inferiore, è anche importante per gli aggiornamenti. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa lo stesso meccanismo di blocco sia per i dati XML che per i dati non XML. Di conseguenza, il blocco a livello di riga causa il blocco di tutte le istanze XML presenti nella riga. Quando la granularità è ampia, in uno scenario multiutente il blocco di istanze XML di grandi dimensioni per gli aggiornamenti provoca una diminuzione della velocità effettiva. Una scomposizione eccessiva, invece, comporta la perdita dell'incapsulamento degli oggetti e l'aumento del costo di riassemblaggio.  
  
 Il giusto equilibrio tra requisiti di modellazione dei dati e caratteristiche di blocco e aggiornamento è pertanto importante per una progettazione ottimale. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tuttavia, le dimensioni delle istanze XML effettivamente archiviate non costituiscono un fattore critico.  
  
 Per aggiornare un'istanza XML, ad esempio, viene utilizzato il nuovo supporto per l'aggiornamento parziale degli oggetti BLOB (Binary Large Object) e degli indici, in cui l'istanza XML archiviata viene confrontata con la versione aggiornata. Nell'aggiornamento parziale degli oggetti BLOB viene eseguito un confronto differenziale tra le due istanze XML e l'aggiornamento viene limitato alle differenze. Nell'aggiornamento parziale degli indici vengono modificate solo le righe che devono essere modificate nell'indice XML.  
  
## <a name="limitations-of-the-xml-data-type"></a>Limiti del tipo di dati xml  
 Considerare i seguenti limiti generali validi per il tipo di dati **xml** :  
  
-   La rappresentazione archiviata delle istanze con tipo di dati **xml** non può superare i 2 GB.  
  
-   Non è utilizzabile come sottotipo di un'istanza **sql_variant** .  
  
-   Non supporta il cast o la conversione in **text** o **ntext**. Usare invece **varchar(max)** or **nvarchar(max)** .  
  
-   Non può essere confrontato o ordinato. Questo significa che un tipo di dati **xml** non può essere utilizzato in un'istruzione GROUP BY.  
  
-   Non può essere utilizzato come parametro nelle funzioni scalari predefinite diverse da ISNULL, COALESCE e DATALENGTH.  
  
-   Non può essere utilizzato come colonna chiave in un indice. Può tuttavia essere inserito come tipo di dati in un indice cluster o essere aggiunto in modo esplicito a un indice non cluster utilizzando la parola chiave INCLUDE durante la creazione dell'indice.  

- Gli elementi XML supportano fino a 128 livelli di annidamento.
  
## <a name="see-also"></a>Vedere anche  
 [Esempi di importazione ed esportazione bulk di documenti XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
  
