---
title: Promuovere i valori XML frequenti tramite colonne calcolate | Microsoft Docs
description: Informazioni su come alzare di livello i valori XML di uso frequente creando colonne calcolate per ottenere query più efficienti.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- promoting properties [XML in SQL Server]
- property promotion [XML in SQL Server]
ms.assetid: f5111896-c2fd-4209-b500-f2baa45489ad
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 15bbb0bc454c8069bdb77750b45de6db2260b280
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487782"
---
# <a name="promote-frequently-used-xml-values-with-computed-columns"></a>Promuovere i valori XML di uso frequente mediante colonne calcolate
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Se le query vengono eseguite principalmente su un piccolo numero di valori di attributi e di elementi, sarà possibile promuovere tali quantità al livello di colonne relazionali. Ciò risulta utile quando le query vengono eseguite su una piccola parte dei dati XML mentre viene recuperata l'intera istanza XML. Non è necessario creare un indice XML sulla colonna XML, ma è possibile indicizzare la colonna promossa. Le query devono essere scritte in modo da utilizzare la colonna promossa, poiché il Query Optimizer non reindirizza alla colonna promossa le query eseguite sulla colonna XML.  
  
 La colonna promossa può essere una colonna calcolata nella stessa tabella oppure una colonna separata e gestita dall'utente in un'altra tabella. Ciò è sufficiente quando i valori singleton vengono promossi da ogni istanza XML. Per le proprietà multivalore, invece, è necessario creare una tabella separata per la proprietà, come illustrato nella sezione seguente.  
  
## <a name="computed-column-based-on-the-xml-data-type"></a>Colonna calcolata basata sul tipo di dati xml  
 Per creare una colonna calcolata è possibile usare una funzione definita dall'utente che richiama metodi per il tipo di dati **xml** . Il tipo della colonna calcolata può essere qualsiasi tipo SQL, incluso il tipo XML, Questo è illustrato nell'esempio seguente.  
  
### <a name="example-computed-column-based-on-the-xml-data-type-method"></a>Esempio: Colonna calcolata basata su un metodo per il tipo di dati xml  
 Creare la funzione definita dall'utente per il codice ISBN di un libro:  
  
```  
CREATE FUNCTION udf_get_book_ISBN (@xData xml)  
RETURNS varchar(20)  
BEGIN  
   DECLARE @ISBN   varchar(20)  
   SELECT @ISBN = @xData.value('/book[1]/@ISBN', 'varchar(20)')  
   RETURN @ISBN   
END  
```  
  
 Aggiungere alla tabella una colonna calcolata per il codice ISBN:  
  
```  
ALTER TABLE      T  
ADD   ISBN AS dbo.udf_get_book_ISBN(xCol)  
```  
  
 La colonna calcolata può essere indicizzata come di consueto.  
  
### <a name="example-queries-on-a-computed-column-based-on-xml-data-type-methods"></a>Esempio: Query su una colonna calcolata basata sui metodi per il tipo di dati XML  
 Per ottenere l'elemento <`book`> il cui ISBN è 0-7356-1588-2:  
  
```  
SELECT xCol  
FROM   T  
WHERE  xCol.exist('/book/@ISBN[. = "0-7356-1588-2"]') = 1  
```  
  
 La query sulla colonna XML può essere riscritta in modo da utilizzare la colonna calcolata, come illustrato di seguito:  
  
```  
SELECT xCol  
FROM   T  
WHERE  ISBN = '0-7356-1588-2'  
```  
  
 È possibile creare una funzione definita dall'utente per restituire il tipo di dati **xml** e una colonna calcolata, ma non è possibile creare un indice XML sulla colonna XML calcolata.  
  
## <a name="creating-property-tables"></a>Creazione di tabelle di proprietà  
 È possibile promuovere alcune delle proprietà multivalore dei dati XML fino a ottenere una o più tabelle, creare indici su tali tabelle e modificare la destinazione delle query in modo da utilizzarle. Un tipico scenario è quello in cui la maggior parte del carico di lavoro delle query è costituita da un piccolo numero di proprietà. È possibile eseguire le operazioni seguenti:  
  
-   Creare una o più tabelle in cui inserire le proprietà multivalore. Risulta conveniente archiviare una proprietà per tabella e duplicare la chiave primaria della tabella di base nelle tabelle di proprietà, per poter eseguire il join all'indietro alla tabella di base.  
  
-   Se si desidera mantenere l'ordine relativo delle proprietà, sarà necessario introdurre una colonna separata per l'ordine relativo.  
  
-   Creare trigger sulla colonna XML per eseguire operazioni di manutenzione delle tabelle di proprietà. Nell'ambito dei trigger, eseguire una delle operazioni seguenti:  
  
    -   Usare i metodi per il tipo di dati **xml** , ad esempio **nodes()** e **value()** , per inserire ed eliminare righe nelle tabelle di proprietà.  
  
    -   Creare funzioni di flusso con valori di tabella in Common Language Runtime (CLR) per inserire ed eliminare righe nelle tabelle di proprietà.  
  
    -   Scrivere query per l'accesso SQL alle tabelle di proprietà e per l'accesso XML alla colonna XML nella tabella di base, utilizzandone la chiave primaria per creare join tra le tabelle.  
  
### <a name="example-create-a-property-table"></a>Esempio: Creare una tabella di proprietà  
 Si supponga ad esempio di voler promuovere i nomi degli autori. Poiché un libro può avere più autori, quel nome è una proprietà multivalore. Ogni nome è archiviato in una riga separata di una tabella di proprietà. La chiave primaria della tabella di base viene duplicata nella tabella di proprietà per consentire il join all'indietro.  
  
```  
create table tblPropAuthor (propPK int, propAuthor varchar(max))  
```  
  
### <a name="example-create-a-user-defined-function-to-generate-a-rowset-from-an-xml-instance"></a>Esempio: Creare una funzione definita dall'utente per la generazione di un set di righe da un'istanza XML  
 La seguente funzione con valori di tabella, udf_XML2Table, accetta un valore di chiave primaria e un'istanza XML. Recupera il nome di tutti gli autori degli elementi <`book`> e restituisce un set di righe composto da coppie di chiave primaria e nome.  
  
```  
create function udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (propPK int, propAuthor varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
      select @pk, nref.value('.', 'varchar(max)')  
      from   @xCol.nodes('/book/author/first-name') R(nref)  
      return  
end  
```  
  
### <a name="example-create-triggers-to-populate-a-property-table"></a>Esempio: Creare trigger per il popolamento di una tabella di proprietà  
 Il trigger di inserimento consente di inserire righe nella tabella di proprietà:  
  
```  
create trigger trg_docs_INS on T for insert  
as  
      declare @wantedXML xml  
      declare @FK int  
      select @wantedXML = xCol from inserted  
      select @FK = PK from inserted  
  
   insert into tblPropAuthor  
   select * from dbo.udf_XML2Table(@FK, @wantedXML)  
```  
  
 Il trigger di eliminazione consente di eliminare righe dalla tabella di proprietà, in base al valore della chiave primaria delle righe eliminate:  
  
```  
create trigger trg_docs_DEL on T for delete  
as  
   declare @FK int  
   select @FK = PK from deleted  
   delete tblPropAuthor where propPK = @FK  
```  
  
 Il trigger di aggiornamento consente di eliminare dalla tabella di proprietà le righe esistenti corrispondenti all'istanza XML aggiornata e di inserire nuove righe nella tabella stessa:  
  
```  
create trigger trg_docs_UPD  
on T  
for update  
as  
if update(xCol) or update(pk)  
begin  
      declare @FK int  
      declare @wantedXML xml  
      select @FK = PK from deleted  
      delete tblPropAuthor where propPK = @FK  
  
   select @wantedXML = xCol from inserted  
   select @FK = pk from inserted  
  
   insert into tblPropAuthor   
      select * from dbo.udf_XML2Table(@FK, @wantedXML)  
end  
```  
  
### <a name="example-find-xml-instances-whose-authors-have-the-same-first-name"></a>Esempio: Cercare istanze XML i cui autori hanno lo stesso nome  
 È possibile creare la query nella colonna XML oppure è possibile ricercare il nome "David" nella tabella di proprietà ed eseguire un join all'indietro alla tabella di base, per restituire l'istanza XML. Ad esempio:  
  
```  
SELECT xCol   
FROM     T JOIN tblPropAuthor ON T.pk = tblPropAuthor.propPK  
WHERE    tblPropAuthor.propAuthor = 'David'  
```  
  
### <a name="example-solution-using-the-clr-streaming-table-valued-function"></a>Esempio: Soluzione che usa una funzione di flusso CLR con valori di tabella  
 Per creare questa soluzione è necessario eseguire i passaggi seguenti:  
  
1.  Definire una classe CLR, SqlReaderBase, che implementa ISqlReader e genera un output di flusso valutato a livello di tabella, applicando un'espressione di percorso a un'istanza XML.  
  
2.  Creare un assembly e una funzione Transact-SQL definita dall'utente per avviare la classe CLR.  
  
3.  Definire i trigger di inserimento, aggiornamento ed eliminazione utilizzando la funzione definita dall'utente per la manutenzione delle tabelle di proprietà.  

 A tale scopo è innanzitutto necessario creare la funzione CLR di flusso. Il tipo di dati **xml** viene esposto come classe SqlXml gestita in ADO.NET e supporta il metodo **CreateReader()** , che restituisce un oggetto XmlReader.  
  
> [!NOTE]  
>  Il codice di esempio in questa sezione utilizza XPathDocument e XPathNavigator, che impongono il caricamento in memoria di tutti i documenti XML. Se nella propria applicazione si utilizza codice analogo per elaborare numerosi documenti XML di grandi dimensioni, sarà necessario ricordare che tale codice non è scalabile. Se possibile, è preferibile utilizzare allocazioni di memoria di piccole dimensioni e utilizzare interfacce di flusso. Per altre informazioni sulle prestazioni, vedere [Architettura dell'integrazione con CLR](../clr-integration/clr-integration-architecture-clr-hosted-environment.md).  
  
```  
public class c_streaming_xml_tvf {  
   public static ISqlReader streaming_xml_tvf   
(SqlXml xmlDoc, string pathExpression) {  
      return (new TestSqlReaderBase (xmlDoc, pathExpression));  
   }  
}  
  
// Class that implements ISqlReader  
public class TestSqlReaderBase : ISqlReader {  
XPathNodeIterator m_iterator;           
   public SqlChars FirstName;  
// Metadata for current resultset  
private SqlMetaData[] m_rgSqlMetaData;        
  
   public TestSqlReaderBase (SqlXml xmlDoc, string pathExpression) {     
      // Variables for XPath navigation  
      XPathDocument xDoc;  
      XPathNavigator xNav;  
      XPathExpression xPath;  
  
      // Set sql metadata  
      m_rgSqlMetaData = new SqlMetaData[1];  
      m_rgSqlMetaData[0] = new SqlMetaData ("FirstName",    
SqlDbType.NVarChar,50);     
  
      //Set up the Navigator  
      if (!xmlDoc.IsNull)  
          xDoc = new XPathDocument (xmlDoc.CreateReader());  
      else  
          xDoc = new XPathDocument ();  
      xNav = xDoc.CreateNavigator();  
      xPath = xNav.Compile (pathExpression);  
      m_iterator = xNav.Select(xPath);  
   }  
   public bool Read() {  
      bool moreRows = true;  
      if (moreRows = m_iterator.MoveNext())  
         FirstName = new SqlChars (m_iterator.Current.Value);  
      return moreRows;  
   }  
}  
```  
  
 Creare quindi un assembly e una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] definita dall'utente, SQL_streaming_xml_tvf (non illustrata), che corrisponde alla funzione CLR streaming_xml_tvf. La funzione definita dall'utente viene utilizzata per definire la funzione con valori di tabella, CLR_udf_XML2Table, per la generazione del set di righe:  
  
```  
create function CLR_udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (FK int, FirstName varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
   select @pk, FirstName   
   FROM   SQL_streaming_xml_tvf (@xCol, '/book/author/first-name')  
      return  
end  
```  
  
 Definire infine i trigger come illustrato nell'esempio "Creazione di trigger per il popolamento di una tabella di proprietà", sostituendo tuttavia la funzione udf_XML2Table con la funzione CLR_udf_XML2Table. Il trigger di inserimento è illustrato nell'esempio seguente:  
  
```  
create trigger CLR_trg_docs_INS on T for insert  
as  
   declare @wantedXML xml  
   declare @FK int  
   select @wantedXML = xCol from inserted  
   select @FK = PK from inserted  
  
   insert into tblPropAuthor  
      select *  
   from    dbo.CLR_udf_XML2Table(@FK, @wantedXML)  
```  
  
 Il trigger di eliminazione è identico alla versione non CLR, mentre nel trigger di inserimento viene semplicemente sostituita la funzione udf_XML2Table() con la funzione CLR_udf_XML2Table().  
  
## <a name="see-also"></a>Vedere anche  
 [Usare codice XML nelle colonne calcolate](../../relational-databases/xml/use-xml-in-computed-columns.md)  
  
