---
description: delete (XML DML)
title: delete (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- delete keyword
- delete statement [XML DML]
- deleting nodes
ms.assetid: b22c93a4-b84d-4356-af4c-6013322a4b71
author: rothja
ms.author: jroth
ms.openlocfilehash: 28aee5ca4b84b2b990e4f35b6d64dcdccdee8481
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491286"
---
# <a name="delete-xml-dml"></a>delete (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Elimina i nodi da un'istanza XML.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
delete Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Espressione*  
 È un'espressione XQuery che identifica i nodi da eliminare. Vengono eliminati tutti i nodi selezionati dall'espressione e i nodi e valori contenuti nei nodi selezionati. Come descritto nell'argomento [insert (XML DML)](../../t-sql/xml/insert-xml-dml.md), è necessario indicare un riferimento a un nodo esistente nel documento. Non può trattarsi di un nodo costruito. L'espressione non può essere il nodo radice (/). Se l'espressione restituisce una sequenza vuota l'eliminazione non viene eseguita e non vengono restituiti errori.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-deleting-nodes-from-a-document-stored-in-an-untyped-xml-variable"></a>R. Eliminazione di nodi da un documento archiviato in una variabile xml non tipizzata  
 Nell'esempio seguente viene illustrata l'eliminazione di vari nodi da un documento. Un'istanza XML viene prima di tutto assegnata a una variabile di tipo **xml**. Le istruzioni XML DML di eliminazione successive eliminano vari nodi dal documento.  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<?Instructions for=TheWC.exe ?>   
<Root>  
 <!-- instructions for the 1st work center -->  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Some text 1  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
SELECT @myDoc  
  
-- delete an attribute  
SET @myDoc.modify('  
  delete /Root/Location/@MachineHours  
')  
SELECT @myDoc  
  
-- delete an element  
SET @myDoc.modify('  
  delete /Root/Location/step[2]  
')  
SELECT @myDoc  
  
-- delete text node (in <Location>  
SET @myDoc.modify('  
  delete /Root/Location/text()  
')  
SELECT @myDoc  
  
-- delete all processing instructions  
SET @myDoc.modify('  
  delete //processing-instruction()  
')  
SELECT @myDoc  
```  
  
### <a name="b-deleting-nodes-from-a-document-stored-in-an-untyped-xml-column"></a>B. Eliminazione di nodi da un documento archiviato in una colonna xml non tipizzata  
 Nell'esempio seguente un'istruzione XML DML **delete** rimuove il secondo elemento figlio di <`Features`> dal documento archiviato nella colonna.  
  
```sql
CREATE TABLE T (i INT, x XML)  
GO  
INSERT INTO T VALUES(1,'<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>')  
GO
-- verify the contents before delete  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
-- delete the second feature  
UPDATE T  
SET x.modify('delete /Root/ProductDescription/Features/*[2]')  
-- verify the deletion  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il [metodo modify() (tipo di dati xml)](../../t-sql/xml/modify-method-xml-data-type.md) viene usato per specificare la parola chiave XML DML **delete**.  
  
-   Il [metodo query() (tipo di dati xml)](../../t-sql/xml/query-method-xml-data-type.md) viene usato per l'esecuzione della query sul documento.  
  
### <a name="c-deleting-nodes-from-a-typed-xml-column"></a>C. Eliminazione di nodi da una colonna xml tipizzata  
 Questo esempio elimina nodi da un documento XML con istruzioni per la produzione archiviato in una colonna **xml** tipizzata.  
  
 Innanzitutto, viene creata una tabella (T) con una colonna **xml** tipizzata nel database AdventureWorks. Un'istanza XML di istruzioni di produzione viene quindi copiata dalla colonna Instructions della tabella ProductModel alla tabella T e uno o più nodi vengono eliminati dal documento.  
  
```sql
USE AdventureWorks  
GO  
DROP TABLE T  
GO  
CREATE TABLE T(
    ProductModelID INT PRIMARY KEY,   
    Instructions XML (Production.ManuInstructionsSchemaCollection))  
GO  
INSERT T   
SELECT ProductModelID, Instructions  
FROM Production.ProductModel  
WHERE ProductModelID = 7  
GO  
SELECT Instructions  
FROM T  
--1) insert <Location 1000/>. Note: <Root> must be singleton in the query  
UPDATE T  
SET Instructions.modify('  
  DECLARE namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  INSERT <MI:Location LocationID="1000"  LaborHours="1000" >  
           These are manu steps at location 1000.   
           <MI:step>New step1 instructions</MI:step>  
           Instructions for step 2 are here  
           <MI:step>New step 2 instructions</MI:step>  
         </MI:Location>  
  AS first  
  INTO   (/MI:root)[1]  
')  
GO 
SELECT Instructions  
FROM T  
  
-- delete an attribute  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/@LaborHours)   
')  
GO  
SELECT Instructions  
FROM T  
-- delete text in <location>  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/text())   
')  
GO  
SET Instructions  
FROM T  
-- delete 2nd manu step at location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/MI:step[2])   
')  
GO  
SELECT Instructions  
FROM T  
-- cleanup  
DROP TABLE T  
GO 
```  
  
## <a name="see-also"></a>Vedere anche  
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
