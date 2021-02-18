---
description: Convenzioni per la combinazione delle condizioni di ricerca nel riquadro Criteri (Visual Database Tools)
title: Convenzioni per la combinazione delle condizioni di ricerca nel riquadro Criteri
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355198"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>Convenzioni per la combinazione delle condizioni di ricerca nel riquadro Criteri (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È possibile creare query con qualsiasi numero di condizioni di ricerca, collegate con qualsiasi numero di operatori AND e OR. Poiché una query con una combinazione di clausole AND e OR può diventare complessa, è utile capire come viene interpretata durante la sua esecuzione e come viene rappresentata nel [riquadro Criteri](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) e nel [riquadro SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
> [!NOTE]  
> Per altre informazioni sulle condizioni di ricerca che contengono solo un operatore AND o OR, vedere [Definizione di più condizioni di ricerca per una sola colonna &#40;Visual Database Tools &#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) e [Definizione di più condizioni di ricerca per più colonne &#40;Visual Database Tools &#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md).  
  
Più avanti sarà possibile reperire informazioni relative a:  
  
-   Precedenza di AND e OR nelle query in cui sono presenti entrambi.  
  
-   Modo in cui le condizioni nelle clausole AND e OR stabiliscono una relazione logica reciproca.  
  
-   Modo in cui le query che contengono AND e OR vengono rappresentate in Progettazione query e Progettazione viste nel riquadro Criteri.  
  
Per semplificare la comprensione degli argomenti illustrati di seguito, si supponga di utilizzare una tabella `employee` che contiene le colonne `hire_date`, `job_lvl`e `status`. Negli esempi si ipotizza la necessità di ottenere informazioni sull'anzianità di servizio dei dipendenti (ovvero la data di assunzione di un dipendente), sul tipo di mansione svolta da un dipendente (il livello dell'attività) e sullo stato del dipendente (ad esempio se è in pensione).  
  
## <a name="precedence-of-and-and-or"></a>Precedenza di AND e OR  
Quando viene eseguita una query, in primo luogo vengono valutate le clausole collegate con l'operatore AND e quindi quelle collegate con OR.  
  
> [!NOTE]  
> L'operatore NOT ha la precedenza sia rispetto a AND che rispetto a OR.  
  
Per trovare, ad esempio, i dipendenti assunti da oltre cinque anni in attività di basso livello o dipendenti con un livello di attività intermedio indipendentemente dalla data di assunzione, è possibile costruire una clausola WHERE come la seguente:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
Per modificare la precedenza predefinita dell'operatore AND rispetto all'operatore OR, è possibile delimitare con parentesi specifiche condizioni nel riquadro SQL. Le condizioni fra parentesi vengono sempre valutate per prime. Per trovare, ad esempio, tutti i dipendenti che appartengono a una società da oltre cinque anni con livello di attività basso o intermedio, è possibile costruire una clausola WHERE come la seguente:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> Per questioni di chiarezza, è sempre consigliabile inserire parentesi quando si combinano clausole AND e OR invece di fare semplicemente affidamento sulla precedenza predefinita.  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>Funzionamento di AND con più clausole OR  
La comprensione dei modi di correlazione delle clausole AND e OR combinate può risultare di aiuto nella costruzione e comprensione di query complesse in Progettazione query e Progettazione viste.  
  
Se vengono collegate più condizioni con AND, il primo set di condizioni collegato con AND viene applicato a tutte le condizioni nel secondo set. In altri termini, una condizione collegata con AND a un'altra condizione viene distribuita a tutte le condizioni nel secondo set. La seguente rappresentazione schematica mostra, ad esempio, una condizione AND collegata a un set di condizioni OR:  
  
```  
A AND (B OR C)  
```  
  
La precedente rappresentazione è logicamente equivalente alla seguente rappresentazione schematica, che mostra come la condizione AND viene distribuita nel secondo set di condizioni:  
  
```  
(A AND B) OR (A AND C)  
```  
  
Questo principio di distribuzione influisce sull'utilizzo di Progettazione query e Progettazione viste. Si supponga, ad esempio, di dover cercare tutti i dipendenti assunti da oltre cinque anni con livello di attività basso o intermedio. È possibile immettere la seguente clausola WHERE nell'istruzione nel riquadro SQL:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
La clausola collegata con AND si applica a entrambe le clausole collegate con OR. Un modo esplicito per esprimere tale istruzione consiste nel ripetere la condizione AND per ogni condizione nella clausola OR. La seguente istruzione è più esplicita (e lunga) rispetto all'istruzione precedente, ma è logicamente equivalente:  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
Il principio di distribuzione delle clausole AND alle clausole OR collegate si applica indipendentemente dal numero di condizioni interessate. Si supponga, ad esempio, di voler trovare i dipendenti di livello superiore o intermedio assunti da oltre cinque anni o che sono in pensione. La clausola WHERE potrebbe essere analoga alla seguente:  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
Una volta distribuite le condizioni collegate con AND, la clausola WHERE assumerà questo aspetto:  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>Rappresentazione di più clausole AND e OR nel riquadro Criteri  
Le condizioni di ricerca vengono rappresentate nel [riquadro Criteri](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)di Progettazione query e Progettazione viste. In alcuni casi che riguardano più clausole collegate con AND e OR tuttavia la rappresentazione nel riquadro Criteri potrebbe risultare diversa da quella desiderata. Inoltre, se si modifica una query nel riquadro Criteri o nel [riquadro Diagramma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md), si potrebbe notare che l'istruzione SQL è diversa rispetto a quella specificata.  
  
In generale, queste regole determinano il modo in cui le clausole AND e OR vengono visualizzate nel riquadro Criteri:  
  
-   Tutte le condizioni collegate con AND vengono visualizzate nella colonna **Filtro** della griglia o nella stessa colonna **Or...** .  
  
-   Tutte le condizioni collegate con OR vengono visualizzate in colonne **Or...** separate.  
  
-   Se il risultato logico di una combinazione di clausole AND e OR è che la clausola AND viene distribuita in più clausole OR, nel riquadro Criteri questa situazione viene rappresentata in modo esplicito ripetendo la clausola AND il numero di volte necessario.  
  
Nel riquadro SQL si potrebbe creare, ad esempio, una condizione di ricerca come la seguente, in cui due clausole collegate con AND hanno la precedenza rispetto alla terza collegata con OR:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
In Progettazione query e Progettazione viste questa clausola WHERE viene rappresentata nel riquadro Criteri come indicato di seguito:  
  
![Precedenza della clausola OR nel riquadro Criteri](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "Precedenza della clausola OR nel riquadro Criteri")  
  
Tuttavia, se la clausola OR collegata ha la precedenza rispetto a una clausola AND, la clausola AND viene ripetuta per ogni clausola OR. In questo modo la clausola AND viene distribuita per ogni clausola OR. Nel riquadro SQL si potrebbe creare ad esempio una clausola WHERE come la seguente:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
In Progettazione query e Progettazione viste questa clausola WHERE viene rappresentata nel riquadro Criteri come indicato di seguito:  
  
![Più clausole AND e OR nel riquadro Criteri](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "Più clausole AND e OR nel riquadro Criteri")  
  
Se le clausole OR collegate interessano soltanto una colonna di dati, Progettazione query e Progettazione viste consentono di inserire l'intera clausola OR in una singola cella della griglia, eliminando la necessità di ripetere la clausola AND. Nel riquadro SQL si potrebbe creare ad esempio una clausola WHERE come la seguente:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
In Progettazione query e Progettazione viste questa clausola WHERE viene rappresentata nel riquadro Criteri come indicato di seguito:  
  
![Clausole OR collegate definite nel riquadro dei criteri](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "Clausole OR collegate definite nel riquadro dei criteri")  
  
Se si apporta una modifica alla query, ad esempio modificando uno dei valori nel riquadro Criteri, in Progettazione query e Progettazione viste l'istruzione SQL verrà ricreata nel riquadro SQL. L'istruzione SQL ricreata sarà analoga alla visualizzazione del riquadro Criteri invece che all'istruzione originaria. Se ad esempio il riquadro Criteri contiene clausole AND distribuite, l'istruzione risultante nel riquadro SQL verrà ricreata con clausole AND distribuite esplicitamente. Per ulteriori informazioni, tornare a "Funzionamento di AND con più clausole OR" in questo argomento.  
  
## <a name="see-also"></a>Vedere anche  
[Specifica dei criteri di ricerca &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
