---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dd06b9daff476b26667c0ef5900f34325b05f7c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100889"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea un oggetto denominato valore predefinito. Quando è associato a una colonna o a un tipo di dati alias, un valore predefinito specifica un valore da inserire nella colonna a cui è associato l'oggetto (o in tutte le colonne, nel caso di un tipo di dati alias) se durante un inserimento non viene specificato in modo esplicito alcun valore.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare definizioni di valori predefiniti create con la parola chiave DEFAULT dell'istruzione ALTER TABLE o CREATE TABLE.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*schema_name*  
 Nome dello schema a cui appartiene il valore predefinito.  
  
*default_name*  
 Nome del valore predefinito. I nomi dei valori predefiniti devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome del proprietario del valore predefinito è facoltativo.  
  
*constant_expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) che include solo valori costanti. Non può includere i nomi di colonne o di altri oggetti di database. È possibile usare qualsiasi costante, funzione predefinita o espressione matematica, ad eccezione di quelle contenenti tipi di dati alias. Non è possibile usare funzioni definite dall'utente. Racchiudere le costanti per valori di carattere e data tra virgolette singole (**'**). Le costanti per valori di valuta, interi e a virgola mobile non richiedono le virgolette. I dati binari devono essere preceduti da 0x, mentre i dati di valuta devono essere preceduti dal simbolo di valuta. Il valore predefinito deve essere compatibile con il tipo di dati della colonna.  
  
## <a name="remarks"></a>Commenti  
 È possibile creare un nome predefinito solo nel database corrente. I nomi di valore predefinito in un database devono essere univoci per ogni schema. Quando si crea un valore predefinito, usare **sp_bindefault** per associarlo a una colonna o a un tipo di dati alias.  
  
 Se il valore predefinito non è compatibile con la colonna a cui è associato, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un messaggio di errore quando cerca di inserire il valore predefinito. Non è possibile, ad esempio, usare N/D come valore predefinito per una colonna **numerica**.  
  
 Se la lunghezza del valore predefinito è eccessiva per la colonna a cui è associato, il valore viene troncato.  
  
 Le istruzioni CREATE DEFAULT non possono essere combinate con altre istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] in un singolo batch.  
  
 Un valore predefinito deve essere eliminato prima di poterne creare uno nuovo con lo stesso nome. È inoltre necessario annullare l'associazione del valore predefinito eseguendo **sp_unbindefault** prima della sua eliminazione.  
  
 Se a una colonna sono associati un valore predefinito e una regola, il valore predefinito non deve violare la regola. Un valore predefinito in conflitto con una regola non viene mai inserito e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un messaggio di errore in corrispondenza di ogni tentativo di immissione del valore predefinito.  
  
 Quando è associato a una colonna, un valore predefinito viene inserito nei casi seguenti:  
  
-   Non è già stato inserito un valore in modo esplicito.  
  
-   Con l'istruzione INSERT è stata specificata la parola chiave DEFAULT VALUES o DEFAULT per l'inserimento di valori predefiniti.  
  
 Se si specifica NOT NULL quando si crea una colonna e non si crea alcun valore predefinito, viene generato un messaggio di errore quando un utente non immette una voce nella colonna. Nella tabella seguente vengono descritte le relazioni tra l'esistenza di un valore predefinito e la definizione di una colonna come NULL o NOT NULL. Le voci nella tabella indicano i risultati ottenuti.  
  
|Definizione di colonna|Nessuna voce, nessun valore predefinito|Nessuna voce, valore predefinito|Voce NULL, nessun valore predefinito|Voce NULL, valore predefinito|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|NULL|default|NULL|NULL|  
|**NOT NULL**|Errore|default|error|error|  
  
 Per rinominare un valore predefinito, usare **sp_rename**. Per ottenere un report relativo a un valore predefinito, usare **sp_help**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per usare CREATE DEFAULT, un utente deve avere almeno l'autorizzazione CREATE DEFAULT nel database corrente e l'autorizzazione ALTER per lo schema in cui viene creato il valore predefinito.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-simple-character-default"></a>R. Creazione di un semplice valore predefinito costituito da una stringa di caratteri  
 Nell'esempio seguente viene creato un valore predefinito costituito dalla stringa di caratteri `unknown`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>B. Associazione di un valore predefinito  
 Nell'esempio seguente viene associato il valore predefinito creato nell'esempio A. Il valore predefinito viene utilizzato solo se non vengono specificate voci per la colonna `Phone` della tabella `Contact`. 
 
 > [!Note] 
 >  Omettere una voce è diverso da specificare esplicitamente NULL in un'istruzione INSERT.  
  
 Dato che il valore predefinito `phonedflt` non esiste, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente ha esito negativo. Questo esempio viene utilizzato solo a scopo illustrativo.  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
