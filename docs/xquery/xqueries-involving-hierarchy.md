---
title: Query XQuery che coinvolgono la gerarchia | Microsoft Docs
description: Visualizzare esempi di query XQuery che coinvolgono le gerarchie.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- hierarchies [XQuery]
- XQuery, hierarchies
ms.assetid: 6953d8b7-bad8-4b64-bf7b-12fa4f10f65c
author: rothja
ms.author: jroth
ms.openlocfilehash: fdebb93459b9519eb6495054908208de610e8222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352788"
---
# <a name="xqueries-involving-hierarchy"></a>Esecuzione di query XQuery che coinvolgono gerarchie
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  La maggior parte delle colonne di tipo **XML** nel database **AdventureWorks** è composta da documenti semi-strutturati. I documenti archiviati nelle diverse righe possono pertanto avere aspetti diversi. Le query di esempio contenute in questo argomento illustrano come estrarre informazioni dai vari documenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-from-the-manufacturing-instructions-documents-retrieve-work-center-locations-together-with-the-first-manufacturing-step-at-those-locations"></a>R. Recupero delle posizioni dei centri di lavorazione, insieme al primo passaggio di produzione eseguito in tali centri, dai documenti contenenti le istruzioni per la produzione  
 Per il modello di prodotto 7, la query costruisce il codice XML che include l' `ManuInstr` elemento <>, con gli attributi **ProductModelID** e **ProductModelName** e uno o più <`Location`> elementi figlio.  
  
 Ogni `Location` elemento <> dispone di un proprio set di attributi e di uno <`step`> elemento figlio. Questo <`step`> elemento figlio è il primo passaggio di produzione nella posizione del centro di lavorazione.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   \<ManuInstr  ProdModelID = "{sql:column("Production.ProductModel.ProductModelID") }"   
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
            {   
              for $wc in //AWMI:root/AWMI:Location  
              return  
                <Location>  
                 {$wc/@* }  
                 <step1> { string( ($wc//AWMI:step)[1] ) } </step1>  
                </Location>  
            }  
          </ManuInstr>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La parola chiave **namespace** nel [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md) definisce un prefisso dello spazio dei nomi. Tale prefisso verrà utilizzato in seguito nel corpo della query.  
  
-   I token per lo scambio di contesto, {) e (}, vengono utilizzati nella query per passare dalla costruzione delle informazioni XML alla valutazione della query.  
  
-   **SQL: Column ()** viene utilizzato per includere un valore relazionale nel codice XML che viene costruito.  
  
-   Nella costruzione dell'elemento <`Location`>, $WC/@ * recupera tutti gli attributi della posizione del centro di lavorazione.  
  
-   La funzione **String ()** restituisce il valore stringa dall'elemento <`step`>.  
  
 Risultato parziale:  
  
```xml
<ManuInstr ProdModelID="7" ProductModelName="HL Touring Frame">  
   <Location LocationID="10" SetupHours="0.5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
     <step1>Insert aluminum sheet MS-2341 into the T-85A   
             framing tool.</step1>  
   </Location>  
   <Location LocationID="20" SetupHours="0.15"   
            MachineHours="2" LaborHours="1.75" LotSize="1">  
      <step1>Assemble all frame components following   
             blueprint 1299.</step1>  
   </Location>  
...  
</ManuInstr>   
```  
  
### <a name="b-find-all-telephone-numbers-in-the-additionalcontactinfo-column"></a>B. Ricerca di tutti i numeri di telefono nella colonna AdditionalContactInfo  
 La query seguente recupera i numeri di telefono aggiuntivi per un contatto del cliente specifico eseguendo una ricerca nell'intera gerarchia per l' `telephoneNumber` elemento <>. Poiché l' `telephoneNumber` elemento <> può trovarsi in qualsiasi punto della gerarchia, la query usa il discendente e l'operatore autonomo (//) nella ricerca.  
  
```sql
SELECT AdditionalContactInfo.query('  
 declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
 declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
for $ph in /ci:AdditionalContactInfo//act:telephoneNumber  
   return  
      $ph/act:number  
') as x  
FROM  Person.Contact  
WHERE ContactID = 1  
```  
  
 Risultato:  
  
```xml
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  112-111-1111  
\</act:number>  
```  
  
 Per recuperare solo i numeri di telefono di primo livello, in particolare il <`telephoneNumber`> elementi figlio di <`AdditionalContactInfo`>, l'espressione for nella query viene modificata in  
  
 `for $ph in /ci:AdditionalContactInfo/act:telephoneNumber`.  
  
## <a name="see-also"></a>Vedi anche  
 [Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)   
 [Costrutto XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
  
