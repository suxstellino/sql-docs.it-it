---
title: OPENXML (SQL Server) | Microsoft Docs
description: Informazioni sull'istruzione OPENXML in SQL Server che offre una visualizzazione del set di righe della rappresentazione interna di un documento XML.
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: jroth
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ColPattern [XML in SQL Server]
- OPENXML statement, about OPENXML statement
- writing XML, OPENXML statement
- OPENXML statement, querying XML
- attribute-centric mapping
- SELECT statement [SQL Server], OPENXML keyword
- column patterns [XML in SQL Server]
- row patterns [XML in SQL Server]
- rowpattern [XML in SQL Server]
- queries [XML in SQL Server], OPENXML statement
- XML [SQL Server], OPENXML statement
- element-centric mapping [SQL Server]
ms.assetid: 060126fc-ed0f-478f-830a-08e418d410dc
author: rothja
ms.author: jroth
ms.openlocfilehash: e7509d21d05ac25b0988e59b95677d2046b20eb2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487788"
---
# <a name="openxml-sql-server"></a>OPENXML (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  La parola chiave [!INCLUDE[tsql](../../includes/tsql-md.md)] OPENXML restituisce un set di righe simile a una tabella o una vista da documenti XML in memoria. OPENXML consente di accedere ai dati XML come se si trattasse di un set di righe relazionali, visualizzando la rappresentazione interna di un documento XML come un set di righe. I record del set di righe possono essere archiviati in tabelle di database.  
  
 È possibile usare OPENXML nelle istruzioni SELECT e SELECT INTO ogni volta che i provider di set di righe, una vista o OPENROWSET possono comparire come origine. Per informazioni sulla sintassi di OPENXML, vedere [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 Per creare query da eseguire su un documento XML tramite OPENXML, è prima necessario chiamare **sp_xml_preparedocument**, che analizza il documento XML e restituisce un handle al documento analizzato e pronto per l'utilizzo. Il documento analizzato è una rappresentazione dell'albero del modello a oggetti documento (DOM, Document Object Model) dei vari nodi inclusi nel documento XML. L'handle del documento viene quindi passato a OPENXML, che a sua volta visualizza il documento come un set di righe in base ai parametri passati.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** usa una versione aggiornata a SQL del parser MSXML, Msxmlsql.dll. Questa versione è stata progettata per supportare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e garantire la compatibilità con MSXML versione 2.6.  
  
 La rappresentazione interna di un documento XML deve essere rimossa dalla memoria tramite una chiamata alla stored procedure di sistema **sp_xml_removedocument** , in modo tale da liberare memoria.  
  
 Nella figura seguente è illustrato questo processo.  
  
 ![Analisi XML con OPENXML](../../relational-databases/xml/media/xmlsp.gif "Analisi XML con OPENXML")  
  
 Si noti che per comprendere l'utilizzo di OPENXML, è necessario avere familiarità con le query XPath e XML. Per altre informazioni sul supporto di XPath in SQL Server, vedere [Utilizzo di query XPath in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md).  
  
> [!NOTE]  
>  OpenXML consente di parametrizzare i modelli XPath di riga e colonna come le variabili. La parametrizzazione può comportare attacchi intrusivi nelle espressioni XPath se viene esposta dal programmatore a utenti esterni, ad esempio se i parametri sono forniti tramite una stored procedure chiamata dall'esterno. Per evitare questi problemi potenziali di sicurezza, è consigliabile non esporre mai i parametri XPath a chiamanti esterni.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene illustrato l'utilizzo di `OPENXML` in un'istruzione `INSERT` e in un'istruzione `SELECT` . Il documento XML di esempio contiene gli elementi `<Customers>` e `<Orders>` .  
  
 La stored procedure `sp_xml_preparedocument` analizza innanzitutto il documento XML. Il documento analizzato corrisponde a una rappresentazione ad albero dei vari nodi (elementi, attributi, testo, commenti e così via) del documento XML. `OPENXML` si riferisce quindi al documento XML analizzato e fornisce una visualizzazione del set di righe di tutto o parte del documento. Un'istruzione `INSERT` che usa `OPENXML` può inserire i dati di tale set di righe in una tabella di database. È possibile usare diverse chiamate a `OPENXML` per ottenere una vista del set di righe di diverse parti del documento XML ed elaborarle, ad esempio inserendole in tabelle diverse. Tale processo è noto anche come "suddivisione del documento XML in tabelle".  
  
 Nell'esempio seguente un documento XML viene suddiviso in modo che gli elementi `<Customers>` vengano archiviati nella tabella `Customers` e gli elementi `<Orders>` vengano archiviati nella tabella `Orders` usando due istruzioni `INSERT` . Nell'esempio viene inoltre illustrata un'istruzione `SELECT` con la funzione `OPENXML` che recupera `CustomerID` e `OrderDate` dal documento XML. L'ultimo passaggio del processo consiste nel chiamare `sp_xml_removedocument` in modo da rilasciare la memoria allocata per includere la rappresentazione dell'albero XML interno creata durante la fase di analisi.  
  
```sql
-- Create tables for later population using OPENXML.  
CREATE TABLE Customers (CustomerID varchar(20) primary key,  
                ContactName varchar(20),   
                CompanyName varchar(20));  
GO  
CREATE TABLE Orders( CustomerID varchar(20), OrderDate datetime);
GO  
DECLARE @docHandle int;  
DECLARE @xmlDocument nvarchar(max); -- or xml type  
SET @xmlDocument = N'<ROOT>  
<Customers CustomerID="XYZAA" ContactName="Joe" CompanyName="Company1">  
<Orders CustomerID="XYZAA" OrderDate="2000-08-25T00:00:00"/>  
<Orders CustomerID="XYZAA" OrderDate="2000-10-03T00:00:00"/>  
</Customers>  
<Customers CustomerID="XYZBB" ContactName="Steve"  
CompanyName="Company2">No Orders yet!  
</Customers>  
</ROOT>';  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument;  
-- Use OPENXML to provide rowset consisting of customer data.  
INSERT Customers   
SELECT *   
FROM OPENXML(@docHandle, N'/ROOT/Customers')   
  WITH Customers;  
-- Use OPENXML to provide rowset consisting of order data.  
INSERT Orders   
SELECT *   
FROM OPENXML(@docHandle, N'//Orders')   
  WITH Orders;  
-- Using OPENXML in a SELECT statement.  
SELECT * FROM OPENXML(@docHandle, N'/ROOT/Customers/Orders')
  WITH (CustomerID nchar(5) '../@CustomerID', OrderDate datetime);
-- Remove the internal representation of the XML document.  
EXEC sp_xml_removedocument @docHandle;   
```  
  
 Nella figura seguente viene illustrato l'albero XML analizzato del documento XML precedente creato tramite sp_xml_preparedocument.  
  
 ![Albero XML analizzato](../../relational-databases/xml/media/xmlparsedtree.gif "Albero XML analizzato")  
  
## <a name="openxml-parameters"></a>Parametri di OPENXML  
 I parametri passati a OPENXML includono:  
  
-   L'handle di un documento XML (*idoc*)  
  
-   Un'espressione XPath che identifica i nodi di cui eseguire il mapping alle righe (*rowpattern*)  
  
-   Una descrizione del set di righe da generare.  
  
-   Il mapping tra le colonne del set di righe e i nodi XML.  
  
### <a name="xml-document-handle-idoc"></a>Handle del documento XML (idoc)  
 L'handle del documento viene restituito dalla stored procedure **sp_xml_preparedocument**  
  
### <a name="xpath-expression-to-identify-the-nodes-to-be-processed-rowpattern"></a>Espressione XPath che identifica i nodi da elaborare (rowpattern)  
 L'espressione XPath specificata come *rowpattern* identifica un set di nodi nel documento XML. Ogni nodo definito da *rowpattern* corrisponde a una riga specifica del set di righe generato da OPENXML.  
  
 I nodi definiti dall'espressione XPath possono essere qualsiasi nodo XML del documento XML. Se *rowpattern* identifica un set di elementi nel documento XML, a ogni nodo identificato corrisponderà una riga nel set di righe. Ad esempio, se *rowpattern* restituisce un attributo, verrà creata una riga per ogni nodo attributo selezionato da *rowpattern*.  
  
### <a name="description-of-the-rowset-to-be-generated"></a>Descrizione del set di righe da generare  
 OPENXML usa uno schema di set di righe per generare il set di righe risultante. Per specificare uno schema di set di righe, è possibile usare le opzioni illustrate di seguito.  
  
#### <a name="using-the-edge-table-format"></a>Utilizzo del formato di tabella edge  
 Per specificare uno schema di set di righe è consigliabile usare il formato di tabella edge, evitando invece di usare la clausola WITH.  
  
 In questo caso, OPENXML restituisce un set di righe nel formato di tabella edge. Il nome di questo formato deriva dal fatto che su ogni margine dell'albero del documento XML analizzato viene eseguito il mapping a una riga del set di righe.  
  
 In una tabella edge viene rappresentata la struttura dettagliata di un documento XML, che include i nomi degli elementi e degli attributi, la gerarchia del documento, gli spazi dei nomi e le istruzioni per l'elaborazione. Il formato di tabella edge consente di ottenere informazioni aggiuntive non esposte tramite le metaproprietà. Per altre informazioni sulle metaproprietà, vedere [Specify Metaproperties in OPENXML](../../relational-databases/xml/specify-metaproperties-in-openxml.md).  
  
 Le informazioni aggiuntive disponibili in una tabella edge consentono di archiviare e di eseguire query sul tipo di dati di un elemento o di un attributo e sul tipo di nodo, di archiviare ed eseguire query sulle informazioni relative alla struttura del documento XML, nonché di compilare un sistema di gestione dei documenti XML personalizzato.  
  
 Tramite una tabella edge è possibile scrivere stored procedure che accettano i documenti XML come input BLOB (Binary Large Object), generano la tabella edge e quindi estraggono e analizzano il documento al massimo livello di dettaglio (ricerca della gerarchia del documento, dei nomi di elementi e attributi, degli spazi dei nomi e delle istruzioni per l'elaborazione).  
  
 La tabella edge può inoltre fungere da formato di archiviazione per i documenti XML quando nessun altro tipo di mapping ad altri formati relazionali risulta appropriato e un campo ntext non restituisce informazioni strutturali sufficienti.  
  
 L'utilizzo di una tabella edge per analizzare un documento XML equivale a usare un parser XML per ottenere le stesse informazioni.  
  
 Nella tabella seguente viene descritta la struttura della tabella edge.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**bigint**|ID univoco del nodo del documento.<br /><br /> Il valore dell'ID dell'elemento radice è 0. I valori di ID negativi sono riservati.|  
|**parentid**|**bigint**|Identifica il padre del nodo. Il padre identificato da questo ID non è necessariamente l'elemento padre, ma dipende dalla proprietà NodeType del nodo il cui padre è identificato da questo ID. Se ad esempio il nodo è di tipo testo, il relativo elemento padre potrebbe essere un nodo attributo.<br /><br /> Se il nodo si trova al livello principale nel documento XML, il relativo valore **ParentID** è NULL.|  
|**node type**|**int**|Identifica il tipo di nodo ed è un intero che corrisponde alla numerazione del tipo di nodo DOM XML.<br /><br /> Di seguito sono riportati i valori che indicano il tipo di nodo visualizzabili nella colonna:<br /><br /> **1** = Nodo elemento<br /><br /> **2** = Nodo attributo<br /><br /> **3** = Nodo di testo<br /><br /> **4** = Nodo sezione CDATA<br /><br /> **5** = Nodo riferimento a entità<br /><br /> **6** = Nodo entità<br /><br /> **7** = Nodo istruzione di elaborazione<br /><br /> **8** = Nodo di commento<br /><br /> **9** = Nodo di documento<br /><br /> **10** = Nodo tipo di documento<br /><br /> **11** = Nodo frammento di documento<br /><br /> **12** = Nodo di notazione<br /><br /> Per altre informazioni, vedere l'argomento relativo alla proprietà nodeType nella documentazione di Microsoft XML (MSXML) SDK.|  
|**localname**|**nvarchar(max)**|Nome locale dell'elemento o attributo. È NULL se l'oggetto DOM non è associato a un nome.|  
|**prefix**|**nvarchar(max)**|Prefisso dello spazio dei nomi del nome del nodo.|  
|**namespaceuri**|**nvarchar(max)**|URI dello spazio dei nomi del nodo. Se il valore è NULL, non sono presenti spazi dei nomi.|  
|**datatype**|**nvarchar(max)**|Rappresenta il tipo di dati effettivo della riga dell'elemento o dell'attributo e in caso contrario è NULL. Il tipo di dati viene ricavato dalla DTD o dallo schema inline.|  
|**prev**|**bigint**|ID XML dell'elemento di pari livello precedente. È NULL se non è presente alcun elemento diretto di pari livello precedente.|  
|**text**|**ntext**|Include il valore dell'attributo o il contenuto dell'elemento in formato testo oppure è NULL se la voce della tabella edge non richiede un valore.|  
||||

#### <a name="using-the-with-clause-to-specify-an-existing-table"></a>Utilizzo della clausola WITH per specificare una tabella esistente  
 È possibile usare la clausola WITH per specificare il nome di una tabella esistente. A tale scopo, specificare il nome della tabella il cui schema può essere usato da OPENXML per generare il set di righe.  
  
#### <a name="using-the-with-clause-to-specify-a-schema"></a>Utilizzo della clausola WITH per specificare uno schema  
 È possibile usare la clausola WITH per specificare uno schema completo. Quando si specifica lo schema del set di righe, si definiscono i nomi delle colonne e i rispettivi tipi di dati, nonché il mapping di tali colonne al documento XML.  
  
 È possibile specificare il modello di colonna usando il parametro ColPattern nell'argomento SchemaDeclaration. Il modello di colonna specificato consente di eseguire il mapping di una colonna del set di righe al nodo XML identificato da rowpattern, nonché di determinare il tipo di mapping.  
  
 Se per una colonna non viene specificato il parametro ColPattern, viene eseguito il mapping della colonna del set di righe al nodo XML con lo stesso nome, in base al mapping specificato dal parametro *flags* . Se viene impostato nell'ambito della specifica dello schema nella clausola WITH, il parametro ColPattern sarà tuttavia prioritario rispetto al mapping specificato dal parametro *flags* .  
  
### <a name="mapping-between-the-rowset-columns-and-the-xml-nodes"></a>Il mapping tra le colonne del set di righe e i nodi XML.  
 Nell'istruzione OPENXML è anche possibile specificare facoltativamente il tipo di mapping (incentrato sugli attributi o sugli elementi) tra le colonne del set di righe e i nodi XML identificati da *rowpattern*. Questa informazione viene usata per la trasformazione tra i nodi XML e le colonne del set di righe.  
  
 Per specificare il tipo di mapping è possibile procedere in due modi e inoltre usare entrambi i sistemi:  
  
-   Tramite il parametro *flags* .  
  
     Il mapping specificato dal parametro *flags* si basa sullo corrispondenza dei nomi, ovvero sui nodi XML viene eseguito il mapping alle colonne del set di righe corrispondenti con lo stesso nome.  
  
-   Tramite il parametro *ColPattern* .  
  
     Il parametro *ColPattern*, ovvero un'espressione XPath, viene specificato nell'ambito di *SchemaDeclaration* nella clausola WITH. Il mapping specificato dal parametro *ColPattern* sovrascrive il mapping specificato dal parametro *flags* .  
  
     Il parametro *ColPattern* consente di specificare il tipo di mapping, incentrato sugli attributi o sugli elementi, che sovrascrive o migliora il mapping predefinito indicato in *flags*.  
  
     Il parametro *ColPattern* viene specificato nei casi seguenti:  
  
    -   Se il nome di colonna nel set di righe è diverso dal nome dell'elemento o dell'attributo al quale viene eseguito il mapping. In questo caso, il parametro *ColPattern* consente di identificare il nome dell'elemento o dell'attributo XML al quale viene eseguito il mapping della colonna del set di righe.  
  
    -   Se si desidera eseguire il mapping di un attributo di una metaproprietà alla colonna. In questo caso, il parametro *ColPattern* consente di definire la metaproprietà alla quale viene eseguito il mapping della colonna del set di righe. Per altre informazioni su come usare le metaproprietà, vedere [Impostazione di metaproprietà in OPENXML](../../relational-databases/xml/specify-metaproperties-in-openxml.md).  
  
 I parametri *flags* e *ColPattern* sono entrambi facoltativi. Se non viene specificato un mapping, viene usato il mapping incentrato sugli attributi, ovvero il valore predefinito del parametro *flags* .  
  
#### <a name="attribute-centric-mapping"></a>mapping incentrato sugli attributi  
 Se il parametro *flags* dell'istruzione OPENXML viene impostato su 1 (XML_ATTRIBUTES), viene definito un mapping **incentrato sugli attributi** . Se il parametro *flags* include XML_ ATTRIBUTES, il set di righe esposto restituisce o usa le righe nelle quali ogni elemento XML è rappresentato come una riga. Sugli attributi XML viene eseguito il mapping agli attributi definiti in SchemaDeclaration o restituiti da Tablename nella clausola WITH, in base alla corrispondenza dei nomi. La corrispondenza dei nomi indica che gli attributi XML con un nome specifico vengono archiviati in una colonna del set di righe con lo stesso nome.  
  
 Se il nome della colonna è diverso dal nome dell'attributo a cui viene eseguito il mapping della colonna, è necessario specificare il parametro *ColPattern* .  
  
 Se l'attributo XML è associato a un qualificatore dello spazio dei nomi, anche il nome della colonna del set di righe deve essere associato a un qualificatore.  
  
#### <a name="element-centric-mapping"></a>Mapping incentrato sugli elementi  
 Se il parametro *flags* dell'istruzione OPENXML viene impostato su 2 (XML_ELEMENTS), viene definito un mapping **incentrato sugli elementi** . Pur essendo simile al mapping **incentrato sugli attributi** , questo tipo di mapping è caratterizzato dalle differenze seguenti:  
  
-   In base alla corrispondenza dei nomi del mapping, ad esempio una colonna su cui viene eseguito il mapping a un elemento XML avente lo stesso nome, verranno selezionati i sottoelementi non complessi a meno che non venga specificato un modello dei livelli delle colonne. Nella fase di recupero dei dati, se il sottoelemento è complesso perché include altri sottoelementi, la colonna verrà impostata su NULL e i valori degli attributi dei sottoelementi verranno ignorati.  
  
-   Nel caso di più sottoelementi con lo stesso nome, verrà restituito il primo nodo.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_xml_preparedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)   
 [sp_xml_removedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)   
 [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
