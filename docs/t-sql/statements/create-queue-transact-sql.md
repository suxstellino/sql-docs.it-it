---
description: CREATE QUEUE (Transact-SQL)
title: CREATE QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 42b5b42ee88c6d562f1ebf8c03f92a1bd4be54f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188540"
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Crea una nuova coda in un database. Le code vengono utilizzate per archiviare i messaggi. Quando viene recapitato un messaggio per un servizio, esso viene inserito da [!INCLUDE[ssSB](../../includes/sssb-md.md)] nella coda associata al servizio.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE QUEUE <object>
   [ WITH
     [ STATUS = { ON | OFF } [ , ] ]
     [ RETENTION = { ON | OFF } [ , ] ]
     [ ACTIVATION (
         [ STATUS = { ON | OFF } , ]
           PROCEDURE_NAME = <procedure> ,
           MAX_QUEUE_READERS = max_readers ,
           EXECUTE AS { SELF | 'user_name' | OWNER }
            ) [ , ] ]
     [ POISON_MESSAGE_HANDLING (
         [ STATUS = { ON | OFF } ] ) ]
    ]
     [ ON { filegroup | [ DEFAULT ] } ]
[ ; ]

<object> ::=
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }

<procedure> ::=
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*database_name* (oggetto) Nome del database in cui creare la nuova coda. *database_name* deve specificare il nome di un database esistente. Se non si specifica *database_name*, la coda viene creata nel database corrente.

*schema_name* (oggetto) Nome dello schema cui appartiene la nuova coda. Per impostazione predefinita, viene utilizzato lo schema predefinito dell'utente che esegue l'istruzione. Se l'istruzione CREATE QUEUE viene eseguita da un membro del ruolo predefinito del server sysadmin o da un membro del ruolo predefinito del database db_dbowner o db_ddladmin nel database specificato da *database_name*, *schema_name* può specificare uno schema diverso da quello associato all'account di accesso della connessione corrente. In caso contrario, *schema_name* deve corrispondere allo schema predefinito per l'utente che esegue l'istruzione.

*queue_name* Nome della coda da creare. Il nome deve essere conforme alle linee guida per gli identificatori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

STATUS (coda) Specifica se la coda è disponibile (ON) o non disponibile (OFF). Quando la coda non è disponibile, non è possibile aggiungere o rimuovere messaggi nella coda. È possibile creare la coda con stato non disponibile per impedire che i messaggi raggiungano la coda fino a quando non viene resa disponibile con un'istruzione ALTER QUEUE. Se questa clausola viene omessa, il valore predefinito è ON e la coda è disponibile.

RETENTION Specifica l'impostazione di conservazione per la coda. Se RETENTION = ON, tutti i messaggi inviati o ricevuti per conversazioni che utilizzano la coda vengono mantenuti nella coda fino al termine delle conversazioni. Questo consente di mantenere i messaggi a scopo di controllo oppure di eseguire transazioni di compensazione se si verificano errori. Se questa clausola viene omessa, il valore predefinito dell'impostazione di memorizzazione è OFF.

> [!NOTE]
> L'impostazione di RETENTION = ON può ridurre le prestazioni. È quindi consigliabile utilizzare questa impostazione solo se richiesto per l'applicazione.

ACTIVATION Specifica le informazioni sulla stored procedure da avviare per l'elaborazione dei messaggi nella coda.

STATUS (attivazione) Specifica se [!INCLUDE[ssSB](../../includes/sssb-md.md)] avvia la stored procedure. Se STATUS = ON, la coda avvia la stored procedure specificata con PROCEDURE_NAME quando il numero di procedure in esecuzione è minore di MAX_QUEUE_READERS e quando i messaggi arrivano nella coda più velocemente di quanto non vengano ricevuti dalle stored procedure. Se STATUS = OFF, la coda non avvia la stored procedure. Se questa clausola viene omessa, il valore predefinito è ON.

PROCEDURE_NAME = \<procedure> Specifica il nome della stored procedure da avviare per l'elaborazione dei messaggi nella coda. Il valore deve essere un identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

*database_name*(stored_procedure) Nome del database contenente la stored procedure.

*schema_name*(stored_procedure) Nome dello schema contenente la stored procedure.

*procedure_name* Nome della stored procedure.

MAX_QUEUE_READERS =*max_readers* Specifica il numero massimo di istanze della stored procedure di attivazione avviate simultaneamente dalla coda. Il valore di *max_readers* deve essere un numero compreso tra **0** e **32767**.

EXECUTE AS Specifica l'account utente del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usato per l'esecuzione della stored procedure di attivazione. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere possibile controllare le autorizzazioni per tale utente nel momento in cui la stored procedure viene avviata dalla coda. Nel caso di un utente di dominio, è necessario che il server sia connesso al dominio quando viene avviata la procedura. In caso contrario, l'attivazione ha esito negativo. Per un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il server è sempre in grado di eseguire il controllo delle autorizzazioni.

SELF Specifica che la stored procedure viene eseguita con l'account dell'utente corrente, ovvero l'entità di database che esegue l'istruzione CREATE QUEUE.

'*user_name*' Nome dell'utente usato per l'esecuzione della stored procedure. Il parametro *user_name* deve essere un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido specificato come identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'utente corrente deve avere l'autorizzazione IMPERSONATE per il nome *user_name* specificato.

OWNER Specifica che la stored procedure viene eseguita con l'account del proprietario della coda.

POISON_MESSAGE_HANDLING Specifica se la gestione del messaggio non elaborabile è abilitata per la coda. Il valore predefinito è ON.

Una coda con la gestione di messaggi non elaborabili impostata su OFF non verrà disabilitata dopo cinque rollback di transazioni consecutivi. In questo modo è possibile che un sistema di gestione di messaggi non elaborabili venga definito dall'applicazione.

ON *filegroup |* [**DEFAULT**] Specifica il filegroup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui creare la coda. È possibile usare il parametro *filegroup* per specificare un filegroup oppure usare l'identificatore DEFAULT per usare il filegroup predefinito del database di Service Broker. Nel contesto di questa clausola DEFAULT non è una parola chiave ed è pertanto necessario delimitarlo come identificatore. Se non si specifica un filegroup, per la coda viene utilizzato il filegroup predefinito del database.

## <a name="remarks"></a>Osservazioni

Una coda può essere la destinazione di un'istruzione SELECT. È tuttavia possibile modificare il contenuto di una coda solo tramite istruzioni che operano in conversazioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)], quali SEND, RECEIVE e END CONVERSATION. Non è possibile specificare una coda come destinazione di un'istruzione INSERT, UPDATE, DELETE o TRUNCATE.

Una coda non può essere un oggetto temporaneo. Pertanto i nomi di coda che iniziano con **#** non sono validi.

La creazione di una coda con stato inattivo consente di predisporre l'infrastruttura necessaria per un servizio prima di consentire la ricezione dei messaggi nella coda.

[!INCLUDE[ssSB](../../includes/sssb-md.md)] non arresta le stored procedure di attivazione se non sono presenti messaggi nella coda. È consigliabile prevedere l'interruzione di una stored procedure di attivazione se nella coda non sono disponibili messaggi per un breve periodo di tempo.

Le autorizzazioni per la stored procedure di attivazione vengono controllate quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] avvia la stored procedure e non al momento della creazione della coda. L'istruzione CREATE QUEUE non verifica che l'utente specificato nella clausola EXECUTE AS disponga delle autorizzazioni per l'esecuzione della stored procedure specificata nella clausola PROCEDURE NAME.

Quando una coda non è disponibile, i messaggi per i servizi che utilizzano tale coda vengono mantenuti da [!INCLUDE[ssSB](../../includes/sssb-md.md)] nella coda di trasmissione per il database. La vista del catalogo sys.transmission_queue consente di visualizzare il contenuto della coda di trasmissione.

Una coda è un oggetto di proprietà dello schema. Le code sono incluse nella vista del catalogo sys.objects.

Nella tabella seguente vengono elencate le colonne di una coda.

|Nome colonna|Tipo di dati|Descrizione|
|-----------------|---------------|-----------------|
|status|**tinyint**|Stato del messaggio. L'istruzione RECEIVE restituisce i messaggi con stato **1**. Se la memorizzazione dei messaggi è attiva, lo stato viene quindi impostato su 0. Se la memorizzazione dei messaggi è disattivata, il messaggio viene eliminato dalla coda. I messaggi nella coda possono contenere uno dei valori seguenti:<br /><br /> **0** = Messaggio ricevuto memorizzato<br /><br /> **1** = Pronto per la ricezione<br /><br /> **2** = Non ancora completo<br /><br /> **3** = Messaggio inviato memorizzato|
|priority|**tinyint**|Livello di priorità assegnato al messaggio.|
|queuing_order|**bigint**|Numero progressivo del messaggio nella coda.|
|conversation_group_id|**uniqueidentifier**|Identificatore del gruppo di conversazioni a cui appartiene il messaggio.|
|conversation_handle|**uniqueidentifier**|Handle della conversazione di cui fa parte il messaggio.|
|message_sequence_number|**bigint**|Numero di sequenza del messaggio nella conversazione.|
|service_name|**nvarchar(512)**|Nome del servizio a cui è destinata la conversazione.|
|service_id|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del servizio a cui è destinata la conversazione.|
|service_contract_name|**nvarchar(256)**|Nome del contratto rispettato dalla conversazione.|
|service_contract_id|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del contratto rispettato dalla conversazione.|
|message_type_name|**nvarchar(256)**|Nome del tipo di messaggio che descrive il messaggio.|
|message_type_id|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del tipo di messaggio che descrive il messaggio.|
|validation|**nchar(2)**|Convalida utilizzata per il messaggio.<br /><br /> E=Vuoto<br /><br /> N=Nessuno<br /><br /> X=XML|
|message_body|**varbinary(max)**|Contenuto del messaggio.|
|message_id|**uniqueidentifier**|Identificatore univoco del messaggio.|

## <a name="permissions"></a>Autorizzazioni

L'autorizzazione per la creazione di una coda viene assegnata ai membri del ruolo predefinito del database `db_ddladmin` o `db_owner` e del ruolo predefinito del server `sysadmin`.

L'autorizzazione `REFERENCES` per una coda viene assegnata per impostazione predefinita al proprietario della coda, ai membri del ruolo predefinito del database `db_ddladmin` o `db_owner` e del ruolo predefinito del server `sysadmin`.

L'autorizzazione `RECEIVE` per una coda viene assegnata per impostazione predefinita al proprietario della coda, ai membri del ruolo predefinito del database `db_owner` o ai membri del ruolo predefinito del server `sysadmin`.

## <a name="examples"></a>Esempi

### <a name="a-creating-a-queue-with-no-parameters"></a>R. Creazione di una coda senza parametri

Nell'esempio seguente viene creata una coda disponibile per la ricezione di messaggi. Per la coda non viene specificata alcuna stored procedure di attivazione.

```sql
CREATE QUEUE ExpenseQueue ;
```

### <a name="b-creating-an-unavailable-queue"></a>B. Creazione di una coda non disponibile

Nell'esempio seguente viene creata una coda non disponibile per la ricezione di messaggi. Per la coda non viene specificata alcuna stored procedure di attivazione.

```sql
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;
```

### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>C. Creazione di una coda e impostazione delle informazioni interne per l'attivazione

Nell'esempio seguente viene creata una coda disponibile per la ricezione di messaggi. La coda avvia la stored procedure `expense_procedure` quando un messaggio raggiunge la coda. La stored procedure viene eseguita con l'account `ExpenseUser`. La coda avvia al massimo `5` istanze della stored procedure.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS=ON,
    ACTIVATION (
        PROCEDURE_NAME = expense_procedure
        , MAX_QUEUE_READERS = 5
        , EXECUTE AS 'ExpenseUser' ) ;
```

### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>D. Creazione di una coda in un filegroup specifico

Nell'esempio seguente viene creata una coda nel filegroup `ExpenseWorkFileGroup`.

```sql
CREATE QUEUE ExpenseQueue
    ON ExpenseWorkFileGroup ;
```

### <a name="e-creating-a-queue-with-multiple-parameters"></a>E. Creazione di una coda con più parametri

Nell'esempio seguente viene creata una coda nel filegroup `DEFAULT`. La coda non è disponibile. I messaggi vengono memorizzati nella coda fino al termine della conversazione cui appartengono. Quando viene resa disponibile tramite ALTER QUEUE, la coda avvia la stored procedure `2008R2.dbo.expense_procedure` per elaborare i messaggi. La stored procedure viene eseguita con l'account dell'utente che ha eseguito l'istruzione `CREATE QUEUE`. La coda avvia al massimo `10` istanze della stored procedure.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS = OFF
      , RETENTION = ON
      , ACTIVATION (
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure
          , MAX_QUEUE_READERS = 10
          , EXECUTE AS SELF )
    ON [DEFAULT];
```

## <a name="see-also"></a>Vedere anche

- [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)
- [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)
- [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)
- [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
