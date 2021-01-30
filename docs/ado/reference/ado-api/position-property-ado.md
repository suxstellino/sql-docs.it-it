---
description: Proprietà Position (ADO)
title: Proprietà Position (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Position
helpviewer_keywords:
- Position property [ADO]
ms.assetid: daa8319a-49aa-4c1c-9af6-0b01e9ab2f9d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9b2da711df951fc79db5034eeb447a71ade21fee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166861"
---
# <a name="position-property-ado"></a>Proprietà Position (ADO)
Indica la posizione corrente all'interno di un oggetto [flusso](./stream-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** che specifica l'offset, espresso in numero di byte, della posizione corrente a partire dall'inizio del flusso. Il valore predefinito è 0, che rappresenta il primo byte nel flusso.  
  
## <a name="remarks"></a>Commenti  
 La posizione corrente può essere spostata in un punto successivo alla fine del flusso. Se si specifica la posizione corrente oltre la fine del flusso, le [dimensioni](./size-property-ado-stream.md) dell'oggetto **flusso** verranno aumentate di conseguenza. Eventuali nuovi byte aggiunti in questo modo saranno null.  
  
> [!NOTE]
>  **Position** misura sempre i byte. Per i flussi di testo che utilizzano set di caratteri multibyte, moltiplicare la posizione per la dimensione del carattere per determinare il numero di caratteri. Per un set di caratteri a due byte, ad esempio, il primo carattere è nella posizione 0, il secondo carattere nella posizione 2, il terzo carattere nella posizione 4 e così via.  
  
> [!NOTE]
>  Non è possibile usare valori negativi per modificare la posizione corrente in un **flusso**. Per la **posizione** è possibile utilizzare solo numeri positivi.  
  
> [!NOTE]
>  Per gli oggetti di **flusso** di sola lettura, ADO non restituirà un errore se la **posizione** è impostata su un valore maggiore della **dimensione** del **flusso**. Questa operazione non modifica le dimensioni del **flusso** o modifica in alcun modo il contenuto del **flusso** . Questa operazione, tuttavia, deve essere evitata perché comporta un valore di **posizione** non significativo.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Charset (ADO)](./charset-property-ado.md)