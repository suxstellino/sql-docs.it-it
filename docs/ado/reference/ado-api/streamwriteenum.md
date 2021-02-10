---
description: StreamWriteEnum
title: StreamWriteEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamWriteEnum
helpviewer_keywords:
- StreamWriteEnum enumeration [ADO]
ms.assetid: bdbf3405-a0bd-4f02-85d4-e3fe8da3f3f7
author: rothja
ms.author: jroth
ms.openlocfilehash: f0f102ec1457b0cb327ab563ee348cf4112c749a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056546"
---
# <a name="streamwriteenum"></a>StreamWriteEnum
Specifica se un separatore di riga viene aggiunto alla stringa scritta in un oggetto [flusso](./stream-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adWriteChar**|0|Valore predefinito. Scrive la stringa di testo specificata (specificata dal parametro *dati* ) nell'oggetto **flusso** .|  
|**adWriteLine**|1|Scrive una stringa di testo e un carattere separatore di riga in un oggetto **flusso** . Se la proprietà [LineSeparator](./lineseparator-property-ado.md) non è definita, viene restituito un errore di run-time.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo WriteText](./writetext-method.md)