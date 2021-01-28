---
title: Funzione RunningValue (Generatore report) | Microsoft Docs
description: Informazioni sulla funzione RunningValue, che restituisce un'aggregazione parziale di tutti i valori numerici non Null specificati dall'espressione in Generatore report.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 6bee2f15-0e69-49c8-9689-b04544063b1d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 75b25a7cb08a7473e6a725a841e53011c6967756
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596018"
---
# <a name="report-builder-functions---runningvalue-function"></a>Funzioni di Generatore report - Funzione RunningValue
  Restituisce un'aggregazione parziale di tutti i valori numerici non Null specificati dall'espressione, valutata per l'ambito specificato.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RunningValue(expression, function, scope)  
```  
  
#### <a name="parameters"></a>Parametri  
 *expression*  
 Espressione su cui eseguire l'aggregazione, ad esempio `[Quantity]`.  
  
 *function*  
 (**Enum**) Nome della funzione di aggregazione da applicare all'espressione, ad esempio **Sum**. Tale funzione non può essere **RunningValue**, **RowNumber** o **Aggregate**.  
  
 *ambito*  
 (**String**) Costante di tipo stringa ovvero il nome di un set di dati, area dati o gruppo oppure valore Null (**Nothing** in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]) tramite cui viene specificato il contesto in cui valutare l'aggregazione. Tramite **Nothing** viene specificato il contesto più esterno, generalmente il set di dati del report.  
  
## <a name="return-type"></a>Tipo restituito  
 Dipende dalla funzione di aggregazione specificata nel parametro *function* .  
  
## <a name="remarks"></a>Osservazioni  
 Il valore per **RunningValue** viene reimpostato su 0 per ogni nuova istanza dell'ambito. Se viene specificato un gruppo, il valore corrente viene reimpostato quando viene modificata l'espressione di raggruppamento. Se viene specificata un'area dati, il valore corrente viene reimpostato per ogni nuova istanza dell'area dati. Se viene specificato un set di dati, il valore corrente non viene reimpostato nell'intero set di dati.  
  
 **RunningValue** non può essere usato in un filtro o un'espressione di ordinamento.  
  
 Il set di dati per il quale il valore corrente è calcolato deve avere lo stesso tipo di dati. Per convertire dati con più tipi di dati numerici nello stesso tipo di dati, usare funzioni di conversione come **CInt**, **CDbl** o **CDec**. Per altre informazioni, vedere [Funzioni di conversione del tipo](/dotnet/visual-basic/language-reference/functions/type-conversion-functions).  
  
 *Scope* non può essere un'espressione.  
  
 *Expression* può contenere chiamate alle funzioni di aggregazione nidificate con le eccezioni e le condizioni seguenti:  
  
-   Scope per le aggregazioni nidificate deve corrispondere o essere contenuto nell'ambito dell'aggregazione esterna. Per tutti gli ambiti distinti nell'espressione, un ambito deve essere in una relazione figlio con tutti gli altri ambiti.  
  
-   Scope per le aggregazioni nidificate non può essere il nome di un set di dati.  
  
-   *Expression* non deve contenere funzioni **First**, **Last**, **Previous** o **RunningValue** .  
  
-   *Expression* non deve contenere aggregazioni nidificate che specificano *recursive*.  
  
 Per calcolare il valore corrente del numero di righe, usare **RowNumber**. Per altre informazioni, vedere [Funzione RowNumber &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-builder-functions-rownumber-function.md).  
  
 Per altre informazioni, vedere [Riferimento a funzioni di aggregazione &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) e [Ambito di espressioni per totali, aggregazioni e raccolte predefinite &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Per altre informazioni sulle aggregazioni ricorsive, vedere [Creazione di gruppi di gerarchie ricorsive &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
## <a name="examples"></a>Esempi  
 L'esempio di codice seguente consente di ottenere una somma parziale del campo denominato `Cost` nell'ambito più esterno, che corrisponde al set di dati.  
  
```  
=RunningValue(Fields!Cost.Value, Sum, Nothing)  
```  
  
 L'esempio di codice seguente consente di ottenere una somma parziale del campo denominato `Score` nel set di dati denominato `DataSet1`.  
  
```  
=RunningValue(Fields!Score.Value,sum,"DataSet1")  
```  
  
 L'esempio di codice seguente consente di ottenere una somma parziale del campo denominato `Traffic Charges` nell'ambito più esterno.  
  
```  
=RunningValue(Fields!Traffic Charges.Value, Sum, Nothing)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo delle espressioni nei report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Esempi di espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Tipi di dati nelle espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Ambito di espressioni per totali, aggregazioni e raccolte predefinite &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
