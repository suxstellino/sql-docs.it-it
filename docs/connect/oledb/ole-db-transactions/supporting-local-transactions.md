---
title: Supporto delle transazioni locali (OLE DB Driver)
description: Informazioni su come OLE DB Driver per SQL Server supporta le transazioni locali e su come usare ITransactionLocal per un controllo più preciso sull'ambito della transazione locale.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- OLE DB Driver for SQL Server, transactions
- local transactions [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41f797115262d928e9ec47ab13144dba1a98e6bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754211"
---
# <a name="supporting-local-transactions"></a>Supporto delle transazioni locali
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Una sessione delimita l'ambito di una transazione locale per un OLE DB Driver per SQL Server. Quando, su indicazione di un consumer, OLE DB Driver per SQL Server sottopone una richiesta a un'istanza collegata di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la richiesta costituisce un'unità di lavoro per tale driver. Le transazioni locali eseguono sempre il wrapping di una o più unità di lavoro in un'unica sessione di OLE DB Driver per SQL Server.  
  
 Tramite la modalità autocommit predefinita del driver OLE DB per SQL Server, un'unità di lavoro singola viene considerata ambito di una transazione locale. Solo un unità partecipa alla transazione locale. Quando viene creata una sessione, OLE DB Driver per SQL Server inizia una transazione per la sessione. Al completamento di un'unità, viene eseguito il commit del lavoro. In caso di errore, viene eseguito il rollback di eventuali lavori iniziati e viene segnalato l'errore al consumer. In entrambi i casi, il driver OLE DB per SQL Server inizia una nuova transazione locale per la sessione, per consentire l'esecuzione di tutto il lavoro all'interno di una transazione.  
  
 Il consumer del driver OLE DB per SQL Server può esercitare un controllo più preciso sull'ambito della transazione locale tramite l'interfaccia **ITransactionLocal**. Quando una sessione del consumer inizia una transazione, tutte le unità di lavoro della sessione che si trovano tra il punto di inizio della transazione e le chiamate finali al metodo **Commit** o **Abort** vengono trattate come un'unità atomica. OLE DB Driver per SQL Server inizia implicitamente una transazione in base all'indicazione del consumer. Se il consumer non richiede la memorizzazione, la sessione ripristina il comportamento a livello di transazione padre, più comunemente la modalità AutoCommit.  
  
 OLE DB Driver per SQL Server supporta i parametri **ITransactionLocal::StartTransaction** come indicato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*isoLevel*[in]|Il livello di isolamento da utilizzare con questa transazione. Nelle transazioni locali, OLE DB Driver per SQL Server supporta quanto segue:<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> Nota: a partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ISOLATIONLEVEL_SNAPSHOT è valido per l'argomento *isoLevel* indipendentemente dall'abilitazione del controllo delle versioni per il database. Se tuttavia l'utente tenta di eseguire un'istruzione e il controllo delle versioni non è abilitato e/o il database non è di sola lettura, si verifica un errore. Si verifica poi l'errore XACT_E_ISOLATIONLEVEL se ISOLATIONLEVEL_SNAPSHOT è specificato come *isoLevel* ed è stata stabilita una connessione a una versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] precedente a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].|  
|*isoFlags*[in]|OLE DB Driver per SQL Server restituisce un errore per qualsiasi valore diverso da zero.|  
|*pOtherOptions*[in]|Se non è NULL, OLE DB Driver per SQL Server richiede l'oggetto opzioni all'interfaccia. OLE DB Driver per SQL Server restituisce XACT_E_NOTIMEOUT se il membro *ulTimeout* dell'oggetto opzioni è diverso da zero. OLE DB Driver per SQL Server ignora il valore del membro *szDescription*.|  
|*pulTransactionLevel*[out]|Se non è NULL, OLE DB Driver per SQL Server restituisce il livello nidificato della transazione.|  
  
 Per le transazioni locali OLE DB Driver per SQL Server implementa i parametri **ITransaction::Abort** come segue.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*pboidReason*[in]|Ignorato se impostato. Può essere NULL.|  
|*fRetaining*[in]|Quando è TRUE, una nuova transazione viene iniziata implicitamente per la sessione. È necessario che il consumer esegua il commit o termini la transazione. Quando è FALSE, OLE DB Driver per SQL Server ripristina la modalità autocommit per la sessione.|  
|*fAsync*[in]|L'interruzione asincrona non è supportata da OLE DB Driver per SQL Server. OLE DB Driver per SQL Server restituisce XACT_E_NOTSUPPORTED se il valore non è FALSE.|  
  
 Per le transazioni locali, OLE DB Driver per SQL Server implementa i parametri **ITransaction::Commit** come segue.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*fRetaining*[in]|Quando è TRUE, una nuova transazione viene iniziata implicitamente per la sessione. È necessario che il consumer esegua il commit o termini la transazione. Quando è FALSE, OLE DB Driver per SQL Server ripristina la modalità autocommit per la sessione.|  
|*grfTC*[in]|Le restituzioni asincrone e di prima fase non sono supportate da OLE DB Driver per SQL Server. OLE DB Driver per SQL Server restituisce XACT_E_NOTSUPPORTED per qualsiasi valore diverso da XACTTC_SYNC.|  
|*grfRM*[in]|Deve essere 0.|  
  
 I set di righe del driver OLE DB per SQL Server della sessione vengono mantenuti in caso di interruzione o commit locale in base ai valori delle proprietà del set di righe DBPROP_ABORTPRESERVE e DBPROP_COMMITPRESERVE. Per impostazione predefinita, entrambe queste proprietà sono VARIANT_FALSE e tutti i set di righe del driver OLE DB per SQL Server della sessione vengono persi in seguito a un'operazione di commit o di interruzione.  
  
 OLE DB Driver per SQL Server non implementa l'interfaccia **ITransactionObject**. Un tentativo del consumer di recuperare un riferimento dell'interfaccia restituisce E_NOINTERFACE.  
  
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
 [Transazioni](../../oledb/ole-db-transactions/transactions.md)   
 [Uso dell'isolamento dello snapshot](../../oledb/features/working-with-snapshot-isolation.md)  
  
  
