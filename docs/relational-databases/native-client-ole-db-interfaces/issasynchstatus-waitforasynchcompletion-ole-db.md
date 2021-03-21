---
description: 'ISSAsynchStatus:: WaitForAsynchCompletion in SQL Server Native Client (OLE DB)'
title: 'ISSAsynchStatus:: WaitForAsynchCompletion (provider OLE DB Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
apitype: COM
helpviewer_keywords:
- WaitForAsynchCompletion method
ms.assetid: 9f65e9e7-eb93-47a1-bc42-acd4649fbd0e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cff9adfc4334031033d02e81a6763cf636ec3971
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748981"
---
# <a name="issasynchstatuswaitforasynchcompletion-in-sql-server-native-client-ole-db"></a>ISSAsynchStatus:: WaitForAsynchCompletion in SQL Server Native Client (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Resta in attesa fino al completamento dell'operazione di esecuzione asincrona o fino al verificarsi di un timeout.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT WaitForAsynchCompletion(   
        DWORD dwMillisecTimeOut);  
```  
  
## <a name="arguments"></a>Argomenti  
 *dwMillisecTimeOut*[in]  
 Timeout in millisecondi.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_UNEXPECTED  
 Un set di righe è in uno stato inutilizzato perché è stato chiamato **ITransaction:: commit** o **ITransaction:: Abort** oppure il set di righe è stato annullato durante la fase di inizializzazione.  
  
 DB_E_CANCELED  
 L'elaborazione asincrona è stata annullata durante il popolamento del set di righe o l'inizializzazione dell'oggetto origine dati.  
  
 DB_S_ASYNCHRONOUS  
 L'operazione non è stata ancora completata anche se è stato raggiunto il timeout specificato.  
  
> [!NOTE]  
>  Oltre ai valori di codice restituiti elencati in precedenza, il metodo **ISSAsynchStatus::WaitForAsynchCompletion** supporta anche i valori di codice restituiti dai metodi OLEDB di base **ICommand::Execute** e **IDBInitialize::Initialize**.  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo **ISSAsynchStatus::WaitForAsynchCompletion** non verrà restituito fino al superamento del valore di timeout (in millisecondi) o al completamento dell'operazione in sospeso. L'oggetto **Command** include una proprietà **CommandTimeout** che controlla il numero di secondi che devono trascorrere prima del timeout di una query. La proprietà **CommandTimeout** verrà ignorata se usata insieme al metodo **ISSAsynchStatus::WaitForAsynchCompletion**.  
  
 La proprietà di timeout viene ignorata per le operazioni asincrone. Il parametro di timeout di **ISSAsynchStatus::WaitForAsynchCompletion** specifica la quantità massima di tempo che deve trascorrere prima che il controllo venga restituito al chiamante. Se questo timeout scade, verrà restituito DB_S_ASYNCHRONOUS. I timeout non annullano mai operazioni asincrone. Se l'applicazione deve annullare un'operazione asincrona che non viene completata entro il periodo di timeout, deve attendere lo scadere del timeout e quindi annullare in modo esplicito l'operazione se viene restituito DB_S_ASYNCHRONOUS.  
  
> [!NOTE]  
>  Quando si usano i componenti del servizio OLE DB, è possibile che venga restituito S_OK quando è previsto DB_S_ASYNCHRONOUS. Quando viene restituito S_OK o DB_S_ASYNCHRONOUS, le applicazioni devono pertanto chiamare [ISSAsynchStatus::GetStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) per verificare il completamento.  
  
 Se il valore di *dwMillisecTimeOut* è impostato su INFINITE, il metodo **ISSAsynchStatus::WaitForAsynchCompletion** mantiene il blocco fino al completamento dell'operazione. Se il valore di *dwMillisecTimeOut* è impostato su 0, il metodo restituirà immediatamente lo stato dell'operazione in sospeso. Se il timeout scade prima del completamento dell'operazione, viene restituito DB_S_ASYNCHRONOUS.  
  
 Se l'operazione viene completata prima dello scadere del timeout, il valore HRESULT restituito sarà quello restituito dall'operazione, ovvero il valore HRESULT che sarebbe stato restituito se l'operazione fosse stata eseguita in modo sincrono.  
  
 È stata inoltre aggiunta la proprietà SSPROP_ISSAsynchStatus al set di proprietà DBPROPSET_SQLSERVERROWSET. I provider che supportano l'interfaccia [ISSAsynchStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md) devono implementare questa proprietà con un valore VARIANT_TRUE.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di operazioni asincrone](../../relational-databases/native-client/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
