---
description: Proprietà CommandTimeout (ADO)
title: Proprietà CommandTimeout (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandTimeout
helpviewer_keywords:
- CommandTimeout property [ADO]
ms.assetid: c611f857-d6b0-4dca-8925-f4a02e769eb0
author: rothja
ms.author: jroth
ms.openlocfilehash: ffd44947f767aa98a835d89d7809b4cb31667615
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034811"
---
# <a name="commandtimeout-property-ado"></a>Proprietà CommandTimeout (ADO)
Indica il tempo di attesa durante l'esecuzione di un comando prima di terminare il tentativo e generare un errore.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** che indica, in secondi, il tempo di attesa per l'esecuzione di un comando. L'impostazione predefinita è 30.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **CommandTimeout** per un oggetto [connessione](./connection-object-ado.md) o un oggetto [comando](./command-object-ado.md) per consentire l'annullamento di una chiamata al metodo [Execute](./execute-method-ado-command.md) , a causa di ritardi dal traffico di rete o da un utilizzo eccessivo del server. Se l'intervallo impostato nella proprietà **CommandTimeout** scade prima del completamento dell'esecuzione del comando, si verifica un errore e ADO Annulla il comando. Se si imposta la proprietà su zero, ADO resterà in attesa per un periodo di tempo indefinito fino al completamento dell'esecuzione. Verificare che il provider e l'origine dati in cui si scrive il codice supportino la funzionalità **CommandTimeout** .  
  
 L'impostazione **CommandTimeout** per un oggetto **Connection** non ha alcun effetto sull'impostazione **CommandTimeout** per un oggetto **Command** nella stessa **connessione**. ovvero la proprietà **CommandTimeout** dell'oggetto **comando** non eredita il valore del valore **CommandTimeout** dell'oggetto **connessione** .  
  
 In un oggetto **Connection** la proprietà **CommandTimeout** rimane in lettura/scrittura dopo l'apertura della **connessione** .  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Command (ADO)](./command-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Proprietà ConnectionTimeout (ADO)](./connectiontimeout-property-ado.md)