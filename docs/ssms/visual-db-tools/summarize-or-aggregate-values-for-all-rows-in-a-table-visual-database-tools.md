---
description: Riepilogo o aggregazione di valori per tutte le righe di una tabella (Visual Database Tools)
title: Riepilogo o aggregazione di valori per tutte le righe di una tabella
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing query results
- aggregate functions [SQL Server], summarizing query results
ms.assetid: f5af876e-f937-4110-ba09-07999c35a699
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 88c4808e8418c248f54c9da8966306020fbb72f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350427"
---
# <a name="summarize-or-aggregate-values-for-all-rows-in-a-table-visual-database-tools"></a>Riepilogo o aggregazione di valori per tutte le righe di una tabella (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
## <a name="aggregate-function"></a>Funzione di aggregazione
Con una funzione di aggregazione è possibile creare un riepilogo di tutti i valori di una tabella. Per visualizzare, ad esempio, il prezzo totale di tutti i libri nella tabella `titles` , è possibile creare una query analoga alla seguente:  
  
```  
SELECT SUM(price)  
FROM titles  
```  
  
Creare aggregazioni multiple nella stessa query tramite le funzioni di aggregazione con più colonne. È ad esempio possibile creare una query per il calcolo del totale della colonna `price` e la media della colonna `discount` .  
  
È possibile aggregare la stessa colonna in modi diversi, calcolando ad esempio totale, conteggio e media nella stessa query. L'esempio seguente riguarda una query in cui viene calcolata la media ed eseguito il riepilogo della colonna `price` della tabella `titles` :  
  
```  
SELECT AVG(price), SUM(price)  
FROM titles  
```  
  
Se si aggiunge un criterio di ricerca sarà possibile aggregare il subset delle righe che soddisfano tale criterio.  

**Nota.** È inoltre possibile contare tutte le righe della tabella o le righe che soddisfano una specifica condizione. Per informazioni dettagliate, vedere [Conteggio delle righe di una tabella &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/count-rows-in-a-table-visual-database-tools.md).  
  
  
Quando si crea un singolo valore di aggregazione per tutte le righe di una tabella, vengono visualizzati solo i valori aggregati. Se ad esempio si calcola il valore totale della colonna `price` della tabella `titles` , non vengono visualizzati i singoli titoli, i nomi dei server di pubblicazione e così via.  
 
 **!** Quando si crea un subtotale e dunque si creano gruppi, è possibile visualizzare i valori delle colonne per ogni gruppo. Per informazioni dettagliate, vedere [Raggruppare righe nei risultati di una query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md).  

## <a name="aggregate-values-for-all-rows"></a>Aggregare i valori di tutte le righe  
  
1.  Assicurarsi che la tabella che si desidera aggregare sia già presente nel riquadro Diagramma.  
  
2.  Fare clic con il pulsante destro del mouse sullo sfondo del riquadro Diagramma e scegliere **Raggruppa** dal menu di scelta rapida. In [Progettazione query e Progettazione viste](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) verrà aggiunta una colonna **Group By** alla griglia nel riquadro Criteri.  
  
3.  Aggiungere al riquadro Criteri la colonna da aggregare. Assicurarsi che la colonna sia contrassegnata per l'output.  
  
    Verrà assegnato automaticamente un alias di colonna alla colonna di cui si effettua il riepilogo. Tale alias può essere sostituito con un alias più significativo. Per altre informazioni dettagliate, vedere [Creazione di alias di colonna &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
4.  Nella colonna della griglia **Group By** selezionare la funzione di aggregazione appropriata, ad esempio **Sum**, **Avg**, **Min**, **Max**, **Count**. Per aggregare solo le righe univoche nel set di risultati, scegliere una funzione di aggregazione con l'opzione DISTINCT, ad esempio **Min Distinct**. Non scegliere **Group By**, **Expression** o **Where**, in quanto tali opzioni non sono applicabili per l'aggregazione di tutte le righe.  
  
    Il nome della colonna nell'istruzione del [riquadro SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) verrà sostituito con la funzione di aggregazione specificata in Progettazione query e Progettazione viste. L'istruzione SQL, ad esempio, può essere analoga alla seguente:  
  
    ```  
    SELECT SUM(price)  
    FROM titles  
    ```  
  
5.  Per creare più aggregazioni in una query, ripetere i passaggi 3 e 4.  
  
    Quando si aggiunge un'altra colonna all'elenco di output della query o all'elenco di ordinamento, il termine **Group By** verrà inserito automaticamente nella colonna **Group By** della griglia. Selezionare la funzione di aggregazione desiderata.  
  
6.  Aggiungere le condizioni di ricerca eventualmente necessarie per specificare il subset di righe da riepilogare.  
  
Quando si esegue la query, nel riquadro Risultati verranno visualizzate le aggregazioni specificate.  
  
> [!NOTE]  
> In Progettazione query e Progettazione viste le funzioni di aggregazione vengono mantenute nell'istruzione SQL nel riquadro SQL fino a quando non viene disabilitata esplicitamente la modalità di raggruppamento. Se pertanto si modifica la query cambiandone il tipo o cambiando le tabelle o gli oggetti con valori di tabella presenti nel riquadro Diagramma, la query risultante potrebbe includere funzioni di aggregazione non valide.  
  
## <a name="see-also"></a>Vedere anche  
[Ordinare e raggruppare i risultati delle query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Creare un riepilogo dei risultati di query &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  
