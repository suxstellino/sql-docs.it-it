---
description: ALTER PARTITION FUNCTION (Transact-SQL)
title: ALTER PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER PARTITION FUNCTION
- ALTER_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- splitting partitions [SQL Server]
- partitioned tables [SQL Server], splitting
- ALTER PARTITION FUNCTION statement
- merging partitions [SQL Server]
- partitioned indexes [SQL Server], merging
- partitioned indexes [SQL Server], splitting
- modifying partition functions
- partition functions [SQL Server], modifying
- partitioned tables [SQL Server], merging
ms.assetid: 70866dac-0a8f-4235-8108-51547949ada4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8358d1fa100458cdd6a383d46e2550d146ca8741
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204122"
---
# <a name="alter-partition-function-transact-sql"></a>ALTER PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Modifica una funzione di partizione mediante la suddivisione o l'unione dei relativi valori limite. L'esecuzione di un'istruzione ALTER PARTITION FUNCTION può causare la suddivisione in due partizioni di una partizione di tabella o un indice che usa la funzione di partizione. L'istruzione può anche unire due partizioni in un'unica partizione.  
  
> [!CAUTION]  
>  La stessa funzione di partizione può essere utilizzata da più tabelle o indici. L'istruzione ALTER PARTITION FUNCTION viene applicata a tutti gli elementi in un'unica transazione.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
ALTER PARTITION FUNCTION partition_function_name()  
{   
    SPLIT RANGE ( boundary_value )  
  | MERGE RANGE ( boundary_value )   
} [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*partition_function_name*  
Nome della funzione di partizione da modificare.  
  
SPLIT RANGE ( *boundary_value* )  
Aggiunge una partizione alla funzione di partizione. *boundary_value* determina l'intervallo della nuova partizione e deve essere diverso dagli intervalli limite esistenti della funzione di partizione. In base a *boundary_value*, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] suddividerà in due uno degli intervalli esistenti. Di questi due intervalli, quello con il nuovo *boundary_value* è la nuova partizione.  
  
È necessario che un filegroup esista online e sia contrassegnato dallo schema di partizione che usa la funzione di partizione come NEXT USED affinché possa contenere la nuova partizione. Un'istruzione CREATE PARTITION SCHEME assegna i filegroup alle partizioni. L'istruzione CREATE PARTITION FUNCTION crea meno partizioni rispetto ai filegroup per contenerli. Un'istruzione CREATE PARTITION SCHEME può riservare più filegroup del necessario. In tal caso, risulteranno filegroup non assegnati. Inoltre, lo schema di partizione contrassegna uno dei filegroup come NEXT USED. Questo filegroup contiene la nuova partizione. Se nessuno dei filegroup viene contrassegnato come NEXT USED dallo schema di partizione, è necessario usare un'istruzione ALTER PARTITION SCHEME. 

L'istruzione ALTER PARTITION SCHEME può aggiungere un filegroup oppure selezionarne uno esistente, per contenere la nuova partizione. È possibile assegnare un filegroup che già contiene partizioni in modo da contenere partizioni aggiuntive. Una funzione di partizione può partecipare a più schemi di partizione. Per questo motivo, tutti gli schemi di partizione che usano la funzione di partizione a cui si stanno aggiungendo partizioni devono avere un filegroup NEXT USED. In caso contrario, l'istruzione ALTER PARTITION FUNCTION avrà esito negativo e restituirà un errore indicante che lo schema o gli schemi di partizione non dispongono di un filegroup NEXT USED.  
  
Se si creano tutte le partizioni nello stesso filegroup, quest'ultimo verrà inizialmente assegnato automaticamente al successivo filegroup NEXT USED. Tuttavia, dopo l'esecuzione dell'operazione di divisione, non è più presente un filegroup NEXT USED selezionato. Assegnare in modo esplicito il filegroup come filegroup NEXT USED tramite l'istruzione ALTER PARTITION SCHEME. In caso contrario, le successive operazioni di divisione avranno esito negativo.  
  
> [!NOTE]  
>  Limitazioni per l'indice columnstore: quando la tabella include un indice columnstore è possibile suddividere solo partizioni vuote. Prima di eseguire questa operazione è necessario eliminare o disabilitare l'indice columnstore.  
  
MERGE [ RANGE ( *boundary_value*) ]  
Elimina una partizione e unisce i valori esistenti in tale partizione in una delle partizioni rimanenti. RANGE (*boundary_value*) deve essere un valore limite esistente della partizione da eliminare. Questo argomento rimuove il filegroup che originariamente conteneva *boundary_value* dallo schema di partizione a meno che non sia usato da una partizione rimanente oppure non sia contrassegnato con la proprietà NEXT USED. La partizione unita esiste nel filegroup che non conteneva *boundary_value* inizialmente. *boundary_value* è un'espressione costante che può fare riferimento a variabili (incluse le variabili di tipo definito dall'utente) o funzioni (incluse le funzioni definite dall'utente). Non può fare riferimento a un'espressione [!INCLUDE[tsql](../../includes/tsql-md.md)]. *boundary_value* deve corrispondere al tipo di dati della colonna di partizionamento corrispondente o deve poter essere convertito in modo implicito in tale tipo di dati. Non è inoltre possibile troncare *boundary_value* durante la conversione implicita in modo che le dimensioni e la scala del valore non corrispondano a quelle del relativo *input_parameter_type*.  
  
> [!NOTE]  
>  Limitazioni per l'indice columnstore: Due partizioni non vuote contenenti un indice columnstore non possono essere unite. Prima di eseguire questa operazione è necessario eliminare o disabilitare l'indice columnstore.  
  
## <a name="best-practices"></a>Procedure consigliate  
Mantenere sempre partizioni vuote a entrambe le estremità dell'intervallo di partizione. Mantenere le partizioni a entrambe le estremità consente di garantire che la divisione delle partizioni e l'unione delle partizioni non comportino lo spostamento di dati. La divisione delle partizioni si verifica all'inizio e l'unione delle partizioni si verifica alla fine. Evitare di suddividere o di unire le partizioni popolate, perché tale operazione può risultare estremamente inefficiente, in quanto può causare la generazione di log quattro volte superiori e può provocare anche un blocco grave.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
ALTER PARTITION FUNCTION rieseguirà il partizionamento di qualsiasi tabella e indice che utilizza la funzione in una singola operazione atomica. Questa operazione si verifica tuttavia in modalità offline e, in base all'estensione del ripartizionamento, potrebbe richiedere un numero elevato di risorse.  
  
Usare l'istruzione ALTER PARTITION FUNCTION solo per la divisione di una partizione oppure per l'unione di due partizioni. Per modificare il modo in cui una tabella viene partizionata, ad esempio da 10 a cinque partizioni, avvalersi di una delle opzioni seguenti. In base alla configurazione del sistema, l'utilizzo delle risorse potrebbe variare in base all'opzione prescelta:  
  
-   Creare una nuova tabella partizionata con la funzione di partizione necessaria. Inserire quindi i dati della vecchia tabella in quella nuova usando un'istruzione INSERT INTO...SELECT FROM.  
  
-   Creazione di un indice cluster partizionato su un heap.  
  
    > [!NOTE]  
    >  L'eliminazione di un indice cluster partizionato ha come risultato un heap partizionato.  
  
-   Eliminazione e ricompilazione di un indice partizionato esistente mediante l'utilizzo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX con la clausola DROP EXISTING = ON.  
  
-   Eseguire una sequenza di istruzioni ALTER PARTITION FUNCTION.  
  
Tutti i filegroup interessati dall'istruzione ALTER PARTITION FUNCTION devono essere online.  
  
L'istruzione ALTER PARTITION FUNCTION ha esito negativo in presenza di un indice cluster disabilitato sulle tabelle che usano la funzione di partizione.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non fornisce il supporto di replica per la modifica di una funzione di partizione. Le modifiche a una funzione di partizione nel database di pubblicazione deve essere applicato manualmente nel database di sottoscrizione.  
  
## <a name="permissions"></a>Autorizzazioni  
Per eseguire l'istruzione ALTER PARTITION FUNCTION, è necessario utilizzare le autorizzazioni seguenti:  
  
-   Autorizzazione ALTER ANY DATASPACE. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
-   Autorizzazione CONTROL o ALTER nel database in cui la funzione di partizione è stata creata.  
  
-   Autorizzazione CONTROL SERVER o ALTER ANY DATABASE nel server del database in cui la funzione di partizione è stata creata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-splitting-a-partition-of-a-partitioned-table-or-index-into-two-partitions"></a>R. Suddivisione in due partizioni di una partizione di una tabella o un indice partizionato  
Nell'esempio seguente viene creata una funzione di partizione per suddividere una tabella o indice in quattro partizioni. `ALTER PARTITION FUNCTION` suddivide una delle partizioni in due per creare un totale di cinque partizioni.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Split the partition between boundary_values 100 and 1000  
--to create two partitions between boundary_values 100 and 500  
--and between boundary_values 500 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
SPLIT RANGE (500);  
```  
  
### <a name="b-merging-two-partitions-of-a-partitioned-table-into-one-partition"></a>B. Unione di due partizioni di una tabella partizionata  
Nell'esempio seguente viene creata la stessa funzione di partizione dell'esempio precedente e quindi due partizioni vengono unite in modo da formare un totale di tre partizioni.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Merge the partitions between boundary_values 1 and 100  
--and between boundary_values 100 and 1000 to create one partition  
--between boundary_values 1 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
MERGE RANGE (100);  
```  
  
## <a name="see-also"></a>Vedere anche  
[Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
[DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
[ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
[DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
[sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
[sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md) [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
[sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
[sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
