---
description: ALTER SECURITY POLICY (Transact-SQL)
title: ALTER SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SECURITY_POLICY_TSQL
- ALTER SECURITY POLICY
- ALTER_SECURITY_TSQL
- ALTER SECURITY
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER SECURITY POLICY statement
ms.assetid: a8efc37e-113d-489c-babc-b914fea2c316
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e7c0976f6fe428b21817a2577cced14dda66852e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182202"
---
# <a name="alter-security-policy-transact-sql"></a>ALTER SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Modifica un criterio di sicurezza.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER SECURITY POLICY schema_name.security_policy_name   
    (  
        { ADD { FILTER | BLOCK } PREDICATE tvf_schema_name.security_predicate_function_name   
           ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ]  }   
        | { ALTER { FILTER | BLOCK } PREDICATE tvf_schema_name.new_security_predicate_function_name   
             ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ] }  
        | { DROP { FILTER | BLOCK } PREDICATE ON table_schema_name.table_name }   
        | [ <additional_add_alter_drop_predicate_statements> [ , ...n ] ]  
    )    [ WITH ( STATE = { ON | OFF } ) ]  
    [ NOT FOR REPLICATION ]  
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>Argomenti
security_policy_name  
Nome del criterio di sicurezza. I nomi dei criteri di sicurezza devono soddisfare le regole per gli identificatori e devono essere univoci all'interno del database e rispetto al relativo schema.  
  
schema_name  
Nome dello schema a cui appartiene il criterio di sicurezza. *schema_name* è necessario per l'associazione allo schema.  
  
[ FILTER | BLOCK ]  
Tipo del predicato di sicurezza per la funzione da associare alla tabella di destinazione. I predicati FILTER filtrano automaticamente le righe disponibili per le operazioni di lettura. I predicati BLOCK bloccano in modo esplicito le operazioni di scrittura che violano la funzione di predicato.  
  
tvf_schema_name.security_predicate_function_name  
È la funzione con valori di tabella inline usata come predicato e applicata per le query su una tabella di destinazione. È possibile definire al massimo un predicato di sicurezza per una specifica operazione DML su una determinata tabella. Creare la funzione con valori di tabella inline con l'opzione SCHEMABINDING.  
  
{ column_name | arguments }  
Espressione o nome di colonna usato come parametro per la funzione di predicato di sicurezza. Tutte le colonne nella tabella di destinazione possono essere usate come argomenti per la funzione di predicato. È possibile usare espressioni che includono valori letterali, valori predefiniti ed espressioni che usano operatori aritmetici.  
  
*table_schema_name.table_name*  
È la tabella di destinazione per il predicato di sicurezza. A una singola tabella possono fare riferimento più criteri di sicurezza disabilitati per un'operazione DML specifica, ma è possibile abilitarne solo uno.  
  
*\<block_dml_operation>*  
Operazione DML specifica per il predicato di blocco applicato. AFTER specifica che il predicato viene valutato in base ai valori delle righe dopo l'esecuzione dell'operazione DML (INSERT o UPDATE). BEFORE specifica che il predicato viene valutato in base ai valori delle righe prima dell'esecuzione dell'operazione DML (UPDATE o DELETE). Se non è specificata alcuna operazione, il predicato verrà applicato a tutte le operazioni.  
  
Non è possibile applicare ALTER all'operazione per un predicato di blocco applicato, perché l'operazione viene usata per identificare in modo univoco il predicato. In alternativa, è necessario eliminare il predicato e aggiungerne uno nuovo per la nuova operazione.  
  
WITH ( STATE = { ON | OFF } )  
Abilita o disabilita il criterio di sicurezza per l'applicazione dei relativi predicati di sicurezza alle tabelle di destinazione. Se non specificato, il criterio di sicurezza creato è abilitato.  
  
NOT FOR REPLICATION  
Indica che il criterio di sicurezza non deve essere eseguito quando un agente di replica modifica l'oggetto di destinazione. Per altre informazioni, vedere [Controllare il comportamento di trigger e vincoli durante la sincronizzazione &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
table_schema_name.table_name  
È la tabella di destinazione per il predicato di sicurezza applicato. A una singola tabella possono fare riferimento più criteri di sicurezza disabilitati, ma è possibile abilitarne solo uno.  
  
## <a name="remarks"></a>Osservazioni
L'istruzione ALTER SECURITY POLICY è nell'ambito di una transazione. L'esecuzione del rollback della transazione comporta il rollback anche per l'istruzione.  
  
Quando si usano le funzioni di predicato con tabelle ottimizzate per la memoria, i criteri di sicurezza devono includere **SCHEMABINDING** e usare l'hint per la compilazione **WITH NATIVE_COMPILATION**. L'argomento SCHEMABINDING non può essere modificato con l'istruzione ALTER perché si applica a tutti i predicati. Per modificare l'associazione dello schema, è necessario eliminare e ricreare i criteri di sicurezza.  
  
I predicati di blocco vengono valutati dopo l'esecuzione dell'operazione DML corrispondente. Pertanto, una query READ UNCOMMITTED può rilevare valori temporanei che saranno sottoposti a rollback.  
  
## <a name="permissions"></a>Autorizzazioni  
Richiede l'autorizzazione ALTER ANY SECURITY POLICY.  
  
Inoltre, per ogni predicato che viene aggiunto sono richieste le autorizzazioni seguenti:  
  
-   Le autorizzazioni SELECT e REFERENCES per la funzione usata come predicato.  
-   L'autorizzazione REFERENCES per la tabella di destinazione associata al criterio.  
-   L'autorizzazione REFERENCES per ogni colonna della tabella di destinazione usata come argomento.  
  
## <a name="examples"></a>Esempi  
Negli esempi seguenti viene illustrato l'uso della sintassi di **CREATE SECURITY POLICY** . Per un esempio di scenario completo dei criteri di sicurezza, vedere [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-adding-an-additional-predicate-to-a-policy"></a>R. Aggiunta di un altro predicato a un criterio  
La sintassi seguente modifica un criterio di sicurezza, aggiungendo un predicato del filtro per la tabella `mytable`.  
  
```sql  
ALTER SECURITY POLICY pol1   
    ADD FILTER PREDICATE schema_preds.SecPredicate(column1)   
    ON myschema.mytable;  
```  
  
### <a name="b-enabling-an-existing-policy"></a>B. Abilitazione di un criterio esistente  
L'esempio seguente usa la sintassi di ALTER per abilitare un criterio di sicurezza.  
  
```sql  
ALTER SECURITY POLICY pol1 WITH ( STATE = ON );  
```  
  
### <a name="c-adding-and-dropping-multiple-predicates"></a>C. Aggiunta ed eliminazione di più predicati  
La sintassi seguente modifica un criterio di sicurezza, aggiungendo i predicati del filtro per le tabelle `mytable1` e `mytable3` e rimuovendo il predicato del filtro per la tabella `mytable2`.  
  
```sql  
ALTER SECURITY POLICY pol1  
ADD FILTER PREDICATE schema_preds.SecPredicate1(column1)   
    ON myschema.mytable1,  
DROP FILTER PREDICATE   
    ON myschema.mytable2,  
ADD FILTER PREDICATE schema_preds.SecPredicate2(column2, 1)   
    ON myschema.mytable3;  
```  
  
### <a name="d-changing-the-predicate-on-a-table"></a>D. Modifica il predicato in una tabella  
La sintassi seguente modifica il predicato del filtro esistente nella tabella mytable per farla diventare la funzione SecPredicate2.  
  
```sql  
ALTER SECURITY POLICY pol1  
    ALTER FILTER PREDICATE schema_preds.SecPredicate2(column1)  
        ON myschema.mytable;  
```  
  
### <a name="e-changing-a-block-predicate"></a>E. Modifica di un predicato di blocco  
Modifica della funzione del predicato di blocco per un'operazione su una tabella.  
  
```sql 
ALTER SECURITY POLICY rls.SecPol  
    ALTER BLOCK PREDICATE rls.tenantAccessPredicate_v2(TenantId) 
    ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>Vedere anche  
[Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
[CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
[DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
[sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
[sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  
