---
description: DBCC DBREINDEX (Transact-SQL)
title: DBCC DBREINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC DBREINDEX
- DBREINDEX_TSQL
- DBREINDEX
- DBCC_DBREINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- index rebuilding [SQL Server]
- rebuilding indexes
- dynamic index rebuilding [SQL Server]
- DBCC DBREINDEX statement
ms.assetid: 6e929d09-ccb5-4855-a6af-b616022bc8f6
author: pmasl
ms.author: umajay
ms.openlocfilehash: 8e29696d30bc46a6e7c9fe644f7d4e19df6db622
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813393"
---
# <a name="dbcc-dbreindex-transact-sql"></a>DBCC DBREINDEX (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Ricompila uno o più indici di una tabella nel database specificato.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] In alternativa, usare [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level))
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC DBREINDEX   
(   
    table_name   
    [ , index_name [ , fillfactor ] ]  
)  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *table_name*  
 Nome della tabella che contiene l'indice o gli indici specificati da ricompilare. I nomi delle tabelle devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md) *.*  
  
 *index_name*  
 Nome dell'indice da ricompilare. I nomi degli indici devono essere conformi alle regole per gli identificatori. Se *index_name* è specificato, deve essere specificato *table_name*. Se *index_name* non è specificato o è specificato come " " vengono ricostruiti tutti gli indici della tabella.  
  
 *fillfactor*  
 Percentuale di spazio su ogni pagina di indice per l'archiviazione di dati quando l'indice viene creato o ricompilato. *fillfactor* sostituisce il fattore di riempimento quando l'indice è stato creato, diventando il nuovo valore predefinito per l'indice e ogni altro indice non cluster ricompilato perché è stato ricompilato un indice cluster.  
 Se *fillfactor* è 0, DBCC DBREINDEX usa l'ultimo valore del fattore di riempimento specificato per l'indice. Questo valore è archiviato nella vista del catalogo **sys.indexes**.   
 Se *fillfactor* è specificato, *table_name* e *index_name* devono essere specificati. Se *fillfactor* non è specificato viene usato il fattore di riempimento predefinito, ovvero 100. Per altre informazioni, vedere [Specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
 WITH NO_INFOMSGS  
 Evita la visualizzazione di tutti i messaggi informativi con livello di gravità compreso tra 0 e 10.  
  
## <a name="remarks"></a>Osservazioni  
DBCC DBREINDEX ricompila un indice di tabella o tutti gli indici definiti per una tabella. Quando è supportata la ricostruzione dinamica di un indice, è possibile ricostruire gli indici che applicano vincoli PRIMARY KEY o UNIQUE senza dover eliminare o ricreare tali vincoli. Ciò significa che un indice può essere ricostruito anche se la struttura di una tabella o i relativi vincoli non sono noti. Questa situazione potrebbe verificarsi dopo una copia bulk di dati in una tabella.

L'istruzione DBCC DBREINDEX può ricompilare tutti gli indici di una tabella in un'unica istruzione. Ciò risulta più semplice rispetto alla specificazione di più istruzioni DROP INDEX e CREATE INDEX a livello di codice. Poiché l'intera operazione viene eseguita da un'unica istruzione, DBCC DBREINDEX risulta automaticamente un'istruzione atomica. Le singole istruzioni DROP INDEX e CREATE INDEX invece risultano atomiche solo quando vengono inserite in una transazione. L'istruzione DBCC DBREINDEX è inoltre caratterizzata da un maggior numero di ottimizzazioni rispetto alle singole istruzioni DROP INDEX e CREATE INDEX.

A differenza di DBCC INDEXDEFRAG oppure di ALTER INDEX con l'opzione REORGANIZE, l'istruzione DBCC DBREINDEX viene eseguita in modalità offline. In caso di ricostruzione di un indice non cluster, viene mantenuto attivo un blocco condiviso sulla tabella in questione per l'intera durata dell'operazione. Ciò impedisce che vengano apportate modifiche alla tabella. In caso di ricostruzione di un indice cluster, viene mantenuto attivo un blocco esclusivo a livello di tabella. Ciò impedisce qualsiasi tipo di accesso alla tabella e la rende effettivamente offline. Per eseguire la ricompilazione di un indice in modalità online oppure per controllare il grado di parallelismo durante l'operazione di ricompilazione dell'indice, utilizzare l'istruzione ALTER INDEX REBUILD con l'opzione ONLINE.

Per altre informazioni sulla scelta di un metodo di riorganizzazione e ricompilazione di un indice, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
  
## <a name="restrictions"></a>Restrizioni  
DBCC DBREINDEX non è utilizzabile con gli oggetti seguenti:
-   Tabelle di sistema  
-   Indici spaziali  
-   indici columnstore ottimizzati memoria xVelocity  
  
## <a name="result-sets"></a>Set di risultati  
A meno che l'opzione NO_INFOMSGS non sia specificata (è necessario specificare il nome della tabella), DBCC DBREINDEX restituisce sempre quanto segue:
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorizzazioni  
Il chiamante deve essere proprietario della tabella o membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_owner** e **db_ddladmin**.
  
## <a name="examples"></a>Esempi  
### <a name="a-rebuilding-an-index"></a>R. Ricompilazione di un indice  
Nell'esempio seguente viene ricompilato l'indice cluster `Employee_EmployeeID` con un fattore di riempimento pari a `80` nella tabella `Employee` del database `AdventureWorks`.
  
```sql  
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', PK_Employee_BusinessEntityID,80);  
GO  
```  
  
### <a name="b-rebuilding-all-indexes"></a>B. Ricompilazione di tutti gli indici  
Nell'esempio seguente vengono ricompilati tutti gli indici nella tabella `Employee` del database `AdventureWorks` in base a un fattore di riempimento il cui valore è `70`.
  
```sql
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', ' ', 70);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
