---
description: Sessioni (provider OLE DB Native Client)
title: Sessioni (provider OLE DB Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- SQL Server Native Client OLE DB provider, sessions
ms.assetid: 3a980816-675c-4fba-acc9-429297d85bbd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 987576102a465b5b1f8600803374882f1b01f4df
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748461"
---
# <a name="sessions-native-client-ole-db-provider"></a>Sessioni (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sessione del provider OLE DB di Native client rappresenta una singola connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client richiede che le sessioni delimitatino lo spazio delle transazioni per un'origine dati. Tutti gli oggetti comando creati da uno specifico oggetto di sessione partecipano alla transazione locale o distribuita dell'oggetto in questione.  
  
 Il primo oggetto di sessione creato nell'origine dati inizializzata riceve la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stabilita in fase di inizializzazione. Quando tutti i riferimenti nelle interfacce dell'oggetto di sessione vengono rilasciati, la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diventa disponibile per un altro oggetto di sessione creato nell'origine dati.  
  
 Un oggetto di sessione aggiuntivo creato nell'origine dati stabilisce una connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come specificato dall'origine dati. La connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eliminata quando l'applicazione rilascia tutti i riferimenti agli oggetti creati nella sessione.  
  
 Nell'esempio seguente viene illustrato come utilizzare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client per connettersi a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database:  
  
```  
int main()  
{  
    // Interfaces used in the example.  
    IDBInitialize*      pIDBInitialize      = NULL;  
    IDBCreateSession*   pIDBCreateSession   = NULL;  
    IDBCreateCommand*   pICreateCmd1        = NULL;  
    IDBCreateCommand*   pICreateCmd2        = NULL;  
    IDBCreateCommand*   pICreateCmd3        = NULL;  
  
    // Initialize COM.  
    if (FAILED(CoInitialize(NULL)))  
    {  
        // Display error from CoInitialize.  
        return (-1);  
    }  
  
    // Get the memory allocator for this task.  
    if (FAILED(CoGetMalloc(MEMCTX_TASK, &g_pIMalloc)))  
    {  
        // Display error from CoGetMalloc.  
        goto EXIT;  
    }  
  
    // Create an instance of the data source object.  
    if (FAILED(CoCreateInstance(CLSID_SQLNCLI10, NULL,  
        CLSCTX_INPROC_SERVER, IID_IDBInitialize, (void**)  
        &pIDBInitialize)))  
    {  
        // Display error from CoCreateInstance.  
        goto EXIT;  
    }  
  
    // The InitFromPersistedDS function   
    // performs IDBInitialize->Initialize() establishing  
    // the first application connection to the instance of SQL Server.  
    if (FAILED(InitFromPersistedDS(pIDBInitialize, L"MyDataSource",  
        NULL, NULL)))  
    {  
        goto EXIT;  
    }  
  
    // The IDBCreateSession interface is implemented on the data source  
    // object. Maintaining the reference received maintains the   
    // connection of the data source to the instance of SQL Server.  
    if (FAILED(pIDBInitialize->QueryInterface(IID_IDBCreateSession,  
        (void**) &pIDBCreateSession)))  
    {  
        // Display error from pIDBInitialize.  
        goto EXIT;  
    }  
  
    // Releasing this has no effect on the SQL Server connection  
    // of the data source object because of the reference maintained by  
    // pIDBCreateSession.  
    pIDBInitialize->Release();  
    pIDBInitialize = NULL;  
  
    // The session created next receives the SQL Server connection of  
    // the data source object. No new connection is established.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd1)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
    // A new connection to the instance of SQL Server is established to support the  
    // next session object created. On successful completion, the  
    // application has two active connections on the SQL Server.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd2)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
    // pICreateCmd1 has the data source connection. Because the  
    // reference on the IDBCreateSession interface of the data source  
    // has not been released, releasing the reference on the session  
    // object does not terminate a connection to the instance of SQL Server.  
    // However, the connection of the data source object is now   
    // available to another session object. After a successful call to   
    // Release, the application still has two active connections to the   
    // instance of SQL Server.  
    pICreateCmd1->Release();  
    pICreateCmd1 = NULL;  
  
    // The next session created gets the SQL Server connection  
    // of the data source object. The application has two active  
    // connections to the instance of SQL Server.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd3)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
EXIT:  
    // Even on error, this does not terminate a SQL Server connection   
    // because pICreateCmd1 has the connection of the data source   
    // object.  
    if (pICreateCmd1 != NULL)  
        pICreateCmd1->Release();  
  
    // Releasing the reference on pICreateCmd2 terminates the SQL  
    // Server connection supporting the session object. The application  
    // now has only a single active connection on the instance of SQL Server.  
    if (pICreateCmd2 != NULL)  
        pICreateCmd2->Release();  
  
    // Even on error, this does not terminate a SQL Server connection   
    // because pICreateCmd3 has the connection of the   
    // data source object.  
    if (pICreateCmd3 != NULL)  
        pICreateCmd3->Release();  
  
    // On release of the last reference on a data source interface, the  
    // connection of the data source object to the instance of SQL Server is broken.  
    // The example application now has no SQL Server connections active.  
    if (pIDBCreateSession != NULL)  
        pIDBCreateSession->Release();  
  
    // Called only if an error occurred while attempting to get a   
    // reference on the IDBCreateSession interface of the data source.  
    // If so, the call to IDBInitialize::Uninitialize terminates the   
    // connection of the data source object to the instance of SQL Server.  
    if (pIDBInitialize != NULL)  
    {  
        if (FAILED(pIDBInitialize->Uninitialize()))  
        {  
            // Uninitialize is not required, but it fails if an  
            // interface has not been released. Use it for  
            // debugging.  
        }  
        pIDBInitialize->Release();  
    }  
  
    if (g_pIMalloc != NULL)  
        g_pIMalloc->Release();  
  
    CoUninitialize();  
  
    return (0);  
}  
```  
  
 La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di oggetti di sessione del provider OLE DB Native client a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può generare un sovraccarico significativo per le applicazioni che creano e rilasciano continuamente oggetti sessione. L'overhead può essere ridotto al minimo gestendo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] modo efficiente gli oggetti sessione del provider OLE DB di Native Client. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le applicazioni del provider OLE DB Native client possono mantenere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attiva la connessione di un oggetto sessione mantenendo un riferimento su almeno un'interfaccia dell'oggetto.  
  
 La gestione di un pool di riferimenti agli oggetti di creazione del comando consente ad esempio di mantenere attive le connessioni a tali oggetti di sessione nel pool. Al momento della richiesta degli oggetti di sessione, il codice di manutenzione del pool passa un puntatore di interfaccia **IDBCreateCommand** valido al metodo dell'applicazione che richiede la sessione. Quando il metodo dell'applicazione non richiede più la sessione, restituisce di nuovo il puntatore all'interfaccia al codice di manutenzione del pool, anziché rilasciare il riferimento dell'applicazione all'oggetto di creazione del comando.  
  
> [!NOTE]  
>  Nell'esempio precedente l'interfaccia **IDBCreateCommand** viene usata perché l'interfaccia **ICommand** implementa il metodo **GetDBSession**, ovvero l'unico metodo nell'ambito del comando o del set di righe che consenta a un oggetto di determinare la sessione nella quale è stato creato. Pertanto, solo ed esclusivamente un oggetto comando consente a un'applicazione di recuperare un puntatore all'oggetto origine dati dal quale possono essere create sessioni aggiuntive.  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetti di origine dati &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
