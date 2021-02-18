---
description: Rappresentazione di join in Progettazione query e Progettazione viste (Visual Database Tools)
title: Rappresentazione di join in Progettazione query e Progettazione visualizzazioni
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL pane [Visual Database Tools]
- joins [SQL Server], Query and View Designer
- Diagram pane [Visual Database Tools]
ms.assetid: 20a99dcb-83bd-4aa6-9139-92e2e5ba4887
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 11b6399adb28a81f5364e6073a6fb0fde757ffe7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338052"
---
# <a name="how-the-query-and-view-designer-represents-joins-visual-database-tools"></a>Rappresentazione di join in Progettazione query e Progettazione viste (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Se è stato creato un join tra le tabelle, in [Progettazione query e Progettazione viste](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) il join verrà rappresentato in forma grafica all'interno del [riquadro Diagramma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) e mediante la sintassi SQL all'interno del [riquadro SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
## <a name="diagram-pane"></a>Riquadro Diagramma  
Nel riquadro Diagramma verrà visualizzata una linea di join fra le colonne di dati coinvolte nel join. In Progettazione query e Progettazione viste verrà visualizzata una linea di join per ogni condizione di join. La seguente figura mostra, ad esempio, una linea di join fra due tabelle unite in join:  
  
![Linea di join che illustra la relazione tra le due tabelle](../../ssms/visual-db-tools/media/dv3wbig.gif "Linea di join che illustra la relazione tra le due tabelle")  
  
Se le tabelle sono unite da più condizioni di join, verranno visualizzate più linee di join, come nel seguente esempio:  
  
![Tabelle unite con più di una condizione di join](../../ssms/visual-db-tools/media/dv3w9n1.gif "Tabelle unite con più di una condizione di join")  
  
Se le colonne di dati unite in join non vengono visualizzate, ad esempio perché il rettangolo che rappresenta la tabella o l'oggetto con struttura a tabella è ridotto a icona o il join utilizza un'espressione, in Progettazione query e Progettazione viste viene inserita la linea di join nella barra del titolo del rettangolo che rappresenta la tabella o l'oggetto con struttura a tabella.  
  
La forma dell'icona al centro della linea di join indica come le tabelle o gli oggetti con struttura a tabella sono uniti in join. Se la clausola di join utilizza un operatore diverso da uguale (=), tale operatore verrà visualizzato nell'icona della linea di join. Nella seguente tabella sono elencate le icone visualizzate nella linea di join.  
  
|**Icona della linea di join**|**Descrizione**|  
|----------------------|-------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbih.gif":::|Inner join (creato con il segno di uguale).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbii.gif":::|Inner join basato sull'operatore "maggiore di".|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbij.gif":::|Outer join in cui verranno incluse tutte le righe della tabella rappresentata a sinistra, anche se non hanno alcuna corrispondenza nella tabella correlata.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbik.gif":::|Outer join in cui verranno incluse tutte le righe della tabella rappresentata a destra, anche se non hanno alcuna corrispondenza nella tabella correlata.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbil.gif":::|Full outer join in cui verranno incluse tutte le righe di entrambe le tabelle, anche se non hanno alcuna corrispondenza nella tabella correlata.|  
  
I simboli alle estremità della linea di join indicano il tipo di join. Nella seguente tabella sono elencati i tipi di join e le icone visualizzate alle estremità della linea di join.  
  
|**Icona alle estremità di una linea di join**|**Tipo di join**|  
|---------------------------------|--------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbim.gif":::|Join uno-a-uno.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbin.gif":::|Join uno-a-molti.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbio.gif":::|In Progettazione query e Progettazione viste non è possibile determinare il tipo di join. Questa situazione si verifica perlopiù con join creati manualmente.|  
  
## <a name="sql-pane"></a>riquadro SQL  
Un join può essere rappresentato in vari modi all'interno di un'istruzione SQL. La sintassi esatta dipende dal database utilizzato e dalla modalità di definizione del join.  
  
Le opzioni della sintassi per il join di tabelle comprendono:  
  
-   **Qualificatore JOIN per la clausola FROM**.   Le parole chiave INNER e OUTER specificano il tipo di join. La sintassi è quella standard per ANSI 92 SQL.  
  
    Se, ad esempio, si esegue il join delle tabelle `publishers` e `pub_info` in base alla colonna `pub_id` di ciascuna tabella, l'istruzione SQL risultante potrebbe essere simile alla seguente:  
  
    ```  
    SELECT *  
    FROM publishers INNER JOIN pub_info ON  
       publishers.pub_id = pub_info.pub_id  
    ```  
  
    Se si crea un outer join, verranno utilizzate le parole LEFT OUTER o RIGHT OUTER al posto di INNER.  
  
-   **Clausola WHERE che confronta colonne di entrambe le tabelle**.   La clausola WHERE viene utilizzata quando il database non supporta la sintassi JOIN oppure in caso di immissione manuale. Se il join viene creato nella clausola WHERE, nella clausola FROM saranno indicati i nomi di entrambe le tabelle.  
  
    Ad esempio, la seguente istruzione esegue il join delle tabelle `publishers` e `pub_info` .  
  
    ```  
    SELECT *  
    FROM publishers, pub_info  
    WHERE publishers.pub_id = pub_info.pub_id  
    ```  
  
## <a name="see-also"></a>Vedere anche  
[Eseguire query con join &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Finestra di dialogo Join &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-dialog-box-visual-database-tools.md)  
  
