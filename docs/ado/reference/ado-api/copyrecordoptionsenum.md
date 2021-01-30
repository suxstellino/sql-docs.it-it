---
description: CopyRecordOptionsEnum
title: CopyRecordOptionsEnum uguale al | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CopyRecordOptionsEnum
helpviewer_keywords:
- CopyRecordOptionsEnum enumeration [ADO]
ms.assetid: 2fa4eec5-d50b-4fd3-8ae7-40af441ba12b
author: rothja
ms.author: jroth
ms.openlocfilehash: 2afe394c4b6251ea21d7245d55ab905ba6347fcc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167690"
---
# <a name="copyrecordoptionsenum"></a>CopyRecordOptionsEnum
Specifica il comportamento del metodo [CopyRecord](./copyrecord-method-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adCopyAllowEmulation**|4|Indica che il provider di *origine* tenta di simulare la copia utilizzando le operazioni di download e caricamento se questo metodo ha esito negativo perché la *destinazione* si trova in un server diverso o viene gestita da un provider diverso da quello di *origine*. Si noti che le funzionalità del provider diverse possono ostacolare le prestazioni o perdere i dati.|  
|**adCopyNonRecursive**|2|Copia la directory corrente, ma nessuna delle relative sottodirectory, nella destinazione. L'operazione di copia non è ricorsiva.|  
|**adCopyOverWrite**|1|Sovrascrive il file o la directory se la *destinazione* punta a un file o a una directory esistente.|  
|**adCopyUnspecified**|-1|Valore predefinito. Esegue l'operazione di copia predefinita: l'operazione ha esito negativo se il file o la directory di destinazione esiste già e l'operazione viene copiata in modo ricorsivo.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo CopyRecord (ADO)](./copyrecord-method-ado.md)