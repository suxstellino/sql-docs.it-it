---
description: ALTER SEQUENCE (Transact-SQL)
title: ALTER SEQUENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SEQUENCE_TSQL
- ALTER SEQUENCE
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, altering
- ALTER SEQUENCE statement
ms.assetid: decc0760-029e-4baf-96c9-4a64073df1c2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 44baafa6b239a2eff1a62d6c5194535d9a87ecbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182206"
---
# <a name="alter-sequence-transact-sql"></a>ALTER SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Modifica gli argomenti di un oggetto sequenza esistente. Se la sequenza è stata creata con l'opzione **CACHE**, la modifica della sequenza comporta la ricreazione della cache.  
  
 Gli oggetti sequenza vengono creati tramite l'istruzione [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). Le sequenze sono valori interi e possono essere di qualsiasi tipo di dati in grado di restituire un intero. Non è possibile modificare il tipo di dati tramite l'istruzione ALTER SEQUENCE. Per modificare il tipo di dati, eliminare e ricreare l'oggetto sequenza.  
  
 Una sequenza è un oggetto associato a schema definito dall'utente che genera una sequenza di valori numerici in base a una specifica. Si generano nuovi valori da una sequenza chiamando la funzione NEXT VALUE FOR. Usare **sp_sequence_get_range** per ottenere immediatamente più numeri di sequenza. Per informazioni e scenari in cui vengono usate le istruzioni CREATE SEQUENCE, **sp_sequence_get_range** e la funzione NEXT VALUE FOR, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER SEQUENCE [schema_name. ] sequence_name  
    [ RESTART [ WITH <constant> ] ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE <constant> } | { NO MINVALUE } ]  
    [ { MAXVALUE <constant> } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *sequence_name*  
 Specifica il nome univoco con il quale è nota la sequenza nel database. Il tipo è **sysname**.  
  
 RESTART [ WITH \<constant> ]  
 Valore successivo che verrà restituito dall'oggetto sequenza. Se specificato, il valore RESTART WITH deve essere un numero intero minore o uguale al valore massimo e maggiore o uguale al valore minimo dell'oggetto sequenza. Se il valore WITH viene omesso, la numerazione della sequenza ricomincia in base alle opzioni CREATE SEQUENCE originali.  
  
 INCREMENT BY \<constant>  
 Valore usato per incrementare (o diminuire, in caso di valore negativo) il valore di base dell'oggetto sequenza per ogni chiamata alla funzione NEXT VALUE FOR. Se l'incremento è un valore negativo, l'oggetto sequenza ha un ordine decrescente, in caso contrario avrà un ordine crescente. L'incremento non può essere 0.  
  
 [ MINVALUE \<constant> | NO MINVALUE ]  
 Specifica i limiti per l'oggetto sequenza. Se viene specificato NO MINVALUE, viene utilizzato il valore possibile minimo del tipo di dati della sequenza.  
  
 [ MAXVALUE \<constant> | NO MAXVALUE  
 Specifica i limiti per l'oggetto sequenza. Se viene specificato NO MAXVALUE, viene utilizzato il valore possibile massimo del tipo di dati della sequenza.  
  
 [ CYCLE | NO CYCLE ]  
 Questa proprietà specifica se l'oggetto sequenza deve riprendere dal valore minimo (o massimo per gli oggetti sequenza con ordine decrescente) o generare un'eccezione quando viene superato il valore massimo o minimo.  
  
> [!NOTE]  
>  Dopo la ripresa del ciclo, il valore successivo sarà il valore minimo o massimo, non il valore START VALUE della sequenza.  
  
 [ CACHE [\<constant> ] | NO CACHE ]  
 Migliora le prestazioni delle applicazioni che utilizzano gli oggetti sequenza riducendo il numero di I/O necessari per rendere persistenti i valori generati nelle tabelle di sistema.  
  
 Per altre informazioni sul comportamento della cache, vedere [REATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 Per informazioni sulla creazione delle sequenze e sulla gestione della cache delle sequenze, vedere [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
 Non è possibile modificare MINVALUE per le sequenze con ordine crescente e MAXVALUE per le sequenze con ordine decrescente in un valore che non consente il valore START WITH della sequenza. Per modificare MINVALUE per una sequenza con ordine crescente in un numero maggiore del valore START WITH o per modificare MAXVALUE di una sequenza con ordine decrescente in un numero più piccolo del valore START WITH, includere l'argomento RESTART WITH per riprendere la sequenza in un punto desiderato compreso nell'intervallo minimo e massimo.  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sulle sequenze, eseguire una query su [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione **ALTER** per la sequenza o **ALTER** per lo schema. Per concedere l'autorizzazione **ALTER** per la sequenza, usare **ALTER ON OBJECT** nel formato seguente:  
  
```sql  
GRANT ALTER ON OBJECT::Test.TinySeq TO [AdventureWorks\Larry]  
```  
  
 È possibile trasferire la proprietà di un oggetto sequenza tramite l'istruzione **ALTER AUTHORIZATION**.  
  
### <a name="audit"></a>Audit  
 Per controllare **ALTER SEQUENCE**, monitorare **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Esempi  
 Per esempi relativi alla creazione di sequenze e all'uso della funzione **NEXT VALUE FOR** per generare numeri di sequenza, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
### <a name="a-altering-a-sequence"></a>R. Alterazione di una sequenza  
 Nell'esempio seguente vengono creati uno schema denominato Test e una sequenza denominata TestSeq usando il tipo di dati **int**, con un intervallo di valori compreso tra 100 e 200. La sequenza inizia con 125 e aumenta di 25 ogni volta che viene generato un numero. Poiché la sequenza è configurata per la ripresa del ciclo quando il valore supera il valore massimo di 200, la sequenza riprende dal valore minimo di 100.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.TestSeq  
    AS int   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
GO  
```  
  
 Nell'esempio seguente la sequenza TestSeq viene modificata in modo da avere un intervallo di valori compreso tra 50 e 200. La sequenza riavvia la serie di numerazione con 100 e aumenta di 50 ogni volta che viene generato un numero.  
  
```sql  
ALTER SEQUENCE Test. TestSeq  
    RESTART WITH 100  
    INCREMENT BY 50  
    MINVALUE 50  
    MAXVALUE 200  
    NO CYCLE  
    NO CACHE  
;  
GO  
```  
  
 Poiché la sequenza non viene ripresa, la funzione **NEXT VALUE FOR** genererà un errore quando la sequenza supererà 200.  
  
### <a name="b-restarting-a-sequence"></a>B. Riavvio di una sequenza  
 Nell'esempio seguente viene creata una sequenza denominata CountBy1. La sequenza utilizza i valori predefiniti.  
  
```sql  
CREATE SEQUENCE Test.CountBy1 ;  
```  
  
 Per generare un valore di sequenza, il proprietario esegue l'istruzione seguente:  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1  
```  
  
 Il valore restituito -9.223.372.036.854.775.808 è il valore più piccolo possibile per il tipo di dati **bigint**. Il proprietario voleva che la sequenza iniziasse con 1, ma non ha indicato la clausola **START WITH** quando ha creato la sequenza. Per risolvere l'errore, il proprietario esegue l'istruzione seguente.  
  
```sql  
ALTER SEQUENCE Test.CountBy1 RESTART WITH 1 ;  
```  
  
 Il proprietario riesegue quindi l'istruzione seguente per generare un numero di sequenza.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1;  
```  
  
 Il numero è ora 1, come previsto.  
  
 La sequenza CountBy1 è stata creata usando il valore predefinito di NO CYCLE, pertanto si arresterà dopo avere generato il numero 9.223.372.036.854.775.807. Le chiamate successive all'oggetto sequenza restituiranno l'errore 11728. Nell'istruzione seguente viene modificato l'oggetto sequenza in modo che venga ripreso il ciclo e viene impostata una cache di 20.  
  
```sql  
ALTER SEQUENCE Test.CountBy1  
    CYCLE  
    CACHE 20 ; 
```  
  
 Quando l'oggetto sequenza raggiunge 9.223.372.036.854.775.807 il ciclo viene ripreso e il numero successivo dopo la ripresa del ciclo sarà il minimo del tipo di dati, ovvero -9.223.372.036.854.775.808.  
  
 Il proprietario nota che il tipo di dati **bigint** usa 8 byte ogni volta che viene impiegato. Il tipo di dati **int** che usa 4 byte è sufficiente. Non è tuttavia possibile modificare il tipo di dati di un oggetto sequenza. Per modificare un tipo di dati **int**, il proprietario deve eliminare l'oggetto sequenza e ricrearlo con il tipo di dati corretto.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [sp_sequence_get_range &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
