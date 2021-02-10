---
description: MoveRecordOptionsEnum
title: MoveRecordOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- MoveRecordOptionsEnum
helpviewer_keywords:
- MoveRecordOptionsEnum enumeration [ADO]
ms.assetid: f53c2ce4-1021-4a45-92b8-775e8bebad99
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c471bf1a5f3a96efd75c62bab2175c83f5ca2b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041661"
---
# <a name="moverecordoptionsenum"></a>MoveRecordOptionsEnum
Specifica il comportamento del metodo [MoveRecord](./moverecord-method-ado.md) dell'oggetto [record](./record-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adMoveUnspecified**|-1|Valore predefinito. Esegue l'operazione di spostamento predefinita: l'operazione ha esito negativo se il file o la directory di destinazione esiste già e l'operazione Aggiorna i collegamenti ipertestuali.|  
|**adMoveOverWrite**|1|Sovrascrive il file o la directory di destinazione, anche se esiste già.|  
|**adMoveDontUpdateLinks**|2|Modifica il comportamento predefinito del metodo **MoveRecord** non aggiornando i collegamenti ipertestuali del **record** di origine. Il comportamento predefinito dipende dalle funzionalità del provider. Spostare i collegamenti degli aggiornamenti dell'operazione se il provider è in grado di supportare. Se il provider non è in grado di correggere i collegamenti o se questo valore non è specificato, lo spostamento ha esito positivo anche quando i collegamenti non sono stati corretti.|  
|**adMoveAllowEmulation**|4|Richiede che il provider tenti di simulare lo spostamento (mediante operazioni di download, caricamento ed eliminazione). Se il tentativo di spostare il **record** ha esito negativo perché l'URL di destinazione si trova in un server diverso o viene servito da un provider diverso da quello di origine, potrebbe verificarsi un aumento della latenza o della perdita di dati, a causa delle diverse funzionalità del provider durante lo spostamento delle risorse tra i provider.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo MoveRecord (ADO)](./moverecord-method-ado.md)