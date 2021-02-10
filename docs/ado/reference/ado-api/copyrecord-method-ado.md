---
description: Metodo CopyRecord (ADO)
title: Metodo CopyRecord (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_CopyRecord
- _Record::CopyRecord
helpviewer_keywords:
- CopyRecord method [ADO]
ms.assetid: b9bcf272-3c74-479f-95dd-0229a32e98fc
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f5a6e6f7d607bbc2f50bfad701056aca93fb822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026039"
---
# <a name="copyrecord-method-ado"></a>Metodo CopyRecord (ADO)
Copia un'entità rappresentata da un [record](./record-object-ado.md) in un'altra posizione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Record.CopyRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>Parametri  
 *Origine*  
 facoltativo. Valore **stringa** che contiene un URL che specifica l'entità da copiare, ad esempio un file o una directory. Se l' *origine* viene omessa o specifica una stringa vuota, il file o la directory rappresentata dal [record](./record-object-ado.md) corrente verrà copiato.  
  
 *Destinazione*  
 facoltativo. Valore **stringa** che contiene un URL che specifica il percorso in cui verrà copiato il *codice sorgente* .  
  
 *UserName*  
 facoltativo. Valore **stringa** che contiene l'ID utente che, se necessario, autorizza l'accesso alla *destinazione*.  
  
 *Password*  
 facoltativo. Valore **stringa** che contiene la password che, se necessario, verifica il *nome utente*.  
  
 *Opzioni*  
 facoltativo. Valore [CopyRecordOptionsEnum uguale al](./copyrecordoptionsenum.md) il cui valore predefinito è **adCopyUnspecified**. Specifica il comportamento di questo metodo.  
  
 *Asincrona*  
 facoltativo. Valore **booleano** che, se impostato su **true**, specifica che questa operazione deve essere asincrona.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **stringa** che restituisce in genere il valore di *Destination*. Tuttavia, il valore esatto restituito è dipendente dal provider.  
  
## <a name="remarks"></a>Commenti  
 I valori di *source* e *Destination* non devono essere identici. in caso contrario, si verificherà un errore di run-time. Almeno uno dei nomi di server, percorsi o risorse deve essere diverso.  
  
 Tutti gli elementi figlio, ad esempio le sottodirectory, dell' *origine* vengono copiati in modo ricorsivo, a meno che non sia specificato **adCopyNonRecursive** . In un'operazione ricorsiva, la *destinazione* non deve essere una sottodirectory dell' *origine*; in caso contrario, l'operazione non viene completata.  
  
 Questo metodo ha esito negativo se la *destinazione* identifica un'entità esistente (ad esempio, un file o una directory), a meno che non sia specificato **adCopyOverwrite** .  
  
> [!IMPORTANT]
>  Usare l'opzione **adCopyOverwrite** in giudizio. Se ad esempio si specifica questa opzione quando si copia un file in una directory, la directory verrà *eliminata* e sostituita con il file.  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Record (ADO)](./record-object-ado.md)