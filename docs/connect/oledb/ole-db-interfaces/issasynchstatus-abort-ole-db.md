---
title: ISSAsynchStatus::Abort (OLE DB Driver) | Microsoft Docs
description: Informazioni su come il metodo ISSAsynchStatus::Abort annulla lo stato di un'operazione asincrona in esecuzione in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0eb1d96d2e3656d046cd738d1a4f38f4f6b7cb9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183743"
---
# <a name="issasynchstatusabort-ole-db"></a>ISSAsynchStatus::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Annulla un'operazione di esecuzione asincrona.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT Abort(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hChapter*[in]  
 Handle del capitolo per il quale interrompere l'operazione. Se l'oggetto chiamato non è un oggetto set di righe o l'operazione non è applicabile a un capitolo, il chiamante deve impostare *hChapter* su DB_NULL_HCHAPTER.  
  
 *eOperation*[in]  
 Operazione da interrompere. Deve essere usato il valore seguente:  
  
 DBASYNCHOP_OPEN: la richiesta di annullamento si applica all'apertura o al popolamento asincrono di un set di righe o all'inizializzazione asincrona di un oggetto origine dati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 La richiesta di annullare l'operazione asincrona è stata elaborata. Ciò non garantisce che l'operazione sia stata annullata. Per determinare se è stata effettivamente annullata, il consumer deve chiamare [ISSAsynchStatus::GetStatus](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) e verificare DB_E_CANCELED. È tuttavia possibile che non venga restituito nella chiamata successiva.  
  
 DB_E_CANTCANCEL  
 Non è stato possibile annullare l'operazione asincrona.  
  
 DB_E_CANCELED  
 La richiesta di interrompere l'operazione asincrona è stata annullata durante le notifiche. L'operazione viene ancora eseguita in modo asincrono.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider.  
  
 E_INVALIDARG  
 Il parametro *hChapter* non è DB_NULL_HCHAPTER o *eOperation* non è DBASYNCH_OPEN.  
  
 E_UNEXPECTED  
 `ISSAsynchStatus::Abort` è stato chiamato su un oggetto origine dati su cui `IDBInitialize::Initialize` non è stato chiamato o non è stato completato.  
  
 `ISSAsynchStatus::Abort` è stato chiamato su un oggetto origine dati su cui `IDBInitialize::Initialize` è stato chiamato ma che è stato annullato prima dell'inizializzazione o si è verificato il timeout. L'oggetto origine dati è ancora non inizializzato.  
  
 `ISSAsynchStatus::Abort` è stato chiamato su un set di righe su cui in precedenza è stato chiamato `ITransaction::Commit` o `ITransaction::Abort` e il set di righe non ha supportato l'operazione di interruzione o commit ed è in uno stato non valido.  
  
 `ISSAsynchStatus::Abort` è stato chiamato su un set di righe annullato in modo asincrono nella fase di inizializzazione. Il set di righe si trova in uno stato non valido.  
  
## <a name="remarks"></a>Osservazioni  
 L'interruzione dell'inizializzazione di un set di righe o di un oggetto origine dati potrebbe lasciare il set di righe o l'oggetto origine dati in uno stato non valido e determinare la restituzione di E_UNEXPECTED da parte di tutti i metodi ad eccezione di `IUnknown`. Quando ciò accade, l'unica azione possibile per il consumer consiste nel rilasciare il set di righe o l'oggetto origine dati.  
  
 Se si chiama `ISSAsynchStatus::Abort` e si passa un valore per *eOperation* diverso da DBASYNCHOP_OPEN, viene restituito S_OK. Questo valore non implica che l'operazione sia stata completata o annullata.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di operazioni asincrone](../../oledb/features/performing-asynchronous-operations.md)  
  
  
