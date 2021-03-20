---
description: Supporto delle transazioni distribuite in SQL Server Native Client
title: Supporto delle transazioni distribuite (provider OLE DB di Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- distributed transactions [OLE DB]
- MS DTC
- transactions [OLE DB]
- SQL Server Native Client OLE DB provider, transactions
- ITransactionJoin interface
- MS DTC, about distributed transaction support
ms.assetid: d250b43b-9260-4ea4-90cc-57d9a2f67ea7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 72215c01a73fe1727d829f1f4cf827e1437b5dae
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743611"
---
# <a name="supporting-distributed-transactions-in-sql-server-native-client"></a>Supporto delle transazioni distribuite in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] I consumer del provider OLE DB di Native client possono utilizzare il metodo **ITransactionJoin:: JoinTransaction** per partecipare a una transazione distribuita coordinata da Microsoft Distributed Transaction Coordinator (MS DTC).  
  
 MS DTC espone oggetti COM che consentono ai client di avviare e partecipare a transazioni coordinate tra più connessioni a un'ampia gamma di archivi dati. Per avviare una transazione, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consumer del provider di OLE DB di Native client utilizza l'interfaccia **ITRANSACTIONDISPENSER** di MS DTC. Il membro **BeginTransaction** di **ITransactionDispenser** restituisce un riferimento in un oggetto della transazione distribuita. Questo riferimento viene passato al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider OLE DB di Native client utilizzando **JoinTransaction**.  
  
 MS DTC supporta il commit asincrono e l'interruzione nelle transazioni distribuite. Come notifica sullo stato della transazione asincrona, il consumer implementa l'interfaccia **ITransactionOutcomeEvents** e connette l'interfaccia a un oggetto transazione MS DTC.  
  
 Per le transazioni distribuite, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa i parametri **ITransactionJoin:: JoinTransaction** come indicato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*punkTransactionCoord*|Puntatore a un oggetto transazione MS DTC.|  
|*IsoLevel*|Ignorato dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client. Il livello di isolamento per le transazioni coordinate da MS DTC viene determinato quando il consumer acquisisce un oggetto transazione da MS DTC.|  
|*IsoFlags*|Deve essere 0. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce XACT_E_NOISORETAIN se un altro valore viene specificato dall'utente.|  
|*POtherOptions*|Se non è NULL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client richiede l'oggetto Options dall'interfaccia. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce XACT_E_NOTIMEOUT se il membro *ulTIMEOUT* dell'oggetto options è diverso da zero. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client ignora il valore del membro *szDescription* .|  
  
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
 [Transazioni](../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
  
