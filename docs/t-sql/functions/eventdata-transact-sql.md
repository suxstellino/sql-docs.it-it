---
description: EVENTDATA (Transact-SQL)
title: EVENTDATA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EVENTDATA
- fn_event_data
- EVENTDATA_TSQL
- fn_event_data_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server instance event data [SQL Server]
- event notifications [SQL Server], event status
- events [SQL Server], status information
- EVENTDATA function
- status information [SQL Server], events
- DDL triggers, returning event data
ms.assetid: 03a80e63-6f37-4b49-bf13-dc35cfe46c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: a9815f7cc0cdcb86ffc716b53c9cbe579aca9824
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350880"
---
# <a name="eventdata-transact-sql"></a>EVENTDATA (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce informazioni sugli eventi del server o del database. La funzione `EVENTDATA` viene chiamata quando viene generata una notifica degli eventi e il Service Broker specificato riceve i risultati. Anche un trigger DDL o LOGON supporta l'uso interno di `EVENTDATA`.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
EVENTDATA( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni  
`EVENTDATA` restituisce dati solo se usata direttamente in un trigger DDL o LOGON. `EVENTDATA` restituisce Null se chiamata da altre routine, anche se queste routine sono chiamate da un trigger DDL o LOGON.
  
I dati restituiti da `EVENTDATA` non sono validi dopo una transazione

+ che chiama `EVENTDATA` in modo esplicito
+ che chiama `EVENTDATA` in modo implicito
+ che esegue il commit
+ di cui viene eseguito il rollback  
  
> [!CAUTION]  
>  `EVENTDATA` restituisce dati XML, inviati al client in formato Unicode che usa 2 byte per ogni carattere. `EVENTDATA` restituisce codice XML che può rappresentare questi punti di codice Unicode:  
>   
>  `0x0009`  
>   
>  `0x000A`  
>   
>  `0x000D`  
>   
>  `>= 0x0020 && <= 0xD7FF`  
>   
>  `>= 0xE000 && <= 0xFFFD`  
>   
>  Alcuni caratteri che possono essere inclusi in identificatori e dati [!INCLUDE[tsql](../../includes/tsql-md.md)] non sono rappresentabili o consentiti in XML. Sui caratteri o i dati con elementi di codice non indicati nell'elenco precedente viene eseguito il mapping a un punto interrogativo (?).  
  
Le password non vengono visualizzate quando si eseguono istruzioni `CREATE LOGIN` o `ALTER LOGIN`. Ciò consente di proteggere la sicurezza degli account di accesso.  
  
## <a name="schemas-returned"></a>Schemi restituiti  
EVENTDATA restituisce un valore con tipo di dati **xml**. Per impostazione predefinita, la definizione dello schema per tutti gli eventi è installata in questa directory: [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools\Binn\schemas\sqlserver\2006\11\events\events.xsd.  
  
Lo schema di eventi è disponibile anche nella pagina Web [Microsoft SQL Server XML Schemas](https://go.microsoft.com/fwlink/?LinkID=31850) (Schemi XML di Microsoft SQL Server).  
  
Per estrarre lo schema di un evento specifico, cercare nello schema il tipo complesso `EVENT_INSTANCE_<event_type>`. Ad esempio, per estrarre lo schema per l'evento `DROP_TABLE`, cercare nello schema `EVENT_INSTANCE_DROP_TABLE`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-querying-event-data-in-a-ddl-trigger"></a>R. Recupero tramite query dei dati di eventi in un trigger DDL  
Questo esempio crea un trigger DDL che impedisce la creazione di nuove tabelle di database. L'uso di XQuery sui dati XML generati da `EVENTDATA` consente di acquisire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che attiva il trigger. Per altre informazioni, vedere [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md).  
  
> [!NOTE]  
>  Quando si usa **Risultati in formato griglia** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per recuperare l'elemento `<TSQLCommand>`, le interruzioni di riga nel testo del comando non vengono visualizzate. Usare invece **Risultati in formato testo**.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER safety   
ON DATABASE   
FOR CREATE_TABLE   
AS   
    PRINT 'CREATE TABLE Issued.'  
    SELECT EVENTDATA().value  
        ('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
   RAISERROR ('New tables cannot be created in this database.', 16, 1)   
   ROLLBACK  
;  
GO  
--Test the trigger.  
CREATE TABLE NewTable (Column1 INT);  
GO  
--Drop the trigger.  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
> [!NOTE]  
>  Per la restituzione dei dati evento, usare il metodo XQuery **value()** invece del metodo **query()**. Il metodo **query()** restituisce istanze XML e CR/LF (ritorno a capo/avanzamento riga) trasformate con il carattere di escape e commerciale (&amp;) nell'output, mentre il metodo **value()** rende le istanze CR/LF invisibili nell'output.  
  
### <a name="b-creating-a-log-table-with-event-data-in-a-ddl-trigger"></a>B. Creazione di una tabella di log con dati di eventi in un trigger DDL  
Questo esempio crea una tabella per l'archiviazione di informazioni su tutti gli eventi a livello di database e tale tabella viene popolata con un trigger DDL. L'uso di XQuery sui dati XML generai da `EVENTDATA` consente di acquisire il tipo di evento e l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```sql 
USE AdventureWorks2012;  
GO  
CREATE TABLE ddl_log (PostTime DATETIME, DB_User NVARCHAR(100), Event NVARCHAR(100), TSQL NVARCHAR(2000));  
GO  
CREATE TRIGGER log   
ON DATABASE   
FOR DDL_DATABASE_LEVEL_EVENTS   
AS  
DECLARE @data XML  
SET @data = EVENTDATA()  
INSERT ddl_log   
   (PostTime, DB_User, Event, TSQL)   
   VALUES   
   (GETDATE(),   
   CONVERT(NVARCHAR(100), CURRENT_USER),   
   @data.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),   
   @data.value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'NVARCHAR(2000)') ) ;  
GO  
--Test the trigger.  
CREATE TABLE TestTable (a INT);  
DROP TABLE TestTable ;  
GO  
SELECT * FROM ddl_log ;  
GO  
--Drop the trigger.  
DROP TRIGGER log  
ON DATABASE;  
GO  
--Drop table ddl_log.  
DROP TABLE ddl_log;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la funzione EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md)   
 [Trigger DDL](../../relational-databases/triggers/ddl-triggers.md)   
 [Notifiche degli eventi](../../relational-databases/service-broker/event-notifications.md)   
 [Trigger LOGON](../../relational-databases/triggers/logon-triggers.md)  
  
  
