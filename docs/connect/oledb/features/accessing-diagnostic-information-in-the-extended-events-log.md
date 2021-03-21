---
title: Accesso alle informazioni di diagnostica nel log degli eventi estesi | Microsoft Docs
description: Traccia di OLE DB Driver per SQL Server e accesso alle informazioni di diagnostica nel log degli eventi estesi
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf877daeda48af4bfc809d1dbf71c6bef604c96f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742701"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accesso alle informazioni di diagnostica nel registro eventi estesi
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  A partire da [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], il driver OLE DB per SQL Server e la traccia di accesso ai dati, [Data Access Tracing](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)) (Traccia di accesso ai dati), sono stati aggiornati per facilitare l'ottenimento di informazioni di diagnostica sugli errori di connessione dal buffer circolare della connettività e informazioni sulle prestazioni dell'applicazione dal registro eventi esteso.  
  
 Per ulteriori informazioni sulla lettura del log degli eventi estesi, vedere [View Event Session Data](../../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md) (Visualizzare i dati di una sessione di eventi). 

  
> [!NOTE]  
>  Questa funzionalità è destinata esclusivamente alla risoluzione dei problemi e a fini diagnostici e potrebbe non essere appropriata per il controllo o per scopi di sicurezza.  
  
## <a name="remarks"></a>Osservazioni  
 Per le operazioni di connessione, OLE DB Driver per SQL Server invierà un ID connessione client. In caso di errore di connessione, è possibile accedere al buffer circolare della connettività (vedere la pagina relativa alla [risoluzione dei problemi di connettività in SQL Server 2008 con il buffer circolare della connettività](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)), individuare il campo **ClientConnectionID** e ottenere le informazioni di diagnostica sul problema di connessione. Gli ID di connessione client vengono registrati nel buffer circolare solo se si verifica un errore. In caso di errore di connessione prima dell'invio del pacchetto di preaccesso, l'ID di connessione client non verrà generato. L'ID di connessione client è un GUID a 16 byte. È possibile trovare l'ID di connessione client anche nella destinazione di output degli eventi estesi, se l'azione **client_connection_id** viene aggiunta agli eventi in una sessione di eventi estesi. È possibile abilitare la traccia di accesso ai dati, eseguire di nuovo il comando di connessione e osservare il campo **ClientConnectionID** nella traccia di accesso ai dati per un'operazione con errori, se è necessaria ulteriore assistenza diagnostica.  
   
  
 OLE DB Driver per SQL Server invia anche un ID attività specifico del thread. L'ID attività viene acquisito nelle sessioni di eventi estesi se avviate con l'opzione TRACK_CAUSAILITY abilitata. Per problemi di prestazioni con una connessione attiva, è possibile ottenere l'ID attività dalla traccia del client (campo **ActivityID**) e quindi individuare l'ID attività nell'output degli eventi estesi. L'ID attività negli eventi estesi è un GUID a 16 byte (diverso dal GUID per l'ID di connessione client) accodato con un numero di sequenza a quattro byte. Il numero di sequenza rappresenta l'ordine di una richiesta all'interno di un thread e indica l'ordine relativo di istruzioni batch e RPC per il thread. L'**ActivityID** viene inviato facoltativamente per istruzioni batch SQL e richieste RPC quando la traccia di accesso ai dati è abilitata e il diciottesimo bit nella configurazione della traccia di accesso ai dati è abilitato.  
  
 Di seguito è riportato un esempio di utilizzo di [!INCLUDE[tsql](../../../includes/tsql-md.md)] per avviare una sessione di eventi estesi che verrà archiviata in un buffer circolare e verrà registrato l'ID attività inviato da un client in operazioni RPC e batch.  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
  
```  
  
## <a name="control-file"></a>File di controllo  
 Il contenuto del file di controllo di OLE DB Driver per SQL Server (ctrl.guid) è:  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x630ff  0   MSDADIAG.ETW
{EE7FB59C-D3E8-9684-AEAC-B214EFD91B31}  0x630ff  0   MSOLEDBSQL.1  
```  
  
## <a name="mof-file"></a>File MOF  
 Il contenuto del file MOF di OLE DB Driver per SQL Server è:  
  
```  
#pragma classflags("forceupdate")  
#pragma namespace ("\\\\.\\Root\\WMI")  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSDADIAG.ETW  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F3-3CC6-0B9C-6651-9649CCE5C752}"),  
 DisplayName("msdadiag"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace : Bid2Etw_MSDADIAG_ETW  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextA : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextW : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSOLEDBSQL.1  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1"),  
 Guid("{EE7FB59C-D3E8-9684-AEAC-B214EFD91B31}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1"),  
 Guid("{EE7FB59D-D3E8-9684-AEAC-B214EFD91B31}"),  
 DisplayName("MSOLEDBSQL.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace : Bid2Etw_MSOLEDBSQL_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace_TextA : Bid2Etw_MSOLEDBSQL_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace_TextW : Bid2Etw_MSOLEDBSQL_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione degli errori](../../oledb/ole-db-errors/errors.md)  
  
