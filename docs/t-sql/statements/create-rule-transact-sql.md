---
description: CREATE RULE (Transact-SQL)
title: CREATE RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RULE_TSQL
- CREATE RULE
- CREATE_RULE_TSQL
- RULE
dev_langs:
- TSQL
helpviewer_keywords:
- list rules [SQL Server]
- unbinding rules
- pattern rules [SQL Server]
- range rules [SQL Server]
- alias data types [SQL Server], rules
- CREATE RULE statement
- reports [SQL Server], rules
- status information [SQL Server], rules
- rules [SQL Server], precedence
- binding rules [SQL Server]
- rules [SQL Server], creating
ms.assetid: b016a289-3a74-46b1-befc-a13183be51e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fc137ea5a6c2b48a523f758619c129133b6a4eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091984"
---
# <a name="create-rule-transact-sql"></a>CREATE RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un oggetto denominato regola. Quando una regola viene associata a una colonna o a un tipo di dati alias, specifica i valori che è possibile inserire in tale colonna.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] È consigliabile utilizzare in alternativa i vincoli CHECK, che vengono creati tramite la parola chiave CHECK dell'istruzione CREATE TABLE o ALTER TABLE. Per altre informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 A una colonna o un tipo di dati alias è possibile associare una sola regola. È tuttavia possibile associare a una colonna sia una regola che uno o più vincoli CHECK. In tal caso, vengono valutate tutte le restrizioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE RULE [ schema_name . ] rule_name   
AS condition_expression  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene la regola.  
  
 *rule_name*  
 Nome della nuova regola. I nomi di regola devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome del proprietario della regola è facoltativo.  
  
 *condition_expression*  
 Una o più condizioni che definiscono la regola. Una regola può essere qualsiasi espressione valida in una clausola WHERE e può includere elementi quali operatori aritmetici o relazionali e predicati, ad esempio IN, LIKE, BETWEEN. Una regola non può fare riferimento a colonne o ad altri oggetti di database. È possibile includere funzioni predefinite che non fanno riferimento a oggetti di database. Non è possibile utilizzare funzioni definite dall'utente.  
  
 *condition_expression* include una variabile. Il nome di ogni variabile locale inizia con il simbolo di chiocciola ( **@** ). L'espressione fa riferimento al valore immesso con l'istruzione UPDATE o INSERT. Quando si crea la regola è possibile usare qualsiasi nome o simbolo per rappresentare il valore, ma il primo carattere deve essere il simbolo di chiocciola ( **@** ).  
  
> [!NOTE]  
>  Evitare di creare regole in espressioni che utilizzano tipi di dati alias. Sebbene sia possibile creare regole in espressioni che utilizzano questi tipi di dati, dopo l'associazione delle regole a colonne o tipi di dati alias, la compilazione dell'espressione ha esito negativo quando vi viene fatto riferimento.  
  
## <a name="remarks"></a>Osservazioni  
 L'istruzione CREATE RULE non può essere utilizzata in combinazione con altre istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] all'interno di un singolo batch. Le regole non vengono applicate ai dati già esistenti nel database quando vengono create e non possono essere associate a tipi di dati di sistema.  
  
 È possibile creare una regola solo nel database corrente. Dopo avere creato una regola, eseguire **sp_bindrule** per associarla a una colonna o un tipo di dati alias. La regola deve essere compatibile con il tipo di dati della colonna. Ad esempio, non è possibile utilizzare "\@value LIKE A%" come regola per una colonna numerica. Una regola non può essere associata a una colonna **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, di tipo CLR definito dall'utente o **timestamp**, né a una colonna calcolata.  
  
 Delimitare le costanti per valori di carattere e di data con virgolette singole (') e anteporre 0x alle costanti binarie. Se la regola non è compatibile con la colonna a cui è associata, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] restituisce un messaggio di errore quando si immette un valore, non durante l'associazione della regola.  
  
 Una regola associata a un tipo di dati alias viene attivata solo quando si tenta di aggiornare o immettere un valore in una colonna del database con il tipo di dati alias. Poiché le regole non impongono controlli sulle variabili, non assegnare alle variabili con un tipo di dati alias un valore che non verrebbe accettato da una regola associata a una colonna dello stesso tipo.  
  
 Per visualizzare un report su una regola, usare **sp_help**. Per visualizzare il testo di una regola, eseguire **sp_helptext** specificando il nome della regola come parametro. Per rinominare una regola, usare **sp_rename**.  
  
 Per creare una nuova regola con lo stesso nome di una già esistente, è in primo luogo necessario eliminare la regola esistente tramite DROP RULE dopo averla disassociata con **sp_unbindrule**. Per disassociare una regola da una colonna, usare **sp_unbindrule**.  
  
 È possibile associare una nuova regola a una colonna o a un tipo di dati senza disassociare la regola precedente. In tal caso, la nuova regola risulta prioritaria rispetto alla regola precedente. Le regole associate a colonne sono sempre prioritarie rispetto a quelle associate a tipi di dati alias. L'associazione di una regola a una colonna comporta la sostituzione di una regola già associata al tipo di dati alias della colonna. L'associazione di una regola a un tipo di dati, al contrario, non comporta la sostituzione di una regola associata a una colonna con tale tipo di dati alias. Nella tabella seguente vengono descritti i criteri di precedenza applicati quando vengono associate regole a colonne e tipi di dati alias per cui esistono già altre regole.  
  
|Nuova regola associata a|Regola esistente associata a<br /><br /> tipo di dati alias|Regola esistente associata a<br /><br /> Colonna|  
|-----------------------|-------------------------------------------|----------------------------------|  
|Tipo di dati alias|Sostituzione della regola esistente|Nessuna modifica|  
|Colonna|Sostituzione della regola esistente|Sostituzione della regola esistente|  
  
 Se a una colonna sono associati sia un valore predefinito che una regola, il valore predefinito deve essere compreso nel dominio definito dalla regola. Un valore predefinito in conflitto con una regola non viene immesso. Ogni tentativo di immettere un valore predefinito di questo tipo causa la visualizzazione di un messaggio di errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione CREATE RULE, è necessario disporre almeno dell'autorizzazione CREATE RULE per il database corrente e dell'autorizzazione ALTER per lo schema in cui la regola viene creata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-rule-with-a-range"></a>R. Creazione di una regola con un intervallo  
 Nell'esempio seguente viene creata una regola che limita l'intervallo di valori interi immessi nella colonna o nelle colonne a cui la regola è associata.  
  
```sql  
CREATE RULE range_rule  
AS   
@range>= $1000 AND @range <$20000;  
```  
  
### <a name="b-creating-a-rule-with-a-list"></a>B. Creazione di una regola con un elenco  
 Nell'esempio seguente viene creata una regola che limita ai soli valori in essa elencati i valori effettivi immessi nella colonna o nelle colonne a cui la regola è associata.  
  
```sql  
CREATE RULE list_rule  
AS   
@list IN ('1389', '0736', '0877');  
```  
  
### <a name="c-creating-a-rule-with-a-pattern"></a>C. Creazione di una regola basata su un modello  
 Nell'esempio seguente viene creata una regola basata su un modello composto da due caratteri seguiti da un segno meno (`-`), quindi da qualsiasi numero di caratteri o da nessun carattere e infine da un valore intero compreso tra `0` e `9`.  
  
```sql  
CREATE RULE pattern_rule   
AS  
@value LIKE '__-%[0-9]'  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
