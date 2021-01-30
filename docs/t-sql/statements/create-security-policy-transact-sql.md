---
description: CREATE SECURITY POLICY (Transact-SQL)
title: CREATE SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SECURITY_POLICY_TSQL
- CREATE SECURITY
- SECURITY
- CREATE_SECURITY_POLICY_TSQL
- CREATE_SECURITY_TSQL
- SECURITY POLICY
- SECURITY_TSQL
- CREATE SECURITY POLICY
dev_langs:
- TSQL
helpviewer_keywords:
- RLS
- CREATE SECURITY POLICY statement
- Row-Level Security
ms.assetid: d6ab70ee-0fa2-469c-96f6-a3c16d673bc8
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ddebebc1a204af546734425ffd76781948cd82de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188490"
---
# <a name="create-security-policy-transact-sql"></a>CREATE SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Crea un criterio di sicurezza per la sicurezza a livello di riga.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE SECURITY POLICY [schema_name. ] security_policy_name    
    { ADD [ FILTER | BLOCK ] } PREDICATE tvf_schema_name.security_predicate_function_name   
      ( { column_name | expression } [ , ...n] ) ON table_schema_name. table_name    
      [ <block_dml_operation> ] , [ , ...n] 
    [ WITH ( STATE = { ON | OFF }  [,] [ SCHEMABINDING = { ON | OFF } ] ) ]  
    [ NOT FOR REPLICATION ] 
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>Argomenti
 *security_policy_name*  
 Nome del criterio di sicurezza. I nomi dei criteri sicurezza devono essere conformi alle regole per gli identificatori e devono essere univoci all'interno del database e rispetto al relativo schema.  
  
 *schema_name*  
 Nome dello schema a cui appartiene il criterio di sicurezza. *schema_name* è necessario per l'associazione allo schema.  
  
 [ FILTER | BLOCK ]  
 Tipo del predicato di sicurezza per la funzione da associare alla tabella di destinazione. I predicati FILTER filtrano automaticamente le righe disponibili per le operazioni di lettura. I predicati BLOCK bloccano in modo esplicito le operazioni di scrittura che violano la funzione di predicato.  
  
 *tvf_schema_name.security_predicate_function_name*  
 Funzione con valori di tabella inline che verrà usata come predicato e che verrà applicata durante l'esecuzione di query su una tabella di destinazione. È possibile definire al massimo un predicato di sicurezza per una specifica operazione DML su una determinata tabella. La funzione con valori di tabella inline deve essere stata creata con l'opzione SCHEMABINDING.  
  
 { *column_name* | *expression* }  
 Espressione o nome di colonna usato come parametro per la funzione di predicato di sicurezza. Si può usare qualsiasi colonna nella tabella di destinazione. Un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) può includere solo costanti, funzioni scalari predefinite, operatori e colonne della tabella di destinazione. È necessario specificare un nome di colonna o un'espressione per ogni parametro della funzione.  
  
 *table_schema_name.table_name*  
 Tabella di destinazione a cui verrà applicato il predicato di sicurezza. A una singola tabella possono fare riferimento più criteri di sicurezza disabilitati per un'operazione DML specifica, ma è possibile abilitarne solo uno.  
  
 *\<block_dml_operation>* Operazione DML specifica per la quale verrà applicato il predicato di blocco. AFTER specifica che il predicato verrà valutato in base ai valori delle righe dopo l'esecuzione dell'operazione DML (INSERT o UPDATE). BEFORE specifica che il predicato verrà valutato in base ai valori delle righe prima dell'esecuzione dell'operazione DML (UPDATE o DELETE). Se non è specificata alcuna operazione, il predicato verrà applicato a tutte le operazioni.  
  
 [ STATE = { **ON** | OFF } ]  
 Abilita o disabilita il criterio di sicurezza per l'applicazione dei relativi predicati di sicurezza alle tabelle di destinazione. Se non specificato, il criterio di sicurezza creato è abilitato.  
  
 [ SCHEMABINDING = { **ON** | OFF } ]  
 Indica se tutte le funzioni di predicato nei criteri devono essere create con l'opzione SCHEMABINDING. L'impostazione predefinita è **ON** e tutte le funzioni devono essere create con SCHEMABINDING.  
  
 NOT FOR REPLICATION  
 Indica che il criterio di sicurezza non deve essere eseguito quando un agente di replica modifica l'oggetto di destinazione. Per altre informazioni, vedere [Controllare il comportamento di trigger e vincoli durante la sincronizzazione &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
 [*table_schema_name*.] *table_name*  
 Tabella di destinazione a cui verrà applicato il predicato di sicurezza. A una singola tabella possono fare riferimento più criteri di sicurezza disabilitati, ma è possibile abilitarne solo uno.  
  

## <a name="remarks"></a>Osservazioni
 Quando si usano le funzioni di predicato con tabelle ottimizzate per la memoria, è necessario includere **SCHEMABINDING** e usare l'hint per la compilazione **WITH NATIVE_COMPILATION**.  
  
 I predicati di blocco vengono valutati dopo l'esecuzione dell'operazione DML corrispondente. Pertanto una query READ UNCOMMITTED può rilevare valori temporanei che saranno sottoposti a rollback.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione ALTER ANY SECURITY POLICY e l'autorizzazione ALTER per lo schema.  
  
 Inoltre, per ogni predicato che viene aggiunto sono richieste le autorizzazioni seguenti:  
  
-   Le autorizzazioni SELECT e REFERENCES per la funzione usata come predicato.  
  
-   L'autorizzazione REFERENCES per la tabella di destinazione associata al criterio.  
  
-   L'autorizzazione REFERENCES per ogni colonna della tabella di destinazione usata come argomento.  
  
## <a name="examples"></a>Esempi  
 Gli esempi seguenti illustrano l'uso della sintassi di **CREATE SECURITY POLICY** . Per un esempio di scenario completo dei criteri di sicurezza, vedere [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-creating-a-security-policy"></a>R. Creazione di un criterio di sicurezza  
 La sintassi seguente crea un criterio di sicurezza con un predicato del filtro per la tabella Customer e lascia il criterio di sicurezza disabilitato.  
  
```  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate]([CustomerId])   
ON [dbo].[Customer];  
```  
  
### <a name="b-creating-a-policy-that-affects-multiple-tables"></a>B. Creazione di un criterio che interessa più tabelle  
 La sintassi seguente crea un criterio di sicurezza con tre predicati del filtro per tre tabelle diverse e abilita il criterio di sicurezza.  
  
```sql  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([CustomerId])   
    ON [dbo].[Customer],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([VendorId])   
    ON [dbo].[ Vendor],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate2]([WingId])   
    ON [dbo].[Patient]  
WITH (STATE = ON);  
```  
  
### <a name="c-creating-a-policy-with-multiple-types-of-security-predicates"></a>C. Creazione di criteri con più tipi di predicati di sicurezza  
 Aggiunta sia di un predicato del filtro sia un predicato di blocco alla tabella Sales.  
  
```sql  
CREATE SECURITY POLICY rls.SecPol  
    ADD FILTER PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales,  
    ADD BLOCK PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)   
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  

