---
description: Supporto delle transazioni locali in SQL Server Native Client
title: Supporto di transazioni locali (provider OLE DB Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- SQL Server Native Client OLE DB provider, transactions
- local transactions [OLE DB]
ms.assetid: 78f2e5fc-b6fb-4eda-9f71-991a4d6c4902
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 019df854b297659f98d16646271c6f2076f41aa9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748731"
---
# <a name="supporting-local-transactions-in-sql-server-native-client"></a>Supporto delle transazioni locali in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una sessione delimita l'ambito della transazione per una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transazione locale del provider OLE DB di Native Client. Quando, alla direzione di un consumer, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client invia una richiesta a un'istanza connessa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la richiesta costituisce un'unità di lavoro per il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client. Nelle transazioni locali viene sempre eseguito il wrapping di una o più unità di lavoro in una singola [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sessione del provider OLE DB Native Client.  
  
 Utilizzando la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] modalità autocommit del provider OLE DB Native client predefinita, una singola unità di lavoro viene considerata come ambito di una transazione locale. Solo un unità partecipa alla transazione locale. Quando viene creata una sessione, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client inizia una transazione per la sessione. Al completamento di un'unità, viene eseguito il commit del lavoro. In caso di errore, viene eseguito il rollback di eventuali lavori iniziati e viene segnalato l'errore al consumer. In entrambi i casi, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client inizia una nuova transazione locale per la sessione in modo che tutto il lavoro venga eseguito all'interno di una transazione.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consumer del provider OLE DB di Native client può indirizzare un controllo più preciso sull'ambito della transazione locale tramite l'interfaccia **ITransactionLocal** . Quando una sessione del consumer inizia una transazione, tutte le unità di lavoro della sessione che si trovano tra il punto di inizio della transazione e le chiamate finali al metodo **Commit** o **Abort** vengono trattate come un'unità atomica. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client inizia in modo implicito una transazione quando viene indirizzata dal consumer. Se il consumer non richiede la memorizzazione, la sessione ripristina il comportamento a livello di transazione padre, più comunemente la modalità AutoCommit.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client supporta i parametri **ITransactionLocal:: StartTransaction** come indicato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*isoLevel*[in]|Il livello di isolamento da utilizzare con questa transazione. Nelle transazioni locali il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client supporta gli elementi seguenti:<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> Nota: a partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], ISOLATIONLEVEL_SNAPSHOT è valido per l'argomento *isoLevel* indipendentemente dall'abilitazione del controllo delle versioni per il database. Se tuttavia l'utente tenta di eseguire un'istruzione e il controllo delle versioni non è abilitato e/o il database non è di sola lettura, si verifica un errore. Si verifica poi l'errore XACT_E_ISOLATIONLEVEL se ISOLATIONLEVEL_SNAPSHOT è specificato come *isoLevel* ed è stata stabilita una connessione a una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedente a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
|*isoFlags*[in]|Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce un errore per qualsiasi valore diverso da zero.|  
|*pOtherOptions*[in]|Se non è NULL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client richiede l'oggetto Options dall'interfaccia. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce XACT_E_NOTIMEOUT se il membro *ulTIMEOUT* dell'oggetto options è diverso da zero. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client ignora il valore del membro *szDescription* .|  
|*pulTransactionLevel*[out]|Se non è NULL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce il livello annidato della transazione.|  
  
 Per le transazioni locali, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa i parametri **ITransaction:: Abort** come indicato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*pboidReason*[in]|Ignorato se impostato. Può essere NULL.|  
|*fRetaining*[in]|Quando è TRUE, una nuova transazione viene iniziata implicitamente per la sessione. È necessario che il consumer esegua il commit o termini la transazione. Se è FALSE, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client Ripristina la modalità autocommit per la sessione.|  
|*fAsync*[in]|L'interruzione asincrona non è supportata dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce XACT_E_NOTSUPPORTED se il valore non è false.|  
  
 Per le transazioni locali, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa i parametri **ITransaction:: commit** come indicato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*fRetaining*[in]|Quando è TRUE, una nuova transazione viene iniziata implicitamente per la sessione. È necessario che il consumer esegua il commit o termini la transazione. Se è FALSE, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client Ripristina la modalità autocommit per la sessione.|  
|*grfTC*[in]|Il provider di OLE DB di Native client non supporta la restituzione asincrona e la fase uno [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce XACT_E_NOTSUPPORTED per qualsiasi valore diverso da XACTTC_SYNC.|  
|*grfRM*[in]|Deve essere 0.|  
  
 I [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] set di righe del provider di OLE DB di Native client nella sessione vengono conservati in un'operazione di commit o di interruzione locale in base ai valori delle proprietà del set di righe DBPROP_ABORTPRESERVE e DBPROP_COMMITPRESERVE. Per impostazione predefinita, queste proprietà sono entrambe VARIANT_FALSE e tutti i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] set di righe del provider OLE DB client nativi della sessione vengono persi dopo un'operazione di interruzione o di commit.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non implementa l'interfaccia **ITransactionObject** . Un tentativo del consumer di recuperare un riferimento dell'interfaccia restituisce E_NOINTERFACE.  
  
 Questo esempio usa **ITransactionLocal**.  
  
```  
// Interfaces used in the example.  
IDBCreateSession*   pIDBCreateSession   = NULL;  
ITransaction*       pITransaction       = NULL;  
IDBCreateCommand*   pIDBCreateCommand   = NULL;  
IRowset*            pIRowset            = NULL;  
  
HRESULT             hr;  
  
// Get the command creation and local transaction interfaces for the  
// session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
if (FAILED(hr = pIDBCreateCommand->QueryInterface(IID_ITransactionLocal,  
    (void**) &pITransaction)))  
    {  
    // Process error. Release any references and return.  
    }  
  
// Start the local transaction.  
if (FAILED(hr = ((ITransactionLocal*) pITransaction)->StartTransaction(  
    ISOLATIONLEVEL_REPEATABLEREAD, 0, NULL, NULL)))  
    {  
    // Process error from StartTransaction. Release any references and  
    // return.  
    }  
  
// Get data into a rowset, then update the data. Functions are not  
// illustrated in this example.  
if (FAILED(hr = ExecuteCommand(pIDBCreateCommand, &pIRowset)))  
    {  
    // Release any references and return.  
    }  
  
// If rowset data update fails, then terminate the transaction, else  
// commit. The example doesn't retain the rowset.  
if (FAILED(hr = UpdateDataInRowset(pIRowset, bDelayedUpdate)))  
    {  
    // Get error from update, then terminate.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, XACTTC_SYNC, 0)))  
        {  
        // Get error from failed commit.  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Transazioni](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [Uso dell'isolamento dello snapshot](../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
  
  
