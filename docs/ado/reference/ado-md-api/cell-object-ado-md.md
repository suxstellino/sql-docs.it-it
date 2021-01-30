---
description: Oggetto Cell (ADO MD)
title: Oggetto Cell (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell
helpviewer_keywords:
- Cell object [ADO MD]
ms.assetid: dcc2f044-b785-4a29-9bc5-b673f66eedf9
author: rothja
ms.author: jroth
ms.openlocfilehash: c1fd0cdb2322b3cad507745a97faa6b74f245b40
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169955"
---
# <a name="cell-object-ado-md"></a>Oggetto Cell (ADO MD)
Rappresenta i dati all'intersezione delle coordinate dell'asse contenute in un oggetto Cell.  
  
## <a name="remarks"></a>Commenti  
 Un oggetto **cella** viene restituito dalla proprietà [Item](./item-property-ado-md-cellset.md) di un oggetto [cellt](./cellset-object-ado-md.md) .  
  
 Con le raccolte e le proprietà di un oggetto **cella** , è possibile eseguire le operazioni seguenti:  
  
-   Restituire i dati nella **cella** con la proprietà [value](./value-property-ado-md.md) .  
  
-   Restituisce la stringa che rappresenta la visualizzazione formattata della proprietà **value** con la proprietà [formattedValue](./formattedvalue-property-ado-md.md) .  
  
-   Restituisce il valore ordinale della **cella** all'interno del **cella con la** proprietà [ordinale](./ordinal-property-ado-md-cell.md) .  
  
-   Determinare la posizione della **cella** all'interno di [CubeDef](./cubedef-object-ado-md.md) con la raccolta [Positions](./positions-collection-ado-md.md) .  
  
-   Recuperare altre informazioni sulla **cella** con la raccolta delle [Proprietà](../ado-api/properties-collection-ado.md) ADO standard.  
  
 La raccolta **Properties** contiene proprietà fornite dal provider. Nella tabella seguente sono elencate le proprietà che potrebbero essere disponibili. L'elenco di proprietà effettivo può variare a seconda dell'implementazione del provider. Per un elenco più completo delle proprietà disponibili, vedere la documentazione relativa al provider.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|ColoreSfondo|Colore di sfondo utilizzato per la visualizzazione della cella.|  
|FontFlags|Maschera di maschera che illustra in dettaglio gli effetti sul tipo di carattere.|  
|FontName|Tipo di carattere utilizzato per visualizzare il valore della cella.|  
|FontSize|Dimensioni del carattere utilizzate per visualizzare il valore della cella.|  
|ColorePrimoPiano|Colore di primo piano utilizzato per la visualizzazione della cella.|  
|FormatString|Valore in una stringa formattata.|  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./cell-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di asse (VBScript)](./axis-example-vbscript.md)   
 [Oggetto cellt (ADO MD)](./cellset-object-ado-md.md)   
 [Raccolta Positions (ADO MD)](./positions-collection-ado-md.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)