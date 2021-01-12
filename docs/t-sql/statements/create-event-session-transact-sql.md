---
description: CREATE EVENT SESSION (Transact-SQL)
title: CREATE EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE EVENT SESSION
- SESSION
- EVENT SESSION
- SESSION_TSQL
- EVENT_SESSION_TSQL
- CREATE_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- CREATE EVENT SESSION statement
ms.assetid: 67683027-2b0f-47aa-b223-604731af8b4d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 006ef49a390045d5c6d7d8c8c174da90e37f4f91
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100851"
---
# <a name="create-event-session-transact-sql"></a>CREATE EVENT SESSION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea una sessione degli eventi estesi che identifica l'origine degli eventi, le destinazioni delle sessione degli eventi e le opzioni della sessione degli eventi.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EVENT SESSION event_session_name
ON { SERVER | DATABASE }
{  
    <event_definition> [ ,...n]
    [ <event_target_definition> [ ,...n] ]
    [ WITH ( <event_session_options> [ ,...n] ) ]
}
;

<event_definition>::=
{
    ADD EVENT [event_module_guid].event_package_name.event_name
         [ ( {
                 [ SET { event_customizable_attribute = <value> [ ,...n] } ]
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n] } ) ]
                 [ WHERE <predicate_expression> ]
        } ) ]
}

<predicate_expression> ::=
{
    [ NOT ] <predicate_factor> | {( <predicate_expression> ) }
    [ { AND | OR } [ NOT ] { <predicate_factor> | ( <predicate_expression> ) } ]
    [ ,...n ]
}  
  
<predicate_factor>::=
{
    <predicate_leaf> | ( <predicate_expression> )
}

<predicate_leaf>::=
{
      <predicate_source_declaration> { = | < > | ! = | > | > = | < | < = } <value>
    | [event_module_guid].event_package_name.predicate_compare_name ( <predicate_source_declaration>, <value> )
}

<predicate_source_declaration>::=
{
    event_field_name | ( [event_module_guid].event_package_name.predicate_source_name )
}

<value>::=
{
    number | 'string'
}

<event_target_definition>::=
{
    ADD TARGET [event_module_guid].event_package_name.target_name
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]
}

<event_session_options>::=
{  
    [    MAX_MEMORY = size [ KB | MB ] ]
    [ [,] EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } ]
    [ [,] MAX_DISPATCH_LATENCY = { seconds SECONDS | INFINITE } ]
    [ [,] MAX_EVENT_SIZE = size [ KB | MB ] ]
    [ [,] MEMORY_PARTITION_MODE = { NONE | PER_NODE | PER_CPU } ]
    [ [,] TRACK_CAUSALITY = { ON | OFF } ]
    [ [,] STARTUP_STATE = { ON | OFF } ]
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*event_session_name* Nome definito dall'utente per la sessione eventi. *event_session_name* è un valore alfanumerico, può essere composto da un massimo di 128 caratteri, deve essere univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e deve essere conforme alle regole relative agli [identificatori](../../relational-databases/databases/database-identifiers.md).

ADD EVENT [ *event_module_guid* ].*event_package_name*.*event_name* Evento da associare alla sessione eventi, dove:

- *event_module_guid* è l'identificatore univoco globale (GUID) del modulo contenente l'evento.
- *event_package_name* è il pacchetto che contiene l'oggetto dell'azione.
- *event_name* è l'oggetto dell'evento.

Gli eventi vengono visualizzati nella vista sys.dm_xe_objects come object_type "event".

SET { *event_customizable_attribute*= \<value> [ ,...*n*] } Consente attributi personalizzabili per l'evento da impostare. Gli attributi personalizzabili vengono visualizzati nella vista sys.dm_xe_object_columns come column_type 'customizable' e object_name = *event_name*.

ACTION ( { [*event_module_guid*].*event_package_name*.*action_name* [ **,** ...*n*] }) Azione da associare alla sessione eventi, dove:

- *event_module_guid* è l'identificatore univoco globale (GUID) del modulo contenente l'evento.
- *event_package_name* è il pacchetto che contiene l'oggetto dell'azione.
- *action_name* è l'oggetto dell'azione.

Le azioni vengono visualizzate nella vista sys.dm_xe_objects come object_type "action".

WHERE \<predicate_expression> Specifica l'espressione del predicato usata per determinare se un evento deve essere elaborato. Se \<predicate_expression> è true l'evento viene elaborato ulteriormente dalle azioni e dalle destinazioni della sessione, mentre se \<predicate_expression> è false l'evento viene eliminato dalla sessione prima di essere elaborato dalle azioni e dalle destinazioni della sessione. Le espressioni del predicato possono essere composte da un massimo di 3000 caratteri, pertanto gli argomenti di tipo stringa risultano limitati.

*event_field_name* Nome del campo relativo all'evento che consente di identificare l'origine del predicato.

[*event_module_guid*].*event_package_name*.*predicate_source_name* Nome dell'origine del predicato globale dove:

- *event_module_guid* è l'identificatore univoco globale (GUID) del modulo contenente l'evento.
- *event_package_name* è il pacchetto che contiene l'oggetto del predicato.
- *predicate_source_name* è definito nella vista sys.dm_xe_objects come object_type 'pred_source'.

[*event_module_guid*].*event_package_name*.*predicate_compare_name* Nome dell'oggetto del predicato da associare all'evento, dove:

- *event_module_guid* è l'identificatore univoco globale (GUID) del modulo contenente l'evento.
- *event_package_name* è il pacchetto che contiene l'oggetto del predicato.
- *predicate_compare_name* è un'origine globale definita nella vista sys.dm_xe_objects come object_type 'pred_compare'.

*number* Qualsiasi tipo numerico incluso **decimal**. Le limitazioni sono la mancanza di memoria fisica disponibile o un numero troppo grande per essere rappresentato come un numero intero a 64 bit.

'*string*' Stringa ANSI o Unicode come richiesto dal paragone del predicato. Non viene eseguita alcuna conversione del tipo di stringa implicita per le funzioni del paragone del predicato. Il passaggio del tipo non corretto comporta un errore.

ADD TARGET [*event_module_guid*].*event_package_name*.*target_name* Destinazione da associare alla sessione eventi, dove:

- *event_module_guid* è l'identificatore univoco globale (GUID) del modulo contenente l'evento.
- *event_package_name* è il pacchetto che contiene l'oggetto dell'azione.
- *target_name* è la destinazione. Le destinazioni vengono visualizzate nella vista sys.dm_xe_objects come object_type 'target'.

SET { *target_parameter_name*= \<value> [, ...*n*] } Imposta un parametro di destinazione. I parametri di destinazione vengono visualizzati nella vista sys.dm_xe_object_columns come column_type 'customizable' e object_name = *target_name*.

> [!IMPORTANT]
> Se si utilizza il buffer circolare come destinazione, si consiglia di impostare il parametro di destinazione max_memory su 2048 kilobyte (KB) per evitare il possibile troncamento dei dati dell'output XML. Per altre informazioni sull'uso dei diversi tipi di destinazione, vedere [Destinazioni degli eventi estesi di SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).

WITH ( \<event_session_options> [ ,...*n*] ) Specifica le opzioni da usare con la sessione dell'evento.

MAX_MEMORY =*size* [ KB | **MB** ] Specifica la quantità di memoria allocata alla sessione per la memorizzazione degli eventi nel buffer. Il valore predefinito è 4 MB. *size* è un numero intero e può essere espresso in kilobyte (KB) o megabyte (MB). La quantità massima non può superare i 2 GB, vale a dire meno di 2048 MB. Non è tuttavia consigliabile usare valori di memoria espressi in GB.

EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } Specifica la modalità di memorizzazione dell'evento da usare per la gestione della perdita di eventi.

**ALLOW_SINGLE_EVENT_LOSS** Un evento può essere perso dalla sessione. Un evento singolo viene eliminato solo quando tutti i buffer dell'evento sono pieni. La perdita di un singolo evento i buffer sono pieni garantisce un livello accettabile delle prestazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], riducendo al minimo il rischio di perdita dei dati nel flusso di eventi elaborati.

ALLOW_MULTIPLE_EVENT_LOSS È possibile che vengano persi i buffer degli eventi pieni contenenti più eventi. Il numero di eventi persi dipende dalla dimensione della memoria allocata alla sessione, dalla partizione della memoria e dalla dimensione degli eventi nel buffer. Questa opzione riduce al minimo l'impatto sulle prestazioni dei server quando i buffer degli eventi vengono riempiti rapidamente, ma numerosi eventi della sessione possono andare persi.

NO_EVENT_LOSS Non è consentita alcuna perdita di eventi. Questa opzione assicura che tutti gli eventi generati vengano mantenuti. L'utilizzo di questa opzione forza tutte le attività che attivano eventi ad aspettare fino a che lo spazio è disponibile in un buffer degli eventi. Questo può condurre a problemi di prestazione mentre la sessione dell'evento è attiva. Le connessioni utente potrebbero bloccarsi in attesa che gli eventi vengano scaricati dal buffer.

MAX_DISPATCH_LATENCY = { *seconds* SECONDS | **INFINITE** } Specifica il tempo di memorizzazione degli eventi nel buffer in memoria prima che vengano inviati alle destinazioni della sessione. Per impostazione predefinita, questo valore è impostato su 30 secondi.

*seconds* SECONDS Tempo di attesa, espresso in secondi, prima che venga avviato lo scaricamento dei buffer nelle destinazioni. *seconds* è un numero intero. Il valore di latenza minimo è 1 secondo. È tuttavia possibile utilizzare il valore 0 per specificare una latenza infinita.

**INFINITE** Scarica i buffer nelle destinazioni solo quando i buffer sono pieni o alla chiusura della sessione dell'evento.

> [!NOTE]
> MAX_DISPATCH_LATENCY = 0 SECONDS è equivalente a MAX_DISPATCH_LATENCY = INFINITE.

MAX_EVENT_SIZE =*size* [ KB | **MB** ] Specifica la dimensione massima consentita per gli eventi. MAX_EVENT_SIZE deve essere impostato solo per consentire singoli eventi di dimensioni superiori a quelle di MAX_MEMORY. L'impostazione su un valore inferiore a quello di MAX_MEMORY genera un errore. *size* è un numero intero e può essere espresso in kilobyte (KB) o megabyte (MB). Se *size* è espresso in kilobyte, la dimensione minima consentita è di 64 KB. Quando viene impostato MAX_EVENT_SIZE, vengono creati due buffer con dimensioni pari a *size* in aggiunta a MAX_MEMORY. Ciò significa che la memoria totale utilizzata per la memorizzazione degli eventi nel buffer è MAX_MEMORY + 2 * MAX_EVENT_SIZE.

MEMORY_PARTITION_MODE = { **NONE** | PER_NODE | PER_CPU } Specifica la posizione di creazione dei buffer degli eventi.

**NONE** Nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene creato un unico set di buffer.

PER NODE Viene creato un set di buffer per ogni nodo NUMA.

PER_CPU Viene creato un set di buffer per ogni CPU.

TRACK_CAUSALITY = { ON | **OFF** } Specifica se viene tenuta traccia della causalità. Se abilitato, la causalità consente la correlazione di eventi correlati in diverse connessioni server.

STARTUP_STATE = { ON | **OFF** } Specifica se avviare automaticamente questa sessione eventi all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Se `STARTUP_STATE = ON`, la sessione dell'evento verrà avviata solo se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestato e quindi riavviato.

ON La sessione eventi ha inizio all'avvio.

**OFF** La sessione eventi non ha inizio all'avvio.

## <a name="remarks"></a>Osservazioni

L'ordine di precedenza degli operatori logici prevede `NOT` come operatore con precedenza massima, seguito da `AND` e quindi da `OR`.

## <a name="permissions"></a>Autorizzazioni

In SQL Server è richiesta l'autorizzazione `ALTER ANY EVENT SESSION`.
Nel database SQL è richiesta l'autorizzazione `ALTER ANY DATABASE EVENT SESSION` nel database.

## <a name="examples"></a>Esempi

### <a name="sql-server-example"></a>Esempio di SQL Server

Nell'esempio seguente viene illustrato come creare una sessione dell'evento denominata `test_session`. In questo esempio vengono aggiunti due eventi e viene utilizzata la destinazione Analisi eventi per Windows.

```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='test_session')
    DROP EVENT session test_session ON SERVER;
GO
CREATE EVENT SESSION test_session
ON SERVER
    ADD EVENT sqlos.async_io_requested,
    ADD EVENT sqlserver.lock_acquired
    ADD TARGET package0.etw_classic_sync_target
        (SET default_etw_session_logfile_path = N'C:\demo\traces\sqletw.etl' )
    WITH (MAX_MEMORY=4MB, MAX_EVENT_SIZE=4MB);
GO
```
### <a name="sql-database-example"></a>Esempio di database SQL

Per un esempio di database SQL di Azure, vedere l'esempio in [Codice di destinazione del file di eventi per eventi estesi nel database SQL](/azure/sql-database/sql-database-xevent-code-event-file#transact-sql-code)

### <a name="code-examples-can-differ-for-azure-sql-database"></a>Gli esempi di codice possono essere diversi per il database SQL di Azure

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Vedere anche

- [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)
- [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)
- [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)
- [sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)
- [sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)