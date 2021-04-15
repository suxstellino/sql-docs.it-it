---
title: Percorsi e hint di ottimizzazione per indici XML selettivi | Microsoft Docs
description: Informazioni su come specificare i percorsi dei nodi e gli hint di ottimizzazione quando si creano o si modificano indici XML selettivi.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 486ee339-165b-4aeb-b760-d2ba023d7d0a
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 71fa7fad610b309232d1fd80e381fbc1674d1143
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487418"
---
# <a name="specify-paths-and-optimization-hints-for-selective-xml-indexes"></a>Specificare percorsi e hint di ottimizzazione per indici XML selettivi
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come specificare i percorsi del nodo da indicizzare e gli hint di ottimizzazione per l'indicizzazione in fase di creazione o modifica di indici XML selettivi.  
  
 Specificare contemporaneamente i percorsi del nodo e gli hint di ottimizzazione in una delle istruzioni seguenti:  
  
-   Nella clausola **FOR** di un'istruzione **CREATE** . Per altre informazioni, vedere [CREATE SELECTIVE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
-   Nella clausola **ADD** di un'istruzione **ALTER** . Per altre informazioni, vedere [ALTER INDEX &#40;indici XML selettivi&#41;](../../t-sql/statements/alter-index-selective-xml-indexes.md).  
  
 Per altre informazioni sugli indici XML selettivi, vedere [Indici XML selettivi &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
##  <a name="understanding-xquery-and-sql-server-types-in-untyped-xml"></a><a name="untyped"></a> Informazioni sui tipi XQuery e SQL Server in dati XML non tipizzati  
 Negli indici XML selettivi sono supportati due sistemi di tipi: tipi XQuery e tipi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il percorso indicizzato può essere utilizzato per la corrispondenza di un'espressione XQuery o del tipo restituito del metodo value() del tipo di dati XML.  
  
-   Se un percorso da indicizzare non viene annotato oppure viene annotato con la parola chiave XQUERY, il percorso corrisponde a un'espressione XQuery. Esistono due varianti per i percorsi dei nodi con annotazioni XQuery:  
  
    -   Se non si specificano la parola chiave XQUERY e il tipo di dati XQuery, vengono utilizzati i mapping predefiniti. In genere le prestazioni e l'archiviazione non raggiungono livelli ottimali.  
  
    -   Se si specificano la parola chiave XQUERY e il tipo di dati XQuery (e facoltativamente altri hint di ottimizzazione), è possibile ottenere prestazioni ottimali e un livello estremamente efficiente di archiviazione. Un cast potrebbe tuttavia avere esito negativo.  
  
-   Se un percorso da indicizzare viene annotato con la parola chiave SQL, il percorso corrisponde al tipo restituito del metodo value() del tipo di dati XML. Specificare il tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appropriato, ovvero il tipo restituito previsto dal metodo value().  
  
 Esistono differenze minime tra il sistema di tipi XML di espressioni XQuery e il sistema di tipi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] validi per il metodo value() del tipo di dati XML. Di seguito sono riportate queste differenze:  
  
-   Il sistema di tipi XQuery riconosce gli spazi finali. Ad esempio, in base alla semantica del tipo XQuery, le stringhe "abc" e "abc " non sono uguali, a differenza di quanto accade in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   I tipi di dati a virgola mobile XQuery supportano valori speciali di +/- zero e +/- infinito. Questi valori speciali non sono supportati nei tipi di dati a virgola mobile [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="xquery-types-in-untyped-xml"></a>Tipi XQuery in dati XML non tipizzati  
  
-   I tipi XQuery corrispondono alle espressioni XQuery in tutti i metodi del tipo di dati XML, incluso il metodo value().  
  
-   I tipi XQuery supportano questi hint di ottimizzazione: node(), SINGLETON, DATA TYPE e MAXLENGTH.  
  
 Per le espressioni XQuery in dati XML non tipizzati, è possibile scegliere tra due modalità di esecuzione:  
  
-   **Modalità di mapping predefinito**. In questa modalità è necessario specificare solo il percorso in fase di creazione di un indice XML selettivo.  
  
-   **Modalità di mapping definito dall'utente**. In questa modalità è necessario specificare il percorso e hint di ottimizzazione facoltativi.  
  
 Nella modalità di mapping predefinito viene utilizzata un'opzione di archiviazione conservativa, generica e sempre sicura. Può corrispondere a qualsiasi tipo di espressione. Alcune limitazioni della modalità di mapping predefinito sono rappresentate da prestazioni non ottimali, dovute alla necessità di eseguire un numero maggiore di cast di runtime, e dalla mancanza di indici secondari.  
  
 Di seguito è riportato un esempio di un indice XML selettivo creato con mapping predefiniti. Per tutti e tre i percorsi vengono usati il tipo di nodo predefinito (**xs:untypedAtomic**) e la cardinalità.  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_default  
ON Tbl(xmlcol)  
FOR  
(  
mypath01 =  '/a/b',  
mypath02 = '/a/b/c',  
mypath03 = '/a/b/d'  
)  
```  
  
 La modalità di mapping definito dall'utente consente di specificare un tipo e la cardinalità del nodo per ottenere prestazioni migliori. Questo miglioramento delle prestazioni viene tuttavia conseguito a discapito della sicurezza, perché un cast potrebbe avere esito negativo, e della generalità, perché la corrispondenza con l'indice XML selettivo viene rilevata solo per il tipo specificato.  
  
 Di seguito sono indicati i tipi XQuery supportati per i dati XML non tipizzati:  
  
-   **xs:boolean**  
  
-   **xs:double**  
  
-   **xs:string**  
  
-   **xs:date**  
  
-   **xs:time**  
  
-   **xs:dateTime**  
  
 Se il tipo viene omesso, il nodo verrà considerato del tipo di dati **xs:untypedAtomic** .  
  
 È possibile ottimizzare l'indice XML selettivo visualizzato nel modo seguente:  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_optimized  
ON Tbl(xmlcol)  
FOR  
(  
mypath= '/a/b' as XQUERY 'node()',  
pathX = '/a/b/c' as XQUERY 'xs:double' SINGLETON,  
pathY = '/a/b/d' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON  
)  
-- mypath - Only the node value is needed; storage is saved.  
-- pathX - Performance is improved; secondary indexes are possible.  
-- pathY - Performance is improved; secondary indexes are possible; storage is saved.  
```  
  
### <a name="sql-server-types-in-untyped-xml"></a>Tipi SQL Server in dati XML non tipizzati  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i tipi corrispondono al valore restituito del metodo value().  
  
-   I tipi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supportano l'hint di ottimizzazione SINGLETON.  
  
 È obbligatori specificare un tipo per i percorsi che restituiscono i tipi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilizzare lo stesso tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che verrebbe utilizzato nel metodo value().  
  
 Si consideri la query seguente:  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/d)[1]', 'NVARCHAR(200)')  
FROM myXMLTable T  
```  
  
 La query specificata restituisce un valore dal percorso `/a/b/d` all'interno di un tipo di dati NVARCHAR(200), in modo che il tipo di dati da specificare per il nodo sia facilmente intuibile. Non esiste tuttavia uno schema per specificare la cardinalità del nodo in dati XML non tipizzati. Per specificare che il nodo `d` venga visualizzato al massimo una volta nel nodo padre `b`, creare un indice XML selettivo in cui venga utilizzato l'hint di ottimizzazione SINGLETON nel modo seguente:  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_US  
ON Tbl(xmlcol)  
FOR  
(  
node1223 = '/a/b/d' as SQL NVARCHAR(200) SINGLETON  
)  
```  
  
  
##  <a name="understanding-selective-xml-index-support-for-typed-xml"></a><a name="typed"></a> Informazioni sul supporto degli indici XML selettivi per dati XML tipizzati  
 I dati XML tipizzati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rappresentano uno schema associato a uno specifico documento XML. Lo schema definisce la struttura generica del documento e i tipi di nodi. Se esiste uno schema, l'indice XML selettivo applica la sua struttura quando l'utente promuove i percorsi, pertanto non è necessario specificare i tipi XQUERY per i percorsi.  
  
 Nell'indice XML selettivo vengono supportati i tipi XSD seguenti:  
  
-   **xs:anyUri**  
  
-   **xs:boolean**  
  
-   **xs:date**  
  
-   **xs:dateTime**  
  
-   **xs:day**  
  
-   **xs: decimal**  
  
-   **xs:double**  
  
-   **xs:float**  
  
-   **xs:int**  
  
-   **xs:integer**  
  
-   **xs:language**  
  
-   **xs:long**  
  
-   **xs:name**  
  
-   **xs:NCName**  
  
-   **xs:negativeInteger**  
  
-   **xs:nmtoken**  
  
-   **xs:nonNegativeInteger**  
  
-   **xs:nonPositiveInteger**  
  
-   **xs:positiveInteger**  
  
-   **xs:qname**  
  
-   **xs:short**  
  
-   **xs:string**  
  
-   **xs:time**  
  
-   **xs:token**  
  
-   **xs:unsignedByte**  
  
-   **xs:unsignedInt**  
  
-   **xs:unsignedLong**  
  
-   **xs:unsignedShort**  
  
 Quando viene creato un indice XML selettivo in un documento con uno schema associato, se si specifica un tipo XQUERY in fase di creazione o modifica dell'indice, verrà restituito un errore. L'utente può utilizzare annotazioni di tipo SQL nell'ambito della promozione del percorso. Il tipo SQL deve essere una conversione valida del tipo CLR definito nello schema. In caso contrario, verrà generato un errore. Sono supportati tutti i tipi SQL con una rappresentazione appropriata in XSD, ad eccezione dei tipi date/time.  
  
> [!NOTE]  
>  L'indice selettivo viene utilizzato se il tipo specificato nella promozione del percorso dell'indice XML selettivo corrisponde al valore restituito del metodo value().  
  
 Con i documenti XML tipizzati è possibile utilizzare gli hint di ottimizzazione seguenti:  
  
-   hint di ottimizzazione node().  
  
-   L'hint di ottimizzazione MAXLENGTH può essere utilizzato con tipi xs:string per abbreviare il valore indicizzato.  
  
 Per altre informazioni sugli hint di ottimizzazione, vedere [Specifica di hint di ottimizzazione](#hints).  
  
##  <a name="specifying-paths"></a><a name="paths"></a> Specifica di percorsi  
 Un indice XML selettivo consente di indicizzare solo un subset di nodi dai dati XML archiviati rilevanti per le query che si intende eseguire. Se il subset dei nodi correlati è nettamente inferiore al numero totale di nodi nel documento XML, nell'indice XML selettivo vengono archiviati solo i nodi rilevanti. Per trarre vantaggio da un indice XML selettivo, identificare il corretto subset di nodi da indicizzare.  
  
### <a name="choosing-the-nodes-to-index"></a>Scelta dei nodi da indicizzare  
 Per identificare il subset corretto di nodi da aggiungere a un indice XML selettivo, è possibile utilizzare i due semplici principi indicati di seguito.  
  
1.  **Principio 1**: per valutare una specifica espressione XQuery, indicizzare tutti i nodi da esaminare.  
  
    -   Indicizzare tutti i nodi la cui esistenza o il cui valore viene utilizzato nell'espressione XQuery.  
  
    -   Indicizzare tutti i nodi dell'espressione XQuery in cui vengono applicati predicati XQuery.  
  
     Si consideri la semplice query sul [documento XML di esempio](#sample) in questo argomento:  
  
    ```sql  
    SELECT T.record FROM myXMLTable T  
    WHERE T.xmldata.exist('/a/b[./c = "43"]') = 1  
    ```  
  
     Per restituire istanze XML che soddisfano la query, un indice XML selettivo deve esaminare due nodi in ogni istanza XML:  
  
    -   Il nodo `c`, in quanto il rispettivo valore viene utilizzato nell'espressione XQuery.  
  
    -   Il nodo `b`, poiché viene applicato un predicato per il nodo`b` nell'espressione XQuery.  
  
2.  **Principio 2**: per ottenere prestazioni ottimali, indicizzare tutti i nodi necessari per valutare una specifica espressione XQuery. Se si indicizzano solo alcuni nodi, l'indice XML selettivo migliora la valutazione delle sottoespressioni che includono solo nodi indicizzati.  

 Per migliorare le prestazioni dell'istruzione SELECT precedentemente illustrata, è possibile creare l'indice XML selettivo seguente:  
  
```sql  
CREATE SELECTIVE XML INDEX simple_sxi  
ON Tbl(xmlcol)  
FOR  
(  
    path123 =  '/a/b',  
    path124 =  '/a/b/c'  
)  
```  
  
### <a name="indexing-identical-paths"></a>Indicizzazione di percorsi identici  
 Non è possibile promuovere percorsi identici come tipo di dati identico in nomi di percorso diversi. Ad esempio, la query seguente genera un errore, in quanto `pathOne` e `pathTwo` sono identici:  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:string',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
 È tuttavia possibile promuovere percorsi diversi come tipi di dati diversi con nomi diversi. Ad esempio, la query seguente è accettabile, poiché i tipi di dati sono diversi:  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:double',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
### <a name="examples"></a>Esempi  
 Di seguito sono riportati alcuni esempi aggiuntivi di scelta dei nodi corretti da indicizzare per tipi XQuery diversi.  
  
 **Esempio 1**  
  
 Di seguito è riportata una semplice espressione XQuery in cui viene utilizzato il metodo exist():  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e/h') = 1  
```  
  
 Nella tabella seguente vengono indicati i nodi da indicizzare per consentire l'utilizzo dell'indice XML selettivo da parte della query.  
  
|Nodo da includere nell'indice|Motivo alla base dell'indicizzazione del nodo|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e/h**|L'esistenza del nodo `h` viene valutata nel metodo exist().|  
  
 **Esempio 2**  
  
 Di seguito è riportata una variante più complessa dell'espressione XQuery precedente, con un predicato applicato:  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e[./f = "SQL"]') = 1  
```  
  
 Nella tabella seguente vengono indicati i nodi da indicizzare per consentire l'utilizzo dell'indice XML selettivo da parte della query.  
  
|Nodo da includere nell'indice|Motivo alla base dell'indicizzazione del nodo|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Viene applicato un predicato per il nodo `e`.|  
|**/a/b/c/d/e/f**|Il valore del nodo `f` viene valutato nel predicato.|  
  
 **Esempio 3**  
  
 Di seguito è riportata una query più complessa con una clausola value():  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/c/d/e[./f = "SQL"]/g)[1]', 'nvarchar(100)')  
FROM myXMLTable T  
```  
  
 Nella tabella seguente vengono indicati i nodi da indicizzare per consentire l'utilizzo dell'indice XML selettivo da parte della query.  
  
|Nodo da includere nell'indice|Motivo alla base dell'indicizzazione del nodo|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Viene applicato un predicato per il nodo `e`.|  
|**/a/b/c/d/e/f**|Il valore del nodo `f` viene valutato nel predicato.|  
|**/a/b/c/d/e/g**|Il valore del nodo `g` viene restituito dal metodo value().|  
  
 **Esempio 4**  
  
 Di seguito è riportata una query in cui viene utilizzata una clausola FLWOR in una clausola exist(). Il nome FLWOR deriva dalle cinque clausole che possono costituire un'espressione XQuery FLWOR: for, let, where, order by e return.  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('  
  For $x in /a/b/c/d/e  
  Where $x/f = "SQL"  
  Return $x/g  
') = 1  
```  
  
 Nella tabella seguente vengono indicati i nodi da indicizzare per consentire l'utilizzo dell'indice XML selettivo da parte della query.  
  
|Nodo da includere nell'indice|Motivo alla base dell'indicizzazione del nodo|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|L'esistenza del nodo `e` viene valutata nella clausola FLWOR.|  
|**/a/b/c/d/e/f**|Il valore del nodo `f` viene valutato nella clausola FLWOR.|  
|**/a/b/c/d/e/g**|L'esistenza del nodo `g` viene valutata dal metodo exist().|  
  
  
##  <a name="specifying-optimization-hints"></a><a name="hints"></a> Specifica di hint di ottimizzazione  
 È possibile utilizzare hint di ottimizzazione facoltativi per specificare ulteriori dettagli sul mapping per un nodo indicizzato da un indice XML selettivo. È ad esempio possibile specificare il tipo di dati e la cardinalità del nodo, nonché determinate informazioni sulla struttura dei dati. Queste informazioni aggiuntive garantiscono un mapping più efficiente, oltre a determinare miglioramenti nelle prestazioni o risparmi in termini di archiviazione, se non entrambi i casi.  
  
 L'utilizzo di hint di ottimizzazione è facoltativo. È sempre possibile accettare i mapping predefiniti, affidabili ma non necessariamente in grado di garantire livelli ottimali di prestazioni e archiviazione.  
  
 Alcuni hint di ottimizzazione, ad esempio l'hint SINGLETON, introducono vincoli sui dati. In alcuni casi, potrebbero verificarsi errori se non si rispettano tali vincoli.  
  
### <a name="benefits-of-optimization-hints"></a>Vantaggi degli hint di ottimizzazione  
 Nella tabella seguente vengono identificati gli hint di ottimizzazione che supportano livelli più efficienti di prestazioni e archiviazione.  
  
|Hint di ottimizzazione|Archiviazione più efficiente|prestazioni migliorate|  
|-----------------------|----------------------------|--------------------------|  
|**node()**|Sì|No|  
|**SINGLETON**|No|Sì|  
|**DATA TYPE**|Sì|Sì|  
|**MAXLENGTH**|Sì|Sì|  
  
### <a name="optimization-hints-and-data-types"></a>Hint di ottimizzazione e tipi di dati  
 È possibile indicizzare nodi come tipi di dati XQuery o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella tabella seguente vengono illustrati gli hint di ottimizzazione supportati da ogni tipo di dati.  
  
|Hint di ottimizzazione|Tipi di dati XQuery|Tipi di dati SQL|  
|-----------------------|-----------------------|--------------------|  
|**node()**|Sì|No|  
|**SINGLETON**|Sì|Sì|  
|**DATA TYPE**|Sì|No|  
|**MAXLENGTH**|Sì|No|  
  
### <a name="node-optimization-hint"></a>hint di ottimizzazione node()  
 Si applica a: Tipi di dati XQuery  
  
 È possibile utilizzare l'ottimizzazione node() per specificare un nodo il cui valore non è necessario per valutare la query tipica. Questo hint riduce i requisiti di archiviazione se la query tipica deve esclusivamente valutare l'esistenza del nodo. Per impostazione predefinita, un indice XML selettivo archivia il valore per tutti i nodi promossi, ad eccezione dei tipi di nodi complessi.  
  
 Prendere in considerazione gli esempi seguenti:  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b[./c=5]') = 1  
```  
  
 Per utilizzare un indice XML selettivo per valutare la query, promuovere i nodi `b` e `c`. Tuttavia, poiché il valore del nodo `b` non è necessario, è possibile utilizzare l'hint node() con la sintassi seguente:  
  
 `/a/b/ as node()`  
  
 Se una query richiede il valore di un nodo indicizzato con l'hint node(), non è possibile utilizzare l'indice XML selettivo.  
  
### <a name="singleton-optimization-hint"></a>Hint di ottimizzazione SINGLETON  
 Si applica a: tipi di dati XQuery o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 L'hint di ottimizzazione SINGLETON specifica la cardinalità di un nodo. Questo hint migliora le prestazioni delle query, poiché è risaputo che un nodo viene visualizzato al massimo una volta nel relativo elemento padre o predecessore.  
  
 Si consideri il [documento XML di esempio](#sample) in questo argomento.  
  
 Per utilizzare un indice XML selettivo per eseguire una query sul documento, è possibile specificare l'hint SINGLETON per il nodo `d` , poiché viene visualizzato al massimo una volta all'interno del relativo padre.  
  
 Se è stato specificato l'hint SINGLETON ma un nodo viene visualizzato più di una volta nell'elemento padre o predecessore, viene generato un errore in fase di creazione dell'indice (per i dati esistenti) oppure in fase di esecuzione di una query (per i nuovi dati).  
  
### <a name="data-type-optimization-hint"></a>Hint di ottimizzazione DATA TYPE  
 Si applica a: Tipi di dati XQuery  
  
 L'hint di ottimizzazione DATA TYPE consente di specificare un tipo di dati XQuery o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il nodo indicizzato. Il tipo di dati viene utilizzato per la colonna nella tabella dati dell'indice XML selettivo che corrisponde al nodo indicizzato.  
  
 Se l'esecuzione del cast di un valore esistente al tipo di dati specificato ha esito negativo, l'operazione di inserimento (nell'indice) ha esito positivo, ma viene inserito un valore Null nella tabella dati dell'indice.  
  
### <a name="maxlength-optimization-hint"></a>Hint di ottimizzazione MAXLENGTH  
 Si applica a: Tipi di dati XQuery  
  
 L'hint di ottimizzazione MAXLENGTH consente di limitare la lunghezza dei dati di tipo xs:string. MAXLENGTH non è rilevante per i tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , poiché la lunghezza viene specificata contestualmente ai tipi di dati VARCHAR o NVARCHAR.  
  
 Se una stringa esistente è maggiore del valore MAXLENGTH specificato, l'inserimento di tale valore nell'indice ha esito negativo.  
  
  
##  <a name="sample-xml-document-for-examples"></a><a name="sample"></a> Documento XML di esempio  
 Negli esempi in questo argomento viene fatto riferimento al documento XML di esempio indicato di seguito:  
  
```xml  
<a>  
    <b>  
         <c atc="aa">10</c>  
         <c atc="bb">15</c>  
         <d atd1="dd" atd2="ddd">md </d>  
    </b>  
     <b>  
        <c></c>  
        <c atc="">117</c>  
     </b>  
</a>  
```  
  
  
## <a name="see-also"></a>Vedere anche  
 [Indici XML selettivi &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Creare, modificare o eliminare indici XML selettivi](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)  
  
  
