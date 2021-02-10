---
description: SaveOptionsEnum
title: SaveOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SaveOptionsEnum
helpviewer_keywords:
- SaveOptionsEnum enumeration [ADO]
ms.assetid: 59339100-6e29-48d1-aea3-6873796d186b
author: rothja
ms.author: jroth
ms.openlocfilehash: db94b409a799a24d0c74dfec5a3d388c55df20e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040671"
---
# <a name="saveoptionsenum"></a>SaveOptionsEnum
Specifica se un file deve essere creato o sovrascritto durante il salvataggio da un oggetto [flusso](./stream-object-ado.md) . I valori possono essere **adSaveCreateNotExist** o **adSaveCreateOverwrite**.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adSaveCreateNotExist**|1|Valore predefinito. Crea un nuovo file se il file specificato dal parametro *filename* non esiste già.|  
|**adSaveCreateOverWrite**|2|Sovrascrive il file con i dati dell'oggetto **flusso** attualmente aperto, se il file specificato dal parametro *filename* esiste già. Se il file specificato dal parametro *filename* non esiste, viene creato un nuovo file.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo SaveToFile](./savetofile-method.md)