---
description: ALTER QUEUE (Transact-SQL)
title: ALTER QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_QUEUE_TSQL
- ALTER QUEUE
dev_langs:
- TSQL
helpviewer_keywords:
- number of queue readers
- modifying queues
- ALTER QUEUE statement
- queue readers [SQL Server]
- queues [Service Broker], modifying
- unavailable queues [SQL Server]
- activation stored procedures [Service Broker]
ms.assetid: d54aa325-8761-4cd4-8da7-acf33df12296
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f82564a5beeadbf17f8e3d4cfac7bb5d01da051
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204101"
---
# <a name="alter-queue-transact-sql"></a>ALTER QUEUE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà di una coda.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER QUEUE <object>   
   queue_settings  
   | queue_action  
[ ; ]  
  
<object> : :=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
  
<queue_settings> : :=  
WITH  
   [ STATUS = { ON | OFF } [ , ] ]  
   [ RETENTION = { ON | OFF } [ , ] ]  
   [ ACTIVATION (  
       { [ STATUS = { ON | OFF } [ , ] ]   
         [ PROCEDURE_NAME = <procedure> [ , ] ]  
         [ MAX_QUEUE_READERS = max_readers [ , ] ]  
         [ EXECUTE AS { SELF | 'user_name'  | OWNER } ]  
       |  DROP }  
          ) [ , ]]  
         [ POISON_MESSAGE_HANDLING (  
          STATUS = { ON | OFF } )  
         ]   
  
<queue_action> : :=  
   REBUILD [ WITH <query_rebuild_options> ]  
   | REORGANIZE [ WITH (LOB_COMPACTION = { ON | OFF } ) ]  
   | MOVE TO { file_group | "default" }  
  
<procedure> : :=  
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }
  
<queue_rebuild_options> : :=  
{  
   ( MAXDOP = max_degree_of_parallelism )  
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *database_name* (oggetto)  
 Nome del database contenente la coda da modificare. Se non si specifica *database_name*, per impostazione predefinita viene usato il database corrente.  
  
 *schema_name* (oggetto)  
 Nome dello schema cui appartiene la nuova coda. Se non si specifica *schema_name*, per impostazione predefinita viene usato lo schema predefinito dell'utente corrente.  
  
 *queue_name*  
 Nome della coda da modificare.  
  
 STATUS (coda)  
 Specifica se la coda è disponibile (ON) o non disponibile (OFF). Quando la coda non è disponibile, non è possibile aggiungere o rimuovere messaggi nella coda.  
  
 RETENTION  
 Specifica l'impostazione di memorizzazione per la coda. Se RETENTION = ON, tutti i messaggi inviati o ricevuti per conversazioni che utilizzano la coda vengono memorizzati nella coda fino al termine delle conversazioni. Ciò consente di memorizzare i messaggi a scopi di controllo oppure di eseguire transazioni di compensazione se si verificano errori.  
  
> [!NOTE]  
>  L'impostazione di RETENTION = ON può ridurre le prestazioni. È consigliabile utilizzare questa impostazione solo se necessario per soddisfare il contratto di servizio per l'applicazione.  
  
 ACTIVATION  
 Specifica informazioni sulla stored procedure attivata per l'elaborazione dei messaggi che arrivano nella coda.  
  
 STATUS (attivazione)  
 Specifica se la coda attiva la stored procedure. Se STATUS = ON, la coda avvia la stored procedure specificata con PROCEDURE_NAME quando il numero di procedure in esecuzione è minore di MAX_QUEUE_READERS e quando i messaggi arrivano nella coda più velocemente di quanto non vengano ricevuti dalle stored procedure. Se STATUS = OFF, la coda non attiva la stored procedure.  
  
 REBUILD [ WITH \<queue_rebuild_options> ]  
 **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 Ricompila tutti gli indici della tabella interna della coda. Usare questa funzionalità quando si verificano problemi di frammentazione a causa di un carico elevato. MAXDOP è l'unica opzione di ricompilazione della coda supportata. L'operazione REBUILD viene sempre eseguita in modalità offline.  
  
 REORGANIZE [ WITH ( LOB_COMPACTION = { ON | OFF } ) ]  
 **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 Riorganizza tutti gli indici della tabella interna della coda.   
A differenza di REORGANIZE nelle tabelle utente, l'operazione REORGANIZE in una coda viene sempre eseguita in modalità offline perché i blocchi a livello di pagina sono disabilitati in modo esplicito nelle code.  
  
> [!TIP]  
>  In linea generale, quando la frammentazione dell'indice è compresa tra il 5 e il 30%, è consigliabile riorganizzare l'indice. Se la frammentazione è superiore al 30%, ricompilare l'indice. Si tratta tuttavia di indicazioni di carattere generale, da considerare come punto di partenza per l'ambiente. Per determinare la percentuale di frammentazione dell'indice, usare [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md). Vedere l'esempio G in tale articolo.  
  
 MOVE TO { *file_group* | "default" }  
 **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 Sposta la tabella interna della coda (con i relativi indici) in un filegroup specificato dall'utente.  Il nuovo filegroup non deve essere di sola lettura.  
  
 PROCEDURE_NAME = \<procedure>  
 Specifica il nome della stored procedure da attivare quando la coda contiene messaggi da elaborare. Il valore deve essere un identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *database_name* (procedura)  
 Nome del database contenente la stored procedure.  
  
 *schema_name* (procedura)  
 Nome dello schema a cui appartiene la stored procedure.  
  
 *stored_procedure_name*  
 Nome della stored procedure.  
  
 MAX_QUEUE_READERS =*max_reader*  
 Specifica il numero massimo di istanze della stored procedure di attivazione che possono essere avviate simultaneamente dalla coda. Il valore di *max_readers* deve essere un numero compreso tra 0 e 32767.  
  
 EXECUTE AS  
 Viene specificato l'account utente del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato per l'esecuzione della stored procedure di attivazione. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere possibile controllare le autorizzazioni per tale utente nel momento in cui la stored procedure viene attivata dalla coda. Per un utente di dominio di Windows, è necessario che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia connesso al dominio e sia in grado di convalidare le autorizzazioni dell'utente specificato quando viene attivata la procedura o l'attivazione ha esito negativo. Per un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il server è sempre in grado di eseguire il controllo delle autorizzazioni.  
  
 SELF  
 Specifica che la stored procedure viene eseguita con l'account dell'utente corrente, (l'entità di database che esegue l'istruzione ALTER QUEUE).  
  
 '*user_name*'  
 Nome dell'utente utilizzato per l'esecuzione della stored procedure. *user_name* deve essere un utente valido di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificato come identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'utente corrente deve avere l'autorizzazione IMPERSONATE per il nome *user_name* specificato.  
  
 OWNER  
 Specifica che la stored procedure viene eseguita con l'account del proprietario della coda.  
  
 DROP  
 Elimina tutte le informazioni di attivazione associate alla coda.  
  
 POISON_MESSAGE_HANDLING  
 Specifica se la gestione del messaggio non elaborabile è abilitata. Il valore predefinito è ON.  
  
 Una coda con la gestione di messaggi non elaborabili impostata su OFF non verrà disabilitata dopo cinque rollback di transazioni consecutivi. In questo modo è possibile che un sistema di gestione di messaggi non elaborabili venga definito dall'applicazione.  
  
## <a name="remarks"></a>Osservazioni  
 Se una coda per cui viene specificata una stored procedure di attivazione contiene messaggi, la modifica dello stato di attivazione da OFF a ON comporta l'attivazione immediata della stored procedure di attivazione. La modifica dello stato di attivazione da ON a OFF impedisce a Service Broker di attivare altre istanze della stored procedure, ma non arresta le istanze della stored procedure già in esecuzione.  
  
 La modifica di una coda per l'aggiunta di una stored procedure di attivazione non modifica lo stato di attivazione della coda. La modifica della stored procedure di attivazione per la coda non influisce sulle istanze della stored procedure di attivazione già in esecuzione.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] controlla il numero massimo di lettori per una coda nell'ambito del processo di attivazione. Se quindi si modifica una coda per aumentare il numero massimo di lettori, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sarà in grado di avviare immediatamente più istanze della stored procedure di attivazione. La modifica di una coda per ridurre il numero massimo di lettori non influisce sulle istanze della stored procedure di attivazione già in esecuzione. In [!INCLUDE[ssSB](../../includes/sssb-md.md)], tuttavia, non viene avviata una nuova istanza della stored procedure fino a quando il numero di istanze per la stored procedure di attivazione non risulta inferiore al numero massimo configurato.  
  
 Quando una coda non è disponibile, i messaggi per i servizi che utilizzano tale coda vengono mantenuti da [!INCLUDE[ssSB](../../includes/sssb-md.md)] nella coda di trasmissione per il database. La vista del catalogo [sys.transmission_queue](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md) consente di visualizzare il contenuto della coda di trasmissione.  
  
 Se in un'istruzione RECEIVE o GET CONVERSATION GROUP viene specificata una coda non disponibile, l'istruzione ha esito negativo e viene generato un errore [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per la modifica di una coda vengono assegnate per impostazione predefinita al proprietario della coda, ai membri del ruolo predefinito del database db_ddladmin o db_owner e ai membri del ruolo predefinito del server sysadmin.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-making-a-queue-unavailable"></a>R. Impostazione di una coda come non disponibile  
 Nell'esempio seguente la coda `ExpenseQueue` viene resa non disponibile per la ricezione di messaggi.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH STATUS = OFF ;  
```  
  
### <a name="b-changing-the-activation-stored-procedure"></a>B. Modifica della stored procedure di attivazione  
 Nell'esempio seguente viene modificata la stored procedure avviata dalla coda. La stored procedure viene eseguita con l'account dell'utente che ha eseguito l'istruzione `ALTER QUEUE`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = new_stored_proc,  
        EXECUTE AS SELF) ;  
```  
  
### <a name="c-changing-the-number-of-queue-readers"></a>C. Modifica del numero di lettori di coda  
 Nell'esempio seguente il numero massimo di istanze della stored procedure avviate da [!INCLUDE[ssSB](../../includes/sssb-md.md)] per la coda viene impostato su `7`.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (MAX_QUEUE_READERS = 7) ;  
```  
  
### <a name="d-changing-the-activation-stored-procedure-and-the-execute-as-account"></a>D. Modifica della stored procedure di attivazione e dell'account EXECUTE AS  
 Nell'esempio seguente viene modificata la stored procedure avviata da [!INCLUDE[ssSB](../../includes/sssb-md.md)]. La stored procedure viene eseguita con l'account `SecurityAccount`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = AdventureWorks2012.dbo.new_stored_proc ,  
        EXECUTE AS 'SecurityAccount') ;  
```  
  
### <a name="e-setting-the-queue-to-retain-messages"></a>E. Impostazione della coda per la memorizzazione dei messaggi  
 Nell'esempio seguente viene impostata la coda per la memorizzazione dei messaggi. Nella coda vengono memorizzati tutti i messaggi inviati ai servizi o provenienti dai servizi che utilizzano la coda fino alla fine della conversazione contenente il messaggio.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH RETENTION = ON ;  
```  
  
### <a name="f-removing-activation-from-a-queue"></a>F. Rimozione delle informazioni di attivazione da una coda  
 Nell'esempio seguente vengono rimosse tutte le informazioni di attivazione dalla coda.  
  
```  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (DROP) ;  
```  
  
### <a name="g-rebuilding-queue-indexes"></a>G. Ricompilazione degli indici della coda  
  
**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 Nell'esempio seguente viene illustrata la ricompilazione degli indici della coda  
  
```sql  
ALTER QUEUE ExpenseQueue REBUILD WITH (MAXDOP = 2)   
```  
  
### <a name="h-reorganizing-queue-indexes"></a>H. Riorganizzazione degli indici della coda  
  
**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 Nell'esempio seguente viene illustrata la riorganizzazione degli indici della coda  
  
```sql  
ALTER QUEUE ExpenseQueue REORGANIZE   
```  
  
### <a name="i-moving-queue-internal-table-to-another-filegroup"></a>I. Spostamento della tabella interna della coda in un altro filegroup  
  
**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
```sql  
ALTER QUEUE ExpenseQueue MOVE TO [NewFilegroup]   
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
  
  
