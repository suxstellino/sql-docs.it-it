---
title: Supporto di transazioni distribuite (OLE DB Driver)
description: Informazioni su come i consumer di OLE DB Driver per SQL Server possono usare il metodo ITransactionJoin::JoinTransaction per partecipare a una transazione distribuita.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- distributed transactions [OLE DB]
- MS DTC
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
- ITransactionJoin interface
- MS DTC, about distributed transaction support
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 88c3b8414c854667ed5722a0fd044e8e28cfd4b9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754231"
---
# <a name="supporting-distributed-transactions"></a>Supporto di transazioni distribuite
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I consumer del driver OLE DB per SQL Server possono usare il metodo **ITransactionJoin::JoinTransaction** per partecipare a una transazione distribuita coordinata da Microsoft Distributed Transaction Coordinator (MS DTC).  
  
 MS DTC espone oggetti COM che consentono ai client di avviare e partecipare a transazioni coordinate tra più connessioni a un'ampia gamma di archivi dati. Per avviare una transazione, il consumer di OLE DB Driver per SQL Server usa l'interfaccia MS DTC **ITransactionDispenser**. Il membro **BeginTransaction** di **ITransactionDispenser** restituisce un riferimento in un oggetto della transazione distribuita. Tale riferimento viene passato a OLE DB Driver per SQL Server usando **JoinTransaction**.  
  
 MS DTC supporta il commit asincrono e l'interruzione nelle transazioni distribuite. Come notifica sullo stato della transazione asincrona, il consumer implementa l'interfaccia **ITransactionOutcomeEvents** e connette l'interfaccia a un oggetto transazione MS DTC.  
  
 Per le transazioni distribuite, il driver OLE DB per SQL Server implementa i parametri di **ITransactionJoin::JoinTransaction** nel modo seguente.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*punkTransactionCoord*|Puntatore a un oggetto transazione MS DTC.|  
|*IsoLevel*|Ignorato da OLE DB Driver per SQL Server. Il livello di isolamento per le transazioni coordinate da MS DTC viene determinato quando il consumer acquisisce un oggetto transazione da MS DTC.|  
|*IsoFlags*|Deve essere 0. OLE DB Driver per SQL Server restituisce XACT_E_NOISORETAIN se il consumer specifica altri valori.|  
|*POtherOptions*|Se non è NULL, OLE DB Driver per SQL Server richiede l'oggetto opzioni all'interfaccia. OLE DB Driver per SQL Server restituisce XACT_E_NOTIMEOUT se il membro *ulTimeout* dell'oggetto opzioni è diverso da zero. OLE DB Driver per SQL Server ignora il valore del membro *szDescription*.|  
  
 In questo esempio viene coordinata la transazione tramite MS DTC.  
  
```  
// Interfaces used in the example.  
IDBCreateSession*       pIDBCreateSession   = NULL;  
ITransactionJoin*       pITransactionJoin   = NULL;  
IDBCreateCommand*       pIDBCreateCommand   = NULL;  
IRowset*                pIRowset            = NULL;  
  
// Transaction dispenser and transaction from MS DTC.  
ITransactionDispenser*  pITransactionDispenser = NULL;  
ITransaction*           pITransaction       = NULL;  
  
    HRESULT             hr;  
  
// Get the command creation interface for the session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
// Get a transaction dispenser object from MS DTC and  
// start a transaction.  
if (FAILED(hr = DtcGetTransactionManager(NULL, NULL,  
    IID_ITransactionDispenser, 0, 0, NULL,  
    (void**) &pITransactionDispenser)))  
    {  
    // Process error message from MS DTC, release any references,  
    // and then return.  
    }  
if (FAILED(hr = pITransactionDispenser->BeginTransaction(  
    NULL, ISOLATIONLEVEL_READCOMMITTED, ISOFLAG_RETAIN_DONTCARE,  
    NULL, &pITransaction)))  
    {  
    // Process error message from MS DTC, release any references,  
    // and then return.  
    }  
  
// Join the transaction.  
if (FAILED(pIDBCreateCommand->QueryInterface(IID_ITransactionJoin,  
    (void**) &pITransactionJoin)))  
    {  
    // Process failure to get an interface, release any references, and  
    // then return.  
    }  
if (FAILED(pITransactionJoin->JoinTransaction(  
    (IUnknown*) pITransaction, 0, 0, NULL)))  
    {  
    // Process join failure, release any references, and then return.  
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
    // Get error from update, then abort.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, 0, 0)))  
        {  
        // Get error from failed commit.  
        //  
        // If a distributed commit fails, application logic could  
        // analyze failure and retry. In this example, terminate. The   
        // consumer must resolve this somehow.  
        pITransaction->Abort(NULL, FALSE, FALSE);  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Un-enlist from the distributed transaction by setting   
// the transaction object pointer to NULL.  
if (FAILED(pITransactionJoin->JoinTransaction(  
    (IUnknown*) NULL, 0, 0, NULL)))  
    {  
    // Process failure, and then return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Transazioni](../../oledb/ole-db-transactions/transactions.md)  
  
  
