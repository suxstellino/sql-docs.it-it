---
description: Utilizzo dei dati multidimensionali
title: Utilizzo di dati multidimensionali | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional data [ADO]
ms.assetid: 84387746-aa3e-44fd-ad6c-a8214a6966dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 33ae3c1ca1cb8f89378b4f2ea468b41d6d3a4ac5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036621"
---
# <a name="working-with-multidimensional-data"></a>Utilizzo dei dati multidimensionali
Un *celle* è il risultato di una query sui dati multidimensionali. È costituito da una raccolta di assi, in genere non più di quattro assi e in genere solo due o tre. Un *asse* è una raccolta di membri di una o più dimensioni, utilizzata per individuare o filtrare valori specifici in un cubo.  
  
 Una *posizione* è un punto lungo un asse. Per un asse costituito da una singola dimensione, queste posizioni sono un subset dei membri della dimensione. Se un asse è costituito da più di una dimensione, ogni posizione è un'entità composta, che include *n* parti, dove *n* è il numero di dimensioni orientate lungo l'asse. Ogni parte della posizione è un membro di una dimensione costituente.  
  
 Se, ad esempio, le dimensioni Geography e Product di un cubo contenente i dati di vendita sono orientate lungo l'asse x di un insieme di celle, una posizione lungo questo asse può contenere i membri "USA" e "computer". In questo esempio, per determinare una posizione lungo l'asse x, è necessario che i membri di ogni dimensione siano orientati lungo l'asse.  
  
 Una *cella* è un oggetto posizionato in corrispondenza dell'intersezione tra le coordinate dell'asse. A ogni cella sono associate più informazioni, inclusi i dati stessi, una stringa formattata (la forma visualizzabile dei dati della cella) e il valore ordinale della cella. Ogni cella è un valore ordinale univoco nel celle. Il valore ordinale della prima cella nel celle è zero, mentre la cella più a sinistra nella seconda riga di un insieme di celle con otto colonne avrà un valore ordinale di otto.  
  
 Un cubo, ad esempio, ha le sei dimensioni seguenti (si noti che questo schema del cubo è leggermente diverso dall'esempio fornito in [Panoramica di schemi e dati multidimensionali](./overview-of-multidimensional-schemas-and-data.md)):  
  
-   Salesperson  
  
-   Geografia (gerarchia naturale)-continenti, Paesi, Stati e così via  
  
-   Trimestri, mesi, giorni  
  
-   Anni  
  
-   Measures-Sales, PercentChange, BudgetedSales  
  
-   Prodotti  
  
 Il seguente insieme di celle rappresenta le vendite per 1991 per tutti i prodotti:  
  
> [!NOTE]
>  I valori delle celle nell'esempio possono essere visualizzati come coppie ordinate di ordinali di posizione dell'asse in cui la prima cifra rappresenta la posizione dell'asse x e la seconda cifra la posizione dell'asse y.  
  
 Di seguito sono riportate le caratteristiche di questo celle:  
  
-   Dimensioni asse: trimestri, venditori, geografia  
  
-   Dimensioni filtro: misure, anni, prodotti  
  
-   Due assi: colonna (x o asse 0) e riga (y o asse 1)  
  
-   asse x: due dimensioni nidificate, venditore e geografia  
  
-   asse y: dimensione trimestri  
  
 L'asse x è costituito da due dimensioni nidificate: venditore e geografia. Da geografia sono selezionati quattro membri: Seattle, Boston, USA-Sud e Giappone. Sono stati selezionati due membri da venditore: Valentine e Nash. Viene restituito un totale di otto posizioni su questo asse (8 = 4 * 2).  
  
 Ogni coordinata è rappresentata come una posizione con due membri, uno dalla dimensione SalesPerson e l'altro dalla dimensione Geography:  
  
```console
(Valentine, Seattle), (Valentine, Boston), (Valentine, USA_North),  
(Valentine, Japan), (Nash, Seattle), (Nash, Boston), (Nash, USA_North),  
(Nash, Japan)  
```  
  
 L'asse y ha una sola dimensione, che contiene le otto posizioni seguenti:  
  
```console
Jan, Feb, Mar, Qtr2, Qtr3, Oct, Nov, Dec  
```  
  
 Celle, celle, assi e posizioni sono tutti rappresentati in ADO MD dagli oggetti corrispondenti: [celle](../../reference/ado-md-api/cellset-object-ado-md.md) [, celle,](../../reference/ado-md-api/cell-object-ado-md.md) [assi](../../reference/ado-md-api/axis-object-ado-md.md)e [posizioni](../../reference/ado-md-api/position-object-ado-md.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Modello a oggetti ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionale) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Panoramica di schemi e dati multidimensionali](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmazione con ADO MD](./programming-with-ado-md.md)   
 [Uso di ADO con ADO MD](./using-ado-with-ado-md.md)