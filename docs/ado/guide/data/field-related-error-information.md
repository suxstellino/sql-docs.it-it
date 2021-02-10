---
description: Informazioni sugli errori correlati ai campi
title: Field-Related informazioni sull'errore | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- field-related errors [ADO]
- errors [ADO], field-related
ms.assetid: 5e7b1af4-996b-47c5-9161-c5575ad4fec9
author: rothja
ms.author: jroth
ms.openlocfilehash: cce74cf105aa7b38c2a3d7b157ef0fa17a1e8c1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037471"
---
# <a name="field-related-error-information"></a>Informazioni sugli errori correlati ai campi
Se un errore è direttamente correlato a un campo, ad esempio se i dati risultano mancanti o se il tipo non è corretto per il campo, è possibile recuperare ulteriori informazioni sulla causa del problema esaminando la proprietà di **stato** dell'oggetto **campo** . Questa proprietà è stata migliorata per fornire informazioni specifiche sul problema. Quindi, ad esempio, quando una chiamata a **UpdateBatch** ha esito negativo, la causa del problema può essere determinata esaminando la proprietà **status** dei **campi** in ognuno dei record effettivi. La proprietà conterrà uno dei valori della costante **FieldStatusEnum** . La tabella seguente include i valori che sono di particolare interesse quando si verifica un errore.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adFieldCantConvertValue**|2|Indica che il campo non può essere recuperato o archiviato senza perdita di dati.|  
|**adFieldDataOverflow**|6|Indica che i dati restituiti dal provider hanno causato un overflow del tipo di dati del campo.|  
|**adFieldDefault**|13|Indica che durante l'impostazione dei dati è stato utilizzato il valore predefinito per il campo.|  
|**adFieldIgnore**|15|Indica che questo campo è stato ignorato durante l'impostazione dei valori dei dati nell'origine. Nessun valore è stato impostato dal provider.|  
|**adFieldIntegrityViolation**|10|Indica che il campo non può essere modificato perché è un'entità calcolata o derivata.|  
|**adFieldIsNull**|3|Indica che il provider ha restituito un valore null.|  
|**adFieldOutOfSpace**|22|Indica che il provider non è in grado di ottenere spazio di archiviazione sufficiente per completare un'operazione di spostamento o copia.|
