---
description: Esempi di istruzioni SELECT (Transact-SQL)
title: Esempi di istruzioni SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- GROUP BY clause, SELECT statement
- query hints [SQL Server]
- ALL keyword
- ROLLUP operator
- SELECT statement [SQL Server], examples
- correlated subqueries, SELECT statement
- SELECT INTO statement
- ORDER BY clause [Transact-SQL]
- GROUPING function
- index hints [SQL Server]
- HAVING clause, SELECT statement
- DISTINCT keyword
- CUBE operator
- UNION operator [SQL Server]
- computed sums
- WHERE clause, SELECT statement
ms.assetid: 9b9caa3d-e7d0-42e1-b60b-a5572142186c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2e12a155eb1a3c710b3289912f9e9e12f3642ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207586"
---
# <a name="select-examples-transact-sql"></a>Esempi di istruzioni SELECT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Questo argomento include alcuni esempi relativi all'uso dell'istruzione [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="a-using-select-to-retrieve-rows-and-columns"></a>R. Utilizzo dell'istruzione SELECT per il recupero di righe e colonne  
 Nell'esempio seguente vengono illustrati tre blocchi di codice. Nel primo esempio di codice vengono restituite tutte le righe (clausola WHERE omessa) e tutte le colonne (utilizzando `*`) della tabella `Product` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
 [!code-sql[Select#SelectExamples1](../../t-sql/queries/codesnippet/tsql/select-examples-transact_1.sql)]  
  
 Nell'esempio seguente vengono restituite tutte le righe (clausola WHERE omessa) e solo un subset delle colonne (`Name`, `ProductNumber`, `ListPrice`) della tabella `Product` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Viene aggiunta, inoltre, un'intestazione di colonna.  
  
 [!code-sql[Select#SelectExamples2](../../t-sql/queries/codesnippet/tsql/select-examples-transact_2.sql)]  
  
 Nell'esempio seguente vengono restituite solo le righe della tabella `Product` caratterizzate da una riga di prodotto `R` e da un numero di giorni per l'invio in produzione minore di `4`.  
  
 [!code-sql[Select#SelectExamples3](../../t-sql/queries/codesnippet/tsql/select-examples-transact_3.sql)]  
  
## <a name="b-using-select-with-column-headings-and-calculations"></a>B. Utilizzo dell'istruzione SELECT con intestazioni e calcoli di colonna  
 Nell'esempio seguente vengono restituite tutte le righe della tabella `Product`. Nel primo esempio vengono restituite le vendite totali e gli sconti per ogni prodotto. Nel secondo esempio vengono calcolati i ricavi totali per ogni prodotto.  
  
 [!code-sql[Select#SelectExamples4](../../t-sql/queries/codesnippet/tsql/select-examples-transact_4.sql)]  
  
 Questa è la query che calcola il ricavo per ogni prodotto di ogni ordine di vendita.  
  
 [!code-sql[Select#SelectExamples5](../../t-sql/queries/codesnippet/tsql/select-examples-transact_5.sql)]  
  
## <a name="c-using-distinct-with-select"></a>C. Utilizzo della clausola DISTINCT con l'istruzione SELECT  
 Nell'esempio seguente viene utilizzata la clausola `DISTINCT` per evitare il recupero di titoli duplicati.  
  
 [!code-sql[Select#SelectExamples6](../../t-sql/queries/codesnippet/tsql/select-examples-transact_6.sql)]  
  
## <a name="d-creating-tables-with-select-into"></a>D. Creazione di tabelle con l'istruzione SELECT INTO  
 Nel primo esempio seguente viene creata una tabella temporanea denominata `#Bicycles` in `tempdb`.  
  
 [!code-sql[Select#SelectExamples7](../../t-sql/queries/codesnippet/tsql/select-examples-transact_7.sql)]  
  
 Nel secondo esempio viene creata la tabella permanente `NewProducts`.  
  
 [!code-sql[Select#SelectExamples8](../../t-sql/queries/codesnippet/tsql/select-examples-transact_8.sql)]  
  
## <a name="e-using-correlated-subqueries"></a>E. Utilizzo di sottoquery correlate
Una query correlata è una query i cui valori dipendono dalla query esterna. Questa query può essere eseguita ripetutamente, una volta per ogni riga che può essere selezionata dalla query esterna.

Nel primo esempio vengono illustrate query semanticamente equivalenti per evidenziare la differenza tra l'uso della parola chiave `EXISTS` e della parola chiave `IN`. Entrambi sono esempi di sottoquery valide che recuperano un'istanza del nome di ogni prodotto del modello "Long-sleeve logo jersey" e con valori `ProductModelID` uguali nelle tabelle `Product` e `ProductModel`.  
  
 [!code-sql[Select#SelectExamples9](../../t-sql/queries/codesnippet/tsql/select-examples-transact_9.sql)]  
  
 Il prossimo esempio usa `IN` e recupera un'istanza del nome e del cognome di ogni dipendente il cui bonus nella tabella `SalesPerson` corrisponde a `5000.00` e con numero di identificazione uguale nelle tabelle `Employee` e `SalesPerson`.  
  
 [!code-sql[Select#SelectExamples10](../../t-sql/queries/codesnippet/tsql/select-examples-transact_10.sql)]  
  
 La sottoquery precedente non può essere valutata indipendentemente dalla query esterna. Richiede un valore per `Employee.EmployeeID`, ma questo valore cambia quando [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] esamina righe diverse della tabella `Employee`.  
  
 È inoltre possibile inserire una sottoquery correlata nella clausola `HAVING` della query esterna. Nell'esempio vengono trovati i modelli il cui prezzo massimo è più del doppio del prezzo medio per il modello.  
  
 [!code-sql[Select#SelectExamples11](../../t-sql/queries/codesnippet/tsql/select-examples-transact_11.sql)]  
  
 In questo esempio vengono utilizzate due sottoquery correlate per trovare i nomi dei dipendenti che hanno venduto un determinato prodotto.  
  
 [!code-sql[Select#SelectExamples12](../../t-sql/queries/codesnippet/tsql/select-examples-transact_12.sql)]  
  
## <a name="f-using-group-by"></a>F. Utilizzo della clausola GROUP BY  
 Nell'esempio seguente viene trovato il totale di ogni ordine di vendita nel database.  
  
 [!code-sql[Select#SelectExamples13](../../t-sql/queries/codesnippet/tsql/select-examples-transact_13.sql)]  
  
 La presenza della clausola `GROUP BY` comporta la restituzione di una sola riga contenente il totale di tutte le vendite per ogni ordine di vendita.  
  
## <a name="g-using-group-by-with-multiple-groups"></a>G. Utilizzo della clausola GROUP BY con più gruppi  
 Nell'esempio seguente vengono individuati il prezzo medio e il totale delle vendite per l'anno in corso raggruppati per ID del prodotto e ID dell'offerta speciale.  
  
 [!code-sql[Select#SelectExamples14](../../t-sql/queries/codesnippet/tsql/select-examples-transact_14.sql)]  
  
## <a name="h-using-group-by-and-where"></a>H. Utilizzo delle clausole GROUP BY e WHERE  
 Nell'esempio seguente i risultati vengono suddivisi in gruppi dopo che sono state recuperate le righe con prezzi maggiori di `$1000`.  
  
 [!code-sql[Select#SelectExamples15](../../t-sql/queries/codesnippet/tsql/select-examples-transact_15.sql)]  
  
## <a name="i-using-group-by-with-an-expression"></a>I. Utilizzo della clausola GROUP BY con un'espressione  
 Nell'esempio seguente vengono creati gruppi in base a un'espressione. È possibile creare gruppi in base a un'espressione se tale espressione non include funzioni di aggregazione.  
  
 [!code-sql[Select#SelectExamples16](../../t-sql/queries/codesnippet/tsql/select-examples-transact_16.sql)]  
  
## <a name="j-using-group-by-with-order-by"></a>J. Utilizzo della clausola GROUP BY con la clausola ORDER BY  
 Nell'esempio seguente viene individuato il prezzo medio di ogni tipo di prodotto e i risultati vengono ordinati in base al prezzo medio.  
  
 [!code-sql[Select#SelectExamples18](../../t-sql/queries/codesnippet/tsql/select-examples-transact_17.sql)]  
  
## <a name="k-using-the-having-clause"></a>K. Utilizzo della clausola HAVING  
 Nel primo esempio viene illustrata la clausola `HAVING` con una funzione di aggregazione. Le righe della tabella `SalesOrderDetail` vengono raggruppate per ID di prodotto e vengono eliminati i prodotti con ordini con quantitativo medio minore o uguale a cinque. Nel secondo esempio viene illustrata una clausola `HAVING` senza funzioni di aggregazione.  
  
 [!code-sql[Select#SelectExamples19](../../t-sql/queries/codesnippet/tsql/select-examples-transact_18.sql)]  
  
 Questa query utilizza la clausola `LIKE` all'interno della clausola `HAVING`.  
  
```sql
USE AdventureWorks2012 ;  
GO  
SELECT SalesOrderID, CarrierTrackingNumber   
FROM Sales.SalesOrderDetail  
GROUP BY SalesOrderID, CarrierTrackingNumber  
HAVING CarrierTrackingNumber LIKE '4BD%'  
ORDER BY SalesOrderID ;  
GO  
```  
  
## <a name="l-using-having-and-group-by"></a>L. Utilizzo delle clausole HAVING e GROUP BY  
 Nell'esempio seguente viene illustrato l'utilizzo delle clausole `GROUP BY`, `HAVING`, `WHERE` e `ORDER BY` in un'istruzione `SELECT`. Vengono creati gruppi e valori di riepilogo, ma solo dopo l'eliminazione dei prodotti con prezzo maggiore di $25 e quantitativo medio minore di 5. I risultati vengono organizzati in base a `ProductID`.  
  
 [!code-sql[Select#SelectExamples21](../../t-sql/queries/codesnippet/tsql/select-examples-transact_19.sql)]  
  
## <a name="m-using-having-with-sum-and-avg"></a>M. Utilizzo della clausola HAVING con le funzioni SUM e AVG  
 Nell'esempio seguente il contenuto della tabella `SalesOrderDetail` viene raggruppato in base all'ID prodotto e vengono inclusi solo i gruppi di prodotti con ordini che ammontano a più di `$1000000.00` e con quantitativo medio minore di `3`.  
  
 [!code-sql[Select#SelectExamples22](../../t-sql/queries/codesnippet/tsql/select-examples-transact_20.sql)]  
  
 Per visualizzare i prodotti con vendite totali maggiori di `$2000000.00`, utilizzare la query seguente:  
  
 [!code-sql[Select#SelectExamples23](../../t-sql/queries/codesnippet/tsql/select-examples-transact_21.sql)]  
  
 Se si desidera che nei calcoli siano inclusi solo i prodotti per cui sono stati venduti almeno 1500 articoli, utilizzare la clausola `HAVING COUNT(*) > 1500` per eliminare i prodotti per cui sono stati venduti meno di `1500` articoli. La query appare come la seguente:  
  
 [!code-sql[Select#SelectExamples24](../../t-sql/queries/codesnippet/tsql/select-examples-transact_22.sql)]  
  
## <a name="n-using-the-index-optimizer-hint"></a>N. Utilizzo dell'hint di ottimizzazione INDEX  
 Nell'esempio seguente vengono illustrati due diversi utilizzi dell'hint di ottimizzazione `INDEX`. Nel primo esempio viene illustrato come forzare l'utilizzo di un indice non cluster per il recupero di righe da una tabella, mentre nel secondo viene forzata un'analisi di tabella in base all'indice 0.  
  
 [!code-sql[Select#SelectExamples45](../../t-sql/queries/codesnippet/tsql/select-examples-transact_23.sql)]  
  
## <a name="m-using-option-and-the-group-hints"></a>M. Utilizzo della clausola OPTION e degli hint GROUP  
 Nell'esempio seguente viene illustrato l'utilizzo della clausola `OPTION (GROUP)` con una clausola `GROUP BY`.  
  
 [!code-sql[Select#SelectExamples46](../../t-sql/queries/codesnippet/tsql/select-examples-transact_24.sql)]  
  
## <a name="o-using-the-union-query-hint"></a>O. Utilizzo dell'hint per la query UNION  
 Nell'esempio seguente viene utilizzato l'hint per la query `MERGE UNION`.  
  
 [!code-sql[Select#SelectExamples47](../../t-sql/queries/codesnippet/tsql/select-examples-transact_25.sql)]  
  
## <a name="p-using-a-simple-union"></a>P. Utilizzo di un semplice operatore UNION  
 Nell'esempio seguente il set di risultati include il contenuto delle colonne `ProductModelID` e `Name` di entrambe le tabelle `ProductModel` e `Gloves`.  
  
 [!code-sql[Select#SelectExamples48](../../t-sql/queries/codesnippet/tsql/select-examples-transact_26.sql)]  
  
## <a name="q-using-select-into-with-union"></a>Q. Utilizzo di SELECT INTO con UNION  
 Nell'esempio seguente la clausola `INTO` nella seconda istruzione `SELECT` specifica che la tabella `ProductResults` contiene il set di risultati finale ottenuto con l'unione delle colonne designate delle tabelle `ProductModel` e `Gloves`. Si noti che la tabella `Gloves` viene creata nella prima istruzione `SELECT`.  
  
 [!code-sql[Select#SelectExamples49](../../t-sql/queries/codesnippet/tsql/select-examples-transact_27.sql)]  
  
## <a name="r-using-union-of-two-select-statements-with-order-by"></a>R. Utilizzo dell'operatore UNION in due istruzioni SELECT con la clausola ORDER BY  
 L'ordine di alcuni parametri utilizzati con la clausola UNION è importante. Nell'esempio seguente vengono illustrati l'utilizzo errato e quello corretto di `UNION` in due istruzioni `SELECT` in cui una colonna deve essere rinominata nell'output.  
  
 [!code-sql[Select#SelectExamples50](../../t-sql/queries/codesnippet/tsql/select-examples-transact_28.sql)]  
  
## <a name="s-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>S. Utilizzo dell'operatore UNION in tre istruzioni SELECT per illustrare gli effetti dell'opzione ALL e delle parentesi  
 Negli esempi seguenti viene utilizzato l'operatore `UNION` per combinare i risultati di tre tabelle contenenti 5 righe di dati identiche. Nel primo esempio viene utilizzato `UNION ALL` per mostrare i record duplicati e vengono restituite tutte le 15 righe. Nel secondo esempio l'operatore `UNION` viene utilizzato senza l'opzione `ALL` per eliminare le righe duplicate dai risultati combinati delle tre istruzioni `SELECT` e vengono restituite 5 righe.  
  
 Nel terzo esempio viene utilizzata l'opzione `ALL` con il primo operatore `UNION` e il secondo operatore `UNION`, che non utilizza l'opzione `ALL`, viene racchiuso tra parentesi. Il secondo operatore `UNION` viene elaborato per primo in quanto è racchiuso tra parentesi e restituisce 5 righe in quanto l'opzione `ALL` è stata omessa e i duplicati vengono rimossi. Queste 5 righe vengono combinate con i risultati della prima istruzione `SELECT` mediante le parole chiave `UNION ALL`. I duplicati tra i due set di 5 righe non vengono rimossi. Il risultato finale include 10 righe.  
  
 [!code-sql[Select#SelectExamples51](../../t-sql/queries/codesnippet/tsql/select-examples-transact_29.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [PathName &#40;Transact-SQL&#41;](../../relational-databases/system-functions/pathname-transact-sql.md)   
 [Clausola INTO &#40;Transact-SQL&#41;](../../t-sql/queries/select-into-clause-transact-sql.md)  
  
  
