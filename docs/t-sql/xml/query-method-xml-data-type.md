---
description: Metodo query() con tipo di dati XML
title: Metodo query() con tipo di dati XML
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query method
- query() method
ms.assetid: f48f6f7b-219f-463a-bf36-bc10f21afaeb
author: rothja
ms.author: jroth
ms.openlocfilehash: aa45cbfb4e629304d3a64c36da530807f7338798
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491226"
---
# <a name="query-method-xml-data-type"></a>Metodo query() con tipo di dati XML
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Specifica una query XQuery su un'istanza con tipo di dati **xml**. Il risultato è di tipo **xml**. Il metodo restituisce un'istanza XML non tipizzata.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
query ('XQuery')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
XQuery  
Stringa costituita da un'espressione XQuery che esegue query su nodi XML, ad esempio elementi e attributi, in un'istanza XML.  
  
## <a name="examples"></a>Esempi  
In questa sezione sono disponibili esempi di uso del metodo query() con tipo di dati **xml**.  
  
### <a name="a-using-the-query-method-against-an-xml-type-variable"></a>R. Utilizzo del metodo query() in una variabile di tipo XML  
L'esempio seguente dichiara una variabile **\@myDoc** di tipo **xml** e assegna a questa un'istanza XML. In seguito viene usato il metodo **query()** per specificare una query XQuery sul documento.  
  
La query recupera l'elemento figlio <`Features`> dell'elemento <`ProductDescription`>:  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc.query('/Root/ProductDescription/Features')  
```  
  
Nell'output seguente viene visualizzato il risultato:  
  
```  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>        
```  
  
### <a name="b-using-the-query-method-against-an-xml-type-column"></a>B. Utilizzo del metodo query() in una colonna di tipo XML  
Nell'esempio seguente viene usato il metodo **query()** per specificare una query XQuery sulla colonna **CatalogDescription** di tipo **xml** nel database **AdventureWorks**:  
  
```sql
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />  
') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
Dalla query precedente si noti quanto segue:  
  
-   La colonna CatalogDescription è una colonna **xml** tipizzata, ovvero ad essa è associata una raccolta di schemi. Nel [prologo di una query XQuery](../../xquery/modules-and-prologs-xquery-prolog.md), la parola chiave **namespace** definisce il prefisso incluso in seguito nel corpo della query.  
  
-   Il metodo **query()** consente di costruire una struttura XML, un elemento <`Product`> con un attributo **ProductModelID** in cui il valore dell'attributo **ProductModelID** viene recuperato dal database. Per altre informazioni sulla costruzione di strutture XML, vedere [XML Construction &#40;XQuery&#41;](../../xquery/xml-construction-xquery.md) (Costruzione XML &#40;XQuery&#41;).  
  
-   Il [metodo exist() (tipo di dati XML)](../../t-sql/xml/exist-method-xml-data-type.md) nella clausola WHERE trova solo le righe che contengono l'elemento <`Warranty`> nella struttura XML. Anche in questo caso la parola chiave **namespace** definisce due prefissi dello spazio dei nomi.  
  
Nell'output seguente viene parzialmente visualizzato il report:  
  
```  
<Product ProductModelID="19"/>   
<Product ProductModelID="23"/>   
...  
```  
  
Osservare come i metodi query() ed exist() dichiarino entrambi il prefisso PD. In tali casi, è possibile utilizzare WITH XMLNAMESPACES per definire innanzitutto i prefissi e utilizzarli nella query.  
  
```sql
WITH XMLNAMESPACES 
(  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM
)  
SELECT CatalogDescription.query('<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />')
       AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/PD:ProductDescription/PD:Features/WM:Warranty ') = 1;
```  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
