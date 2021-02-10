---
description: Proprietà Source (Record - ADO)
title: Proprietà Source (record ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::Source
- _Record::PutRefSource
- _Record::GetSource
- _Record::put_Source
- _Record::putref_Source
- _Record::get_Source
helpviewer_keywords:
- Source property [ADO Record]
ms.assetid: 2c18279e-6f35-4af0-b12e-8f1543d9ed20
author: rothja
ms.author: jroth
ms.openlocfilehash: 850f3d493962db94671bb1f2e8ef65c1b2120f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051402"
---
# <a name="source-property-ado-record"></a>Proprietà Source (Record - ADO)
Indica l'origine dati o l'oggetto rappresentato dal [record](./record-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Variant** che indica l'entità rappresentata dal **record**.  
  
## <a name="remarks"></a>Commenti  
 La proprietà **source** restituisce l'argomento di *origine* del metodo di [apertura](./open-method-ado-record.md) dell'oggetto **record** . Può contenere una stringa URL assoluta o relativa. È possibile utilizzare un URL assoluto senza impostare la proprietà [ActiveConnection](./activeconnection-property-ado.md) per aprire direttamente l'oggetto **record** . In questo caso viene creato un oggetto **connessione** implicito.  
  
 La proprietà **source** può inoltre contenere un riferimento a un **Recordset** già aperto, che consente di aprire un oggetto **record** che rappresenta la riga corrente nel **Recordset**.  
  
 La proprietà **source** può inoltre contenere un riferimento a un oggetto [Command](./command-object-ado.md) che restituisce una singola riga di dati dal provider.  
  
 Se viene impostata anche la proprietà **ActiveConnection** , la proprietà **source** deve puntare a un oggetto esistente nell'ambito di tale connessione. Negli spazi dei nomi strutturati ad albero, ad esempio, se la proprietà di **origine** contiene un URL assoluto, deve puntare a un nodo presente all'interno dell'ambito del nodo identificato dall'URL nella stringa di connessione. Se la proprietà di **origine** contiene un URL relativo, viene convalidata nel contesto impostato dalla proprietà **ActiveConnection** .  
  
 La proprietà di **origine** è di lettura/scrittura mentre l'oggetto **record** è chiuso ed è di sola lettura mentre l'oggetto **record** è aperto.  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Record (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Source (errore ADO)](./source-property-ado-error.md)   
 [Proprietà Source (Recordset - ADO)](./source-property-ado-recordset.md)