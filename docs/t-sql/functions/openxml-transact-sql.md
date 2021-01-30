---
description: OPENXML (Transact-SQL)
title: OPENXML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENXML_TSQL
- OPENXML
dev_langs:
- TSQL
helpviewer_keywords:
- OPENXML statement
- rowsets [SQL Server], XML documents
- XML [SQL Server], rowset views
ms.assetid: 8088b114-7d01-435a-8e0d-b81abacc86d6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6a37cdab9cc3adda4043cd7e849a4d743eb9779f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183984"
---
# <a name="openxml-transact-sql"></a>OPENXML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  OpenXML fornisce una vista di un documento XML basata su un set di righe. In quanto provider di set di righe, è possibile utilizzare OPENXML nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che supportano provider di set di righe quali una tabella, una vista o la funzione OPENROWSET.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OPENXML( idoc int [ in] , rowpattern nvarchar [ in ] , [ flags byte [ in ] ] )   
[ WITH ( SchemaDeclaration | TableName ) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *idoc*  
 Handle di documento della rappresentazione interna di un documento XML. Tale rappresentazione viene creata tramite **sp_xml_preparedocument**.  
  
 *rowpattern*  
 Modello XPath usato per identificare i nodi da elaborare come righe. I nodi provengono dal documento XML di cui viene passato l'handle nel parametro *idoc*.
  
 *flags*  
 Indica il mapping da usare tra i dati XML e il set di righe relazionale e la modalità di riempimento della colonna di overflow. *flags* è un parametro di input facoltativo. I possibili valori sono i seguenti.  
  
|Valore byte|Descrizione|  
|----------------|-----------------|  
|**0**|Per impostazione predefinita viene impostato il mapping **incentrato sugli attributi**.|  
|**1**|Viene usato il mapping **incentrato sugli attributi**. Può essere utilizzato in combinazione con XML_ELEMENTS. In questo caso, il mapping **incentrato sugli attributi** viene applicato per primo. Successivamente il mapping **incentrato sugli elementi** viene applicato a tutte le colonne non ancora elaborate.|  
|**2**|Viene usato il mapping **incentrato sugli elementi**. Può essere utilizzato in combinazione con XML_ATTRIBUTES. In questo caso, il mapping **incentrato sugli attributi** viene applicato per primo. Successivamente il mapping **incentrato sugli elementi** viene applicato a tutte le colonne non ancora elaborate.|  
|**8**|Può essere utilizzato in combinazione (OR logico) con XML_ATTRIBUTES o XML_ELEMENTS. Nel contesto di operazioni di recupero, questo flag indica che i dati consumati non devono essere copiati nella proprietà di overflow **\@mp:xmltext**.|  
  
 _SchemaDeclaration_  
 È la definizione dello schema nel formato: _ColName_*ColType* [_ColPattern_ | _MetaProperty_] [**,**_ColNameColType_ [_ColPattern_ | _MetaProperty_]...]  
  
 _ColName_  
 Nome della colonna nel set di righe.  
  
 *ColType*  
 Tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della colonna nel set di righe. Se i tipi di colonna sono diversi dal tipo di dati **xml** sottostante dell'attributo, viene eseguita un'assegnazione forzata del tipo.  
  
 *ColPattern*  
 Modello generale XPath facoltativo che indica il criterio di mapping dei nodi XML alle colonne. Se *ColPattern* viene omesso, viene applicato il mapping predefinito, ovvero il mapping **incentrato sugli attributi** o **sugli elementi**, a seconda del valore dei *flag*.  
  
 Per il mapping *incentrato sugli attributi* o **sugli elementi**, il modello XPath specificato come **ColPattern** consente di specificare la natura speciale del mapping che sovrascrive o migliora il mapping predefinito indicato in *flag*.  
  
 Il modello XPath generale specificato come *ColPattern* supporta inoltre le metaproprietà.  
  
 *Metaproprietà*  
 Una delle metaproprietà definite da OPENXML. Se si specifica *MetaProperty*, la colonna contiene le informazioni definite dalla metaproprietà. Le metaproprietà consentono di estrarre informazioni, ad esempio sulla posizione relativa e sullo spazio dei nomi, relative ai nodi XML. Queste metaproprietà restituiscono una quantità di informazioni maggiore rispetto al quelle visibili nella rappresentazione testuale.  
  
 *TableName*  
 Nome di tabella che è possibile assegnare in sostituzione di *SchemaDeclaration* se una tabella avente lo schema desiderato esiste già e non sono necessari modelli di colonna specifici.  
  
## <a name="remarks"></a>Commenti  
 La clausola WITH restituisce un formato di set di righe (e informazioni aggiuntive relative al mapping, se necessario) tramite *SchemaDeclaration* o specificando un valore esistente per *TableName*. Se la clausola facoltativa WITH viene omessa, i risultati vengono restituiti nel formato di tabella **edge**. Le tabelle edge rappresentano la struttura specifica del documento XML, ad esempio nomi di elementi/attributi, gerarchia del documento, spazi dei nomi, istruzioni di elaborazione e così via, in un'unica tabella.  
  
 Nella tabella seguente viene descritta la struttura della tabella **edge**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**bigint**|ID univoco del nodo del documento.<br /><br /> Il valore dell'ID dell'elemento radice è 0. I valori di ID negativi sono riservati.|  
|**parentid**|**bigint**|Identifica il padre del nodo. Il padre identificato da questo ID non corrisponde necessariamente all'elemento padre. Dipende infatti dal valore NodeType del nodo il cui padre è identificato da questo ID. Se ad esempio il nodo è di tipo testo, il relativo elemento padre potrebbe essere un nodo attributo.<br /><br /> Se il nodo si trova al livello principale nel documento XML, il relativo valore **ParentID** è NULL.|  
|**nodetype**|**int**|Identifica il tipo di nodo. Valore intero corrispondente alla numerazione del tipo di nodo XML DOM.<br /><br /> I possibili tipi di nodo sono i seguenti:<br /><br /> 1 = Nodo elemento<br /><br /> 2 = Nodo attributo<br /><br /> 3 = Nodo testo|  
|**localname**|**nvarchar**|Nome locale dell'elemento o attributo. È NULL se l'oggetto DOM non ha un nome.|  
|**prefix**|**nvarchar**|Prefisso dello spazio dei nomi del nome del nodo.|  
|**namespaceuri**|**nvarchar**|URI dello spazio dei nomi del nodo. Se il valore è NULL, non sono presenti spazi dei nomi.|  
|**datatype**|**nvarchar**|Tipo di dati effettivo della riga dell'elemento o attributo. In caso contrario, restituisce NULL. Il tipo di dati viene ricavato dalla DTD o dallo schema inline.|  
|**prev**|**bigint**|ID XML dell'elemento di pari livello precedente. È NULL se non è presente alcun elemento diretto di pari livello precedente.|  
|**text**|**ntext**|Include il valore dell'attributo o il contenuto dell'elemento in formato testuale (oppure è NULL se la voce della tabella **edge** non richiede un valore).|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-simple-select-statement-with-openxml"></a>R. Utilizzo di un'istruzione SELECT semplice con OPENXML  
 Nell'esempio seguente viene creata una rappresentazione interna dell'immagine XML tramite `sp_xml_preparedocument`. Su tale rappresentazione viene quindi eseguita un'istruzione `SELECT` che utilizza un provider di set di righe `OPENXML`.  
  
 Il valore di *flag* viene impostato su `1`, a indicare il mapping **incentrato sugli attributi**. Sugli attributi XML viene pertanto eseguito il mapping alle colonne del set di righe. Il valore di *rowpattern* specificato come `/ROOT/Customer` identifica i nodi `<Customers>` da elaborare.  
  
 Il parametro facoltativo *ColPattern* (modello di colonna) viene omesso in quanto il nome di colonna corrisponde ai nomi di attributo XML.  
  
 Il provider di set di righe `OPENXML` crea un set di righe composto dalle due colonne `CustomerID` e `ContactName`, da cui vengono ricavate le colonne necessarie, in questo caso tutte le colonne, tramite l'istruzione `SELECT`.  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;  
-- Execute a SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customer',1)  
            WITH (CustomerID  VARCHAR(10),  
                  ContactName VARCHAR(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Se si esegue la stessa istruzione `SELECT` con i *flag* impostati su `2`, a indicare il mapping **incentrato sugli elementi**, per i valori di `CustomerID` e `ContactName` di entrambi i clienti nel documento XML viene restituito NULL, perché non è disponibile alcun elemento denominato `CustomerID` o `ContactName` nel documento XML.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName  
---------- -----------  
NULL       NULL  
NULL       NULL  
```  
  
### <a name="b-specifying-colpattern-for-mapping-between-columns-and-the-xml-attributes"></a>B. Utilizzo di ColPattern per l'impostazione del mapping tra colonne e attributi XML  
 La query seguente restituisce gli attributi ID cliente, data dell'ordine, ID prodotto e quantità dal documento XML. Il *rowpattern* identifica gli elementi `<OrderDetails>`. `ProductID` e `Quantity` sono attributi dell'elemento `<OrderDetails>`. Tuttavia, `OrderID`, `CustomerID` e `OrderDate` sono gli attributi dell'elemento padre (`<Orders>`).  
  
 Il parametro facoltativo *ColPattern* viene specificato per i mapping seguenti:  
  
-   Sulle colonne `OrderID`, `CustomerID` e `OrderDate` del set di righe viene eseguito il mapping agli attributi del padre dei nodi identificati da *rowpattern* nel documento XML.  
  
-   Sulla colonna `ProdID` del set di righe viene eseguito il mapping all'attributo `ProductID`, mentre sulla colonna `Qty` viene eseguito il mapping all'attributo `Quantity` dei nodi identificati in *rowpattern*.  
  
 Il mapping **incentrato sugli elementi** specificato nel parametro *flag* viene sovrascritto dal mapping specificato in *ColPattern*.  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">v  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';   
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT stmt using OPENXML rowset provider  
SELECT *  
FROM   OPENXML (@idoc, '/ROOT/Customer/Order/OrderDetail',2)   
         WITH (OrderID       int         '../@OrderID',
               CustomerID  varchar(10) '../@CustomerID',
               OrderDate   datetime    '../@OrderDate',
               ProdID      int         '@ProductID',
               Qty         int         '@Quantity');
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
OrderID CustomerID           OrderDate                 ProdID    Qty  
------------------------------------------------------------------------  
10248      VINET       1996-07-04 00:00:00.000   11      12  
10248      VINET       1996-07-04 00:00:00.000   42      10  
10283      LILAS       1996-08-16 00:00:00.000   72      3  
```  
  
### <a name="c-obtaining-results-in-an-edge-table-format"></a>C. Restituzione di risultati in formato di tabella edge  
 Il documento XML utilizzato nell'esempio seguente è composto dagli elementi `<Customers>`, `<Orders>` e `<Order_0020_Details>`. Viene innanzitutto richiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento, quindi l'handle di documento viene passato a `OPENXML`.  
  
 Nell'istruzione `OPENXML`*rowpattern* (`/ROOT/Customers`) identifica i nodi `<Customers>` da elaborare. Poiché la clausola WITH non è specificata, `OPENXML` restituisce il set di righe nel formato di tabella **edge**.  
  
 L'istruzione `SELECT` recupera infine tutte le colonne della tabella **edge**.  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customers CustomerID="VINET" ContactName="Paul Henriot">  
   <Orders CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <Order_x0020_Details OrderID="10248" ProductID="11" Quantity="12"/>  
      <Order_x0020_Details OrderID="10248" ProductID="42" Quantity="10"/>  
   </Orders>  
</Customers>  
<Customers CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Orders CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <Order_x0020_Details OrderID="10283" ProductID="72" Quantity="3"/>  
   </Orders>  
</Customers>  
</ROOT>';  
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customers')   
EXEC sp_xml_removedocument @idoc;   
```  
  
## <a name="see-also"></a>Vedere anche  
 [Esempi: Uso di OPENXML](../../relational-databases/xml/examples-using-openxml.md)  
  
