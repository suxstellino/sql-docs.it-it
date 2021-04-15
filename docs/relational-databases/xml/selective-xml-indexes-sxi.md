---
title: Indici XML selettivi (SXI) | Microsoft Docs
description: Informazioni su come usare indici XML selettivi (SXI) per migliorare le prestazioni delle query, supportare l'indicizzazione più veloce e ridurre i costi di archiviazione degli indici XML.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 598ecdcd-084b-4032-81b2-eed6ae9f5d44
author: rothja
ms.author: jroth
ms.openlocfilehash: c8b0fd1e26def618b9645f9de65dfe8a255892f5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487528"
---
# <a name="selective-xml-indexes-sxi"></a>Indici XML selettivi
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Gli indici XML selettivi rappresentano un altro tipo di indice XML disponibile oltre agli indici XML comuni. Di seguito sono indicati gli obiettivi della funzionalità degli indici XML selettivi.  
  
-   Migliorare le prestazioni delle query sui dati XML archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Supportare un'indicizzazione più rapida di carichi di lavoro di dati XML di grandi dimensioni.  
  
-   Migliorare la scalabilità riducendo i costi di archiviazione degli indici XML.  
  
 La limitazione principale negli indici XML più comuni è rappresentata dal fatto che viene eseguita l'indicizzazione dell'intero documento XML. Questa condizione determina significativi svantaggi, tra cui prestazioni ridotte per le query e costi di manutenzione dell'indice più elevati, in particolar modo per quanto riguarda i costi di archiviazione.  
  
 La funzionalità degli indici XML selettivi consente di promuovere solo determinati percorsi dai documenti XML da indicizzare. In fase di creazione dell'indice, questi percorsi vengono sottoposti a valutazione e i nodi a cui puntano vengono suddivisi e archiviati in una tabella relazionale in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questa funzionalità viene utilizzato un efficiente algoritmo di mapping sviluppato da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Research in collaborazione con il team di prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Con questo algoritmo i nodi XML vengono mappati a una singola tabella relazionale, garantendo prestazioni eccezionali senza richiedere uno spazio di archiviazione di dimensioni eccessive.  
  
 La funzionalità degli indici XML selettivi supporta inoltre indici XML selettivi secondari nei nodi indicizzati da un indice XML selettivo. Questi indici selettivi secondari risultano particolarmente efficaci e implicano un miglioramento delle prestazioni delle query.  
  
##  <a name="benefits-of-selective-xml-indexes"></a><a name="benefits"></a> Vantaggi degli indici XML selettivi  
 Di seguito sono indicati i vantaggi offerti dagli indici XML selettivi.  
  
1.  Prestazioni delle query notevolmente migliorate sul tipo di dati XML per carichi di query tipici.  
  
2.  Requisiti di archiviazione ridotti rispetto agli indici XML comuni.  
  
3.  Costi di manutenzione dell'indice ridotti rispetto agli indici XML comuni.  
  
4.  Nessuna necessità di aggiornare le applicazioni per ricavare vantaggi dagli indici XML selettivi.  
  
  
##  <a name="selective-xml-indexes-and-primary-xml-indexes"></a><a name="compare"></a> Indici XML selettivi e indici XML primari  
  
> [!IMPORTANT]  
>  Creare un indice XML selettivo anziché un indice XML comune nella maggior parte dei casi per migliorare le prestazioni e usufruire di uno spazio di archiviazione più efficiente.  
  
 Non è tuttavia consigliabile creare un indice XML selettivo in presenza di una delle condizioni indicate di seguito.  
  
-   Viene eseguito il mapping di un numero elevato di percorsi del nodo.  
  
-   Vengono supportate le query per elementi sconosciuti o in una posizione sconosciuta nella struttura del documento.  
  
  
##  <a name="simple-example-of-a-selective-xml-index"></a><a name="example"></a> Semplice esempio di un indice XML selettivo  
 Considerare il frammento XML indicato di seguito come un documento XML in una tabella di circa 500.000 righe:  
  
```xml  
<book>  
    <created>2004-03-01</created>   
    <authors>Various</authors>  
    <subjects>  
        <subject>English wit and humor -- Periodicals</subject>  
        <subject>AP</subject>   
    </subjects>  
    <title>Punch, or the London Charivari, Volume 156, April 2, 1919</title>  
    <id>etext11617</id>  
</book>  
```  
  
 Creare un indice XML primario per un numero così elevato di righe in questo semplice schema richiede molto tempo. L'esecuzione di query su questi dati risente inoltre del fatto che un indice XML primario non supporta l'indicizzazione selettiva.  
  
 Se è necessario esclusivamente eseguire una query su questi dati per il percorso `/book/title` e il percorso `/book/subjects` , è possibile creare l'indice XML selettivo seguente:  
  
```sql  
CREATE SELECTIVE XML INDEX SXI_index  
ON Tbl(xmlcol)  
FOR   
(  
    pathTitle = '/book/title/text()' AS XQUERY 'xs:string',   
    pathAuthors = '/book/authors' AS XQUERY 'node()',  
    pathId = '/book/id' AS SQL NVARCHAR(100)  
)  
```  
  
 L'istruzione precedente rappresenta un esempio calzante della sintassi CREATE utilizzata per la creazione di un indice XML selettivo. Nell'istruzione CREATE è necessario in primo luogo fornire un nome per l'indice e identificare la tabella e la colonna XML da indicizzare. È quindi necessario specificare i percorsi per l'indice. Un percorso include tre parti:  
  
1.  Un nome che identifica in modo univoco il percorso.  
  
2.  Un'espressione XQuery che descrive il percorso.  
  
3.  Hint di ottimizzazione facoltativi.  
  
 Per ulteriori informazioni su questi elementi, vedere [Attività correlate](#reltasks).  
  
  
## <a name="supported-features-prerequisites-and-limitations"></a>Funzionalità supportate, prerequisiti e limitazioni  
  
###  <a name="supported-xml-features"></a><a name="features"></a> Funzionalità XML supportate  
 Gli indici XML selettivi supportano l'espressione XQuery, a sua volta supportata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nei metodi exist(), value() e nodes().  
  
-   Per i metodi exist(), value() e nodes(), gli indici XML selettivi contengono informazioni sufficienti a trasformare l'intera espressione.  
  
-   Per i metodi query() e modify(), gli indici XML selettivi possono essere utilizzati esclusivamente per il filtraggio dei nodi.  
  
-   Per il metodo query(), gli indici XML selettivi non vengono utilizzati per recuperare i risultati.  
  
-   Per il metodo modify(), gli indici XML selettivi non vengono utilizzati per aggiornare i documenti XML.  
  
  
###  <a name="unsupported-xml-features"></a><a name="unsupported"></a> Funzionalità XML non supportate  
 Gli indici XML selettivi non supportano le seguenti funzionalità supportate nell'implementazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di XML:  
  
-   Indicizzazione dei nodi con tipi XS complessi: tipi unione, tipi sequenza e tipi elenco.  
  
-   Indicizzazione di nodi con tipi XS binari: ad esempio, base64Binary e hexBinary.  
  
-   Specifica dei nodi da indicizzare con espressioni XPath contenenti il carattere jolly `*` come elemento finale: ad esempio,  `/a/b/c/*`, `/a//b/*`o `/a/b/*:c`.  
  
-   Indicizzazione di un asse diverso da figlio, attributo o discendente. Il case `//<step>` è consentito come case speciale.  
  
-   Indicizzazione di istruzioni di elaborazione XML e commenti.  
  
-   Specifica e recupero dell'identificatore di un nodo mediante la funzione id().  
  
  
###  <a name="prerequisites"></a><a name="prereq"></a> Prerequisiti  
 Sono necessari i prerequisiti indicati di seguito per poter creare un indice XML selettivo per una colonna XML in una tabella utente:  
  
-   È necessario che esista un indice cluster sulla chiave primaria della tabella utente.  
  
-   La chiave primaria della tabella utente è limitata a una dimensione di 128 byte quando viene utilizzata con gli indici XML selettivi.  
  
-   La chiave di clustering della tabella utente è limitata a 15 colonne quando viene utilizzata con gli indici XML selettivi.  
  
  
###  <a name="limitations"></a><a name="limits"></a> Limitazioni  
 **Requisiti e limitazioni generali**  
  
-   Ogni indice XML selettivo deve necessariamente essere creato su una singola colonna XML.  
  
-   Non è possibile creare un indice XML selettivo su una colonna non di tipo XML.  
  
-   Ogni colonna XML in una tabella può includere solo un indice XML selettivo.  
  
-   Per ogni tabella è possibile creare al massimo 249 indici XML selettivi.  
  
 **Limitazioni relative a oggetti supportati**  
  
 Non è possibile creare indici XML selettivi per gli oggetti seguenti:  
  
1.  Colonne XML in una vista.  
  
2.  Variabile con valori di tabella con colonne XML.  
  
3.  Variabili di tipo XML.  
  
4.  Colonne XML calcolate.  
  
5.  Colonne XML con una profondità superiore a 128 nodi nidificati.  
  
 **Limitazioni relative all'archiviazione**  
  
 Esiste un limite finito al numero di nodi di un documento XML che è possibile aggiungere all'indice. Un indice XML selettivo esegue il mapping di documenti XML a una singola tabella relazionale. Pertanto non possono essere presenti più di 1.024 colonne non Null in una determinata riga della tabella. Inoltre, gran parte delle limitazioni delle colonne di tipo sparse vengono applicate anche agli indici XML selettivi, poiché negli indici vengono utilizzate colonne di tipo sparse per l'archiviazione.  
  
 Il numero massimo di colonne non Null supportate in qualsiasi riga specificata dipende dalle dimensioni dei dati nelle colonne:  
  
-   Nel migliore dei casi sono supportate 1024 colonne non Null se tutte le colonne sono di tipo **bit**.  
  
-   Nel peggiore dei casi sono supportate solo 236 colonne non Null se tutte le colonne sono oggetti di grandi dimensioni di tipo **varchar**.  
  
 Negli indici XML selettivi vengono utilizzate da una a quattro colonne a livello interno per ogni percorso del nodo indicizzato. Il numero totale di nodi che è possibile indicizzare varia da 60 a diverse centinaia, a seconda della dimensione effettiva dei dati nei percorsi indicizzati.  
  
-   Nel peggiore dei casi, se qualcuno o tutti i nodi vengono mappati mediante `//` nella definizione del percorso del nodo, il numero massimo di nodi indicizzati è 60.  
  
-   Nel migliore dei casi, se i nodi vengono mappati senza utilizzare `//` nella definizione del percorso del nodo, il numero massimo di nodi indicizzati è 200.  
  
 **Gli indici XML selettivi vengono ricompilati se si esegue un'istruzione CREATE o ALTER per l'indice.**  
  
 Quando si esegue un'istruzione CREATE o ALTER per un indice XML selettivo, quest'ultimo viene ricompilato in modalità offline a thread singolo. Le istruzioni ALTER hanno in genere un impatto negativo sulle prestazioni delle query sui documenti XML indicizzati.  
  
 **Altre limitazioni**  
  
-   Gli indici XML selettivi non sono supportati negli hint per la query.  
  
-   Gli indici XML selettivi e gli indici XML selettivi secondari non sono supportati nell'ottimizzazione guidata motore di database.  
  
  
##  <a name="related-tasks"></a><a name="reltasks"></a> Attività correlate  
  
| Attività | Argomento |
| ---- | ----- |
|Specificare i percorsi del nodo che si desidera indicizzare e gli hint di ottimizzazione facoltativi quando si crea o si modifica un indice XML selettivo.|[Specificare percorsi e hint di ottimizzazione per indici XML selettivi](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)|  
|Creare, modificare o eliminare un indice XML selettivo.|[Creare, modificare o eliminare indici XML selettivi](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)|  
|Creare, modificare o eliminare un indice XML selettivo secondario.|[Creare, modificare o eliminare indici XML selettivi secondari](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)|  
  
  
  
