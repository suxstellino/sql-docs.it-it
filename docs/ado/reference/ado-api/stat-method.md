---
description: Metodo Stat
title: Metodo stat | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Stat
helpviewer_keywords:
- Stat method [ADO]
ms.assetid: 99a2b2d4-e6b1-4205-b011-72d024ea7240
author: rothja
ms.author: jroth
ms.openlocfilehash: 18571c88e604b21856195c9675e10b2c86dad35e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166484"
---
# <a name="stat-method"></a>Metodo Stat
Recupera le informazioni su un oggetto [flusso](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Long stream.Stat(StatStg, StatFlag)  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **Long** che indica lo stato dell'operazione.  
  
#### <a name="parameters"></a>Parametri  
 *StatStg*  
 Struttura STATSTG che verrà compilata con le informazioni sul flusso. L'implementazione del metodo **Stat** utilizzato dall'oggetto flusso ADO non compila tutti i campi della struttura.  
  
 *StatFlag*  
 Specifica che questo metodo non restituisce alcuni membri della struttura STATSTG, salvando così un'operazione di allocazione della memoria. I valori vengono ricavati dall'enumerazione STATFLAG. L'enumerazione STATFLAG ha due valori  
  
|Costante|Valore|  
|--------------|-----------|  
|STATFLAG_DEFAULT|0|  
|STATFLAG_NONAME|1|  
  
## <a name="remarks"></a>Commenti  
 La versione del metodo stat implementato per l'oggetto flusso ADO compila i campi seguenti della struttura STATSTG:  
  
 *pwcsName*  
 Stringa che contiene il nome del flusso, se ne è disponibile uno e il valore StatFlag STATFLAG_NONAME non è stato specificato.  
  
 *cbSize*  
 Specifica la dimensione in byte del flusso o della matrice di byte.  
  
 *mtime*  
 Indica l'ora dell'ultima modifica di questo archivio, flusso o matrice di byte.  
  
 *ctime*  
 Indica l'ora di creazione di questo archivio, flusso o matrice di byte.  
  
 *dell'atime*  
 Indica l'ora dell'ultimo accesso di questo archivio, flusso o matrice di byte.  
  
 Se STATFLAG_NONAME viene specificato nel parametro StatFlag, il nome del flusso non viene restituito.  
  
 Se STATFLAG_NONAME non è stato specificato nel parametro StatFlag e non è disponibile alcun nome per il flusso corrente, questo valore verrà E_NOTIMPL.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)