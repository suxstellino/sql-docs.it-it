---
title: Indici XML (SQL Server) | Microsoft Docs
description: Informazioni sulla creazione di indici XML per le colonne con tipo di dati XML, che consente di migliorare le prestazioni di esecuzione delle query.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- deleting indexes
- secondary indexes [XML in SQL Server]
- xml data type [SQL Server], indexes
- dropping indexes
- PATH index
- DROP_EXISTING clause
- XML [SQL Server], indexes
- primary indexes [XML in SQL Server]
- indexes [SQL Server], XML
- XML indexes [SQL Server], secondary
- BLOBs, XML indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- XML indexes [SQL Server]
- XML indexes [SQL Server], primary
- modifying indexes
- XML indexes [SQL Server], dropping
- VALUE index
- XML indexes [SQL Server], xml data type
- PROPERTY index
- XML indexes [SQL Server], creating
ms.assetid: f5c9209d-b3f3-4543-b30b-01365a5e7333
author: rothja
ms.author: jroth
ms.openlocfilehash: d9e7d5a233b3f93d3588c70dd233db44c275719c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486591"
---
# <a name="xml-indexes-sql-server"></a>Indici XML (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile creare indici XML sulle colonne con tipo di dati **xml** . Tutti i tag, i valori e i percorsi delle istanze XML presenti nella colonna vengono indicizzati, migliorando le prestazioni delle query. Per le applicazioni in uso l'utilizzo di un indice XML può risultare vantaggioso nelle situazioni seguenti:  
  
-   Le query sulle colonne XML sono frequenti nel carico di lavoro. È necessario tenere in considerazione il costo di manutenzione dell'indice XML durante la modifica dei dati.  
  
-   I valori XML sono di grandi dimensioni mentre le parti recuperate sono relativamente piccole. Compilando un indice sarà possibile evitare l'analisi completa dei dati in fase di esecuzione ed eseguire ricerche basate sull'indice per una efficiente elaborazione delle query.  
  
 Gli indici XML rientrano nelle categorie seguenti:  
  
-   Indice XML primario  
  
-   Indice XML secondario  
  
 Il primo indice nella colonna di tipo **xml** deve essere l'indice XML primario, il cui utilizzo consente di supportare i tipi di indici secondari seguenti: PATH, VALUE e PROPERTY. In base al tipo di query, questi indici secondari possono facilitare il miglioramento delle prestazioni delle query.  
  
> [!NOTE]  
>  Non è possibile creare o modificare un indice XML a meno che le opzioni del database siano correttamente impostate per l'uso del tipo di dati **xml** . Per altre informazioni, vedere [Utilizzo della ricerca full-text con colonne XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
 Le istanze XML vengono archiviate nelle colonne di tipo **xml** come oggetti BLOB (Binary Large Object). Tali istanze XML possono essere di grandi dimensioni e la rappresentazione binaria archiviata delle istanze del tipo di dati **xml** può raggiungere dimensioni massime di 2 GB. Senza un indice, questi oggetti BLOB vengono suddivisi in fase di esecuzione per valutare una query. Questa suddivisione può richiedere molto tempo. Si consideri ad esempio la query seguente:  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.query('  
  /PD:ProductDescription/PD:Summary  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist ('/PD:ProductDescription/@ProductModelID[.="19"]') = 1  
```  
  
 Per selezionare le istanze XML che soddisfano la condizione della clausola `WHERE` , gli oggetti BLOB XML in ogni riga della tabella `Production.ProductModel` vengono suddivisi in fase di esecuzione. In seguito, viene valutata l'espressione `(/PD:ProductDescription/@ProductModelID[.="19"]`) nel metodo `exist()` . La suddivisione in fase di esecuzione può essere costosa, a seconda delle dimensioni e del numero di istanze archiviate nella colonna.  
  
 Se l'esecuzione di query sugli oggetti BLOB XML è un processo comune nell'ambiente di lavoro specifico, può facilitare l'indicizzazione delle colonne di tipo **xml** . Tuttavia, la manutenzione dell'indice durante la modifica dei dati presuppone un costo associato.  
  
## <a name="primary-xml-index"></a>Indice XML primario  
 L'indice XML primario consente di indicizzare tutti i tag, i valori e i percorsi contenuti nelle istanze XML di una colonna XML. Per creare un indice XML primario, è necessario che la tabella contenente la colonna XML da indicizzare includa un indice cluster nella chiave primaria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa questa chiave primaria per correlare righe nell'indice XML primario con le righe nella tabella che contiene la colonna XML.  
  
 L'indice XML primario è una rappresentazione suddivisa e persistente degli oggetti BLOB XML contenuti nella colonna con tipo di dati **xml** . Per ogni BLOB XML contenuto nella colonna, l'indice crea diverse righe di dati. Il numero di righe dell'indice corrisponde approssimativamente al numero di nodi del BLOB XML. Quando una query recupera l'istanza XML completa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce l'istanza dalla colonna XML. Le query all'interno delle istanze XML utilizzano l'indice XML primario e possono restituire valori scalari o sottoalberi XML utilizzando l'indice stesso.  
  
 In ogni riga vengono archiviate le informazioni seguenti:  
  
-   Nome di tag, ad esempio un nome di elemento o di attributo.  
  
-   Valore di nodo.  
  
-   Tipo di nodo, ad esempio un nodo elemento, un nodo attributo o un nodo testo.  
  
-   Informazioni sull'ordine dei dati nel documento, rappresentate da un identificatore di nodo interno.  
  
-   Percorso da ogni nodo al nodo radice dell'albero XML. In questa colonna viene eseguita la ricerca delle espressioni di percorso della query.  
  
-   Chiave primaria della tabella di base. La chiave primaria della tabella di base viene duplicata nell'indice XML primario per eseguire un join all'indietro con la tabella di base e il numero massimo di colonne nella chiave primaria della tabella di base è limitato a 15.  
  
 Le informazioni sul nodo vengono utilizzate per valutare e costruire i risultati XML di una query specificata. A scopo di ottimizzazione, il nome di tag e le informazioni sul tipo di nodo vengono codificati come valori integer e per la colonna Path viene utilizzata la stessa codifica. Inoltre, i percorsi vengono archiviati in ordine inverso per consentirne la corrispondenza quando è noto solo il relativo suffisso. Ad esempio:  
  
-   `//ContactRecord/PhoneNumber` in cui sono noti solo gli ultimi due passaggi  
  
 OR  
  
-   `/Book/*/Title` in cui il carattere jolly (`*`) viene specificato nella parte centrale dell'espressione.  
  
 Query Processor usa l'indice XML primario per le query che implicano [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md) e restituisce valori scalari o sottoalberi XML dall'indice primario stesso. In tale indice vengono archiviate tutte le informazioni necessarie per ricostruire l'istanza XML.  
  
 Ad esempio, la query seguente restituisce informazioni di riepilogo archiviate nella colonna di tipo `CatalogDescription`**xml** nella tabella `ProductModel` . La query restituisce informazioni <`Summary`> solo per modelli di prodotti la cui descrizione di catalogo contiene inoltre la descrizione <`Features`>.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")SELECT CatalogDescription.query('  /PD:ProductDescription/PD:Summary') as ResultFROM Production.ProductModelWHERE CatalogDescription.exist ('/PD:ProductDescription/PD:Features') = 1  
```  
  
 In relazione all'indice XML primario, anziché suddividere ogni istanza degli oggetti BLOB XML nella tabella di base, nelle righe dell'indice che corrispondono a ogni oggetto BLOB XML viene eseguita la ricerca sequenziale dell'espressione specificata nel metodo `exist()` . Se il percorso viene individuato nella colonna Path dell'indice, l'elemento <`Summary`> e i relativi sottoalberi vengono recuperati dall'indice XML primario e convertiti in un oggetto BLOB XML come risultato del metodo `query()`.  
  
 Si noti che l'indice XML primario non viene utilizzato per il recupero di un'istanza XML completa. Ad esempio, la query seguente recupera dalla tabella l'intera istanza XML che descrive le istruzioni di produzione per un modello di prodotto specifico.  
  
```  
USE AdventureWorks2012;SELECT InstructionsFROM Production.ProductModel WHERE ProductModelID=7;  
```  
  
## <a name="secondary-xml-indexes"></a>Indici XML secondari  
 Per migliorare le prestazioni di ricerca, è possibile creare indici XML secondari. A tale scopo, deve esistere innanzitutto un indice XML primario, prima di poterne creare di secondari. Tipi di indici secondari:  
  
-   Indice XML secondario PATH  
  
-   Indice XML secondario VALUE  
  
-   Indice XML secondario PROPERTY  
  
 Di seguito sono riportate alcune linee guida per la creazione di uno o più indici di questo tipo:  
  
-   Se nel carico di lavoro viene fatto un utilizzo significativo di espressioni di percorso sulle colonne XML, per migliorare le prestazioni di lavoro è possibile utilizzare l'indice XML secondario PATH. Il caso più comune è rappresentato dall'uso del metodo **exist()** sulle colonne XML nella clausola WHERE di Transact-SQL.  
  
-   Se il carico di lavoro recupera più valori da singole istanze XML utilizzando espressioni di percorso, può essere utile eseguire il clustering dei percorsi nell'ambito di ogni istanza XML nell'indice PROPERTY. Questa situazione si presenta in genere negli scenari che prevedono l'utilizzo di contenitori di proprietà, quando vengono recuperate le proprietà di un oggetto di cui è noto il valore della chiave primaria.  
  
-   Se il carico di lavoro richiede l'esecuzione di query per il recupero di valori nelle istanze XML, senza conoscere i nomi degli elementi o attributi che contengono tali valori, sarà possibile creare l'indice VALUE. Questa situazione si verifica in genere nelle ricerche su assi discendenti, ad esempio //author[last-name="Howard"], in cui gli elementi \<author> possono essere presenti a ogni livello della gerarchia. Si verifica anche nelle query che usano caratteri jolly, ad esempio /book [@* = "novel"], in cui la query cerca elementi \<book> contenenti un attributo con valore "novel".  
  
### <a name="path-secondary-xml-index"></a>Indice XML secondario PATH  
 Se in genere le query specificano espressioni di percorso nelle colonne di tipo **xml** , un indice secondario PATH potrebbe velocizzare la ricerca. Come precedentemente descritto in questo argomento, l'indice primario è utile nel caso di query che specificano il metodo **exist()** nella clausola WHERE. Se si aggiunge un indice secondario PATH, è possibile migliorare le prestazioni della ricerca in tali query.  
  
 Sebbene un indice XML primario consenta di evitare la suddivisione dei BLOB XML in fase di esecuzione, potrebbe non garantire prestazioni ottimali per le query basate su espressioni di percorso. Poiché in tutte le righe dell'indice XML primario corrispondenti a un BLOB XML viene eseguita la ricerca sequenziale di istanze XML di grandi dimensioni, tale ricerca potrebbe risultare lenta. In tal caso, l'utilizzo di un indice secondario basato sui valori di percorso e di nodo dell'indice primario può velocizzare in modo significativo la ricerca nell'indice. Nell'indice secondario PATH, i valori di percorso e di nodo sono colonne chiave che consentono ricerche di percorsi più efficienti. Query Optimizer può utilizzare l'indice PATH per espressioni analoghe alle seguenti:  
  
-   `/root/Location` che specifica solo un percorso  
  
 OR  
  
-   `/root/Location/@LocationID[.="10"]` in cui vengono specificati sia il valore di percorso che il valore di nodo.  
  
 Nella query seguente viene illustrato il caso in cui è utile l'indice PATH:  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.query('  
  /PD:ProductDescription/PD:Summary  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist ('/PD:ProductDescription/@ProductModelID[.="19"]') = 1  
```  
  
 Nella query l'espressione di percorso `/PD:ProductDescription/@ProductModelID` e il valore `"19"` nel metodo `exist()` corrispondono ai campi chiave dell'indice PATH. In tal modo, è possibile eseguire una ricerca diretta nell'indice PATH e ottenere prestazioni di ricerca migliori rispetto a quelle della ricerca sequenziale di valori di percorso nell'indice primario.  
  
### <a name="value-secondary-xml-index"></a>Indice XML secondario VALUE  
 Se le query sono basate su valori, come ad esempio nel caso di `/Root/ProductDescription/@*[. = "Mountain Bike"]` o `//ProductDescription[@Name = "Mountain Bike"]`, e il percorso specificato non è completo o include un carattere jolly, è possibile velocizzare la ricerca compilando un indice XML secondario basato sui valori di nodo dell'indice XML primario.  
  
 Le colonne chiave dell'indice VALUE sono il valore di nodo e il percorso dell'indice XML primario. Se il carico di lavoro implica l'esecuzione di query per valori da istanze XML senza conoscere i nomi di elemento o di attributo che contengono tali valori, un indice VALUE può risultare utile. Ad esempio, un indice VALUE risulta vantaggioso per l'espressione seguente:  
  
-   `//author[LastName="someName"]` dove si conosce il valore dell'elemento <`LastName`> ma l'elemento padre <`author`> può trovarsi ovunque.  
  
-   `/book[@* = "someValue"]` dove la query cerca l'elemento <`book`> che dispone di un qualche attributo con valore `"someValue"`.  
  
 La query seguente restituisce `ContactID` dalla tabella `Contact` . La clausola `WHERE` specifica un filtro che esegue la ricerca di valori nella colonna di tipo `AdditionalContactInfo`**xml** . Gli ID dei contatti vengono restituiti solo se il BLOB XML con le informazioni aggiuntive corrispondenti include un numero di telefono specifico. Poiché l'elemento <`telephoneNumber`> può trovarsi ovunque nell'XML, l'espressione del percorso specifica l'asse discendente o stesso.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS CI,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS ACT)  
  
SELECT ContactID   
FROM   Person.Contact  
WHERE  AdditionalContactInfo.exist('//ACT:telephoneNumber/ACT:number[.="111-111-1111"]') = 1  
```  
  
 In questo caso, il valore di ricerca di <`number`> è noto ma può trovarsi ovunque nell'istanza XML come figlio dell'elemento <`telephoneNumber`>. Per questo tipo di query può risultare utile eseguire una ricerca nell'indice basata su un valore specifico.  
  
### <a name="property-secondary-index"></a>Indice secondario PROPERTY  
 Per le query che recuperano uno o più valori da singole istanze XML può essere utile un indice PROPERTY. Questa situazione si verifica quando si recuperano proprietà dell'oggetto tramite il metodo **value()** del tipo **xml** e quando si conosce il valore della chiave primaria dell'oggetto.  
  
 L'indice PROPERTY viene compilato in base alle colonne PK e Path e al valore di nodo dell'indice XML primario, in cui PK è la chiave primaria della tabella di base.  
  
 Ad esempio, per il modello di prodotto `19`, la query seguente recupera i valori degli attributi `ProductModelID` e `ProductModelName` tramite il metodo `value()` . Anziché utilizzare l'indice XML primario o gli altri indici XML secondari, l'indice PROPERTY consente di velocizzare l'esecuzione.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.value('(/PD:ProductDescription/@ProductModelID)[1]', 'int') as ModelID,  
       CatalogDescription.value('(/PD:ProductDescription/@ProductModelName)[1]', 'varchar(30)') as ModelName          
FROM Production.ProductModel     
WHERE ProductModelID = 19  
```  
  
 Ad eccezione delle differenze descritte più avanti in questo argomento, la creazione di un indice XML in una colonna di tipo **xml** è simile alla creazione di un indice in una colonna di tipo non **xml** . Per la creazione e la gestione di indici XML, è possibile utilizzare le istruzioni DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti:  
  
-   [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
-   [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
-   [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
  
## <a name="getting-information-about-xml-indexes"></a>Informazioni sugli indici XML  
 Le voci di un indice XML compaiono nella vista del catalogo sys.indexes con tipo di indice 3. La colonna del nome contiene il nome dell'indice XML.  
  
 Gli indici XML vengono registrati anche nella vista del catalogo sys.xml_indexes, che contiene tutte le colonne della vista sys.indexes e alcune colonne specifiche, utili per gli indici XML. Il valore NULL nella colonna secondary_type indica un indice XML primario, mentre i valori 'P', 'R' e 'V' indicano, rispettivamente, indici XML secondari di tipo PATH, PROPERTY e VALUE.  
  
 Per ottenere informazioni sullo spazio usato dagli indici XML è possibile usare la funzione con valori di tabella [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md). che restituisce informazioni come il numero di pagine del disco occupate, le dimensioni medie delle righe in byte e il numero dei record, per tutti i tipi di indici, inclusi gli indici XML. Tali informazioni sono disponibili per ogni partizione del database. Gli indici XML utilizzano lo stesso schema di partizionamento e la stessa funzione di partizionamento della tabella di base.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
