---
description: Proprietà Dialect
title: Proprietà dialetto | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::Dialect
helpviewer_keywords:
- Dialect property
ms.assetid: 329c3a71-ba88-4009-b04f-2f52195a5957
author: rothja
ms.author: jroth
ms.openlocfilehash: ed95df8ca18a60cf34108258e7b3f624cb0cb1fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171251"
---
# <a name="dialect-property"></a>Proprietà Dialect
Indica il dialetto delle proprietà [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) . Il dialetto definisce la sintassi e le regole generali utilizzate dal provider per analizzare la stringa o il flusso.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 La proprietà **dialetto** contiene un GUID valido che rappresenta il dialetto del testo o del flusso del comando. Il valore predefinito per questa proprietà è {C8B521FB-5CF3-11CE-ADE5-00AA0044773D}, che indica che il provider deve scegliere come interpretare il testo o il flusso del comando.  
  
## <a name="remarks"></a>Commenti  
 ADO non esegue una query sul provider quando l'utente legge il valore di questa proprietà. Restituisce una rappresentazione di stringa del valore attualmente archiviato nell'oggetto [Command](../../../ado/reference/ado-api/command-object-ado.md) .  
  
 Quando l'utente imposta la proprietà **dialetto** , ADO convalida il GUID e genera un errore se il valore specificato non è un GUID valido. Per determinare i valori GUID supportati dalla proprietà **dialetto** , vedere la documentazione del provider.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Execute (Command - ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)
