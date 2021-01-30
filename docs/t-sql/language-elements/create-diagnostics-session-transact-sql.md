---
description: CREATE DIAGNOSTICS SESSION (Transact-SQL)
title: CREATE DIAGNOSTICS SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 662d019e-f217-49df-9e2f-b5662fa0342d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 14a913922b62500d527928ca37e5b10f50acb0da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208690"
---
# <a name="create-diagnostics-session-transact-sql"></a>CREATE DIAGNOSTICS SESSION (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Le sessioni di diagnostica consentono di salvare informazioni di diagnostica dettagliate definite dall'utente, sulle prestazioni del sistema o delle query.  
  
 Le sessioni di diagnostica vengono in genere usate per eseguire il debug delle prestazioni per una query specifica o per monitorare il comportamento di un componente specifico di un'appliance durante un'operazione dell'appliance.  
  
> [!NOTE]  
>  Per usare sessioni di diagnostica, è necessario conoscere il linguaggio XML.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Creating a new diagnostics session:  
CREATE DIAGNOSTICS SESSION diagnostics_name AS N'{<session_xml>}';  
  
<session_xml>::  
<Session>  
   [ <MaxItemCount>max_item_count_num</MaxItemCount> ]  
   <Filter>  
      { \<Event Name="event_name"/>  
         [ <Where>\<filter_property_name Name="value" ComparisonType="comp_type"/></Where> ] [ ,...n ]  
      } [ ,...n ]  
   </Filter> ]   
   <Capture>  
      \<Property Name="property_name"/> [ ,...n ]  
   </Capture>  
<Session>  
  
-- Retrieving results for a diagnostics session:  
SELECT * FROM master.sysdiag.diagnostics_name ;  
  
-- Removing results for a diagnostics session:  
DROP DIAGNOSTICS SESSION diagnostics_name ;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *diagnostics_name*  
 Nome della sessione di diagnostica. I nomi delle sessioni di diagnostica possono includere solo i caratteri a-z, A-Z e 0-9. I nomi delle sessioni di diagnostica, poi, devono iniziare con un carattere. *diagnostics_name* ha un limite di 127 caratteri.  
  
 *max_item_count_num*  
 Numero di eventi che devono essere persistenti in una visualizzazione. Se ad esempio si specifica 100, i 100 eventi più recenti corrispondenti ai criteri di filtro verranno resi persistenti per la sessione di diagnostica. Se vengono trovati meno di 100 eventi corrispondenti, la sessione di diagnostica conterrà meno di 100 eventi. *max_item_count_num* deve essere almeno 100 e minore o uguale a 100.000.  
  
 *event_name*  
 Definisce gli eventi effettivi da raccogliere nella sessione di diagnostica.  *event_name* è uno degli eventi elencati in [sys.pdw_diag_events](../../relational-databases/system-catalog-views/sys-pdw-diag-events-transact-sql.md) dove `sys.pdw_diag_events.is_enabled='True'`.  
  
 *filter_property_name*  
 Nome della proprietà in base alla quale limitare i risultati. Se ad esempio si vuole applicare la limitazione in base all'ID di sessione *filter_property_name* deve corrispondere a *SessionId*. Vedere *property_name* più avanti per un elenco di valori possibili per *filter_property_name*.  
  
 *value*  
 Valore da valutare rispetto a *filter_property_name*. Il tipo valore deve corrispondere al tipo della proprietà. S ad esempio la proprietà è di tipo decimal, il tipo di *value* deve essere decimal.  
  
 *comp_type*  
 Tipo di confronto. Valori possibili: Equals, EqualsOrGreaterThan, EqualsOrLessThan, GreaterThan, LessThan, NotEquals, Contains, RegEx  
  
 *property_name*  
 Proprietà correlata all'evento.  I nomi di proprietà possono far parte del tag di acquisizione o essere usati all'interno di criteri di filtro.  
  
|Nome proprietà|Descrizione|  
|-------------------|-----------------|  
|UserName|Nome (ID) dell'utente.|  
|SessionId|ID della sessione.|  
|QueryId|ID della query.|  
|CommandType|Tipo di comando.|  
|CommandText|Testo all'interno di un comando elaborato.|  
|Tipo operazione|Tipo di operazione per l'evento.|  
|Duration|Durata dell'evento.|  
|SPID|ID di processo del servizio.|  
  
## <a name="remarks"></a>Osservazioni  
 Per ogni utente è consentito un massimo di 10 sessioni di diagnostica simultanee. Vedere [sys.pdw_diag_sessions](../../relational-databases/system-catalog-views/sys-pdw-diag-sessions-transact-sql.md) per un elenco delle sessioni correnti e per rilasciare le sessioni non necessarie tramite `DROP DIAGNOSTICS SESSION`.  
  
 Le sessioni di diagnostica continuano a raccogliere metadati finché non vengono rilasciate.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **ALTER SERVER STATE**.  
  
## <a name="locking"></a>Blocco  
 Acquisisce un blocco condiviso sulla tabella delle sessioni di diagnostica.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-diagnostics-session"></a>R. Creazione di una sessione di diagnostica  
 Questo esempio crea una sessione di diagnostica per la registrazione di metriche delle prestazioni del motore di database. L'esempio crea una sessione di diagnostica in ascolto di eventi di esecuzione o fine di query motore e di un evento del Servizio Migrazione del database bloccante. Vengono restituiti il testo del comando, il nome del computer, l'ID della richiesta (ID query) e la sessione in cui è stato creato l'evento.  
  
```sql  
CREATE DIAGNOSTICS SESSION MYDIAGSESSION AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:EngineQueryRunningEvent" />  
      <Event Name="DmsCoreInstrumentation:DmsBlockingQueueEnqueueBeginEvent" />  
      <Where>  
         <SessionId Value="381" ComparisonType="NotEquals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Query.CommandText" />  
      <Property Name="MachineName" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Alias" />  
      <Property Name="Duration" />  
      <Property Name="Session.SessionId" />  
   </Capture>  
</Session>';  
```  
  
 Dopo la creazione della sessione di diagnostica, eseguire una query.  
  
```sql  
SELECT COUNT(EmployeeKey) FROM AdventureWorksPDW2012..FactSalesQuota;  
```  
  
 Visualizzare quindi i risultati della sessione di diagnostica effettuando una selezione dallo schema sysdiag.  
  
```sql  
SELECT * FROM master.sysdiag.MYDIAGSESSION;  
```  
  
 Si noti che lo schema sysdiag contiene una visualizzazione con il nome della sessione di diagnostica.  
  
 Per visualizzare solo l'attività relativa alla propria connessione, aggiungere la proprietà `Session.SPID` e aggiungere `WHERE [Session.SPID] = @@spid;` alla query.  
  
 Al termine della sessione di diagnostica, rilasciarla tramite il comando **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION MYDIAGSESSION;  
```  
  
### <a name="b-alternative-diagnostic-session"></a>B. Sessione di diagnostica alternativa  
 Un altro esempio con proprietà leggermente diverse.  
  
```sql  
-- Determine the session_id of your current session  
SELECT TOP 1 session_id();  
-- Replace \<*session_number*> in the code below with the numbers in your session_id  
CREATE DIAGNOSTICS SESSION PdwOptimizationDiagnostics AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:MemoGenerationBeginEvent" />  
      <Event Name="EngineInstrumentation:MemoGenerationEndEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationBeginEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationEndEvent" />  
      <Event Name="DSQLInstrumentation:BuildRelOpContextTreeBeginEvent" />  
      <Event Name="DSQLInstrumentation:PostPlanGenModifiersEndEvent" />  
      <Where>  
         <SessionId Value="\<*session_number*>" ComparisonType="Equals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Session.SessionId" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Query.CommandText" />  
      <Property Name="Name" />  
      <Property Name="DateTimePublished" />  
      <Property Name="DateTimePublished.Ticks" />  
  </Capture>  
</Session>';  
```  
  
 Eseguire una query, ad esempio:  
  
```sql  
USE ssawPDW;  
GO  
SELECT * FROM dbo.FactFinance;  
```  
  
 La query seguente restituisce l'intervallo di autorizzazione:  
  
```sql  
SELECT *   
FROM master.sysdiag.PdwOptimizationDiagnostics   
ORDER BY DateTimePublished;  
```  
  
 Al termine della sessione di diagnostica, rilasciarla tramite il comando **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION PdwOptimizationDiagnostics;  
```  
  
  
