---
title: Sicurezza a livello di riga | Microsoft Docs
description: Informazioni su come la sicurezza a livello di riga usa l'appartenenza a gruppi o il contesto di esecuzione per controllare l'accesso alle righe in una tabella di database in SQL Server.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
ms.assetid: 7221fa4e-ca4a-4d5c-9f93-1b8a4af7b9e8
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6eb3fe21d1b5ea41e1f7c70818a7b998f0d3ca42
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172713"
---
# <a name="row-level-security"></a>Sicurezza a livello di riga

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  ![Rappresentazione grafica della sicurezza a livello di riga](../../relational-databases/security/media/row-level-security-graphic.png "Rappresentazione grafica della sicurezza a livello di riga")  
  
La sicurezza a livello di riga consente di usare l'appartenenza a gruppi o il contesto di esecuzione per controllare l'accesso alle righe in una tabella di database.
  
La sicurezza a livello di riga semplifica la progettazione e la codifica della sicurezza nell'applicazione e facilita l'implementazione delle restrizioni di accesso alle righe di dati. Ad esempio, è possibile assicurarsi che i dipendenti possano accedere solo alle righe di dati pertinenti per il loro reparto. Un altro esempio può essere la limitazione dell'accesso ai dati dei clienti solo ai dati rilevanti per l'azienda.  
  
La logica di restrizione dell'accesso si trova sul livello del database e non su un altro livello applicazione lontano dai dati. Il sistema del database applica le restrizioni di accesso a ogni tentativo di accesso ai dati da qualsiasi livello. Il sistema di sicurezza è così più affidabile e solido, grazie alla riduzione della superficie di attacco del sistema di sicurezza.  
  
Implementare la sicurezza a livello di riga tramite l'istruzione [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] e i predicati creati come [funzioni con valori di tabella inline](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([fare clic qui per ottenerlo](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)), [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
  
> [!NOTE]
> Azure Synapse supporta solo predicati di filtro. I predicati di blocco non sono attualmente supportati in Azure Synapse.

## <a name="description"></a><a name="Description"></a> Descrizione

La sicurezza a livello di riga supporta due tipi di predicati di sicurezza.  
  
- I predicati del filtro filtrano automaticamente le righe disponibili per le operazioni di lettura (SELECT, UPDATE e DELETE).  
  
- I predicati di blocco bloccano esplicitamente le operazioni di scrittura (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE e BEFORE DELETE) che violano il predicato.  
  
 L'accesso ai dati a livello di riga in una tabella è limitato da un predicato di sicurezza definito come una funzione inline con valori di tabella. La funzione viene quindi richiamata e applicata dai criteri di sicurezza. Nel caso dei predicati del filtro, l'applicazione non rileva le righe filtrate dal set di risultati. Se vengono filtrate tutte le righe, viene restituito un set Null. Per i predicati di blocco, qualsiasi operazione che violi il predicato non verrà completata e genererà un errore.  
  
 I predicati di filtro vengono applicati durante la lettura dei dati dalla tabella di base e influiscono su tutte le operazioni Get: **SELECT**, **DELETE** e **UPDATE**. Gli utenti non possono selezionare o eliminare le righe filtrate. L'utente non può aggiornare le righe filtrate. Tuttavia, è possibile aggiornare le righe in modo che vengano filtrate in un secondo momento. I predicati di blocco influiscono su tutte le operazioni di scrittura.  
  
- I predicati AFTER INSERT e AFTER UPDATE possono impedire agli utenti di aggiornare le righe con valori che violano il predicato.  
  
- I predicati BEFORE UPDATE possono impedire agli utenti di aggiornare le righe che attualmente violano il predicato.  
  
- I predicati BEFORE DELETE possono bloccare le operazioni di eliminazione.  
  
 I predicati del filtro e di blocco e i criteri di sicurezza si comportano nel modo seguente:  
  
- È possibile definire una funzione di predicato che si unisca a un'altra tabella e/o chiami una funzione. Se i criteri di sicurezza vengono creati con `SCHEMABINDING = ON` (valore predefinito), il join o la funzione è accessibile dalla query e funziona come previsto senza controlli aggiuntivi delle autorizzazioni. Se i criteri di sicurezza vengono creati con `SCHEMABINDING = OFF`, gli utenti dovranno avere autorizzazioni **SELECT** su queste tabelle e funzioni aggiuntive per eseguire query sulla tabella di destinazione. Se la funzione di predicato richiama una funzione CLR a valori scalari, è necessaria anche l'autorizzazione **EXECUTE**.
  
- È possibile inviare una query in una tabella con un predicato di sicurezza definito ma disabilitato. Le righe filtrate o bloccate non sono interessate.  
  
- Se un utente dbo, un membro del ruolo **db_owner** o il proprietario della tabella esegue query su una tabella con criteri di sicurezza definiti e abilitati, le righe vengono filtrate o bloccate in base a quanto definito nei criteri di sicurezza.  
  
- I tentativi di modificare lo schema di una tabella tramite un criterio di sicurezza associato allo schema generano un errore. Le colonne a cui non fa riferimento il predicato possono tuttavia essere modificate.  
  
- I tentativi di aggiunta di un predicato in una tabella in cui è già presente un predicato definito per l'operazione specificata generano un errore. Ciò si verifica indipendentemente dal fatto che il predicato sia abilitato o meno.
  
- I tentativi di modifica di una funzione usata come predicato in una tabella inclusa in criteri di sicurezza associati a schema generano un errore.  
  
- La definizione di più criteri di sicurezza attivi contenenti predicati non sovrapposti riesce correttamente.  
  
 I predicati del filtro si comportano nel modo seguente:  
  
- Definire i criteri di sicurezza per filtrare le righe di una tabella. L'applicazione non rileva le righe filtrate per le operazioni **SELECT**, **UPDATE** e **DELETE**, anche nelle situazioni in cui tutte le righe sono state escluse. L'applicazione può eseguire operazioni **INSERT** di righe, anche se verranno filtrate durante altre operazioni.  
  
 I predicati di blocco si comportano nel modo seguente:  
  
- I predicati di blocco di UPDATE vengono suddivisi in operazioni distinte BEFORE e AFTER. Di conseguenza non è possibile, ad esempio, impedire agli utenti di aggiornare una riga con un valore superiore a quello corrente. Se si deve applicare una logica di questo tipo, occorre usare i trigger con le tabelle intermedie [DELETED e INSERTED](../triggers/use-the-inserted-and-deleted-tables.md) per rimandare ai valori precedenti e nuovi insieme.  
  
- L'ottimizzatore non controllerà il predicato di blocco AFTER UPDATE se non è stata modificata alcuna delle colonne usate dalla funzione del predicato. Ad esempio: Alice non deve essere in grado di modificare uno stipendio in modo che sia maggiore di 100.000. Alice può modificare l'indirizzo di un dipendente il cui stipendio è già maggiore di 100.000, purché le colonne a cui fa riferimento il predicato non siano state modificate.  
  
- Non sono state modificate le API in blocco, compresa l'API BULK INSERT. Questo significa che i predicati di blocco AFTER INSERT verranno applicati alle operazioni di inserimento in blocco come se fossero operazioni di inserimento regolari.  
  
## <a name="use-cases"></a><a name="UseCases"></a> Modalità di utilizzo comuni

 Di seguito sono riportati degli esempi di progettazione relativi alle modalità di utilizzo della sicurezza a livello di riga:  
  
- Un ospedale può creare criteri di sicurezza che consentono agli infermieri di visualizzare le righe di dati solo per i propri pazienti.  
  
- Una banca può creare criteri per limitare l'accesso alle righe di dati finanziari in base alla divisione aziendale del dipendente o al suo ruolo nell'azienda.  
  
- Un'applicazione multi-tenant può creare dei criteri per applicare una separazione logica delle righe di dati di ciascun tenant da qualsiasi altra riga del tenant. L'efficienza viene raggiunta archiviando i dati per diversi tenant in un'unica tabella. Ogni tenant può visualizzare solo le proprie righe di dati.  
  
 I predicati di filtro della sicurezza a livello di riga sono funzionalmente equivalenti all'aggiunta di una clausola **WHERE** . Il predicato può essere sofisticato, se lo richiedono le procedure aziendali, oppure è possibile usare una clausola semplice, ad esempio `WHERE TenantId = 42`.  
  
 In termini più formali, la sicurezza a livello di riga introduce il controllo degli accessi basato su predicato. Tale controllo include una valutazione flessibile e centralizzata basata su predicato. Il predicato può essere basato su metadati o su qualsiasi altro criterio considerato appropriato dall'amministratore. Il predicato viene usato come criterio per determinare se l'utente dispone dell'accesso appropriato ai dati in base agli attributi utente. Il controllo di accesso basato su etichetta può essere implementato usando il controllo di accesso basato su predicato.  
  
## <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

 La creazione, la modifica o l'eliminazione dei criteri di sicurezza richiede l'autorizzazione **ALTER ANY SECURITY POLICY** . La creazione o l'eliminazione dei criteri di sicurezza richiede l'autorizzazione **ALTER** nello schema.  
  
 Inoltre, per ogni predicato che viene aggiunto sono richieste le autorizzazioni seguenti:  
  
- Le autorizzazioni **SELECT** e **REFERENCES** per la funzione usata come predicato.  
  
- L'autorizzazione **REFERENCES** per la tabella di destinazione associata ai criteri.  
  
- L'autorizzazione **REFERENCES** per ogni colonna della tabella di destinazione usata come argomento.  
  
 I criteri di sicurezza sono applicati a tutti gli utenti, inclusi gli utenti dbo nel database. Gli utenti dbo possono modificare o eliminare i criteri di sicurezza, tuttavia tali modifiche ai criteri di sicurezza possono essere controllate. Se un utente con privilegi elevati, ad esempio sysadmin o db_owner, deve visualizzare tutte le righe per risolvere i problemi o convalidare i dati, i criteri di sicurezza devono essere scritti in modo da consentire tale operazione.  
  
 Se i criteri di sicurezza vengono creati con `SCHEMABINDING = OFF`, per eseguire query sulla tabella di destinazione gli utenti devono avere l'autorizzazione  **SELECT** o **EXECUTE** sulla funzione di predicato e su qualsiasi tabella, vista o funzione aggiuntiva usata nella funzione di predicato. Se i criteri di sicurezza vengono creati con `SCHEMABINDING = ON` (impostazione predefinita), questi controlli delle autorizzazioni vengono ignorati quando gli utenti eseguono query sulla tabella di destinazione.  
  
## <a name="best-practices"></a><a name="Best"></a> Procedure consigliate  
  
- È consigliabile creare uno schema separato per gli oggetti con sicurezza a livello di riga, vale a dire funzioni di predicato e criteri di sicurezza. In questo modo si separano le autorizzazioni necessarie per questi oggetti speciali dalle tabelle di destinazione. La separazione aggiuntiva per diversi criteri e funzioni di predicato può essere necessaria nei database multi-tenant, ma non rappresenta uno standard per ogni caso.
  
- L'autorizzazione **ALTER ANY SECURITY POLICY** è destinata agli utenti con privilegi elevati (ad esempio il gestore dei criteri di sicurezza). Il gestore dei criteri di sicurezza non richiede l'autorizzazione **SELECT** per le tabelle che protegge.  
  
- Non usare le conversioni del tipo nelle funzioni di predicato per evitare potenziali errori di run-time.  
  
- Se possibile, evitare la ricorsione nelle funzioni di predicato per evitare un calo delle prestazioni. Query Optimizer proverà a rilevare le ricorsioni dirette, ma non è garantito che trovi quelle indirette. Una ricorsione indiretta si verifica quando una seconda funzione chiama la funzione di predicato.  
  
- Evitare di usare un numero eccessivo di join di tabella nelle funzioni di predicato per ottimizzare le prestazioni.  
  
 Evitare una logica del predicato dipendente da [opzioni SET](../../t-sql/statements/set-statements-transact-sql.md) specifiche della sessione: nonostante sia improbabile che vengano usate in applicazioni pratiche, le funzioni del predicato la cui logica dipende da determinate opzioni **SET** specifiche della sessione possono causare la perdita di informazioni se gli utenti possono eseguire query arbitrarie. Ad esempio, una funzione di predicato che converte implicitamente una stringa in **datetime** potrebbe filtrare righe diverse in base all'opzione **SET DATEFORMAT** per la sessione corrente. In generale le funzioni di predicato devono rispettare le regole seguenti:  
  
- Le funzioni di predicato non devono convertire implicitamente stringhe di caratteri in **date**, **smalldatetime**, **datetime**, **datetime2** o **datetimeoffset** né viceversa, perché queste conversioni sono influenzate dalle opzioni [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md) e [SET LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/statements/set-language-transact-sql.md). Utilizzare invece la funzione **CONVERT** e specificare esplicitamente il parametro di stile.  
  
- Le funzioni di predicato non devono basarsi sul valore del primo giorno della settimana, perché questo valore è influenzato dall'opzione [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).  
  
- Le funzioni di predicato non devono basarsi su espressioni aritmetiche o di aggregazione che restituiscono **NULL** in caso di errore, come overflow o divisione per zero, perché questo comportamento è influenzato dalle opzioni [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md), [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) e [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
- Le funzioni di predicato non devono confrontare stringhe concatenate con **NULL**, perché questo comportamento è influenzato dall'opzione [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  

## <a name="security-note-side-channel-attacks"></a><a name="SecNote"></a> Nota sulla sicurezza: Attacchi side-channel

### <a name="malicious-security-policy-manager"></a>Gestore dei criteri di sicurezza malintenzionato

è importante osservare che un gestore dei criteri di sicurezza malintenzionato, con autorizzazioni sufficienti per creare criteri di sicurezza per una colonna sensibile e per creare o modificare le funzioni inline con valori di tabella, può agire in collusione con un altro utente con autorizzazioni Select su una tabella al fine di estrarre dolosamente i dati creando funzioni inline con valori di tabella progettate per usare attacchi al canale laterale per estrapolare i dati. Questi attacchi richiedono la collusione con altre persone (o autorizzazioni eccessive concesse a un utente malintenzionato) e richiedono probabilmente diversi tentativi di modifica dei criteri (che richiede autorizzazioni per rimuovere il predicato per interrompere l'associazione allo schema), la modifica delle funzioni con valori di tabella inline e diverse esecuzioni delle istruzioni Select nella tabella di destinazione. È consigliabile limitare le autorizzazioni in base alle esigenze e monitorare le attività sospette. Si dovrebbero monitorare le attività come modifiche costanti dei criteri e di funzioni inline con valori di tabella correlate alla sicurezza a livello di riga.  
  
### <a name="carefully-crafted-queries"></a>Query appositamente create

È possibile causare perdite di informazioni mediante l'utilizzo di query appositamente create. Ad esempio, `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` consentirebbe a un utente malintenzionato di sapere che lo stipendio di John Doe ammonta a 100.000 dollari. Anche se è disponibile un predicato di sicurezza per impedire le query dirette di un utente malintenzionato relative allo stipendio degli altri dipendenti, l'utente può determinare quando la query restituisce un'eccezione di divisione per zero.  

## <a name="cross-feature-compatibility"></a><a name="Limitations"></a> Compatibilità tra funzionalità

 In generale la sicurezza a livello di riga funziona tra varie funzionalità nel modo previsto. Esistono tuttavia alcune eccezioni a questa regola. Questa sezione contiene diverse note e avvertenze per l'uso della sicurezza a livello di riga con altre funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- **DBCC SHOW_STATISTICS** genera statistiche sui dati non filtrati e può causare perdite di informazioni altrimenti protette da criteri di sicurezza. Per questo motivo, l'accesso per visualizzare un oggetto statistiche per una tabella con criteri di sicurezza a livello di riga è limitato. L'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server sysadmin o del ruolo predefinito del database db_owner o db_ddladmin.  
  
- **Filestream:** la sicurezza a livello di riga non è compatibile con Filestream.  
  
- **PolyBase:** la sicurezza a livello di riga è supportata solo con tabelle esterne Polybase per Azure Synapse.

- **Tabelle ottimizzate per la memoria:** la funzione inline con valori di tabella usata come predicato di sicurezza in una tabella ottimizzata per la memoria deve essere definita con l'opzione `WITH NATIVE_COMPILATION`. Con questa opzione le funzionalità del linguaggio non supportate dalle tabelle ottimizzate per la memoria verranno escluse e verrà generato l'errore appropriato al momento della creazione. Per altre informazioni, vedere la sezione relativa alla **sicurezza a livello di riga nelle tabelle con ottimizzazione per la memoria** in [Introduzione alle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
- **Viste indicizzate:** in generale è possibile creare criteri di sicurezza nelle viste ed è possibile creare viste su tabelle associate a criteri di sicurezza. Non è possibile tuttavia creare viste indicizzate in tabelle con un criterio di sicurezza, poiché le ricerche di righe tramite l'indice potrebbero ignorare il criterio.  
  
- **Change Data Capture:** Change Data Capture può causare la perdita di intere righe che devono essere filtrate per i membri di **db_owner** o gli utenti membri del ruolo di "controllo" specificato quando Change Data Capture viene abilitato per una tabella (si noti che è possibile impostare esplicitamente questa funzione su **NULL** per consentire a tutti gli utenti l'accesso ai dati di modifica). I membri di questo ruolo di controllo e **db_owner** possono infatti visualizzare tutte le modifiche dei dati in una tabella anche se per la tabella esistono criteri di sicurezza.  
  
- **Rilevamento delle modifiche:** il rilevamento delle modifiche può provocare la perdita della chiave primaria delle righe che devono essere filtrate per gli utenti con autorizzazioni **SELECT** e **VIEW CHANGE TRACKING**. I valori dei dati effettivi non vengono perduti, ma va perduto solo il fatto che la colonna A è stata aggiornata/inserita/eliminata per la riga con la chiave primaria B. Questo rappresenta un problema se la chiave primaria contiene un elemento riservato, ad esempio un codice fiscale. In pratica però **CHANGETABLE** è quasi sempre unito alla tabella originale per ottenere i dati più recenti.  
  
- **Ricerca full-text:** è previsto un peggioramento delle prestazioni per le query che usano le funzioni seguenti di ricerca full-text e ricerca semantica, a causa di un join aggiuntivo introdotto per applicare la sicurezza a livello di riga ed evitare la perdita di chiavi primarie delle righe che devono essere filtrate: **CONTAINSTABLE**, **FREETEXTTABLE**, semantickeyphrasetable, semanticsimilaritydetailstable, semanticsimilaritytable.  
  
- **Indici columnstore:** la sicurezza a livello di riga è compatibile con indici columnstore sia cluster che non cluster. Dato però che la sicurezza a livello di riga applica una funzione, l'ottimizzatore può modificare il piano di query in modo che non usi la modalità batch.  
  
- **Viste partizionate:** non è possibile definire predicati di blocco nelle viste partizionate e non è possibile creare viste partizionate in tabelle che usano predicati di blocco. I predicati di filtro sono compatibili con le viste partizionate.  
  
- **Tabelle temporali:** le tabelle temporali sono compatibili con la sicurezza a livello di riga. I predicati di sicurezza nella tabella corrente non vengono tuttavia replicati automaticamente nella tabella di cronologia. Per applicare un criterio di sicurezza alla tabella della cronologia e alla tabella corrente, è necessario aggiungere singolarmente un predicato di sicurezza in ogni tabella.  
  
## <a name="examples"></a><a name="CodeExamples"></a> Esempi  
  
### <a name="a-scenario-for-users-who-authenticate-to-the-database"></a><a name="Typical"></a> A. Scenari per gli utenti che eseguono l'autenticazione nel database

 In questo esempio vengono creati tre utenti, quindi viene creata e popolata una tabella con sei righe. Vengono quindi creati una funzione inline con valori di tabella e criteri di sicurezza per la tabella. L'esempio mostra poi in che modo le istruzioni Select vengono filtrate per i diversi utenti.  
  
 Creare tre account utente per mostrare le diverse capacità di accesso.  


```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```

Creare una tabella per contenere i dati.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

 Popolare la tabella con sei righe di dati che visualizzano tre ordini per ogni rappresentante.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Concedere l'accesso in lettura alla tabella a ciascuno degli utenti.  

```sql
GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO Sales1;  
GRANT SELECT ON Sales TO Sales2;  
```

Creare un nuovo schema e una funzione con valori di tabella inline. La funzione restituisce 1 quando una riga nella colonna SalesRep è uguale all'utente che esegue la query (`@SalesRep = USER_NAME()`) o se l'utente che esegue la query è l'utente gestore (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Creare i criteri di sicurezza aggiungendo la funzione come predicato di filtro. Lo stato deve essere impostato su ON per abilitare i criteri.

```sql
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales  
WITH (STATE = ON);  
```

Consentire le autorizzazioni SELECT per la funzione fn_securitypredicate 
```sql
GRANT SELECT ON security.fn_securitypredicate TO Manager;  
GRANT SELECT ON security.fn_securitypredicate TO Sales1;  
GRANT SELECT ON security.fn_securitypredicate TO Sales2;  
```

Testare il predicato di filtro mediante la selezione dalla tabella Sales per ciascun utente.

```sql
EXECUTE AS USER = 'Sales1';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Sales2';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales;
REVERT;  
```

Il gestore dovrebbe visualizzare tutte e sei le righe. Gli utenti Sales1 e Sales2 dovrebbero visualizzare solo le proprie vendite.

Modificare i criteri di sicurezza per disabilitarli.

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

Ora gli utenti Sales1 e Sales2 possono visualizzare tutte e sei le righe.

Connettersi al database SQL per pulire le risorse

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

### <a name="b-scenarios-for-using-row-level-security-on-an-azure-synapse-external-table"></a><a name="external"></a> B. Scenari per l'uso della sicurezza a livello di riga in una tabella esterna di Azure Synapse

Questo breve esempio crea tre utenti e una tabella esterna con sei righe. Vengono quindi creati una funzione inline con valori di tabella e criteri di sicurezza per la tabella esterna. L'esempio mostra in che modo le istruzioni Select vengono filtrate per i diversi utenti. 

### <a name="prerequisites"></a>Prerequisiti

1. È necessario disporre di un pool SQL dedicato. Vedere [Creare un pool SQL dedicato](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal)
1. Il server che ospita il pool SQL dedicato deve essere registrato in AAD ed è necessario avere un account di archiviazione di Azure con le autorizzazioni di collaboratore al BLOB di archiviazione. Seguire i passaggi [qui](/azure/azure-sql/database/vnet-service-endpoint-rule-overview#steps).
1. Creare un file system per l'account di archiviazione di Azure. Usare Storage Explorer per visualizzare l'account di archiviazione. Fare clic con il pulsante destro del mouse su contenitori e scegliere *Crea file system*.  

Una volta soddisfatti i prerequisiti, creare tre account utente che dimostreranno le diverse funzionalità di accesso.

```sql
--run in master
CREATE LOGIN Manager WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales1 WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales2 WITH PASSWORD = '<user_password>'
GO

--run in master and your dedicated SQL pool database
CREATE USER Manager FOR LOGIN Manager;  
CREATE USER Sales1  FOR LOGIN Sales1;  
CREATE USER Sales2  FOR LOGIN Sales2 ;
```

Creare una tabella per contenere i dati.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

Popolare la tabella con sei righe di dati che visualizzano tre ordini per ogni rappresentante.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Creare una tabella esterna di Azure Synapse dalla tabella Sales appena creata.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<user_password>';

CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://<file_system_name@storage_account>.dfs.core.windows.net', CREDENTIAL = msi_cred);

CREATE EXTERNAL FILE FORMAT MSIFormat  WITH (FORMAT_TYPE=DELIMITEDTEXT);
  
CREATE EXTERNAL TABLE Sales_ext WITH (LOCATION='<your_table_name>', DATA_SOURCE=ext_datasource_with_abfss, FILE_FORMAT=MSIFormat, REJECT_TYPE=Percentage, REJECT_SAMPLE_VALUE=100, REJECT_VALUE=100)
AS SELECT * FROM sales;
```

Concedere SELECT per i tre utenti nella tabella esterna Sales_ext creata.

```sql
GRANT SELECT ON Sales_ext TO Sales1;  
GRANT SELECT ON Sales_ext TO Sales2;  
GRANT SELECT ON Sales_ext TO Manager;
```

Creare un nuovo schema e una funzione inline con valori di tabella (è possibile che queste operazioni siano state completate nell'esempio A). La funzione restituisce 1 quando una riga nella colonna SalesRep è uguale all'utente che esegue la query (`@SalesRep = USER_NAME()`) o se l'utente che esegue la query è l'utente gestore (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Creare criteri di sicurezza per la tabella esterna usando la funzione inline con valori di tabella come predicato del filtro. Lo stato deve essere impostato su ON per abilitare i criteri.

```sql
CREATE SECURITY POLICY SalesFilter_ext
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales_ext  
WITH (STATE = ON);
```

Testare ora il predicato di filtro mediante la selezione dalla tabella esterna Sales_ext. Eseguire l'accesso con ogni utente, ovvero Sales1, Sales2 e manager. Eseguire il comando seguente per ogni utente.

```sql
SELECT * FROM Sales_ext;
```

Il gestore dovrebbe visualizzare tutte e sei le righe. Gli utenti Sales1 e Sales2 dovrebbero visualizzare solo le proprie vendite.

Modificare i criteri di sicurezza per disabilitarli.

```sql
ALTER SECURITY POLICY SalesFilter_ext  
WITH (STATE = OFF);  
```

Ora gli utenti Sales1 e Sales2 possono visualizzare tutte e sei le righe.

Connettersi al database Azure Synapse per pulire le risorse

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter_ext;
DROP TABLE Sales;
DROP EXTERNAL TABLE Sales_ext;
DROP EXTERNAL DATA SOURCE ext_datasource_with_abfss ;
DROP EXTERNAL FILE FORMAT MSIFormat;
DROP DATABASE SCOPED CREDENTIAL msi_cred; 
DROP MASTER KEY;
```

Connettersi al database master logico per pulire le risorse.

```sql
DROP LOGIN Sales1;
DROP LOGIN Sales2;
DROP LOGIN Manager;
```

### <a name="c-scenario-for-users-who-connect-to-the-database-through-a-middle-tier-application"></a><a name="MidTier"></a> C. Scenari per gli utenti che si connettono al database tramite un'applicazione di livello intermedio

> [!NOTE]
> In questo esempio poiché la funzionalità di predicati di blocco non è attualmente supportata per Azure Synapse, l'inserimento di righe per l'ID utente errato non viene bloccato con Azure Synapse.

Questo esempio mostra in che modo un'applicazione di livello intermedio può implementare il filtro della connessione, in cui gli utenti dell'applicazione (o i tenant) condividono lo stesso utente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (l'applicazione). L'applicazione imposta l'ID utente dell'applicazione corrente in [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md) dopo la connessione al database, quindi i criteri di sicurezza filtrano in modo trasparente le righe che non devono essere visibili a tale ID e impediscono all'utente di inserire righe per l'ID utente errato. Non sono necessarie altre modifiche all'applicazione.  
  
 Creare una tabella per contenere i dati.

```sql
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```

Popolare la tabella con sei righe di dati che visualizzano tre ordini per ogni utente dell'applicazione.

```sql
INSERT Sales VALUES
    (1, 1, 'Valve', 5),
    (2, 1, 'Wheel', 2),
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),
    (5, 2, 'Wheel', 5),
    (6, 2, 'Seat', 5);  
```

Creare un utente con privilegi limitati che l'applicazione userà per connettersi.

```sql
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```

Creare un nuovo schema e una nuova funzione di predicato con cui usare l'ID utente dell'applicazione archiviato in **SESSION_CONTEXT** per filtrare le righe.

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;
GO  
```

Creare un criterio di sicurezza che aggiunga questa funzione come predicato di filtro e predicato di blocco in `Sales`. Nel predicato di blocco è necessario solo **AFTER INSERT**, perché **BEFORE UPDATE** e **BEFORE DELETE** sono già filtrati e **AFTER UPDATE** non è necessario perché la colonna `AppUserId` non può essere aggiornata con altri valori a causa dell'autorizzazione per la colonna impostata in precedenza.

```sql
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales AFTER INSERT
    WITH (STATE = ON);  
```

Ora è possibile simulare il filtro della connessione con la selezione dalla tabella `Sales` dopo l'impostazione di ID utente diversi in **SESSION_CONTEXT**. In pratica, l'applicazione è responsabile dell'impostazione dell'ID utente corrente in **SESSION_CONTEXT** dopo l'apertura di una connessione.

```sql
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
/* Note: @read_only prevents the value from changing again until the connection is closed (returned to the connection pool)*/
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```

Pulire le risorse del database.

```sql
DROP USER AppUser;

DROP SECURITY POLICY Security.SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

## <a name="see-also"></a>Vedere anche

 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)</br>
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)</br>
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)</br>
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)</br>
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)</br>
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)</br>
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)</br>
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)</br>
 [Creare funzioni definite dall'utente &#40;motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)