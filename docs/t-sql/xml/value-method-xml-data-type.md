---
description: Metodo value() (tipo di dati xml)
title: Metodo value() (tipo di dati xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- value method
- value() method
ms.assetid: 298a7361-dc9a-4902-9b1e-49a093cd831d
author: rothja
ms.author: jroth
ms.openlocfilehash: 40f5d022ed662c7cbddbd70bee57619605ef1b49
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491317"
---
# <a name="value-method-xml-data-type"></a>Metodo value() (tipo di dati xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Esegue un'espressione XQuery sul codice XML e restituisce un valore di tipo SQL, rappresentato da un valore scalare.  
  
 Questo metodo viene in genere usato per estrarre un valore da un'istanza XML archiviata in una colonna, un parametro o una variabile di tipo **xml**. Ciò consente di specificare query SELECT che combinano o confrontano dati XML con dati di colonne non di tipo XML.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
value (XQuery, SQLType)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *XQuery*  
 Espressione *XQuery*, un valore letterale stringa che recupera i dati contenuti nell'istanza XML. Deve restituire almeno un valore, In caso contrario, viene restituito un errore.  
  
 *SQLType*  
 Tipo SQL preferito che verrà restituito, rappresentato da un valore letterale stringa. Il tipo restituito dal metodo corrisponde al parametro *SQLType*. *SQLType* non può essere un tipo di dati **xml**, un tipo di dati CLR definito dall'utente o un tipo di dati **image**, **text**, **ntext** o **sql_variant**. *SQLType* può tuttavia essere un tipo di dati SQL definito dall'utente.  
  
 Il metodo **value()** usa l'operatore [!INCLUDE[tsql](../../includes/tsql-md.md)] CONVERT in modo implicito e prova a convertire il risultato dell'espressione XQuery, ovvero la rappresentazione della stringa serializzata, dal tipo XSD al tipo SQL corrispondente specificato dalla conversione [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per altre informazioni sulle regole relative al cast del tipo per CONVERT, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
> [!NOTE]  
>  Per motivi di prestazioni, anziché usare il metodo **value()** in un predicato per eseguire il confronto con un valore relazionale, è consigliabile usare **exist()** con **sql:column()** . Questa operazione è illustrata nell'esempio D seguente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-the-value-method-against-an-xml-type-variable"></a>R. Utilizzo del metodo value() su una variabile di tipo xml  
 Nell'esempio seguente, un'istanza XML viene archiviata in una variabile di tipo `xml`. Il metodo `value()` recupera il valore dell'attributo `ProductID` dal codice XML e quindi lo assegna a una variabile `int`.  
  
```sql
DECLARE @myDoc XML  
DECLARE @ProdID INT  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
  
SET @ProdID =  @myDoc.value('(/Root/ProductDescription/@ProductID)[1]', 'int' )  
SELECT @ProdID  
```  
  
 Il risultato restituito è il valore 1.  
  
 Benché nell'istanza XML sia presente un solo attributo `ProductID`, in base alle regole della tipizzazione statica è necessario specificare in modo esplicito che l'espressione di percorso restituisce un singleton. Alla fine dell'espressione di percorso viene pertanto specificato `[1]`. Per altre informazioni sulla tipizzazione statica, vedere [XQuery e tipizzazione statica](../../xquery/xquery-and-static-typing.md).  
  
### <a name="b-using-the-value-method-to-retrieve-a-value-from-an-xml-type-column"></a>B. Utilizzo del metodo value() per recuperare un valore da una colonna di tipo xml  
 La query seguente viene eseguita su una colonna di tipo **xml** (`CatalogDescription`) nel database `AdventureWorks`. La query recupera i valori dell'attributo `ProductModelID` da ognuna delle istanze XML archiviate nella colonna.  
  
```sql
SELECT CatalogDescription.value('             
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";             
       (/PD:ProductDescription/@ProductModelID)[1]', 'int') AS Result             
FROM Production.ProductModel             
WHERE CatalogDescription IS NOT NULL             
ORDER BY Result DESC             
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Viene utilizzata la parola chiave `namespace` per definire un prefisso dello spazio dei nomi.  
  
-   In base ai requisiti per la tipizzazione statica, nel metodo `[1]` viene aggiunto `value()` alla fine dell'espressione di percorso, allo scopo di indicare in modo esplicito che l'espressione restituisce un singleton.  
  
 Risultato parziale:  
  
```  
-----------  
35           
34           
...  
```  
  
### <a name="c-using-the-value-and-exist-methods-to-retrieve-values-from-an-xml-type-column"></a>C. Utilizzo dei metodi value() ed exist() per recuperare valori da una colonna di tipo xml  
 Nell'esempio seguente viene illustrato l'uso del metodo `value()` e del metodo [exist()](../../t-sql/xml/exist-method-xml-data-type.md) del tipo di dati **xml**. Il metodo `value()` consente di recuperare i valori dell'attributo `ProductModelID` dal codice XML. Il metodo `exist()` nella clausola `WHERE` consente di filtrare le righe della tabella.  
  
 La query recupera dalle istanze XML gli ID dei modelli del prodotto che includono tra le funzionalità informazioni relative alla garanzia (elemento <`Warranty`>). La condizione della clausola `WHERE` utilizza il metodo `exist()` per recuperare solo le righe che soddisfano tale condizione.  
  
```sql
SELECT CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           (/PD:ProductDescription/@ProductModelID)[1] ', 'int') AS Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La colonna `CatalogDescription` è una colonna XML tipizzata, ovvero a tale colonna è associato una raccolta di schemi. Nel [prologo XQuery](../../xquery/modules-and-prologs-xquery-prolog.md) la dichiarazione dello spazio dei nomi consente di definire il prefisso usato successivamente nel corpo della query.  
  
-   Se il metodo `exist()` restituisce `1` (True), significa che l'istanza XML include l'elemento figlio <`Warranty`> come una delle funzionalità.  
  
-   Il metodo `value()` della clausola `SELECT` recupera quindi i valori dell'attributo `ProductModelID` come valori interi.  
  
 Risultato parziale:  
  
```  
Result       
-----------  
19           
23           
...  
```  
  
### <a name="d-using-the-exist-method-instead-of-the-value-method"></a>D. Utilizzo del metodo exist() in sostituzione del metodo value()  
 Per motivi relativi alle prestazioni, anziché utilizzare il metodo `value()` in un predicato per eseguire il confronto con un valore relazionale, è consigliabile utilizzare `exist()` con `sql:column()`. Ad esempio:  
  
```sql
CREATE TABLE T (c1 INT, c2 VARCHAR(10), c3 XML)  
GO  
  
SELECT c1, c2, c3   
FROM T  
WHERE c3.value( '/root[1]/@a', 'integer') = c1  
GO  
```  
  
 È possibile scrivere il codice nel modo seguente:  
  
```sql
SELECT c1, c2, c3   
FROM T  
WHERE c3.exist( '/root[@a=sql:column("c1")]') = 1  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
