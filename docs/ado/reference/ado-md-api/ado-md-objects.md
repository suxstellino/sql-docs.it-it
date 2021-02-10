---
description: Oggetti ADO MD
title: Oggetti ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO MD, objects
- objects [ADO MD]
ms.assetid: 2a32e873-3282-4520-a7ed-89493f1da80e
author: rothja
ms.author: jroth
ms.openlocfilehash: 489228bb44322c6db39c3ced8b1854749c36deeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055856"
---
# <a name="ado-md-objects"></a>Oggetti ADO MD

|Oggetto|Descrizione|  
|-|-|  
|[Asse](./axis-object-ado-md.md)|Rappresenta un asse posizionale o di filtro di un oggetto Cell, contenente i membri selezionati di una o più dimensioni.|  
|[Catalogo](./catalog-object-ado-md.md)|Contiene informazioni di schema multidimensionali, ovvero cubi e dimensioni sottostanti, gerarchie, livelli e membri, specifiche di un provider di dati multidimensionali (MDP).|  
|[Cell (Cella)](./cell-object-ado-md.md)|Rappresenta i dati in corrispondenza dell'intersezione tra le coordinate dell'asse, contenute in un oggetto Cell.|  
|[Celle](./cellset-object-ado-md.md)|Rappresenta i risultati di una query multidimensionale. Si tratta di una raccolta di celle selezionate da cubi o altri celle.|  
|[CubeDef](./cubedef-object-ado-md.md)|Rappresenta un cubo da uno schema multidimensionale, contenente un set di dimensioni correlate.|  
|[Dimensione](./dimension-object-ado-md.md)|Rappresenta una delle dimensioni di un cubo multidimensionale che contiene una o più gerarchie di membri.|  
|[Gerarchia](./hierarchy-object-ado-md.md)|Rappresenta un modo in cui i membri di una dimensione possono essere aggregati o sottoposti a rollup. Una dimensione può essere aggregata in una o più gerarchie.|  
|[Level](./level-object-ado-md.md)|Contiene un set di membri, ognuno dei quali ha lo stesso rango all'interno di una gerarchia.|  
|[Membro](./member-object-ado-md.md)|Rappresenta un membro di un livello in un cubo, gli elementi figlio di un membro di un livello o un membro di una posizione lungo un asse di un insieme di celle.|  
|[Position](./position-object-ado-md.md)|Rappresenta un set di uno o più membri di dimensioni diverse che definiscono un punto lungo un asse.|  
  
 Inoltre, l'oggetto **Catalogo** è connesso a un oggetto **connessione** ADO, incluso nella libreria ADO standard:  
  
|Oggetto|Descrizione|  
|------------|-----------------|  
|[Connection](../ado-api/connection-object-ado.md)|Rappresenta una connessione aperta a un'origine dati.|  
  
 Le relazioni tra questi oggetti sono illustrate nel [modello a oggetti ADO MD](./ado-md-object-model.md).  
  
 Molti oggetti ADO MD possono essere contenuti in una raccolta corrispondente. Un oggetto [CubeDef](./cubedef-object-ado-md.md) , ad esempio, può essere contenuto in una raccolta [CubeDefs](./cubedefs-collection-ado-md.md) di un **Catalogo**. Per ulteriori informazioni, vedere [ADO MD Collections](./ado-md-collections.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sull'API ADO MD](./ado-md-object-model.md)   
 [Esempi di codice ADO MD](./ado-md-code-examples.md)   
 [Raccolte di ADO MD](./ado-md-collections.md)   
 [ADO MD costanti enumerate](./ado-md-enumerated-constants.md)   
 [Metodi di ADO MD](./ado-md-methods.md)   
 [Modello a oggetti ADO MD](./ado-md-object-model.md)   
 [Proprietà ADO MD](./ado-md-properties.md)