---
title: Creare istanze di dati XML | Microsoft Docs
description: Informazioni su come creare istanze di dati XML usando il caricamento bulk, le assegnazioni di costanti, l'istruzione SELECT e la clausola FOR XML o con il cast dei tipi delle istanze di dati d tipo stringa.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- type casting string instances [XML in SQL Server]
- XML [SQL Server], typed
- xml data type [SQL Server], generating instances
- casting string instances [XML in SQL Server]
- typed XML
- generating XML instances [SQL Server]
- XML [SQL Server], generating instances
- white space [XML in SQL Server]
ms.assetid: dbd6c06f-db6e-44a7-855a-6a55bf374907
author: rothja
ms.author: jroth
ms.openlocfilehash: a0078fe1981b30db78e60ba56ba6fb760c8b7f82
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488897"
---
# <a name="create-instances-of-xml-data"></a>Creare istanze di dati XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come generare istanze XML.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]è possibile generare istanze XML tramite i metodi seguenti:  
  
-   Cast dei tipi delle istanze di dati di tipo stringa.  
  
-   Utilizzo dell'istruzione SELECT con la clausola FOR XML.  
  
-   Utilizzo di assegnazioni di costanti.  
  
-   Utilizzo del caricamento bulk.  
  
## <a name="type-casting-string-and-binary-instances"></a>Cast di tipo di istanze binarie e di stringa  
 È possibile analizzare qualsiasi tipo di dati stringa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio [**n**][**var**]**char**, **[n]text**, **varbinary** e **image**, nel tipo di dati **xml** eseguendo il cast o la conversione della stringa (rispettivamente tramite l'istruzione CAST o CONVERT) nel tipo di dati **xml** . L'istanza XML non tipizzata viene controllata per verificare che il formato sia corretto. Se al tipo **xml** è associato uno schema, viene eseguita anche la convalida. Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 I documenti XML possono essere codificati con codifiche diverse, ad esempio UTF-8, UTF-16, windows-1252). Di seguito vengono descritte le regole in base alle quali i tipi di origine di stringa e binari interagiscono con la codifica del documento XML, nonché il comportamento del parser.  
  
 Dato che **nvarchar** presuppone una codifica unicode a 2 byte come UTF-16 o UCS-2, il parser XML tratterà il valore stringa come un documento o frammento XML codificato Unicode a 2 byte. Questo significa che anche il documento XML dovrà essere codificato tramite codifica Unicode a 2 byte per essere compatibile con il tipo di dati di origine. Un documento XML con codifica UTF-16 potrà, ma non dovrà obbligatoriamente, includere un indicatore dell'ordine dei byte (BOM) UTF-16, in quanto il contesto del tipo di origine chiarisce che può trattarsi solo di un documento con codifica Unicode a 2 byte.  
  
 Il contenuto di una stringa **varchar** viene trattato come documento/frammento XML con codifica a 1 byte da parte del parser XML. Dato che alla stringa di origine **varchar** è associata una tabella codici, il parser la utilizzerà per la codifica se non nel documento XML non viene specificata alcuna codifica. Se un'istanza XML include un BOM o una dichiarazione di codifica, quest'ultima deve essere consistente con la tabella codici, altrimenti il parser genererà un errore.  
  
 Il contenuto di **varbinary** viene trattato come flusso di punti di codice passato direttamente al parser XML. Di conseguenza, il documento o frammento XML deve specificare il BOM o altra informazione sulla codifica inline. Il parser esaminerà solo il flusso per determinare la codifica. Questo significa che il documento XML con codifica UTF-16 deve specificare il BOM UTF-16 e che un'istanza senza BOM e senza dichiarazione di codifica verrà interpretata come UTF-8.  
  
 Se la codifica del documento XML non è già nota e i dati vengono passati dati di tipo stringa o binari anziché come dati XML prima che venga eseguito il cast sull'XML, è consigliabile trattare i dati come **varbinary**. Ad esempio, nella lettura dei dati da un file XML utilizzando OpenRowset(), specificare i dati da leggere come valore **varbinary(max)** :  
  
```  
select CAST(x as XML)   
from OpenRowset(BULK 'filename.xml', SINGLE_BLOB) R(x)  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre internamente una rappresentazione binaria efficiente dell'istanza XML, basata sulla codifica UTF-16. La codifica specificata dall'utente non viene mantenuta, ma viene considerata durante il processo di analisi.  
  
### <a name="type-casting-clr-user-defined-types"></a>Cast di tipo di tipi CLR definiti dall'utente  
 Se un tipo CLR definito dall'utente include una serializzazione XML, per le istanze di tale tipo è possibile eseguire il cast in modo esplicito su un tipo di dati XML. Per altri dettagli sulla serializzazione XML di un tipo CLR definito dall'utente, vedere [Serializzazione XML da oggetti di database CLR](/dotnet/standard/serialization/introducing-xml-serialization).  
  
### <a name="white-space-handling-in-typed-xml"></a>Gestione degli spazi vuoti nell'istanza XML tipizzata  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], gli spazi vuoti all'interno del contenuto degli elementi vengono considerati non significativi se ricorrono in una sequenza di dati di tipo carattere composti solo da spazi vuoti delimitati da markup, ad esempio tag di inizio e di fine, e non vengono sostituiti con entità. Le sezioni CDATA vengono ignorate. Questa modalità di gestione degli spazi vuoti è diversa da quella descritta nella specifica XML 1.0 pubblicata dal World Wide Web Consortium (W3C). Tale differenza è dovuta al fatto che il parser XML in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riconosce solo un numero limitato di subset DTD, come definito nella specifica XML 1.0. Per altre informazioni sul numero limitato di subset DTD supportati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 Per impostazione predefinita, il parser XML elimina gli spazi vuoti durante la conversione di dati di tipo stringa nel formato XML in uno dei casi seguenti:  
  
-   L'attributo `xml:space` non viene definito per un elemento o per gli elementi predecessori.  
  
-   Viene assegnato il valore predefinito all'attributo `xml:space` attivo per un elemento o i relativi predecessori.  
  
 Ad esempio:  
  
```  
declare @x xml  
set @x = '<root>      <child/>     </root>'  
select @x   
```  
  
 Risultato:  
  
```  
<root><child/></root>  
```  
  
 È tuttavia possibile modificare tale impostazione. Per mantenere gli spazi vuoti in un'istanza XML DT, è possibile utilizzare l'operatore CONVERT e il parametro facoltativo *style* corrispondente impostato sul valore 1. Ad esempio:  
  
```  
SELECT CONVERT(xml, N'<root>      <child/>     </root>', 1)  
```  
  
 Se il parametro *style* non viene utilizzato o è impostato sul valore 0, gli spazi vuoti non significativi non vengono mantenuti per la conversione dell'istanza XML DT. Per altre informazioni sull'uso dell'operatore CONVERT e del parametro *style* durante la conversione di dati di tipo stringa in istanze XML DT, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
### <a name="example-cast-a-string-value-to-typed-xml-and-assign-it-to-a-column"></a>Esempio: Eseguire il cast di un valore stringa al tipo XML tipizzato e assegnarlo a una colonna  
 Nell'esempio seguente viene eseguito il cast di una variabile stringa contenente un frammento XML al tipo di dati **xml** e quindi viene archiviata tale variabile nella colonna di tipo **xml** :  
  
```  
CREATE TABLE T(c1 int primary key, c2 xml)  
go  
DECLARE  @s varchar(100)  
SET @s = '<Cust><Fname>Andrew</Fname><Lname>Fuller</Lname></Cust>'   
```  
  
 L'operazione di inserimento seguente converte in modo implicito una stringa nel tipo di dati **xml** :  
  
```  
INSERT INTO T VALUES (3, @s)   
```  
  
 È possibile utilizzare cast() per eseguire il cast esplicito della stringa al tipo **xml** :  
  
```  
INSERT INTO T VALUES (3, cast (@s as xml))  
```  
  
 In alternativa, è possibile utilizzare convert(), come illustrato nell'esempio seguente:  
  
```  
INSERT INTO T VALUES (3, convert (xml, @s))   
```  
  
### <a name="example-convert-a-string-to-typed-xml-and-assign-it-to-a-variable"></a>Esempio: Convertire una stringa nel tipo XML tipizzato e assegnarla a una variabile  
 Nell'esempio seguente, una stringa viene convertita nel tipo **xml** e assegnata a una variabile con tipo di dati **xml** :  
  
```  
declare @x xml  
declare  @s varchar(100)  
SET @s = '<Cust><Fname>Andrew</Fname><Lname>Fuller</Lname></Cust>'   
set @x =convert (xml, @s)  
select @x  
```  
  
## <a name="using-the-select-statement-with-a-for-xml-clause"></a>Utilizzo dell'istruzione SELECT con la clausola FOR XML  
 È possibile utilizzare la clausola FOR XML in un'istruzione SELECT per restituire i risultati in formato XML. Ad esempio:  
  
```  
DECLARE @xmlDoc xml  
SET @xmlDoc = (SELECT Column1, Column2  
               FROM   Table1, Table2  
               WHERE   Some condition  
               FOR XML AUTO)  
 ...  
```  
  
 L'istruzione SELECT restituisce un frammento di testo XML che viene quindi analizzato durante l'assegnazione alla variabile con tipo di dati **xml** .  
  
 Nella clausola FOR XML, è inoltre possibile utilizzare la [direttiva TYPE](../../relational-databases/xml/type-directive-in-for-xml-queries.md) che restituisce direttamente il risultato di una query FOR XML come tipo **xml** :  
  
```  
Declare @xmlDoc xml  
SET @xmlDoc = (SELECT ProductModelID, Name  
               FROM   Production.ProductModel  
               WHERE  ProductModelID=19  
               FOR XML AUTO, TYPE)  
SELECT @xmlDoc  
```  
  
 Risultato:  
  
```  
<Production.ProductModel ProductModelID="19" Name="Mountain-100" />...  
```  
  
 Nell'esempio seguente, il risultato **xml** tipizzato di una query FOR XML viene inserito in una colonna di tipo **xml**:  
  
```  
CREATE TABLE T1 (c1 int, c2 xml)  
go  
INSERT T1(c1, c2)  
SELECT 1, (SELECT ProductModelID, Name  
           FROM Production.ProductModel  
           WHERE ProductModelID=19  
           FOR XML AUTO, TYPE)  
SELECT * FROM T1  
go  
```  
  
 Per altre informazioni su FOR XML, vedere [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce al client istanze con tipo di dati **xml** come risultato di diversi costrutti server, quali query FOR XML che utilizzano la direttiva TYPE, o nel caso in cui il tipo di dati **xml** venga utilizzato per restituire il codice XML da colonne SQL, variabili o parametri di output. Nel codice dell'applicazione client il provider ADO.NET richiede che queste informazioni con tipo di dati **xml** siano inviate dal server utilizzando una codifica binaria. Se tuttavia si utilizza FOR XML senza la direttiva TYPE, i dati XML vengono restituiti come tipo string. In tutti i casi, il provider client sarà sempre in grado di gestire entrambe i formati di XML.  
  
## <a name="using-constant-assignments"></a>Utilizzo di assegnazioni di costanti  
 È possibile utilizzare una costante stringa nei casi in cui è prevista un'istanza con tipo di dati **xml** . Tale operazione è equivalente a un CAST implicito della stringa al formato XML. Ad esempio:  
  
```  
DECLARE @xmlDoc xml  
SET @xmlDoc = '<Cust><Fname>Andrew</Fname><Lname>Fuller</Lname></Cust>'   
-- Or  
SET @xmlDoc = N'<?xml version="1.0" encoding="ucs-2"?><doc/>'  
```  
  
 Nell'esempio precedente, la stringa viene convertita in modo implicito nel tipo di dati **xml** e assegnata a una variabile di tipo **xml** .  
  
 Nell'esempio seguente viene inserita una stringa costante in una colonna di tipo **xml** :  
  
```  
CREATE TABLE T(c1 int primary key, c2 xml)  
INSERT INTO T VALUES (3, '<Cust><Fname>Andrew</Fname><Lname>Fuller</Lname></Cust>')   
```  
  
> [!NOTE]  
>  L'istanza XML tipizzata viene convalidata in base allo schema specificato. Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
## <a name="using-bulk-load"></a>Utilizzo del caricamento bulk  
 La funzionalità avanzata [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md) consente di eseguire il caricamento bulk dei documenti XML nel database. È possibile eseguire un caricamento bulk delle istanze XML dai file nelle colonne di tipo **xml** del database. Per esempi funzionanti, vedere [Esempi di importazione ed esportazione bulk di documenti XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md). Per altre informazioni sul caricamento dei documenti XML, vedere [Caricare dati XML](../../relational-databases/xml/load-xml-data.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Recuperare ed eseguire query su dati XML](../../relational-databases/xml/retrieve-and-query-xml-data.md)|Descrizione delle parti di istanze XML che non sono mantenute quando vengono archiviate nei database.|  
  
## <a name="see-also"></a>Vedere anche  
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
