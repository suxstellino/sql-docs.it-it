---
description: Creazione di query mediante l'utilizzo di altre origini oltre a una tabella (Visual Database Tools)
title: Creare query mediante l'uso di altre origini oltre a una tabella
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350452"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>Creazione di query mediante l'utilizzo di altre origini oltre a una tabella (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Quando si crea una query di recupero dati, si definiscono le colonne e le righe da estrarre e la posizione dei dati originali. I dati originali in genere sono costituiti da una tabella o da più tabelle unite in join, ma possono anche provenire da origini diverse dalle tabelle, quali viste, query, sinonimi o funzioni definite dall'utente che restituiscono una tabella.  
  
## <a name="using-a-view-in-place-of-a-table"></a>Utilizzo di una vista al posto di una tabella  
È possibile selezionare le righe di una vista. Si supponga ad esempio che il database includa una vista denominata "ExpensiveBooks", nella quale ogni riga descrive un libro il cui prezzo supera i 19,99 dollari. La definizione della vista potrebbe essere simile alla seguente:  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
È possibile selezionare i libri di psicologia più costosi semplicemente selezionando i libri di psicologia nella vista ExpensiveBooks. Il codice SQL risultante potrebbe essere simile al seguente:  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
Analogamente, una vista può essere utilizzata in un'operazione di JOIN. È possibile, ad esempio, trovare le vendite dei libri più costosi semplicemente eseguendo il join della tabella sales con la vista ExpensiveBooks. Il codice SQL risultante potrebbe essere simile al seguente:  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
Per altre informazioni sull'aggiunta di una vista a una query, vedere [Aggiunta di tabelle a query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-query-in-place-of-a-table"></a>Utilizzo di una query al posto di una tabella  
È possibile selezionare le righe di una query. Si supponga, ad esempio, di avere già scritto una query che recupera i titoli e gli identificatori dei libri scritti da più autori. Il codice SQL potrebbe essere simile al seguente:  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
È quindi possibile scrivere un'altra query che sfrutta questo risultato. Ad esempio, è possibile scrivere una query che recupera i libri di psicologia scritti da più autori. Per scrivere questa nuova query, si può utilizzare la query esistente come origine dei dati della nuova query. Il codice SQL risultante potrebbe essere simile al seguente:  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
Il testo evidenziato indica la query esistente utilizzata come origine dei dati della nuova query. Si noti che la nuova query utilizza un alias ("co_authored_books") per la query esistente. Per altre informazioni sugli alias, vedere [Creazione di alias di tabella &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) e [Creazione di alias di colonna &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
Analogamente, una query può essere utilizzata in un'operazione di JOIN. È possibile, ad esempio, trovare le vendite dei libri più costosi scritti da più autori semplicemente eseguendo il join della vista ExpensiveBooks con la query che recupera i libri scritti da più autori. Il codice SQL risultante potrebbe essere simile al seguente:  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
Per altre informazioni sull'aggiunta di una tabella a una query, vedere [Aggiunta di tabelle a query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>Utilizzo di una funzione definita dall'utente al posto di una tabella  
In SQL Server 2000 o versione successiva è possibile creare una funzione definita dall'utente che restituisca una tabella. Tali funzioni risultano utili per l'esecuzione di logiche procedurali o complesse.  
  
Si supponga ad esempio che la tabella dei dipendenti contenga un'ulteriore colonna, employee.manager_emp_id, e che una chiave esterna di manager_emp_id sia presente in employee.emp_id. All'interno di ciascuna riga della tabella dei dipendenti, la colonna manager_emp_id indica il superiore di un dipendente o più precisamente, indica l'emp_id del superiore del dipendente. È possibile creare una funzione definita dall'utente che restituisca una tabella contenente una riga per ciascun dipendente facente parte della gerarchia organizzativa di un particolare responsabile di alto livello. La funzione potrebbe essere denominata fn_GetWholeTeam e progettata in modo da accettare una variabile di input, ovvero l'emp_id del responsabile di cui si vuole recuperare il team.  
  
È possibile scrivere una query che utilizzi la funzione fn_GetWholeTeam come origine dei dati. Il codice SQL risultante potrebbe essere simile al seguente:  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
"VPA30890F" è l'emp_id del responsabile di cui si desidera recuperare l'organizzazione. Per altre informazioni sull'aggiunta di una funzione definita dall'utente a una query, vedere [Aggiunta di tabelle a query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md). Per una descrizione completa delle funzioni definite dall'utente, vedere [Funzioni definite dall'utente](../../relational-databases/user-defined-functions/user-defined-functions.md).  
