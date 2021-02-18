---
description: Definizione di più condizioni di ricerca per una sola colonna (Visual Database Tools)
title: Definire più condizioni di ricerca per una sola colonna
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], multiple conditions
- multiple search conditions
- search conditions [SQL Server], multiple
- OR operator
- AND, Criteria pane
ms.assetid: 2c006e36-56b1-4992-89b4-c6c0b19808f3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ddb90eb0beaabac86e8d422fac24fa3cf8c66168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346068"
---
# <a name="specify-multiple-search-conditions-for-one-column-visual-database-tools"></a>Definizione di più condizioni di ricerca per una sola colonna (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In alcuni casi può essere necessario applicare diverse condizioni di ricerca a una stessa colonna di dati. Può, ad esempio, essere necessario:  
  
-   Cercare diversi nomi o dipendenti con diverse fasce di stipendio in una tabella `employee` . Questo tipo di ricerca richiede una condizione OR.  
  
-   Cercare il titolo di un libro che inizi con la parola "Il" e contenga la parola "cuoco". Questo tipo di ricerca richiede una condizione AND.  
  
> [!NOTE]  
> Le informazioni contenute in questo argomento sono valide per le condizioni di ricerca nelle clausole WHERE e HAVING di una query. Gli esempi sono incentrati sulla creazione di clausole WHERE, ma i principi sono validi per entrambi i tipi di condizione di ricerca.  
  
Per cercare valori alternativi nella stessa colonna di dati, specificare una condizione OR. Per cercare valori che soddisfino più condizioni, specificare una condizione AND.  
  
## <a name="specifying-an-or-condition"></a>Specifica di una condizione OR  
La condizione OR consente di specificare diversi valori alternativi da cercare in una colonna. Questa opzione amplia l'ambito della ricerca e può restituire più righe rispetto alla ricerca di un valore singolo.  
  
> [!TIP]  
> In alternativa, è spesso possibile utilizzare l'operatore IN per cercare più valori in una stessa colonna di dati.  
  
#### <a name="to-specify-an-or-condition"></a>Per specificare una condizione OR  
  
1.  Nel [riquadro Criteri](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)aggiungere la colonna da includere nella ricerca.  
  
2.  Nella colonna **Filtro** per la colonna di dati appena aggiunta specificare la prima condizione.  
  
3.  Nella colonna **Or...** per la stessa colonna di dati specificare la seconda condizione.  
  
In Progettazione query e Progettazione viste viene creata una clausola WHERE contenente una condizione OR analoga alla seguente:  
  
```  
SELECT fname, lname  
FROM employees  
WHERE (salary < 30000) OR (salary > 100000)  
```  
  
## <a name="specifying-an-and-condition"></a>Specifica di una condizione AND  
La condizione AND consente di specificare che i valori in una colonna devono soddisfare due o più condizioni affinché la riga venga inclusa nel set di risultati. Questa opzione restringe l'ambito della ricerca e in genere restituisce meno righe rispetto alla ricerca di un valore singolo.  
  
> [!TIP]  
> Se si cerca un intervallo di valori, sarà possibile utilizzare l'operatore BETWEEN anziché collegare due condizioni con AND.  
  
#### <a name="to-specify-an-and-condition"></a>Per specificare una condizione AND  
  
1.  Nel riquadro Criteri aggiungere la colonna da includere nella ricerca.  
  
2.  Nella colonna **Filtro** per la colonna di dati appena aggiunta specificare la prima condizione.  
  
3.  Aggiungere nuovamente la stessa colonna di dati al riquadro Criteri, collocandola in una riga vuota della griglia.  
  
4.  Nella colonna **Filtro** per la seconda istanza della colonna di dati specificare la seconda condizione.  
  
In Progettazione query viene creata una clausola WHERE contenente una condizione AND analoga alla seguente:  
  
```  
SELECT title_id, title  
FROM titles  
WHERE (title LIKE '%Cook%') AND   
  (title LIKE '%Recipe%')  
```  
  
## <a name="see-also"></a>Vedere anche  
[Convenzioni per la combinazione delle condizioni di ricerca nel riquadro Criteri &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[Specifica dei criteri di ricerca &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
