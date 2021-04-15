---
title: 'Esempio: Recupero di informazioni sui dipendenti | Microsoft Docs'
description: Visualizzare un esempio di come recuperare le informazioni sui dipendenti da un database in formato XML usando una query con FOR XML EXPLICIT.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- EXPLICIT mode
ms.assetid: 63cd6569-2600-485b-92b4-1f6ba09db219
author: rothja
ms.author: jroth
ms.openlocfilehash: 89136f9e67022c619c1d9ad4e78e4196fe131a68
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490919"
---
# <a name="example-retrieving-employee-information"></a>Esempio: Recupero di informazioni sui dipendenti
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo esempio vengono recuperati ID e nome di ogni dipendente. Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] è possibile ottenere il valore employeeID dalla colonna BusinessEntityID della tabella Employee. I nomi dei dipendenti possono essere ottenuti dalla tabella Person. La colonna BusinessEntityID può essere utilizzata per unire in join le tabelle.  
  
 Si supponga di voler utilizzare la trasformazione FOR XML EXPLICIT per generare codice XML come illustrato di seguito:  
  
```  
<Employee EmpID="1" >  
  <Name FName="Ken" LName="Sánchez" />  
</Employee>  
...  
```  
  
 Poiché la gerarchia contiene due livelli, è necessario scrivere due query `SELECT` e applicare UNION ALL. Questa è la prima query che recupera i valori per l'elemento <`Employee`> e i relativi attributi. La query assegna `1` come valore di `Tag` per l'elemento <`Employee`> e NULL come `Parent`, poiché si tratta dell'elemento di livello principale.  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID AS [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 Di seguito è riportata la seconda query, che recupera i valori per l'elemento <`Name`>. Assegna `2` come valore di `Tag` per l'elemento <`Name`> e `1` come valore di tag `Parent`, identificando <`Employee`> come elemento padre.  
  
```  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 È necessario combinare queste query con `UNION AL`L, applicare `FOR XML EXPLICIT`e specificare la clausola obbligatoria `ORDER BY` . Il set di righe deve essere ordinato prima per `BusinessEntityID` e quindi per nome, in modo che i valori NULL nel nome vengano visualizzati per primi. Eseguendo la query successiva senza la clausola FOR XML viene generata la tabella universale.  
  
 La query finale sarà pertanto:  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID as [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
ORDER BY [Employee!1!EmpID],[Name!2!FName]  
FOR XML EXPLICIT;  
```  
  
 Risultato parziale:  
  
```
<Employee EmpID="1">
  <Name FName="Ken" LName="Sánchez" />
</Employee>
<Employee EmpID="2">
  <Name FName="Terri" LName="Duffy" />
</Employee>
...
```
  
 La prima istruzione `SELECT` specifica i nomi per le colonne nel set di righe risultante. I nomi formano due gruppi di colonne. Il gruppo con valore di `Tag` pari a `1` nel nome della colonna identifica `Employee` come elemento ed `EmpID` come attributo. L'altro gruppo di colonne contiene il valore di `Tag``2` nella colonna e identifica <`Name`> come elemento e `FName` e `LName` come attributi.  
  
 La tabella seguente mostra il set di righe parziale generato dalla query:  
  
Tag | Parent | Employee!1!EmpID | Name!2!FName | Name!2!LName
-|-|-|-|-
1 | NULL | 1 | NULL | NULL 
2 | 1 | 1 | Ken | Sánchez 
1 | NULL | 2 | NULL | NULL 
2 | 1 | 2 | Terri | Duffy 
1 | NULL | 3 | NULL | NULL 
2 | 1 | 3 | Roberto | Tamburello 
... | ... | ... | ... | ...
  
 Di seguito viene descritto il processo di elaborazione delle righe della tabella universale per la creazione dell'albero XML risultante:  
  
 La prima riga identifica il valore `Tag` come `1`. Viene pertanto identificato il gruppo di colonne con il valore `Tag` di `1` , `Employee!1!EmpID`. Questa colonna identifica `Employee` come nome dell'elemento. Viene quindi creato un elemento <`Employee`> con attributi `EmpID`. A questi attributi vengono assegnati i valori di colonna corrispondenti.  
  
 La seconda riga ha il valore `Tag` di `2`. Viene pertanto identificato il gruppo di colonne con il valore `Tag` di `2` nel nome della colonna, `Name!2!FName`, `Name!2!LName`. Questi nomi di colonna identificano `Name` come nome dell'elemento. Viene creato un elemento <`Name`> con attributi `FName` e `LName`. A questi attributi vengono quindi assegnati i valori di colonna corrispondenti. Questa riga identifica `1` come valore di `Parent`. Questo elemento figlio viene aggiunto all'elemento <`Employee`> precedente.  
  
 Il processo viene ripetuto per tutte le righe del set di righe. Si noti l'importanza dell'ordinamento delle righe nella tabella universale, in modo che l'istruzione FOR XML EXPLICIT possa elaborare il set di righe nell'ordine corretto e generare il codice XML desiderato.  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
