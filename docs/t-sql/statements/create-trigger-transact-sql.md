---
title: CREATE TRIGGER (Transact-SQL)
description: Informazioni di riferimento Transact-SQL per l'istruzione CREATE TRIGGER, usata per creare un trigger DML, DDL o LOGON.
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE TRIGGER
- TRIGGER
- CREATE_TRIGGER_TSQL
- TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- CREATE TRIGGER statement
- multiple triggers
- deferred name resolution, DML triggers
- DML triggers, creating
- nested triggers
- server-scoped triggers
- DDL triggers, creating
- triggers [SQL Server], creating
- database-scoped triggers [SQL Server]
ms.assetid: edeced03-decd-44c3-8c74-2c02f801d3e7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.openlocfilehash: ff81e043894af70e9a51991cec000fede3c31521
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345261"
---
# <a name="create-trigger-transact-sql"></a>CREATE TRIGGER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea un trigger DML, DDL o LOGON. Un trigger è una stored procedure di tipo speciale che viene eseguita automaticamente quando si verifica un evento nel server di database. I trigger DML vengono eseguiti quando un utente prova a modificare i dati tramite un evento DML (Data Manipulation Language). Gli eventi DML sono istruzioni INSERT, UPDATE o DELETE eseguite su una tabella o una vista. Questi trigger vengono attivati quando viene generato un evento valido, indipendentemente dal fatto che le righe della tabella siano interessate o meno. Per altre informazioni, vedere [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
I trigger DDL vengono eseguiti in risposta a vari eventi DDL (Data Definition Language). Questi eventi corrispondono principalmente alle istruzioni CREATE, ALTER e DROP [!INCLUDE[tsql](../../includes/tsql-md.md)] e ad alcune stored procedure di sistema che eseguono operazioni di tipo DDL. 

I trigger LOGON vengono attivati in risposta all'evento LOGON generato quando viene stabilita una sessione utente. È possibile creare i trigger direttamente dalle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o dai metodi di assembly creati in Common Language Runtime (CLR) di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] e caricati in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di creare più trigger per qualsiasi istruzione specifica.  
  
> [!IMPORTANT]  
>  L'innalzamento di livello dei privilegi consente l'esecuzione di malware all'interno dei trigger. Per altre informazioni su come limitare tale minaccia, vedere [Gestione della sicurezza dei trigger](../../relational-databases/triggers/manage-trigger-security.md).  
  
> [!NOTE]  
>  In questo articolo viene illustrata l'integrazione di CLR di .NET Framework in SQL Server. L'integrazione di CLR non si applica al database SQL di Azure.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="sql-server-syntax"></a>Sintassi SQL Server  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
[ WITH APPEND ]  
[ NOT FOR REPLICATION ]   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME <method specifier [ ; ] > }  
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
```  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a 
-- table (DML Trigger on memory-optimized tables)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
AS { sql_statement  [ ; ] [ ,...n ] }  
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE or UPDATE statement (DDL Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { ALL SERVER | DATABASE }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type | event_group } [ ,...n ]  
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a LOGON event (Logon Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON    
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
## <a name="azure-sql-database-syntax"></a>Sintassi database SQL di Azure  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
  AS { sql_statement  [ ; ] [ ,...n ] [ ; ] > }  
  
<dml_trigger_option> ::=   
        [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE, or UPDATE STATISTICS statement (DDL Trigger)   
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type | event_group } [ ,...n ]   
AS { sql_statement  [ ; ] [ ,...n ]  [ ; ] }  
  
<ddl_trigger_option> ::=   
    [ EXECUTE AS Clause ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
OR ALTER  
**Si applica a**: Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1). 
  
Modifica in modo condizionale il trigger solo se esiste già. 
  
*schema_name*  
Nome dello schema a cui appartiene un trigger DML. L'ambito dei trigger DML è definito nello schema della tabella o della vista in cui sono creati i trigger stessi. *schema_name* non può essere specificato per i trigger DDL o LOGON.  
  
*trigger_name*  
Nome del trigger. *trigger_name* deve seguire le regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md), a eccezione del fatto che *trigger_name* non può iniziare con # o ##.  
  
*table* | *view*  
Tabella o vista nella quale viene eseguito il trigger DML. Questa tabella o vista è anche nota come tabella di trigger o vista di trigger. Il nome completo della tabella o della vista è facoltativo. È possibile fare riferimento a una vista solo tramite un trigger INSTEAD OF. Non è possibile definire trigger DML in tabelle temporanee globali o locali.  
  
DATABASE  
Applica l'ambito di un trigger DDL al database corrente. Se viene specificato questo parametro, il trigger viene attivato quando si verifica *event_type* o *event_group* nel database corrente.  
  
ALL SERVER  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
Applica l'ambito di un trigger DDL o LOGON al server corrente. Se viene specificato questo parametro, il trigger viene attivato quando si verifica *event_type* o *event_group* nel server corrente.  
  
WITH ENCRYPTION  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
Occulta il testo dell'istruzione CREATE TRIGGER. Tramite il parametro WITH ENCRYPTION è possibile evitare la pubblicazione del trigger come parte della replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è possibile specificare WITH ENCRYPTION per i trigger CLR.  
  
EXECUTE AS  
Specifica il contesto di sicurezza nel quale viene eseguito il trigger. Consente di controllare l'account utente utilizzato dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per convalidare le autorizzazioni per ogni oggetto di database a cui fa riferimento il trigger.  
  
Questa opzione è obbligatoria per i trigger sulle tabelle ottimizzate per la memoria.  
  
Per altre informazioni, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
NATIVE_COMPILATION  
Indica che il trigger viene compilato in modo nativo.  
  
Questa opzione è obbligatoria per i trigger sulle tabelle ottimizzate per la memoria.  
  
SCHEMABINDING  
Assicura che le tabelle a cui si fa riferimento in un trigger non possano essere eliminate o modificate.  
  
Questa opzione è necessaria per i trigger sulle tabelle ottimizzate per la memoria e non è supportata per i trigger su tabelle tradizionali.  
  
FOR | AFTER  
FOR o AFTER specifica che il trigger DML viene attivato solo al termine dell'avvio di tutte le operazioni specificate nell'istruzione di trigger SQL. Perché il trigger venga attivato, è anche necessario che siano stati completati tutti i controlli dei vincoli e le operazioni referenziali di propagazione.  
  
Non è possibile definire trigger i AFTER sulle viste.  
  
INSTEAD OF  
Specifica che il trigger DML viene avviato *al posto* dell'istruzione di trigger SQL. Il trigger risulta quindi prioritario rispetto alle operazioni delle istruzioni di trigger. Non è possibile specificare INSTEAD OF per i trigger DDL o LOGON.  
  
In una tabella o vista è possibile definire al massimo un trigger INSTEAD OF per ogni istruzione INSERT, UPDATE o DELETE. È anche possibile definire viste che fanno riferimento ad altre viste. Ogni vista include un trigger INSTEAD OF.  
  
Non è possibile definire i trigger INSTEAD OF in viste aggiornabili che usano WITH CHECK OPTION. Questa operazione genera un errore quando un trigger INSTEAD OF viene aggiunto a una vista aggiornabile per la quale è stato specificato WITH CHECK OPTION. Per poter definire il trigger INSTEAD OF, si rimuove l'opzione usando l'istruzione ALTER VIEW.  
  
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }  
Specifica le istruzioni di modifica dei dati che attivano il trigger DML quando vengono eseguite sulla tabella o sulla vista. Specificare almeno un'opzione. Usare qualsiasi combinazione delle opzioni nell'ordine desiderato nella definizione di trigger.  
  
Per i trigger INSTEAD OF, non è possibile usare l'opzione DELETE in tabelle contenenti una relazione referenziale che specifica un'operazione di propagazione ON DELETE. In modo analogo, l'opzione UPDATE non è consentita in tabelle contenenti una relazione referenziale che specifica un'operazione di propagazione ON UPDATE.  
  
WITH APPEND  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].  
  
Specifica che deve essere aggiunto un altro trigger di un tipo già esistente. La clausola WITH APPEND non può essere usata con trigger INSTEAD OF o quando un trigger AFTER viene dichiarato in modo esplicito. Per la compatibilità con le versioni precedenti, usare la clausola WITH APPEND solo quando viene specificata la parola chiave FOR, senza INSTEAD OF o AFTER. Non è possibile specificare WITH APPEND se si usa EXTERNAL NAME (ovvero se il trigger è un trigger CLR).  
  
*event_type*  
Nome di un evento del linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] che, dopo l'avvio, attiva un trigger DDL. Gli eventi supportati dai trigger DDL sono elencati in [Eventi DDL](../../relational-databases/triggers/ddl-events.md).  
  
*event_group*  
Nome di un raggruppamento predefinito di eventi del linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il trigger DDL viene attivato dopo l'avvio di qualsiasi evento del linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] appartenente a *event_group*. I gruppi di eventi supportati dai trigger DDL sono elencati in [Gruppi di eventi DDL](../../relational-databases/triggers/ddl-event-groups.md).  
  
Dopo il completamento dell'esecuzione di CREATE TRIGGER, *event_group* funge anche da macro aggiungendo i tipi di eventi che include alla vista del catalogo sys.trigger_events.  
  
NOT FOR REPLICATION  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
Indica che il trigger non deve essere eseguito quando un agente di replica modifica la tabella coinvolta nel trigger.  
  
*sql_statement*  
Condizioni e azioni del trigger. Le condizioni del trigger specificano ulteriori criteri che determinano se gli eventi DML, DDL o LOGON che si prova a eseguire avviano l'esecuzione delle azioni del trigger.  
  
Le azioni del trigger specificate nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono attivate quando viene tentato di eseguire l'operazione.  
  
I trigger possono includere un numero qualsiasi di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di qualunque tipo, con alcune eccezioni. Per altre informazioni, vedere la sezione Osservazioni. Un trigger verifica o modifica dati in base a un'istruzione di modifica o di definizione dei dati, senza restituire dati all'utente. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di un trigger spesso includono [elementi del linguaggio per il controllo di flusso](~/t-sql/language-elements/control-of-flow.md).  
  
I trigger DML utilizzano le tabelle logiche o concettuali Inserted e Deleted. Da un punto di vista strutturale queste tabelle sono simili alla tabella in cui viene definito il trigger, ovvero la tabella in cui si prova a eseguire l'azione utente. Le tabelle Deleted e Inserted contengono i valori precedenti o i nuovi valori delle righe che potrebbero essere modificate dall'azione utente. Ad esempio, per recuperare tutti i valori nella tabella `deleted`, è possibile utilizzare il codice seguente:  
  
```sql  
SELECT * FROM deleted;  
```  
  
Per altre informazioni, vedere [Usare le tabelle inserted e deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md).  
  
I trigger DDL e LOGON acquisiscono informazioni sull'evento che attiva il trigger tramite la funzione [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md). Per altre informazioni, vedere [Utilizzo della funzione EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile aggiornare le colonne di tipo **text**, **ntext** o **image** di tabelle o viste tramite il trigger INSTEAD OF.  
  
> [!IMPORTANT]
>  I tipi di dati **ntext**, **text** e **image** verranno rimossi in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di utilizzare questi tipi di dati in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente li utilizzano. Usare in alternativa [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)e [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) . I trigger AFTER e INSTEAD OF supportano i dati **varchar(MAX)** , **nvarchar(MAX)** e **varbinary(MAX)** nelle tabelle inserted e deleted.  
  
Per i trigger sulle tabelle ottimizzate per la memoria, un blocco ATOMIC è l'unica istruzione *sql_statement* consentita al livello superiore. L'unico codice T-SQL consentito all'interno del blocco ATOMIC è quello consentito nelle procedure native.  
  
\< method_specifier > 
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
Per un trigger CLR, specifica il metodo di un assembly da associare al trigger. Il metodo non deve accettare nessun argomento e restituire void. *class_name* deve essere un identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e deve esistere come classe nell'assembly con visibilità dell'assembly. Se alla classe è stato assegnato un nome completo con lo spazio dei nomi le cui parti sono separate da '.', il nome della classe deve essere delimitato tramite [ ] o " ". La classe non può essere nidificata.  
  
> [!NOTE]  
>  Per impostazione predefinita, la capacità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di eseguire il codice CLR è disattivata. È possibile creare, modificare ed eliminare oggetti di database che fanno riferimento a moduli di codice gestito, ma tali riferimenti non vengono eseguiti in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a meno che non si abiliti l'[opzione clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) tramite [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
## <a name="remarks-for-dml-triggers"></a>Considerazioni sui trigger DML  
I trigger DML vengono utilizzati di frequente per applicare regole business e integrità dei dati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce l'integrità referenziale dichiarativa tramite le istruzioni ALTER TABLE e CREATE TABLE. DRI, tuttavia, non supporta l'integrità referenziale tra database. L'integrità referenziale fa riferimento alle regole riguardanti le relazioni tra le chiavi primarie ed esterne delle tabelle. Per applicare l'integrità referenziale, utilizzare i vincoli PRIMARY KEY e FOREIGN KEY in ALTER TABLE e CREATE TABLE. Gli eventuali vincoli esistenti nella tabella di trigger vengono controllati dopo l'esecuzione del trigger INSTEAD OF e prima dell'esecuzione del trigger AFTER. In caso di violazione dei vincoli, viene eseguito il rollback delle azioni del trigger INSTEAD OF e il trigger AFTER non viene attivato.  
  
È possibile specificare il primo e l'ultimo trigger AFTER da eseguire in una tabella usando sp_settriggerorder. In una tabella è possibile specificare un solo trigger AFTER da eseguire per primo e un solo trigger AFTER da eseguire per ultimo per ogni operazione INSERT, UPDATE e DELETE. Se nella stessa tabella sono inclusi altri trigger AFTER, vengono eseguiti in modo casuale.  
  
Se il primo o l'ultimo trigger viene modificato tramite un'istruzione ALTER TRIGGER, l'attributo first (primo) o last (ultimo) impostato per il trigger modificato viene rimosso ed è necessario reimpostare il valore di ordinamento tramite sp_settriggerorder.  
  
Un trigger AFTER viene eseguito solo dopo il completamento dell'esecuzione dell'istruzione SQL di trigger, comprese tutte le operazioni referenziali di propagazione e le verifiche di vincolo associate all'oggetto aggiornato o eliminato. Un trigger AFTER non genera in modo ricorsivo un trigger INSTEAD OF nella stessa tabella.  
  
Se un trigger INSTEAD OF definito in una tabella esegue un'istruzione sulla tabella che normalmente comporterebbe una seconda attivazione del trigger INSTEAD OF, il trigger non viene chiamato in modo ricorsivo. L'istruzione viene elaborata come se la tabella non includesse un trigger INSTEAD OF e avvia la serie di operazioni sui vincoli e di esecuzioni dei trigger AFTER, ad esempio se un trigger viene definito come trigger INSTEAD OF INSERT per una tabella. Il trigger esegue inoltre un'istruzione INSERT sulla stessa tabella e l'istruzione INSERT avviata dal trigger INSTEAD OF non chiama nuovamente il trigger. L'istruzione INSERT lanciata dal trigger avvia il processo di esecuzione delle operazioni sui vincoli e di attivazione di tutti i trigger AFTER INSERT definiti per la tabella.  
  
Quando un trigger INSTEAD OF definito in una vista esegue un'istruzione sulla vista che normalmente comporterebbe una seconda attivazione del trigger INSTEAD OF, il trigger non viene chiamato in modo ricorsivo. L'istruzione viene risolta sotto forma di modifiche delle tabelle di base sottostanti della vista. In tal caso, la definizione della vista deve rispettare tutte le restrizioni previste per una vista aggiornabile. Per una definizione di viste aggiornabili, vedere [Modificare i dati tramite una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
Ad esempio, se un trigger viene definito come trigger INSTEAD OF UPDATE per una vista. Il trigger esegue inoltre un'istruzione UPDATE che fa riferimento alla stessa vista e l'istruzione UPDATE avviata dal trigger INSTEAD OF non chiama nuovamente il trigger. L'istruzione UPDATE avviata dal trigger viene elaborata rispetto alla vista come se la vista non includesse un trigger INSTEAD OF. Le colonne modificate da UPDATE devono essere risolte in una singola tabella di base. Ogni modifica di una tabella di base sottostante avvia il processo di applicazione dei vincoli e di attivazione dei trigger AFTER definiti per la tabella.  
  
### <a name="testing-for-update-or-insert-actions-to-specific-columns"></a>Test di azioni UPDATE o INSERT eseguite su colonne specifiche  
È possibile progettare un trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] in modo che esegua determinate azioni in base a modifiche di tipo UPDATE o INSERT in colonne specifiche. A tale scopo, usare [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md) o [COLUMNS_UPDATED](../../t-sql/functions/columns-updated-transact-sql.md) nel corpo del trigger. UPDATE() consente di testare i tentativi di esecuzione di UPDATE o INSERT per una colonna. COLUMNS_UPDATED controlla l'esecuzione delle operazioni UPDATE o INSERT in più colonne. Questa funzione restituisce uno schema di bit che indica le colonne inserite o aggiornate.  
  
### <a name="trigger-limitations"></a>Limitazioni dei trigger  
CREATE TRIGGER deve essere la prima istruzione del batch e può essere applicata a una sola tabella.  
  
I trigger vengono creati solo nel database corrente, ma possono fare riferimento a oggetti esterni a tale database.  
  
Se viene specificato il nome dello schema del trigger, è necessario qualificare allo stesso modo anche il nome della tabella.  
  
All'interno di un'istruzione CREATE TRIGGER è possibile definire la stessa azione di trigger per più azioni utente, ad esempio INSERT e UPDATE.  
  
Non è possibile definire trigger INSTEAD OF DELETE/UPDATE in una tabella con una chiave esterna per cui è stata definita un'operazione di propagazione ON DELETE/UPDATE.  
  
In un trigger è possibile specificare qualsiasi istruzione SET. L'opzione SET scelta rimane attiva durante l'esecuzione del trigger, dopodiché viene ripristinata l'impostazione precedente.  
  
Quando un trigger viene attivato, i risultati vengono restituiti all'applicazione chiamante, esattamente come per le stored procedure. Per impedire la restituzione di risultati a un'applicazione in seguito all'attivazione di un trigger, non includere istruzioni SELECT che restituiscono risultati o istruzioni che eseguono assegnazioni di variabili in un trigger. Un trigger che include istruzioni SELECT che restituiscono risultati all'utente oppure istruzioni che eseguono l'assegnazione di variabili richiede una gestione speciale. Sarebbe necessario scrivere i risultati restituiti in ogni applicazione in cui è consentito apportare modifiche alla tabella di trigger. Se è necessario eseguire un'assegnazione di variabile in un trigger, utilizzare un'istruzione SET NOCOUNT all'inizio del trigger per impedire la restituzione dei set di risultati.  
  
Anche se un'istruzione TRUNCATE TABLE è in effetti un'istruzione DELETE, non attiva un trigger perché l'operazione non registra singole eliminazioni di righe. Solo gli utenti con le autorizzazioni per eseguire un'istruzione TRUNCATE TABLE devono tuttavia fare attenzione a non eludere un trigger DELETE in questo modo.  
  
L'istruzione WRITETEXT non attiva alcun trigger, indipendentemente dal fatto che sia registrata o meno.  
  
Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti non sono consentite in un trigger DML:  

- ALTER DATABASE
- CREATE DATABASE
- DROP DATABASE
- RESTORE DATABASE
- RESTORE LOG
- RECONFIGURE

Inoltre, le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti non sono consentite nel corpo di un trigger DML eseguito sulla tabella o sulla vista che rappresenta la destinazione dell'azione del trigger.  
  
- CREATE INDEX (incluse CREATE SPATIAL INDEX e CREATE XML INDEX)
- ALTER INDEX
- DROP INDEX
- DROP TABLE
- DBCC DBREINDEX
- ALTER PARTITION FUNCTION
- ALTER TABLE quando viene utilizzata per eseguire le operazioni seguenti:
    - Aggiungere, modificare o eliminare colonne.
    - Passare da una partizione all'altra.
    - Aggiungere o eliminare vincoli PRIMARY KEY o UNIQUE.

> [!NOTE]  
>  Poiché in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è supportata l'esecuzione di trigger definiti dall'utente su tabelle di sistema, è consigliabile evitare di creare trigger definiti dall'utente per tabelle di sistema. 

### <a name="optimizing-dml-triggers"></a>Ottimizzazione di trigger DML
I trigger funzionano all'interno di transazioni (implicite o meno) e, mentre sono aperti, bloccano risorse. Il blocco rimane finché la transazione non viene confermata (con COMMIT) o rifiutata (con ROLLBACK). Più a lungo viene eseguito un trigger, maggiore è la probabilità che un altro processo venga bloccato. Scrivere quindi i trigger in modo da ridurne la durata, quando è possibile. Un modo per ottenere una durata più breve consiste nel rilasciare un trigger quando un'istruzione DML modifica zero righe. 

Per rilasciare il trigger per un comando che non modifica alcuna riga, usare la variabile di sistema [ROWCOUNT_BIG](../functions/rowcount-big-transact-sql.md). 

Il frammento di codice T-SQL seguente illustra come rilasciare il trigger per un comando che non modifica alcuna riga. Questo codice dovrebbe essere presente all'inizio di ogni trigger DML:

```sql
IF (ROWCOUNT_BIG() = 0)
RETURN;
```
  
  
## <a name="remarks-for-ddl-triggers"></a>Considerazioni sui i trigger DDL  
I trigger DDL, analogamente ai trigger standard, avviano stored procedure in risposta a un evento. A differenza dei trigger standard, tuttavia, non vengono eseguiti in risposta a istruzioni UPDATE, INSERT o DELETE su una tabella o una vista. I trigger DDL in genere vengono eseguiti in risposta a istruzioni DDL (Data Definition Language). I tipi di istruzioni includono CREATE, ALTER, DROP, GRANT, DENY, REVOKE e UPDATE STATISTICS. Alcune stored procedure di sistema che eseguono operazioni di tipo DDL possono anche attivare trigger DDL.  
  
> [!IMPORTANT]  
>  Testare i trigger DDL per determinarne la risposta all'esecuzione delle stored procedure di sistema. Sia l'istruzione CREATE TYPE che le stored procedure sp_addtype e sp_rename, ad esempio, attivano un trigger DDL creato in un evento CREATE_TYPE.  
  
Per altre informazioni sui trigger DDL, vedere [Trigger DDL](../../relational-databases/triggers/ddl-triggers.md).  
  
I trigger DDL non vengono attivati in risposta a eventi che interessano stored procedure e tabelle temporanee globali o locali.  
  
Diversamente dai trigger DML, i trigger DDL non sono definiti a livello di ambito di schema. Non è quindi possibile usare funzioni quali OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY e OBJECTPROPERTYEX durante l'esecuzione di query sui metadati relativi ai trigger DDL. Utilizzare in alternativa le viste del catalogo. Per altre informazioni, vedere [Ottenere informazioni sui trigger DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
> [!NOTE]  
>  I trigger DDL con ambito server sono disponibili in Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] nella cartella **Trigger**. all'interno della cartella **Oggetti server** . I trigger DDL con ambito database sono disponibili nella cartella **Trigger database**. all'interno della cartella **Programmabilità** del database corrispondente.  
  
## <a name="logon-triggers"></a>Trigger LOGON  
I trigger LOGON attivano stored procedure in risposta a un evento LOGON che si verifica quando viene stabilita una sessione utente a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I trigger LOGON vengono attivati dopo il completamento della fase di autenticazione della procedura di accesso, ma prima che la sessione utente venga stabilita, quindi tutti i messaggi generati all'interno del trigger che verrebbero normalmente visualizzati all'utente, come i messaggi di errore e i messaggi dall'istruzione PRINT, vengono invece indirizzati al log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Trigger LOGON](../../relational-databases/triggers/logon-triggers.md).  
  
I trigger LOGON non vengono attivati in caso di esito negativo dell'autenticazione.  
  
In un trigger LOGON non sono supportate transazioni distribuite. Quando viene attivato un trigger LOGON contenente una transazione distribuita, viene restituito l'errore 3969.  
  
### <a name="disabling-a-logon-trigger"></a>Disabilitazione di un trigger di accesso  
Un trigger di accesso può impedire le connessioni al [!INCLUDE[ssDE](../../includes/ssde-md.md)] per tutti gli utenti, inclusi i membri del ruolo predefinito del server **sysadmin** . Quando un trigger di accesso impedisce le connessioni, i membri del ruolo predefinito del server **sysadmin** possono connettersi tramite la connessione amministrativa dedicata o avviando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] nella modalità di configurazione minima (- f). Per altre informazioni, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
## <a name="general-trigger-considerations"></a>Considerazioni generali sui trigger  
  
### <a name="returning-results"></a>Restituzione di risultati  
Nelle versioni future di SQL Server la possibilità di ottenere risultati dai trigger non sarà più disponibile. I trigger che restituiscono set di risultati possono provocare comportamenti imprevisti nelle applicazioni che non sono state progettate per il loro utilizzo. Evitare di restituire set di risultati dai trigger in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente li restituiscono. Per impedire che i trigger restituiscano set di risultati, impostare l'opzione [disallow results from triggers](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) (Non consentire risultati dai trigger) su 1.  
  
I trigger LOGON non consentono mai la restituzione di set di risultati e questo comportamento non è configurabile. Se un trigger LOGON genera un set di risultati, l'avvio del trigger ha esito negativo e viene negato il tentativo di accesso che ha attivato il trigger.  
  
### <a name="multiple-triggers"></a>Più trigger  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di creare più trigger per ogni evento DML, DDL o LOGON. Se, ad esempio, viene eseguita l'istruzione CREATE TRIGGER FOR UPDATE per una tabella che ha già un trigger UPDATE, viene creato un ulteriore trigger di aggiornamento. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile creare un solo trigger per ogni evento di modifica dei dati INSERT, UPDATE o DELETE per ogni tabella.  
  
### <a name="recursive-triggers"></a>Trigger ricorsivi  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta anche chiamate ricorsive di trigger quando viene abilitata l'impostazione RECURSIVE_TRIGGERS con ALTER DATABASE.  
  
I trigger ricorsivi supportano i tipi di ricorsione seguenti:  
  
-   Ricorsione indiretta  
  
     Quando viene utilizzata la ricorsione indiretta, un'applicazione aggiorna la tabella T1, il che attiva il trigger TR1 che aggiorna la tabella T2. Viene quindi attivato il trigger T2 che aggiorna la tabella T1.  
  
-   Ricorsione diretta  
  
     Nella ricorsione diretta un'applicazione aggiorna la tabella T1, il che attiva il trigger TR1 che aggiorna la tabella T1. Poiché la tabella T1 è stata aggiornata, viene di nuovo attivato il trigger TR1 e il processo viene quindi ripetuto.  
  
Nell'esempio seguente vengono utilizzati entrambi i tipi di ricorsione indiretta e diretta. Si supponga che per la tabella T1 vengano definiti due trigger di aggiornamento, TR1 e TR2. Il trigger TR1 aggiorna in modo ricorsivo la tabella T1. Un'istruzione UPDATE esegue ogni trigger TR1 e TR2 una sola volta. Inoltre, l'avvio di TR1 attiva l'esecuzione di TR1 in modo ricorsivo e di TR2. Le tabelle inserted e deleted per un trigger specifico includono solo le righe corrispondenti all'istruzione UPDATE che ha richiamato il trigger.  
  
> [!NOTE]  
>  La sequenza illustrata nell'esempio precedente ha luogo solo quando viene abilitata l'impostazione RECURSIVE_TRIGGERS tramite ALTER DATABASE. Non esiste un ordine prestabilito per l'esecuzione di più trigger definiti per un evento specifico. Ogni trigger deve essere autonomo.  
  
La disabilitazione di RECURSIVE_TRIGGERS consente di evitare solo la ricorsione diretta. Per disabilitare anche la ricorsione indiretta, impostare l'opzione del server nested triggers su 0 utilizzando sp_configure.  
  
Se un trigger esegue un'istruzione ROLLBACK TRANSACTION non vengono eseguiti altri trigger, indipendentemente dal livello di nidificazione.  
  
### <a name="nested-triggers"></a>Trigger nidificati  
È possibile annidare i trigger fino a un massimo di 32 livelli. Se un trigger modifica una tabella che include un altro trigger, viene attivato il secondo trigger, che può chiamare a sua volta un terzo trigger e così via. Se un trigger della catena attiva un ciclo infinito, viene superato il livello massimo di nidificazione e il trigger viene annullato. Quando un trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] avvia il codice gestito facendo riferimento a una routine, un tipo o una funzione di aggregazione CLR, questo riferimento viene conteggiato come un livello per il calcolo del limite di annidamento massimo pari a 32 livelli. I metodi richiamati dal codice gestito non vengono inclusi nel conteggio per questo limite.  
  
Per disabilitare i trigger nidificati, impostare l'opzione nested triggers di sp_configure su 0 (off). La configurazione predefinita supporta i trigger annidati. Quando i trigger annidati sono disattivati, vengono disabilitati anche i trigger ricorsivi, indipendentemente dall'impostazione RECURSIVE_TRIGGERS attivata usando ALTER DATABASE.  
  
Il primo trigger AFTER annidato in un trigger INSTEAD OF viene attivato anche se l'opzione di configurazione del server **nested triggers** è 0, ma, con questa impostazione, i successivi trigger AFTER non vengono attivati. Verificare la presenza di trigger annidati nelle applicazioni per determinare se le applicazioni seguono le regole business quando l'opzione di configurazione del server **nested triggers** è impostata su 0. In caso contrario, apportare le modifiche appropriate.  
  
### <a name="deferred-name-resolution"></a>Risoluzione dei nomi posticipata  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile includere all'interno di stored procedure, trigger e batch [!INCLUDE[tsql](../../includes/tsql-md.md)] riferimenti a tabelle che non esistono in fase di compilazione. Questa funzionalità è denominata risoluzione dei nomi posticipata.  
  
## <a name="permissions"></a>Autorizzazioni  
Per creare un trigger DML, è necessaria l'autorizzazione ALTER per la tabella o la vista in cui creare il trigger.  
  
Per creare un trigger DDL con ambito server (ON ALL SERVER) o un trigger LOGON, è necessaria l'autorizzazione CONTROL SERVER per il server. Per creare un trigger DDL con ambito database (ON DATABASE), è necessaria l'autorizzazione ALTER ANY DATABASE DDL TRIGGER per il database corrente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-dml-trigger-with-a-reminder-message"></a>R. Utilizzo di un trigger DML con un messaggio di promemoria  
Il trigger DML seguente visualizza un messaggio nel client quando un utente tenta di aggiungere o di modificare i dati nella tabella `Customer` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE TRIGGER reminder1  
ON Sales.Customer  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Customer Relations', 16, 10);  
GO  
```  
  
### <a name="b-using-a-dml-trigger-with-a-reminder-e-mail-message"></a>B. Utilizzo di un trigger DML con un messaggio di promemoria inviato tramite posta elettronica  
Nell'esempio seguente viene inviato un messaggio di posta elettronica a un utente specificato (`MaryM`) quando viene apportata una modifica alla tabella `Customer`.  
  
```sql  
CREATE TRIGGER reminder2  
ON Sales.Customer  
AFTER INSERT, UPDATE, DELETE   
AS  
   EXEC msdb.dbo.sp_send_dbmail  
        @profile_name = 'AdventureWorks2012 Administrator',  
        @recipients = 'danw@Adventure-Works.com',  
        @body = 'Don''t forget to print a report for the sales force.',  
        @subject = 'Reminder';  
GO  
```  
  
### <a name="c-using-a-dml-after-trigger-to-enforce-a-business-rule-between-the-purchaseorderheader-and-vendor-tables"></a>C. Utilizzo di un trigger DML AFTER per applicare una regola business tra le tabelle PurchaseOrderHeader e Vendor  
Poiché i vincoli CHECK fanno riferimento solo alle colonne in cui è definito il vincolo a livello di colonna o di tabella, è necessario definire come trigger qualsiasi vincolo tra tabelle, in questo caso le regole business.  
  
Nell'esempio seguente viene creato un trigger DML nel database AdventureWorks2012. Questo trigger verifica che la posizione creditizia del fornitore sia buona (non 5) quando viene eseguito un tentativo di inserimento di un nuovo ordine di acquisto nella tabella `PurchaseOrderHeader`. Per ottenere la posizione creditizia del fornitore, è necessario fare riferimento alla tabella `Vendor`. Se la posizione creditizia è troppo bassa, viene visualizzato un messaggio e l'operazione di inserimento non viene eseguita.  
  
```sql  
-- This trigger prevents a row from being inserted in the Purchasing.PurchaseOrderHeader 
-- table when the credit rating of the specified vendor is set to 5 (below average).  
  
CREATE TRIGGER Purchasing.LowCredit ON Purchasing.PurchaseOrderHeader  
AFTER INSERT  
AS  
IF (ROWCOUNT_BIG() = 0)
RETURN;
IF EXISTS (SELECT *  
           FROM Purchasing.PurchaseOrderHeader AS p   
           JOIN inserted AS i   
           ON p.PurchaseOrderID = i.PurchaseOrderID   
           JOIN Purchasing.Vendor AS v   
           ON v.BusinessEntityID = p.VendorID  
           WHERE v.CreditRating = 5  
          )  
BEGIN  
RAISERROR ('A vendor''s credit rating is too low to accept new  
purchase orders.', 16, 1);  
ROLLBACK TRANSACTION;  
RETURN   
END;  
GO  
  
-- This statement attempts to insert a row into the PurchaseOrderHeader table  
-- for a vendor that has a below average credit rating.  
-- The AFTER INSERT trigger is fired and the INSERT transaction is rolled back.  
  
INSERT INTO Purchasing.PurchaseOrderHeader (RevisionNumber, Status, EmployeeID,  
VendorID, ShipMethodID, OrderDate, ShipDate, SubTotal, TaxAmt, Freight)  
VALUES (  
2  
,3  
,261  
,1652  
,4  
,GETDATE()  
,GETDATE()  
,44594.55  
,3567.564  
,1114.8638 );  
GO  
  
```  
  
### <a name="d-using-a-database-scoped-ddl-trigger"></a>D. Utilizzo di un trigger DDL con ambito database  
Nell'esempio seguente viene utilizzato un trigger DDL per impedire l'eliminazione di qualsiasi sinonimo in un database.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_SYNONYM  
AS   
IF (@@ROWCOUNT = 0)
RETURN;
   RAISERROR ('You must disable Trigger "safety" to remove synonyms!', 10, 1)  
   ROLLBACK  
GO  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
### <a name="e-using-a-server-scoped-ddl-trigger"></a>E. Utilizzo di un trigger DDL con ambito server  
Nell'esempio seguente viene utilizzato un trigger DDL per visualizzare un messaggio se si verifica un evento CREATE DATABASE nell'istanza del server corrente e viene utilizzata la funzione `EVENTDATA` per recuperare il testo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] corrispondente. Per altri esempi di utilizzo di EVENTDATA nei trigger DDL, vedere [Usare la funzione EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
```  
  
### <a name="f-using-a-logon-trigger"></a>F. Utilizzo di un trigger LOGON  
Nell'esempio seguente di trigger LOGON viene negato il tentativo di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come membro dell'account di accesso *login_test* se esistono già tre sessioni utente in esecuzione con tale account di accesso.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
USE master;  
GO  
CREATE LOGIN login_test WITH PASSWORD = '3KHJ6dhx(0xVYsdf' MUST_CHANGE,  
    CHECK_EXPIRATION = ON;  
GO  
GRANT VIEW SERVER STATE TO login_test;  
GO  
CREATE TRIGGER connection_limit_trigger  
ON ALL SERVER WITH EXECUTE AS 'login_test'  
FOR LOGON  
AS  
BEGIN  
IF ORIGINAL_LOGIN()= 'login_test' AND  
    (SELECT COUNT(*) FROM sys.dm_exec_sessions  
            WHERE is_user_process = 1 AND  
                original_login_name = 'login_test') > 3  
    ROLLBACK;  
END;  
  
```  
  
### <a name="g-viewing-the-events-that-cause-a-trigger-to-fire"></a>G. Visualizzazione degli eventi che attivano un trigger  
Nell'esempio seguente viene eseguita una query sulle viste del catalogo `sys.triggers` e `sys.trigger_events` per determinare gli eventi del linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] che attivano il trigger `safety`. Il trigger, `safety`, viene creato nell'esempio 'D' sopra riportato.  
  
```sql  
SELECT TE.*  
FROM sys.trigger_events AS TE  
JOIN sys.triggers AS T ON T.object_id = TE.object_id  
WHERE T.parent_class = 0 AND T.name = 'safety';  
GO  
```  

    

## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [TRIGGER_NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/trigger-nestlevel-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_settriggerorder &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-settriggerorder-transact-sql.md)   
 [UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)   
 [Ottieni informazioni sui trigger DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [Ottenere informazioni sui trigger DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
  
  


