---
description: replace value of (XML DML)
title: replace value of (XML DML)
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
- update keyword
- replacement values [XML DML]
- updating node values
- replace value of XML DML statement
ms.assetid: c310f6df-7adf-493b-b56b-8e3143b13ae7
author: rothja
ms.author: jroth
ms.openlocfilehash: cc3c4b3ca7dec02a4b01c426a2f3503071822393
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491315"
---
# <a name="replace-value-of-xml-dml"></a>replace value of (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Aggiorna il valore di un nodo nel documento.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
replace value of Expression1   
with Expression2  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*Expression1*  
Identifica un nodo di cui è necessario aggiornare il valore. Deve identificare solo un singolo nodo, ovvero *Expression1* deve essere un singleton statico. Se l'istanza XML è tipizzata, il nodo deve essere di tipo semplice. Quando vengono selezionati più nodi, viene generato un errore. Se *Expression1* restituisce una sequenza vuota, non viene eseguita alcuna sostituzione di valori e non vengono restituiti errori. *Expression1* deve restituire un singolo elemento con contenuto di tipo semplice (tipo elenco o atomico), un nodo di testo o un nodo di attributo. *Expression1* non può essere un tipo unione, un tipo complesso, un'istruzione di elaborazione, un nodo di documento o un nodo di commento. In caso contrario verrà restituito un errore.  
  
*Expression2*  
Identifica il nuovo valore del nodo. Può essere un'espressione che restituisce un nodo di tipo semplice, perché **data()** verrà usato in modo implicito. Se il valore è un elenco di valori, l'istruzione **update** sostituisce il valore precedente con l'elenco. In caso di modifica di un'istanza XML tipizzata, *Expression2* deve essere dello stesso tipo o sottotipo di *Expression* 1. In caso contrario, viene restituito un errore. In caso di modifica di un'istanza XML non tipizzata, *Expression2* deve essere un'espressione che è possibile atomizzare. In caso contrario, viene restituito un errore.  
  
## <a name="examples"></a>Esempi  
Negli esempi seguenti di istruzione XML DML **replace value of** viene illustrata la modalità di aggiornamento dei nodi in un documento XML.  
  
### <a name="a-replacing-values-in-an-xml-instance"></a>R. Sostituzione di valori in un'istanza XML  
Nell'esempio seguente, viene assegnata prima un'istanza di documento a una variabile di tipo **xml**. Successivamente, le istruzioni XML DML **replace value of** aggiornano i valori nel documento.  
  
```sql
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Manufacturing steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>';  
SELECT @myDoc;  
  
-- update text in the first manufacturing step  
SET @myDoc.modify('  
  replace value of (/Root/Location/step[1]/text())[1]  
  with "new text describing the manu step"  
');  
SELECT @myDoc;  
-- update attribute value  
SET @myDoc.modify('  
  replace value of (/Root/Location/@LaborHours)[1]  
  with "100.0"  
');  
SELECT @myDoc;  
```  
  
La destinazione da aggiornare deve essere al massimo un singolo nodo specificato in modo esplicito nell'espressione di percorso aggiungendo "[1]" alla fine dell'espressione.  
  
### <a name="b-using-the-if-expression-to-determine-replacement-value"></a>B. Utilizzo dell'espressione if per determinare il valore di sostituzione  
È possibile specificare l'espressione **if** in Expression2 all'interno dell'istruzione XML DML **replace value of**, come illustrato nell'esempio seguente. Expression1 indica che l'attributo LaborHours del primo centro di lavoro deve essere aggiornato. Expression2 usa un'espressione **if** per determinare il nuovo valore dell'attributo LaborHours.  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours=".1"  
            MachineHours=".2" >Manu steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
--SELECT @myDoc  
  
SET @myDoc.modify('  
  replace value of (/Root/Location[1]/@LaborHours)[1]  
  with (  
       if (count(/Root/Location[1]/step) > 3) then  
         "3.0"  
       else  
          "1.0"  
      )  
')  
SELECT @myDoc  
```  
  
### <a name="c-updating-xml-stored-in-an-untyped-xml-column"></a>C. Aggiornamento di un'istanza XML archiviata in una colonna XML non tipizzata  
Nell'esempio seguente viene aggiornata un'istanza XML archiviata in una colonna:  
  
```sql
DROP TABLE T  
GO  
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
go  
-- verify the current <ProductDescription> element  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
-- update the ProductName attribute value  
UPDATE T  
SET x.modify('  
  replace value of (/Root/ProductDescription/@ProductName)[1]  
  with "New Road Bike" ')  
-- verify the update  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
```  
  
### <a name="d-updating-xml-stored-in-a-typed-xml-column"></a>D. Aggiornamento di un'istanza XML archiviata in una colonna XML tipizzata  
In questo esempio, vengono sostituiti i valori di un documento di istruzioni di produzione archiviato in una colonna XML tipizzata.  
  
Innanzitutto, viene creata una tabella (T) con una colonna XML tipizzata nel database AdventureWorks. Un'istanza del codice XML delle istruzioni di produzione viene quindi copiata dalla colonna Instructions della tabella ProductModel nella tabella T. Gli inserimenti vengono quindi applicati al codice XML nella tabella T.  
  
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
WHERE ProductModelID=7  
GO
--insert a new location - <Location 1000/>.   
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
insert <MI:Location LocationID="1000"  LaborHours="1000"  LotSize="1000" >  
           <MI:step>Do something using <MI:tool>hammer</MI:tool></MI:step>  
         </MI:Location>  
  as first  
  into (/MI:root)[1]  
')  
GO  
SELECT Instructions  
FROM T  
GO  
-- Now replace manu. tool in location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/MI:step/MI:tool)[1]   
  with "screwdriver"  
')  
GO  
SELECT Instructions  
FROM T  
-- Now replace value of lot size  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/@LotSize)[1]   
  with 500 cast as xs:decimal ?  
')  
GO  
SELECT Instructions  
FROM T  
```  
  
Si noti l'uso di **cast** per la sostituzione del valore LotSize, necessario quando il valore deve essere di un tipo specifico. In questo esempio, se il valore è 500, non è necessario eseguire il cast esplicito.  
  
## <a name="see-also"></a>Vedere anche  
[Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
[Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
[metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
[Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
