---
description: Proprietà CommandStream (ADO)
title: Proprietà CommandStream (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::CommandStream
helpviewer_keywords:
- CommandStream property [ADO]
ms.assetid: f78f61b6-87e0-48dc-961e-83d0e20da58e
author: rothja
ms.author: jroth
ms.openlocfilehash: 817ee84681b4b13895bc493034ceabade1f4773e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167753"
---
# <a name="commandstream-property-ado"></a>Proprietà CommandStream (ADO)
Indica il flusso utilizzato come input per un oggetto [comando](./command-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce il flusso utilizzato come input per un oggetto **comando** . Il formato di questo flusso è specifico del provider. per informazioni dettagliate, vedere la documentazione del provider. Questa proprietà è simile alla proprietà [CommandText](./commandtext-property-ado.md) , che consente di specificare una stringa per l'input di un **comando**.  
  
## <a name="remarks"></a>Commenti  
 **CommandStream** e **CommandText** si escludono a vicenda. Quando l'utente imposta la proprietà **CommandStream** , la proprietà **CommandText** verrà impostata sulla stringa vuota (""). Se l'utente imposta la proprietà **CommandText** , la proprietà **CommandStream** verrà impostata su **Nothing**.  
  
 I comportamenti dei metodi **Command. Parameters. Refresh** e **Command. Prepare** sono definiti dal provider. Non è possibile aggiornare i valori dei parametri in un flusso.  
  
 Il flusso di input non è disponibile per altri oggetti ADO che restituiscono l'origine di un **comando**. Se, ad esempio, l' [origine](./source-property-ado-recordset.md) di un [Recordset](./recordset-object-ado.md) è impostata su un oggetto **Command** con un flusso come input, **Recordset. Source** continua a restituire la proprietà **CommandText** , che contiene una stringa vuota (""), anziché il contenuto del flusso della proprietà **CommandStream** .  
  
 Quando si usa un flusso di comandi (come specificato da **CommandStream**), gli unici valori [CommandTypeEnum](./commandtypeenum.md) validi per la proprietà [CommandType](./commandtype-property-ado.md) sono **adCmdText** e **adCmdUnknown**. Qualsiasi altro valore causa un errore.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà CommandText (ADO)](./commandtext-property-ado.md)   
 [Proprietà dialetto](./dialect-property.md)   
 [CommandTypeEnum](./commandtypeenum.md)