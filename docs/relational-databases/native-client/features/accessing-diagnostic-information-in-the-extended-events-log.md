---
description: Accesso alle informazioni di diagnostica SQL Server Native Client nel log degli eventi estesi
title: Informazioni di diagnostica nel log degli eventi estesi
ms.date: 03/14/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: native-client
ms.topic: reference
ms.assetid: aaa180c2-5e1a-4534-a125-507c647186ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dbc9398f964089b33d24b499fe25d9ec559189b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463402"
---
# <a name="accessing-sql-server-native-client-diagnostic-information-in-the-extended-events-log"></a>Accesso alle informazioni di diagnostica SQL Server Native Client nel log degli eventi estesi
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  A partire da [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] , [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client e la traccia di accesso ai dati ([traccia di accesso ai dati](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100))) sono stati aggiornati per semplificare l'ottenimento di informazioni di diagnostica sugli errori di connessione dal buffer circolare della connettività e dalle informazioni sulle prestazioni dell'applicazione dal registro eventi estesi.  
  
 Per ulteriori informazioni sulla lettura del log degli eventi estesi, vedere [View Event Session Data](../../extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md) (Visualizzare i dati di una sessione di eventi).  
  
> [!NOTE]  
>  Questa funzionalità è destinata esclusivamente alla risoluzione dei problemi e a fini diagnostici e potrebbe non essere appropriata per il controllo o per scopi di sicurezza.  
  
## <a name="remarks"></a>Osservazioni  
 Per le operazioni di connessione, tramite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verrà inviato un ID di connessione client. In caso di errore di connessione, è possibile accedere al buffer circolare della connettività, [Connectivity troubleshooting in SQL Server 2008 with the Connectivity Ring Buffer](https://techcommunity.microsoft.com/t5/sql-server/connectivity-troubleshooting-in-sql-server-2008-with-the/ba-p/383393) (Risoluzione dei problemi relativi alla connettività in SQL Server 2008 con il buffer circolare della connettività), individuare il campo **ClientConnectionID** e ottenere le informazioni di diagnostica sul problema di connessione. Gli ID di connessione client vengono registrati nel buffer circolare solo se si verifica un errore. In caso di errore di connessione prima dell'invio del pacchetto di preaccesso, l'ID di connessione client non verrà generato. L'ID di connessione client è un GUID a 16 byte. È possibile trovare l'ID di connessione client anche nella destinazione di output degli eventi estesi, se l'azione **client_connection_id** viene aggiunta agli eventi in una sessione di eventi estesi. È possibile abilitare la traccia di accesso ai dati, eseguire di nuovo il comando di connessione e osservare il campo **ClientConnectionID** nella traccia di accesso ai dati per un'operazione con errori, se è necessaria ulteriore assistenza diagnostica.  
  
 Se si utilizza ODBC in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client e una connessione ha esito positivo, è possibile ottenere l'ID connessione client utilizzando l'attributo **SQL_COPT_SS_CLIENT_CONNECTION_ID** con [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
 Tramite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client viene inviato anche un ID attività specifico del thread. L'ID attività viene acquisito nelle sessioni di eventi estesi se avviate con l'opzione TRACK_CAUSAILITY abilitata. Per problemi di prestazioni con una connessione attiva, è possibile ottenere l'ID attività dalla traccia del client (campo **ActivityID**) e quindi individuare l'ID attività nell'output degli eventi estesi. L'ID attività negli eventi estesi è un GUID a 16 byte (diverso dal GUID per l'ID di connessione client) accodato con un numero di sequenza a quattro byte. Il numero di sequenza rappresenta l'ordine di una richiesta all'interno di un thread e indica l'ordine relativo di istruzioni batch e RPC per il thread. L'**ActivityID** viene inviato facoltativamente per istruzioni batch SQL e richieste RPC quando la traccia di accesso ai dati è abilitata e il diciottesimo bit nella configurazione della traccia di accesso ai dati è abilitato.  
  
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
 In [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], il contenuto del file di controllo di SQL Server Native Client (ctrl.guid.snac11) è:  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x00000000  0   MSDADIAG.ETW  
{2DA81B52-908E-7DB6-EF81-76856BB47C4F}  0xFFFFFFFF  0   SQLNCLI11.1  
```  
  
## <a name="mof-file"></a>File MOF  
 In [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], il contenuto del file MOF di SQL Server Native Client è:  
  
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
//  SQLNCLI11.1  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B52-908E-7DB6-EF81-76856BB47C4F}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B53-908E-7DB6-EF81-76856BB47C4F}"),  
 DisplayName("SQLNCLI11.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace : Bid2Etw_SQLNCLI11_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextA : Bid2Etw_SQLNCLI11_1_Trace  
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
 Description("SQLNCLI11.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextW : Bid2Etw_SQLNCLI11_1_Trace  
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
 [Gestione di errori e messaggi](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
