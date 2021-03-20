---
title: ISSAbort::Abort (OLE DB Driver) | Microsoft Docs
description: Informazioni sul modo in cui il metodo ISSAbort::Abort annulla il set di righe corrente e tutti i comandi in batch associati al comando corrente in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAbort::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5739727f82779f2ef32d7dd8359c2c3d47f0cad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738751"
---
# <a name="issabortabort-ole-db"></a>ISSAbort::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Annulla il set di righe corrente oltre a qualsiasi comando eseguito in batch associato al comando corrente.  
  
L'interfaccia `ISSAbort`, esposta nel driver OLE DB per SQL Server, fornisce il metodo `ISSAbort::Abort` che viene utilizzato per annullare il set di righe corrente oltre a qualsiasi comando eseguito in batch insieme al comando che ha inizialmente generato il set di righe e non ha ancora completato l'esecuzione.  
  
 `ISSAbort` è un'interfaccia specifica del driver OLE DB per SQL Server disponibile mediante `QueryInterface` sull'oggetto `IMultipleResults` restituito da `ICommand::Execute` o `IOpenRowset::OpenRowset`.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT Abort(void);  
```  
  
## <a name="remarks"></a>Osservazioni  
 Se il comando interrotto è in una stored procedure, l'esecuzione della stored procedure (e di qualsiasi procedura che ha chiamato la procedura) verrà terminata insieme al batch di comandi che contiene la chiamata alla stored procedure. Se nel server è in corso il trasferimento di un set di risultati al client, questo processo verrà arrestato. Se il client non vuole usare un set di risultati, viene chiamato `**`ISSAbort::Abort` before releasing the rowset will speed up the rowset release, but if there is an open transaction and XACT_ABORT is ON, the transaction will be rolled back when `ISSAbort::Abort  
  
 Dopo che `ISSAbort::Abort` restituisce S_OK, l'interfaccia `IMultipleResults` associata passa in uno stato inutilizzabile e restituisce DB_E_CANCELED a tutte le chiamate ai metodi, ad eccezione dei metodi definiti dall'interfaccia `IUnknown`, fino a quando non viene rilasciata. Se si ottiene un'interfaccia `IRowset` da `IMultipleResults` prima di una chiamata a `Abort`, anche questa interfaccia passa in uno stato inutilizzabile e restituisce DB_E_CANCELED a tutte le chiamate ai metodi, ad eccezione dei metodi definiti dall'interfaccia `IUnknown` e da `IRowset::ReleaseRows`, fino a quando non viene rilasciata in seguito a una chiamata riuscita a `ISSAbort::Abort`.  
  
> [!NOTE]  
>  A partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], se lo stato XACT_ABORT del server è ON, l'esecuzione di `ISSAbort::Abort` terminerà qualsiasi transazione implicita o esplicita e ne eseguirà il rollback durante la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la transazione corrente non viene interrotta.  
  
## <a name="arguments"></a>Argomenti  
 No.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo `ISSAbort::Abort` restituisce S_OK se il batch è stato annullato e DB_E_CANTCANCEL in caso contrario. Se il batch è già stato annullato, viene restituito DB_E_CANCELED.  
  
 DB_E_CANCELED  
 Il batch è già stato annullato.  
  
 DB_E_CANTCANCEL  
 Il batch non è stato annullato.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Per informazioni dettagliate, usare l'interfaccia [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 La chiamata al metodo non era prevista. Lo stato dell'oggetto, ad esempio, è in dubbio in quanto `ISSAbort::Abort` è già stato chiamato.  
  
 E_OUTOFMEMORY  
 Errore di memoria insufficiente.  
  
