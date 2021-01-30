---
description: Argomenti e proprietà delle stored procedure relative agli indici spaziali
title: Argomenti e proprietà delle stored procedure dell'indice spaziale | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], stored procedures
ms.assetid: ee26082b-c0ed-40ff-b5ad-f5f6b00f0475
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9faf0901643b07420f8627d6afda4b9713d034a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201754"
---
# <a name="spatial-index-stored-procedures---arguments-and-properties"></a>Stored procedure per l'indice spaziale-argomenti e proprietà
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono descritti gli argomenti e le proprietà per le stored procedure relative agli indici spaziali.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
 Per la sintassi di stored procedure relative agli indici spaziali specifiche, vedere gli argomenti seguenti:  
  
-   [sp_help_spatial_geometry_index &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="arguments"></a>Argomenti  
`[ @tabname = ] 'tabname'` Nome completo o non qualificato della tabella per la quale è stato specificato l'indice spaziale.  
  
 Le virgolette sono necessarie solo se viene specificata una tabella qualificata. Nel caso di un nome completo, ovvero contenente un nome di database, il nome del database deve corrispondere a quello del database corrente. *TabName* è di **tipo nvarchar**(776) e non prevede alcun valore predefinito.  
  
`[ @indexname = ] 'indexname'` Nome dell'indice spaziale specificato. *IndexName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @verboseoutput = ] 'verboseoutput'` Intervallo di nomi e valori di proprietà da restituire.  
  
 0 = proprietà principali  
  
 \>0 = tutte le proprietà  
  
 *verboseoutput* è di **tinyint** e non prevede alcun valore predefinito.  
  
`[ @query_sample = ] 'query_sample'` È un esempio di query rappresentativo che può essere utilizzato per verificare l'utilità dell'indice. Tale campione può essere un oggetto rappresentativo o una finestra Query. *query_sample* è di tipo **Geometry** e non prevede alcun valore predefinito.  
  
`[ @xml_output = ] 'xml_output'` Parametro di output che restituisce il set di risultati in un frammento XML. *xml_output* è di **XML** e non prevede alcun valore predefinito.  
  
## <a name="properties"></a>Proprietà  
 Impostare **\@ verboseoutput** = 0 per restituire le proprietà principali, come illustrato nella tabella seguente. **\@ verboseoutput** > 0 per restituire tutte le proprietà dell'indice spaziale.  
  
 **Base_Table_Rows**  
 Numero di righe nella tabella di base. Il valore è **bigint**.  
  
 **Bounding_Box_xmin**  
 Proprietà X-Minimum del rettangolo di delimitazione dell'indice spaziale per il tipo **Geometry** . Il valore di questa proprietà è NULL per il tipo **geography**. Il valore è **float**.  
  
 **Bounding_Box_ymin**  
 Proprietà Y-Minimum del rettangolo di delimitazione dell'indice spaziale per il tipo **Geometry** . Il valore di questa proprietà è NULL per il tipo **geography** . Il valore è **float**.  
  
 **Bounding_Box_xmax**  
 X-proprietà massime del rettangolo di delimitazione dell'indice spaziale per il tipo di **geometria** . Il valore di questa proprietà è NULL per il tipo **geography** . Il valore è **float**.  
  
 **Bounding_Box_ymax**  
 Proprietà del rettangolo di delimitazione Y-massimo dell'indice spaziale per il tipo di **geometria** . Il valore di questa proprietà è NULL per il tipo **geography** . Il valore è **float**.  
  
 **Grid_Size_Level_1**  
 Densità della griglia di livello 1 dell'indice spaziale:  
  
 16 per LOW  
  
 64 per MEDIUM  
  
 256 per HIGH  
  
 Value è di **tipo int**.  
  
 **Grid_Size_Level_2**  
 Densità della griglia di livello 2 dell'indice spaziale:  
  
 16 per LOW  
  
 64 per MEDIUM  
  
 256 per HIGH  
  
 Value è di **tipo int**.  
  
 **Grid_Size_Level_3**  
 Densità della griglia di livello 3 dell'indice spaziale:  
  
 16 per LOW  
  
 64 per MEDIUM  
  
 256 per HIGH  
  
 Value è di **tipo int**.  
  
 **Grid_Size_Level_4**  
 Densità della griglia di livello 4 dell'indice spaziale:  
  
 16 per LOW  
  
 64 per MEDIUM  
  
 256 per HIGH  
  
 Value è di **tipo int**.  
  
 **Cells_Per_Object**  
 Numero di celle per oggetto (proprietà dell'indice). Value è di **tipo int**.  
  
 **Total_Primary_Index_Rows**  
 Numero di righe nell'indice. Il valore è **bigint**.  
  
 **Total_Primary_Index_Pages**  
 Numero di pagine nell'indice. Il valore è **bigint**.  
  
 **Average_Number_Of_Index_Rows_Per_Base_Row**  
 Rapporto tra il numero di righe dell'indice e il numero di righe della tabella di base. Il valore è **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample**  
 Indica se il campione di query rappresentativo è esterno al rettangolo di delimitazione dell'indice **geometrico** e alla cella radice (cella di livello 0). Il valore può essere 0 (campione esterno alla cella di livello 0) oppure 1. Se il campione di query è interno alla cella di livello 0, l'indice controllato non è appropriato. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_In_Index**  
 Numero di istanze di celle di oggetti indicizzati tassellati nel livello 0 (cella radice, all'esterno del rettangolo di delimitazione della **geometria**). Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 Per gli indici **Geometry** , questo errore si verifica se il rettangolo di delimitazione dell'indice è inferiore al dominio dei dati. Un numero elevato di oggetti nel livello 0 può richiedere filtri secondari se la finestra di query è parzialmente esterna al rettangolo di delimitazione e diminuisce le prestazioni dell'indice (ad esempio **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample** è 1). Se la finestra Query è interna al riquadro, un numero elevato di oggetti presenti a livello 0 può migliorare effettivamente le prestazioni dell'indice.  
  
 Le istanze NULL e vuote vengono conteggiate a livello 0, ma non avranno alcun impatto sulle prestazioni. A livello 0 sarà presente un numero di celle pari a quello delle istanze NULL e vuote nella tabella di base. Per gli indici **geography** , il livello 0 avrà un numero di celle pari a quello delle istanze null e vuote + 1 cella, poiché l'esempio di query viene conteggiato come 1.  
  
 **Total_Number_Of_ObjectCells_In_Level1_In_Index**  
 Numero di istanze di celle di oggetti indicizzati tassellati con precisione di livello 1. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level2_In_Index**  
 Numero di istanze di celle di oggetti indicizzati tassellati con precisione di livello 2. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level3_In_Index**  
 Numero di istanze di celle di oggetti indicizzati tassellati con precisione di livello 3. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level4_In_Index**  
 Numero di istanze della cella di oggetti indicizzati suddivisi a mosaico con precisione di livello 4. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level1_In_Index**  
 Numero di celle completamente coperte da un oggetto a livello di mosaico 1 e pertanto interne all'oggetto. (Cell_attributevalue è 2). Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level2_In_Index**  
 Numero di celle completamente coperte da un oggetto a livello di mosaico 2 e pertanto interne all'oggetto. (Cell_attribute valore è 2). Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level3_In_Index**  
 Numero di celle completamente coperte da un oggetto a livello di mosaico 3 e pertanto interne all'oggetto. (Cell_attribute valore è 2). Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level4_In_Index**  
 Numero di celle completamente coperte da un oggetto a livello 4 del mosaico e di conseguenza interne all'oggetto. (Cell_attribute valore è 2). Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level1_In_Index**  
 Numero di celle intersecate da un oggetto a livello di mosaico 1. (Cell_attribute valore è 1.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level2_In_Index**  
 Numero di celle intersecate da un oggetto a livello di mosaico 2. (Cell_attribute valore è 1.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level3_In_Index**  
 Numero di celle intersecate da un oggetto a livello di mosaico 3. (Cell_attribute valore è 1.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level4_In_Index**  
 Numero di celle intersecate da un oggetto a livello 4 del mosaico. (Cell_attribute valore è 1.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_For_QuerySample**  
 Indica se il campione di query si trova nella cella radice 0 esterna al riquadro, ma contigua a esso. Si tratta di una proprietà principale. Il valore è **bigint**.  
  
> [!NOTE]  
>  Questa informazione è utile solo per determinare se vi sono oggetti eventualmente mancanti nel riquadro.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_In_Index**  
 Numero di oggetti a livello 0 contigui al riquadro. (Cell_attribute valore è 0.)  Il valore è **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level1_In_Index**  
 Numero di celle oggetto che toccano un limite di celle della griglia a livello 1 a mosaico. (Cell_attribute valore è 0.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level2_In_Index**  
 Numero di celle oggetto che toccano un limite di celle della griglia a livello 2 a mosaico. (Cell_attribute valore è 0.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level3_In_Index**  
 Numero di celle oggetto che toccano un limite di celle della griglia a livello 3 a mosaico. (Cell_attribute valore è 0.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level4_In_Index**  
 Numero di celle dell'oggetto contigue a un limite della cella della griglia a livello 4 del mosaico. (Cell_attribute valore è 0.) Si tratta di una proprietà di base. Il valore è **bigint**.  
  
 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Percentuale dell'area totale (celle foglia totali) della griglia in cui sono contenute celle foglia coperte da un oggetto.  
  
 Si supponga che un oggetto sia suddiviso a mosaico in 10 celle a 4 livelli della griglia diversi coprendo un'area equivalente a 100 celle foglia totali e che siano presenti 3 celle interne completamente coperte dall'oggetto. L'area coperta dalle 3 celle interne è equivalente a 42 celle foglia. Di conseguenza, la percentuale di area coperta è 42%. Tale valore rappresenta una misura efficace del livello di suddivisione degli oggetti nell'indice.  
  
 Il valore è **float**.  
  
 **Intersecting_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Come **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**, ad eccezione del fatto che queste sono celle parzialmente coperte. Il valore è **float**.  
  
 **Border_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Uguale a **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage** con la differenza che si tratta di celle del bordo. Il valore è **float**.  
  
 **Average_Cells_Per_Object_Normalized_To_Leaf_Grid**  
 Numero medio di celle per oggetto normalizzato rispetto alla griglia foglia. Indica le dimensioni spaziali dell'oggetto o le dimensioni degli oggetti. Il valore è **float**.  
  
 **Average_Objects_PerLeaf_GridCell**  
 Livello di distribuzione dell'indice. Numero medio di oggetti per cella foglia. Il valore è **float**.  
  
 **Number_Of_SRIDs_Found**  
 Numero di identificatori SRID univoci presenti nell'indice e nella colonna. Value è di **tipo int**.  
  
 Poiché una colonna può contenere più di un identificatore SRID e gli oggetti con identificatori SRID diversi non si intersecano mai, il numero di identificatori SRID indica la selettività dell'indice.  
  
 **Width_Of_Cell_In_Level1**  
 Proprietà relativa alla larghezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Width_Of_Cell_In_Level2**  
 Proprietà relativa alla larghezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Width_Of_Cell_In_Level3**  
 Proprietà relativa alla larghezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Width_Of_Cell_In_Level4**  
 Proprietà relativa alla larghezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Height_Of_Cell_In_Level1**  
 Proprietà relativa all'altezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Height_Of_Cell_In_Level2**  
 Proprietà relativa all'altezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Height_Of_Cell_In_Level3**  
 Proprietà relativa all'altezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Height_Of_Cell_In_Level4**  
 Proprietà relativa all'altezza della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Area_Of_Cell_In_Level1**  
 Proprietà relativa all'area della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Area_Of_Cell_In_Level2**  
 Proprietà relativa all'area della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Area_Of_Cell_In_Level3**  
 Proprietà relativa all'area della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **Area_Of_Cell_In_Level4**  
 Proprietà relativa all'area della cella nella griglia di indicizzazione. L'unità di misura viene specificata dall'indice e dipende dall'identificatore SRID dei dati indicizzati. Il valore è **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level1**  
 Percentuale di copertura del rettangolo di delimitazione in base a una cella di livello 1. Il valore è **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level2**  
 Percentuale di copertura del rettangolo di delimitazione in base a una cella di livello 2. Il valore è **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level3**  
 Percentuale di copertura del rettangolo di delimitazione di una cella di livello 3. Il valore è **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level4**  
 Percentuale di copertura del riquadro da una cella di livello 4. Il valore è **float**.  
  
 **Number_Of_Rows_Selected_By_Primary_Filter**  
 Numero di righe selezionate dal filtro primario. Si tratta di una proprietà principale. Il valore è **bigint.**  
  
 **Number_Of_Rows_Selected_By_Internal_Filter**  
 Numero di righe selezionate dal filtro interno. Per queste righe il filtro secondario non viene utilizzato. Si tratta di una proprietà principale. Il valore è **bigint.**  
  
 Il numero restituito è applicabile solo per **STIntersects**.  
  
 **Number_Of_Times_Secondary_Filter_Is_Called**  
 Numero di volte in cui viene utilizzato il filtro secondario. Si tratta di una proprietà principale. Il valore è **bigint.**  
  
 **Percentage_Of_Rows_NotSelected_By_Primary_Filter**  
 Se nella tabella di base sono presenti N righe e se P è il numero di righe selezionate dal filtro primario, questa proprietà restituisce il valore (N-P)/N come percentuale. Si tratta di una proprietà principale. Il valore è **float.**  
  
 **Percentage_Of_Primary_Filter_Rows_Selected_By_internal_Filter**  
 Se P è il numero di righe selezionate dal filtro primario, mentre S è quello delle righe selezionate dal filtro interno, questa proprietà restituisce il valore S/P come percentuale. Maggiore è la percentuale, migliore è il livello di efficacia dell'indice nell'evitare il filtro secondario più costoso in termini di prestazioni. Si tratta di una proprietà principale. Il valore è **float.**  
  
 **Number_Of_Rows_Output**  
 Numero di righe di output dalla query. Si tratta di una proprietà principale. Il valore è **bigint.**  
  
 **Internal_Filter_Efficiency**  
 Se O è il numero di righe di output, questa proprietà restituisce il valore S/O come percentuale. Si tratta di una proprietà principale. Il valore è **float.**  
  
 **Primary_Filter_Efficiency**  
 Se il filtro primario seleziona P righe e O è il numero di righe restituite, viene restituito come percentuale. Maggiore è l'efficienza dell'indice principale, minore è il numero di falsi positivi che il filtro secondario deve elaborare. Si tratta di una proprietà principale. Il valore è **float.**  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere un membro del ruolo **public** . È necessario disporre dell'autorizzazione READ ACCESS per il server e l'oggetto. Queste condizioni si applicano a tutte le stored procedure relative agli indici spaziali.  
  
## <a name="remarks"></a>Commenti  
 Le proprietà che contengono valori NULL non sono incluse nel set restituito.  
  
## <a name="examples"></a>Esempi  
 Per gli esempi, vedere gli argomenti seguenti:  
  
-   [sp_help_spatial_geometry_index &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="requirements"></a>Requisiti  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di indice spaziale &#40;&#41;Transact-SQL ]()   
 [sp_help_spatial_geometry_index &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Nozioni fondamentali su XQuery](../../xquery/xquery-basics.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
