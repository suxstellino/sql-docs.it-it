---
title: Query XQuery che coinvolgono l'ordine | Microsoft Docs
description: Esempi di query XQuery basati sulla sequenza di visualizzazione dei nodi in un documento.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- XQuery, sequence
- ordered expressions [XQuery]
ms.assetid: 4f1266c5-93d7-402d-94ed-43f69494c04b
author: rothja
ms.author: jroth
ms.openlocfilehash: d877c75223c42a8dff3a9a4c4615975a136cd6b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352330"
---
# <a name="xqueries-involving-order"></a>Query XQuery che implicano l'ordinamento
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Nei database relazionali non esiste il concetto di sequenza. Ad esempio, non è possibile eseguire una richiesta per ottenere il primo cliente del database, Tuttavia, è possibile eseguire una query su un documento XML e recuperare il primo \<Customer> elemento. In questo modo si otterrà lo stesso cliente.  
  
 In questo argomento vengono illustrate le query basate sulla sequenza di visualizzazione dei nodi nel documento.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-retrieve-manufacturing-steps-at-the-second-work-center-location-for-a-product"></a>R. Recupero delle fasi di produzione di un prodotto nel secondo centro di lavorazione  
 La query seguente recupera le fasi di produzione di un modello di prodotto specifico nel secondo centro di lavorazione di una sequenza di centri coinvolti nel processo di produzione.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    <ManuStep ProdModelID = "{sql:column("Production.ProductModel.ProductModelID")}"  
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
     <Location>  
       { (//AWMI:root/AWMI:Location)[2]/@* }  
       <Steps>  
         { for $s in (//AWMI:root/AWMI:Location)[2]//AWMI:step  
           return  
              <Step>  
               { string($s) }  
              </Step>  
         }  
        </Steps>  
      </Location>  
     </ManuStep>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Le espressioni tra parentesi graffe vengono sostituite dal risultato della relativa valutazione. Per ulteriori informazioni, vedere la pagina relativa alla [costruzione XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md).  
  
-   **@\*** Recupera tutti gli attributi del secondo centro di lavorazione.  
  
-   Iterazione FLWOR (per... RETURN) recupera tutti i <`step`> elementi figlio del secondo centro di lavorazione.  
  
-   La [funzione SQL: Column () (XQuery)](../xquery/xquery-extension-functions-sql-column.md) include il valore relazionale nell'XML che viene costruito.  
  
 Risultato:  
  
```  
<ManuStep ProdModelID="7" ProductModelName="HL Touring Frame">  
  <Location LocationID="20" SetupHours="0.15"   
              MachineHours="2"  LaborHours="1.75" LotSize="1">  
  <Steps>  
   <Step>Assemble all frame components following blueprint 1299.</Step>  
     ...  
  </Steps>  
 </Location>  
</ManuStep>    
```  
  
 La query precedente recupera unicamente i nodi di testo. Se invece si desidera che venga `step` restituita l'intera <elemento>, rimuovere la funzione **String ()** dalla query:  
  
### <a name="b-find-all-the-material-and-tools-used-at-the-second-work-center-location-in-the-manufacturing-of-a-product"></a>B. Ricerca di tutti i materiali e gli strumenti utilizzati nel secondo centro di lavorazione per la produzione di un prodotto  
 La query seguente recupera gli strumenti e i materiali utilizzati per un modello di prodotto specifico nel secondo centro di lavorazione della sequenza di centri coinvolta nel processo di produzione.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <Location>  
      { (//AWMI:root/AWMI:Location)[1]/@* }  
       <Tools>  
         { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:tool  
           return  
              <Tool>  
                { string($s) }  
              </Tool>  
          }  
        </Tools>  
        <Materials>  
            { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:material  
              return  
                 <Material>  
                    { string($s) }  
                 </Material>  
             }  
         </Materials>  
  </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La query crea l'elemento <loca `tion`> e recupera i relativi valori di attributo dal database.  
  
-   La query utilizza due iterazioni di FLWOR (for...return), una per recuperare gli strumenti e l'altra per recuperare i materiali utilizzati.  
  
 Risultato:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
          MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
    <Tool>router with a carbide tip 15</Tool>  
    <Tool>Forming Tool FT-15</Tool>  
  </Tools>  
  <Materials>  
    <Material>aluminum sheet MS-2341</Material>  
  </Materials>  
</Location>  
```  
  
### <a name="c-retrieve-the-first-two-product-feature-descriptions-from-the-product-catalog"></a>C. Recupero delle descrizioni delle prime due caratteristiche di un prodotto dal catalogo dei prodotti  
 Per un modello di prodotto specifico, la query recupera le prime due descrizioni delle funzioni dall' `Features` elemento <> nel catalogo del modello del prodotto.  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <ProductModel ProductModelID= "{ data( (/p1:ProductDescription/@ProductModelID)[1] ) }"  
                   ProductModelName = "{ data( (/p1:ProductDescription/@ProductModelName)[1] ) }" >  
       {  
         for $F in /p1:ProductDescription/p1:Features  
         return   
           $F/*[position() <= 2]   
       }  
     </ProductModel>  
      ') as x  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Dalla query precedente si noti quanto segue:  
  
 Il corpo della query costruisce il codice XML che include l' `ProductModel` elemento <> con gli attributi ProductModelID e ProductModelName.  
  
-   La query usa un per... Restituisce un ciclo per recuperare le descrizioni delle caratteristiche del modello di prodotto. La funzione **Position ()** viene utilizzata per recuperare le prime due funzionalità.  
  
 Risultato:  
  
```xml
<ProductModel ProductModelID="19" ProductModelName="Mountain 100">  
 <p1:Warranty   
  xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
  <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
 <p2:Maintenance   
  xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p2:NoOfYears>10</p2:NoOfYears>  
  <p2:Description>maintenance contact available through your dealer   
            or any AdventureWorks retail store.  
  </p2:Description>  
 </p2:Maintenance>  
</ProductModel>   
```  
  
### <a name="d-find-the-first-two-tools-used-at-the-first-work-center-location-in-the-manufacturing-process-of-the-product"></a>D. Ricerca dei primi due strumenti utilizzati nel primo centro di lavorazione coinvolto nel processo di produzione del prodotto  
 La query seguente restituisce i primi due strumenti utilizzati per un modello di prodotto nel primo centro di lavorazione della sequenza di centri coinvolti nel processo di produzione. La query viene specificata in base alle istruzioni di produzione archiviate nella colonna **instructions** della tabella **Production. ProductModel** .  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   for $Inst in (//AWMI:root/AWMI:Location)[1]  
   return   
     <Location>  
       { $Inst/@* }  
       <Tools>  
         { for $s in ($Inst//AWMI:step//AWMI:tool)[position() <= 2]  
           return  
             <Tool>  
               { string($s) }  
             </Tool>  
         }  
       </Tools>  
     </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Risultato:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
  </Tools>  
</Location>   
```  
  
### <a name="e-find-the-last-two-manufacturing-steps-at-the-first-work-center-location-in-the-manufacturing-of-a-specific-product"></a>E. Ricerca delle ultime due fasi di produzione nel primo centro di lavorazione coinvolto nella produzione di un prodotto specifico  
 La query usa la funzione **Last ()** per recuperare le ultime due fasi di produzione.  
  
```sql
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  <LastTwoManuSteps>  
   <Last-1Step>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[(last()-1)]/text() }  
   </Last-1Step>  
   <LastStep>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[last()]/text() }  
   </LastStep>  
  </LastTwoManuSteps>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Risultato:  
  
```xml
<LastTwoManuSteps>  
   <Last-1Step>When finished, inspect the forms for defects per   
               Inspection Specification .</Last-1Step>  
   <LastStep>Remove the frames from the tool and place them in the   
             Completed or Rejected bin as appropriate.</LastStep>  
</LastTwoManuSteps>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Costrutto XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
