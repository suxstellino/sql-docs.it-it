---
description: DBCC CHECKCONSTRAINTS (Transact-SQL)
title: DBCC CHECKCONSTRAINTS (Transact-SQL)
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC CHECKCONSTRAINTS
- DBCC_CHECKCONSTRAINTS_TSQL
- CHECKCONSTRAINTS
- CHECKCONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKCONSTRAINTS statement
- consistency [SQL Server], constraints
- checking constraint consistency
- constraints [SQL Server], consistency checks
- integrity [SQL Server], constraints
ms.assetid: da6c9cee-6687-46e8-b504-738551f9068b
author: pmasl
ms.author: umajay
ms.openlocfilehash: 66b0b155d8fb3bc6c35292469edd4fa40a6ce21f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237292"
---
# <a name="dbcc-checkconstraints-transact-sql"></a>DBCC CHECKCONSTRAINTS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Controlla l'integrità di un vincolo specificato oppure di tutti i vincoli di una tabella specificata nel database corrente.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC CHECKCONSTRAINTS  
[   
    (   
    table_name | table_id | constraint_name | constraint_id   
    )  
]  
    [ WITH   
    [ { ALL_CONSTRAINTS | ALL_ERRORMSGS } ]  
    [ , ] [ NO_INFOMSGS ]   
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *table_name* \| *table_id* \| *constraint_name* \| *constraint_id*  
 Tabella o vincolo da controllare. Se si specifica *table_name* o *table_id* vengono controllati tutti i vincoli abilitati nella tabella. Se si specifica *constraint_name* o *constraint_id* viene controllato solo il vincolo specificato. Se non si specifica un identificatore di tabella o un identificatore di vincolo, vengono controllati tutti i vincoli abilitati in tutte le tabelle del database corrente.  
 Un nome di vincolo identifica in modo univoco la tabella a cui appartiene. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
 WITH  
 Consente di specificare opzioni.  
  
 ALL_CONSTRAINTS  
 Controlla tutti i vincoli abilitati e disabilitati della tabella se viene specificato il nome della tabella o se vengono controllate tutte le tabelle. In caso contrario, viene controllato solo il vincolo abilitato. ALL_CONSTRAINTS non ha alcun effetto quando viene specificato un nome di vincolo.  
  
 ALL_ERRORMSGS  
 Restituisce tutte le righe che violano i vincoli della tabella controllata. Per impostazione predefinita vengono restituite le prime 200 righe.  
  
 NO_INFOMSGS  
 Disattiva tutti i messaggi informativi.  
  
## <a name="remarks"></a>Osservazioni  
DBCC CHECKCONSTRAINTS consente di creare ed eseguire una query per ottenere tutti i vincoli FOREIGN KEY e CHECK di una tabella.
  
Una query di chiave esterna, ad esempio, presenta il seguente formato:
  
```sql
SELECT <columns>  
FROM <table_being_checked> LEFT JOIN <referenced_table>  
    ON <table_being_checked.fkey1> = <referenced_table.pkey1>   
    AND <table_being_checked.fkey2> = <referenced_table.pkey2>  
WHERE <table_being_checked.fkey1> IS NOT NULL   
    AND <referenced_table.pkey1> IS NULL  
    AND <table_being_checked.fkey2> IS NOT NULL  
    AND <referenced_table.pkey2> IS NULL  
```  
  
I dati della query vengono archiviati in una tabella temporanea. Dopo che tutte le tabelle o tutti i vincoli richiesti sono stati controllati, viene restituito il set di risultati.
L'istruzione DBCC CHECKCONSTRAINTS controlla l'integrità dei vincoli FOREIGN KEY e CHECK, ma non l'integrità delle strutture di dati su disco di una tabella. Il controllo di queste strutture può essere eseguito tramite [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) e [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
  
**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive
  
Se si specifica *table_name* o *table_id* e la tabella è abilitata per il controllo delle versioni di sistema, DBCC CHECKCONSTRAINTS esegue anche le verifiche di coerenza dei dati temporali sulla tabella specificata. Se non è stato specificato *NO_INFOMSGS*, questo comando restituirà ogni violazione della coerenza nell'output in una riga separata. Il formato dell'output sarà ([pkcol1], [pkcol2]..) = (\<pkcol1_value>, \<pkcol2_value>...) AND \<what is wrong with temporal table record>.
  
|Controllo|Informazioni aggiuntive nell'output in caso di verifica non riuscita|  
|-----------|-----------------------------------------------|  
|PeriodEndColumn ≥ PeriodStartColumn (current)|[sys_end] = '{0}' AND MAX(DATETIME2) = '9999-12-31 23:59:59.99999'|  
|PeriodEndColumn ≥ PeriodStartColumn (current, history)|[sys_start] = '{0}' AND [sys_end] = '{1}'|  
|PeriodStartColumn < current_utc_time (current)|[sys_start] = '{0}' AND SYSUTCTIME|  
|PeriodEndColumn < current_utc_time (history)|[sys_end] = '{0}' AND SYSUTCTIME|  
|Sovrapposizioni|(sys_start1, sys_end1) , (sys_start2, sys_end2) per due record sovrapposti.<br /><br /> In presenza di più di due record sovrapposti, l'output conterrà più righe e ognuna riporterà una coppia di sovrapposizioni.|  
  
Non è possibile specificare constraint_name o constraint_id per eseguire solo verifiche di coerenza temporali.
  
## <a name="result-sets"></a>Set di risultati  
DBCC CHECKCONSTRAINTS restituisce un set di righe con le colonne seguenti.
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|Nome tabella|**varchar**|Nome della tabella.|  
|Constraint Name|**varchar**|Nome del vincolo violato.|  
|Where|**varchar**|Assegnazioni di valori di colonna che identificano una o più righe che violano il vincolo.<br /><br /> È possibile utilizzare il valore di questa colonna in una clausola WHERE di un'istruzione SELECT che esegue una query per individuare le righe che violano il vincolo.|  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **db_owner** .
  
## <a name="examples"></a>Esempi  
  
### <a name="a-checking-a-table"></a>R. Controllo di una tabella  
Nell'esempio seguente viene controllata l'integrità dei vincoli della tabella `Table1` del database `AdventureWorks`.
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE Table1 (Col1 INT, Col2 CHAR(30));  
GO  
INSERT INTO Table1 VALUES (100, 'Hello');  
GO  
ALTER TABLE Table1 WITH NOCHECK ADD CONSTRAINT chkTab1 CHECK (Col1 > 100);  
GO  
DBCC CHECKCONSTRAINTS(Table1);  
GO  
```  
  
### <a name="b-checking-a-specific-constraint"></a>B. Controllo di un vincolo specifico  
Nell'esempio seguente viene controllata l'integrità del vincolo `CK_ProductCostHistory_EndDate`.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC CHECKCONSTRAINTS ('Production.CK_ProductCostHistory_EndDate');  
GO  
```  
  
### <a name="c-checking-all-enabled-and-disabled-constraints-on-all-tables"></a>C. Controllo di tutti i vincoli abilitati e disabilitati in tutte le tabelle  
 Nell'esempio seguente viene controllata l'integrità di tutti i vincoli abilitati e disabilitati di tutte le tabelle nel database corrente.  
  
```sql  
DBCC CHECKCONSTRAINTS WITH ALL_CONSTRAINTS;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
