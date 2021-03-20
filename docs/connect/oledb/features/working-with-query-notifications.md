---
title: Uso delle notifiche delle query | Microsoft Docs
description: Informazioni sulle notifiche delle query che consentono di comunicare alle applicazioni che i dati sono stati modificati in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], query notifications
- rowsets [SQL Server], notifications
- OLE DB Driver for SQL Server, query notifications
- notifications [OLE DB Driver for SQL Server]
- query notifications [SQL Server], OLE DB Driver for SQL Server
- canceling rowset changes
- IRowsetNotify interface
- MSOLEDBSQL, query notifications
- OLE DB Driver for SQL Server, query notifications
- consumer notification for rowset changes [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b21f3a8b17c5174c4e9f8b037aa891bdfcc6d10
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751071"
---
# <a name="working-with-query-notifications"></a>Utilizzo delle notifiche delle query

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Le notifiche delle query sono state introdotte in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] e in OLE DB Driver per SQL Server. Basate sull'infrastruttura di Service Broker introdotta in SQL Server 2005 (9.x), tali notifiche consentono di segnalare modifiche dei dati alle applicazioni. Questa funzionalità è particolarmente utile per le applicazioni che forniscono una cache di informazioni da un database, ad esempio un'applicazione Web, e devono ricevere notifica della modifica dei dati di origine.

L'uso delle notifiche delle query consente di richiedere una notifica entro un determinato periodo di timeout quando i dati sottostanti di una query cambiano. La richiesta specifica le opzioni di notifica che includono il nome del servizio, il testo del messaggio e il valore di timeout per il server. Le notifiche vengono recapitate tramite una coda di Service Broker di cui le applicazioni possono eseguire il polling per recuperare le notifiche disponibili.

La sintassi delle opzioni delle notifiche delle query è la seguente:

`service=<service-name>[;(local database=<database> | broker instance=<broker instance>)]`

 Ad esempio:

`service=mySSBService;local database=mydb`

Le sottoscrizioni delle notifiche sopravvivono al processo che le ha avviate. Ciò è dovuto al fatto che un'applicazione può creare una sottoscrizione delle notifiche e quindi terminare. La sottoscrizione rimane valida e la notifica viene creata se i dati vengono modificati entro il periodo di timeout specificato al momento della creazione della sottoscrizione. Una notifica viene identificata dalla query eseguita, dalle opzioni di notifica e dal testo del messaggio. È possibile annullarla impostandone il valore di timeout su zero.

Le notifiche vengono inviate una sola volta. Per avere notifiche continue delle modifiche dei dati, creare una nuova sottoscrizione rieseguendo la query al termine dell'elaborazione di ogni notifica.

OLE DB Driver per le applicazioni SQL Server in genere riceve le notifiche usando il comando [!INCLUDE[tsql](../../../includes/tsql-md.md)] [RECEIVE](../../../t-sql/statements/receive-transact-sql.md). Questo comando viene usato per leggere le notifiche dalla coda associata al servizio specificato nelle opzioni delle notifiche.

> [!NOTE]
> I nomi di tabella devono essere qualificati nelle query per le quali è richiesta la notifica. Ad esempio: `dbo.myTable`. I nomi di tabella devono essere qualificati con nomi in due parti. La sottoscrizione non è valida se vengono utilizzati nomi in tre o quattro parti.

L'infrastruttura della notifica si basa su una caratteristica di accodamento introdotta in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. In genere, le notifiche generate nel server vengono inviate tramite queste code in modo da essere elaborate in un secondo momento.

Per usare le notifiche delle query, è necessario che nel server siano disponibili una coda e un servizio. Tali elementi possono essere creati usando il comando [!INCLUDE[tsql](../../../includes/tsql-md.md)], come illustrato di seguito:

```sql
CREATE QUEUE myQueue
CREATE SERVICE myService ON QUEUE myQueue
([https://schemas.microsoft.com/SQL/Notifications/PostQueryNotification])
```

> [!NOTE]
> Come mostrato sopra, il servizio deve usare il contratto predefinito.

## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server

OLE DB Driver per SQL Server supporta le notifiche del consumer durante la modifica del set di righe. Il consumer riceve una notifica a ogni fase di modifica dei set di righe e a ogni tentativo di modifica.

> [!NOTE]
> Il passaggio di una query di notifica al server mediante **ICommand::Execute** rappresenta l'unico modo valido per sottoscrivere le notifiche delle query con il driver OLE DB per SQL Server.

### <a name="dbpropset_sqlserverrowset-property-set"></a>Set di proprietà DBPROPSET_SQLSERVERROWSET

Per supportare le notifiche delle query tramite OLE DB, il driver OLE DB per SQL Server aggiunge le nuove proprietà seguenti al set di proprietà `DBPROPSET_SQLSERVERROWSET`.

|Nome|Tipo|Descrizione|
|----------|----------|-----------------|
|SSPROP_QP_NOTIFICATION_TIMEOUT|VT_UI4|Numero di secondi durante i quali la notifica di query deve rimanere attiva.<br /><br /> Il valore predefinito è 432,000 secondi (5 giorni). Il valore minimo è 1 secondo e il valore massimo è 2^31-1 secondi.|
|SSPROP_QP_NOTIFICATION_MSGTEXT|VT_BSTR|Testo del messaggio di notifica. Tale testo è definito dall'utente e non ha un formato predefinito.<br /><br /> Per impostazione predefinita, la stringa è vuota. Specificare un messaggio usando da 1 a 2000 caratteri.|
|SSPROP_QP_NOTIFICATION_OPTIONS|VT_BSTR|Opzioni di notifica delle query. Tali opzioni vengono specificate in una stringa con la sintassi *nome*=*valore*. L'utente è responsabile della creazione del servizio e della lettura delle notifiche all'esterno della coda.<br /><br /> Il valore predefinito è una stringa vuota.|

Viene sempre eseguito il commit della sottoscrizione delle notifiche, indipendentemente dall'esecuzione dell'istruzione in una transazione utente o in modalità di commit automatico o se la transazione in cui è stata eseguita l'istruzione sia stata sottoposta a commit o a rollback. La notifica server viene generata in seguito a una delle condizioni di notifica non valide seguenti: modifica dello schema o dei dati sottostanti o raggiungimento del periodo di timeout, a seconda dell'evento che si verifica per primo. 

Le registrazioni della notifica vengono eliminate subito dopo essere state generate. Al momento della ricezione delle notifiche, l'applicazione deve pertanto effettuare nuovamente la sottoscrizione se si vogliono ottenere ulteriori aggiornamenti.

Un'altra connessione o un altro thread può verificare la presenza di notifiche nella coda di destinazione. Ad esempio:

```sql
WAITFOR (RECEIVE * FROM MyQueue); -- Where MyQueue is the queue name.
```

> [!NOTE]
> `SELECT *` non elimina la voce dalla coda, al contrario di `RECEIVE * FROM`. Di conseguenza un thread del server viene bloccato se la coda è vuota. Se al momento della chiamata sono presenti voci della coda, vengono restituite immediatamente. In caso contrario, la chiamata attende fino a quando non viene creata una voce della coda.

```sql
RECEIVE * FROM MyQueue
```

Questa istruzione restituisce immediatamente un set di risultati vuoto se la coda è vuota. In caso contrario, restituisce tutte le notifiche della coda.

Se `SSPROP_QP_NOTIFICATION_MSGTEXT` e `SSPROP_QP_NOTIFICATION_OPTIONS` non sono Null e non sono vuoti, l'intestazione TDS per le notifiche delle query che contiene le tre proprietà definite in precedenza viene inviata al server. Questa operazione si verifica a ogni esecuzione del comando. Se una di esse è Null o vuota, l'intestazione non viene inviata e viene generato `DB_E_ERRORSOCCURRED` oppure `DB_S_ERRORSOCCURRED` se le proprietà sono entrambe contrassegnate come facoltative. Il valore dello stato viene quindi impostato su `DBPROPSTATUS_BADVALUE`. La convalida viene eseguita in fase di esecuzione e di preparazione. Analogamente, `DB_S_ERRORSOCCURED` viene generato quando le proprietà della notifica di query sono impostate per le connessioni alle versioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Il valore dello stato in questo caso è `DBPROPSTATUS_NOTSUPPORTED`.

L'avvio di una sottoscrizione non garantisce il corretto recapito dei messaggi successivi. Inoltre, non viene effettuato alcun controllo della validità del nome di servizio specificato.

> [!NOTE]
> La preparazione delle istruzioni non causerà mai l'avvio della sottoscrizione. L'avvio viene eseguito solo dall'esecuzione di istruzioni. L'uso dei servizi principali OLE DB non influisce sulle notifiche delle query.

Per altre informazioni sul set di proprietà `DBPROPSET_SQLSERVERROWSET`, vedere [Proprietà e comportamenti dei set di righe](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md).

## <a name="see-also"></a>Vedere anche

[Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)
