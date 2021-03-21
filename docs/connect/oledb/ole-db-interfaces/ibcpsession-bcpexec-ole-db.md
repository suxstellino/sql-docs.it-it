---
title: IBCPSession::BCPExec (OLE DB Driver) | Microsoft Docs
description: Il metodo IBCPSession::BCPExec copia i dati da un file utente a una tabella di database o viceversa in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPExec (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPExec method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9d1350ebc60a411b505f7750bda7e3b5493bb312
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748701"
---
# <a name="ibcpsessionbcpexec-ole-db"></a>IBCPSession::BCPExec (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Esegue l'operazione di copia bulk.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT BCPExec(   
      DBROWCOUNT *pRowsCopied);  
```  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo **BCPExec** copia i dati da un file utente a una tabella di database o viceversa, a seconda del valore del parametro *eDirection* usato con il metodo [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
 Prima di chiamare **BCPExec**, chiamare il metodo **BCPInit** con un nome di file utente valido. In caso contrario, viene generato un errore. L'unica eccezione riguarda le query da utilizzare per operazioni di copia bulk per l'esportazione. In un caso di questo tipo, specificare NULL per il nome di tabella nel metodo **BCPInit** e quindi specificare la query usando l'opzione BCP_OPTION_HINTS.  
  
 Il metodo **BCPExec** è l'unico metodo di copia bulk che potrebbe rimanere in attesa per un certo periodo di tempo, pertanto è l'unico metodo di copia bulk che supporta la modalità asincrona. Per usare la modalità asincrona, impostare la proprietà di sessione SSPROP_ASYNCH_BULKCOPY specifica del provider su VARIANT_TRUE prima di chiamare il metodo **BCPExec**. Questa proprietà è disponibile nel set di proprietà DBPROPSET_SQLSERVERSESSION. Per verificare che l'operazione sia stata completata, chiamare il metodo **BCPExec** con gli stessi parametri. Se la copia bulk non è stata ancora completata, il metodo **BCPExec** restituisce DB_S_ASYNCHRONOUS. Nell'argomento *pRowsCopied* restituisce anche un conteggio dello stato del numero di righe inviate al server o ricevute dal server. Il commit delle righe inviate al server non viene eseguito fino a quando non viene raggiunta la fine di un batch.  
  
## <a name="arguments"></a>Argomenti  
 *pRowsCopied*[out]  
 Puntatore a DWORD. Il metodo **BCPExec** inserisce in DWORD il numero di righe copiate correttamente. Se l'argomento *pRowsCopied* è impostato su NULL, viene ignorato dal metodo **BCPExec**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Per informazioni dettagliate, usare l'interfaccia [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 La chiamata al metodo non era prevista. Non è stato ad esempio chiamato il metodo **BCPInit** prima della chiamata a questo metodo. Si verifica anche se l'operazione è stata interrotta tramite l'opzione BCP_OPTION_ABORT e successivamente è stato chiamato il metodo **BCPExec**.  
  
 E_OUTOFMEMORY  
 Errore di memoria insufficiente.  
  
 DB_S_ENDOFROWSET  
 L'operazione di copia bulk è terminata e il trasferimento dei dati è stato completato.  
  
 DB_S_ASYNCHRONOUS  
 Il batch corrente di righe è stato copiato. Chiamare di nuovo il metodo **BCPExec** per trasferire il batch successivo.  
  
 DB_S_ERRORSOCCURRED  
 Si sono verificati errori durante l'operazione di copia bulk ed è possibile che alcune righe non siano state copiate. Il numero di errori è ancora al di sotto del numero massimo di errori consentiti.  
  
## <a name="see-also"></a>Vedere anche  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Esecuzione di operazioni di copia bulk](../../oledb/features/performing-bulk-copy-operations.md)  
  
