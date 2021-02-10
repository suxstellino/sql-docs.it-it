---
description: Oggetto Field
title: Oggetto Field | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field
helpviewer_keywords:
- Field object [ADO]
ms.assetid: b10a72fc-3c4b-4186-a70b-993dc9f7a092
author: rothja
ms.author: jroth
ms.openlocfilehash: 04e7faf5525c676ecc9c20bb312414742e17fc4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034091"
---
# <a name="field-object"></a>Oggetto Field
Rappresenta una colonna di dati con un tipo di dati comune.  
  
## <a name="remarks"></a>Commenti  
 Ogni oggetto **campo** corrisponde a una colonna nel [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md). Usare la proprietà [value](../../../ado/reference/ado-api/value-property-ado.md) degli oggetti **Field** per impostare o restituire i dati per il record corrente. A seconda delle funzionalità esposte dal provider, alcune raccolte, metodi o proprietà di un oggetto **campo** potrebbero non essere disponibili.  
  
 Con le raccolte, i metodi e le proprietà di un oggetto **campo** , è possibile eseguire le operazioni seguenti:  
  
-   Restituisce il nome di un campo con la proprietà [Name](../../../ado/reference/ado-api/name-property-ado.md) .  
  
-   Visualizzare o modificare i dati nel campo con la proprietà **value** . **Value** è la proprietà predefinita dell'oggetto **Field** .  
  
-   Restituire le caratteristiche di base di un campo con il [tipo](../../../ado/reference/ado-api/type-property-ado.md), la [precisione](../../../ado/reference/ado-api/precision-property-ado.md)e le proprietà [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) .  
  
-   Restituisce la dimensione dichiarata di un campo con la proprietà [DefinedSize](../../../ado/reference/ado-api/definedsize-property.md) .  
  
-   Restituisce le dimensioni effettive dei dati in un campo specificato con la proprietà [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) .  
  
-   Determinare quali tipi di funzionalità sono supportati per un determinato campo con la proprietà [Attributes](../../../ado/reference/ado-api/attributes-property-ado.md) e la raccolta [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) .  
  
-   Modificare i valori dei campi contenenti dati di tipo long binary o Long character con i metodi [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) e [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) .  
  
-   Se il provider supporta gli aggiornamenti batch, risolvere le discrepanze nei valori di campo durante l'aggiornamento in batch con le proprietà [OriginalValue](../../../ado/reference/ado-api/originalvalue-property-ado.md) e [UnderlyingValue](../../../ado/reference/ado-api/underlyingvalue-property.md) .  
  
 Tutte le proprietà dei metadati (**Name**, **Type**, **DefinedSize**, **Precision** e **NumericScale**) sono disponibili prima di aprire il **Recordset** dell'oggetto **campo** . L'impostazione in quel momento è utile per la costruzione dinamica dei moduli.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Field](../../../ado/reference/ado-api/field-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Raccolta Fields (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Raccolta Properties (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Oggetto Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
