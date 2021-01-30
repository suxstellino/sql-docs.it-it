---
description: CREATE SEQUENCE (Transact-SQL)
title: CREATE SEQUENCE (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SEQUENCE
- CREATE SEQUENCE
- SEQUENCE_TSQL
- CREATE_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SEQUENCE statement
- sequence number object, creating
- sequence object
- number, sequence
ms.assetid: 419f907b-8a72-4d6c-80cb-301df44c24c1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fb687f42e65bb1f1ec74046927282dc90563ccd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188494"
---
# <a name="create-sequence-transact-sql"></a>CREATE SEQUENCE (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea un oggetto sequenza e ne specifica le proprietà. Una sequenza è un oggetto associato a schema definito dall'utente che genera una sequenza di valori numerici in base alla specifica con la quale è stata creata la sequenza. La sequenza di valori numerici viene generata in ordine crescente o decrescente a un intervallo definito e può essere configurata per riprendere dall'inizio (ciclo) quando è esaurita. Le sequenze, a differenza delle colonne Identity, non sono associate a tabelle specifiche. Le applicazioni fanno riferimento a un oggetto sequenza per recuperare il relativo valore successivo. La relazione tra sequenze e tabelle è controllata dall'applicazione. Le applicazioni utente possono fare riferimento a un oggetto sequenza e coordinare i valori di più righe e tabelle.  
  
 A differenza dei valori delle colonne Identity che vengono generati quando vengono inserite le righe, un'applicazione può ottenere il numero di sequenza successivo senza inserire la riga chiamando la [funzione NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md). Usare [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) per ottenere immediatamente più numeri di sequenza.  
  
 Per informazioni e scenari in cui vengono usate entrambe le funzioni **CREATE SEQUENCE** e **NEXT VALUE FOR** , vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE SEQUENCE [schema_name . ] sequence_name  
    [ AS [ built_in_integer_type | user-defined_integer_type ] ]  
    [ START WITH <constant> ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE [ <constant> ] } | { NO MINVALUE } ]  
    [ { MAXVALUE [ <constant> ] } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*sequence_name*  
Specifica il nome univoco con il quale è nota la sequenza nel database. Il tipo è **sysname**.  
  
[ built_in_integer_type | user-defined_integer_type  
Una sequenza può essere definita come qualsiasi tipo Integer. I tipi seguenti sono consentiti.  
  
-   **tinyint** -intervallo da 0 a 255  
-   **smallint** -intervallo tra -32.768 e 32.767  
-   **int** - intervallo tra -2,147,483,648 e 2,147,483,647  
-   **bigint** -intervallo tra -9.223.372.036.854.775.808 e 9.223.372.036.854.775.807  
-   **decimal** e **numeric** con scala 0.  
-   Qualsiasi tipo di dati definito dall'utente (tipo di alias) basato su uno dei tipi consentiti.  
  
Se non viene specificato un tipo di dati, per impostazione predefinita viene utilizzato il tipo di dati **bigint**.  
  
START WITH \<constant>  
Primo valore restituito dall'oggetto sequenza. Il valore **START** deve essere minore o uguale al valore massimo e maggiore o uguale al valore minimo dell'oggetto sequenza. Il valore iniziale predefinito per un nuovo oggetto sequenza è il valore minimo per un oggetto sequenza con ordine crescente e il valore massimo per un oggetto sequenza con ordine decrescente.  
  
INCREMENT BY \<constant>  
Valore utilizzato per incrementare (o diminuire se negativo) il valore dell'oggetto sequenza per ogni chiamata alla funzione **NEXT VALUE FOR**. Se l'incremento è un valore negativo, l'oggetto sequenza presenta un ordine decrescente. In caso contrario, l'ordine sarà crescente. L'incremento non può essere 0. L'incremento predefinito per un nuovo oggetto sequenza è 1.  
  
[ MINVALUE \<constant> | **NO MINVALUE** ]  
Specifica i limiti per l'oggetto sequenza. Il valore minimo predefinito per un nuovo oggetto sequenza è il valore minimo del tipo di dati dell'oggetto sequenza. Tale valore è zero per il tipo di dati **tinyint** e un numero negativo per tutti gli altri tipi di dati.  
  
[ MAXVALUE \<constant> | **NO MAXVALUE**  
Specifica i limiti per l'oggetto sequenza. Il valore massimo predefinito per un nuovo oggetto sequenza è il valore massimo del tipo di dati dell'oggetto sequenza.  
  
[ CYCLE | **NO CYCLE** ]  
Proprietà che specifica se l'oggetto sequenza deve riprendere dal valore minimo (o massimo per gli oggetti sequenza con ordine decrescente) o generare un'eccezione quando viene superato il relativo valore massimo o minimo. L'opzione predefinita che determina la ripresa dei nuovi oggetti sequenza è NO CYCLE.  
  
> [!NOTE]
> Il ciclo di una SEQUENZA riprende dal valore minimo o massimo, non dal valore iniziale.  
  
[ **CACHE** [\<constant> ] | NO CACHE ]  
Migliora le prestazioni per le applicazioni che utilizzano oggetti sequenza riducendo il numero di I/O del disco necessari per generare numeri di sequenza. Il valore predefinito è CACHE.  
  
Se ad esempio la dimensione della cache viene impostata su 50, nella cache di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono memorizzati 50 singoli valori, ma solo il valore corrente e il numero di valori rimanenti nella cache. La quantità di memoria necessaria per l'archiviazione della cache è pertanto sempre corrispondente a due istanze del tipo di dati dell'oggetto sequenza.  
  
> [!NOTE]  
> Se l'opzione cache viene abilitata senza specificare la dimensione, quest'ultima verrà selezionata dal Motore di database. È tuttavia consigliabile non basarsi sul fatto che la selezione sia coerente. [!INCLUDE[msCoName](../../includes/msconame-md.md)] potrebbe cambiare il metodo di calcolo della dimensione della cache senza preavviso.  
  
Se per la creazione si utilizza l'opzione **CACHE**, un arresto imprevisto, ad esempio un'interruzione dell'alimentazione, potrebbe causare la perdita dei numeri di sequenza rimanenti nella cache.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 I numeri di sequenza vengono generati esternamente all'ambito della transazione corrente. Vengono utilizzati sia in caso di commit che di rollback della transazione che utilizza il numero di sequenza. La convalida dei duplicati viene eseguita solo quando un record è completamente popolato. Ciò può provocare, in alcuni casi, l'assegnazione di uno stesso numero a più di un record durante la creazione, ma il numero viene comunque identificato come duplicato. Se si verifica questo errore e ai record successivi sono stati applicati altri valori di numerazione automatica, può verificarsi uno scarto tra i valori di numerazione automatica e il comportamento previsto.
  
### <a name="cache-management"></a>Gestione della cache  
 Per migliorare le prestazioni, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il numero di numeri di sequenza specificati dall'argomento **CACHE** viene pre-allocato.  
  
 Si supponga, ad esempio, che una nuova sequenza venga creata con un valore iniziale pari a 1 e una dimensione della cache pari a 15. Quando viene richiesto il primo valore, i valori compresi tra 1 e 15 vengono resi disponibili dalla memoria. L'ultimo valore memorizzato nella cache (15) viene scritto nelle tabelle di sistema sul disco. Dopo che tutti e 15 i numeri sono stati utilizzati, la richiesta successiva (del numero 16) comporterà la riallocazione della cache. Il nuovo ultimo valore memorizzato nella cache (30) verrà scritto nelle tabelle di sistema.  
  
 Se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene arrestato dopo che sono stati utilizzati 22 numeri, il numero di sequenza desiderato successivo in memoria (23) viene scritto nelle tabelle di sistema, andando a sostituire il numero archiviato in precedenza.  
  
 Quando SQL Server viene riavviato e viene richiesto un numero di sequenza, il numero iniziale viene letto dalle tabelle di sistema (23). La quantità della cache di 15 numeri (23-38) viene allocata nella memoria e il numero non memorizzato nella cache successivo (39) viene scritto nelle tabelle di sistema.  
  
 Se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene arrestato in modo anomalo per un evento quale un'interruzione dell'alimentazione, la sequenza viene riavviata con il numero letto dalle tabelle di sistema (39). Qualsiasi numero di sequenza allocato nella memoria (ma mai richiesto da un utente o da un'applicazione) va perso. Questa funzionalità potrebbe lasciare dei vuoti, ma garantisce che lo stesso valore non venga mai generato due volte per un singolo oggetto sequenza, a meno che non venga definito come **CYCLE** o venga riavviato manualmente.  
  
 La cache viene mantenuta nella memoria tenendo traccia del valore corrente (l'ultimo valore generato) e del numero di valori rimanenti nella cache. Pertanto, la quantità di memoria utilizzata dalla cache è sempre corrispondente a due istanze del tipo di dati dell'oggetto sequenza.  
  
 L'impostazione dell'argomento della cache su **NO CACHE** comporta la scrittura del valore di sequenza corrente nelle tabelle di sistema ogni volta che viene utilizzata una sequenza. Le prestazioni potrebbero diminuire in seguito all'aumento dell'accesso al disco, tuttavia diminuiscono anche le probabilità di vuoti indesiderati. I vuoti possono comunque verificarsi se i numeri vengono richiesti utilizzando la funzione **NEXT VALUE FOR** o **sp_sequence_get_range**, ma in questo caso i numeri non vengono utilizzati o vengono utilizzati nelle transazioni di cui non è stato eseguito il commit.  
  
 Quando un oggetto sequenza utilizza l'opzione **CACHE**, se si riavvia l'oggetto sequenza o si modifica **INCREMENT**, **CYCLE**, **MINVALUE**, **MAXVALUE** o le proprietà della dimensione della cache, la cache viene scritta nelle tabelle di sistema prima che si verifichi la modifica. La cache verrà quindi ricaricata a partire dal valore corrente (non viene ignorato alcun numero). La modifica della dimensione della cache diventa effettiva immediatamente.  
  
 **Opzione CACHE quando sono disponibili valori memorizzati nella cache**  
  
 Il processo seguente si verifica ogni volta che viene richiesto un oggetto sequenza per generare il valore successivo per l'opzione **CACHE** se nella cache in memoria sono disponibili valori inutilizzati per l'oggetto sequenza.  
  
1.  Viene calcolato il valore successivo per l'oggetto sequenza.  
  
2.  Viene aggiornato in memoria il nuovo valore corrente per l'oggetto sequenza.  
  
3.  Il valore calcolato viene restituito all'istruzione di chiamata.  
  
**Opzione CACHE quando la cache è esaurita**  
  
 Il processo seguente si verifica ogni volta che viene richiesto un oggetto sequenza per generare il valore successivo per l'opzione **CACHE** se la cache è esaurita.  
  
1.  Viene calcolato il valore successivo per l'oggetto sequenza.  
  
2.  Viene calcolato l'ultimo valore per la nuova cache.  
  
3.  Viene bloccata la riga della tabella di sistema per l'oggetto sequenza e il valore calcolato nel passaggio 2 (l'ultimo valore) viene scritto nella tabella di sistema. Viene generato un Xevent di cache esaurita per notificare all'utente il nuovo valore persistente.  
  
**Opzione NO CACHE**  
  
 Il processo seguente si verifica ogni volta che viene generato un oggetto sequenza per generare il valore successivo per l'opzione **NO CACHE**.  
  
1.  Viene calcolato il valore successivo per l'oggetto sequenza.  
  
2.  Il nuovo valore corrente per l'oggetto sequenza viene scritto nella tabella di sistema.  
  
3.  Il valore calcolato viene restituito all'istruzione di chiamata.  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sulle sequenze, eseguire una query su [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **CREATE SEQUENCE**, **ALTER** o **CONTROL** per l'oggetto SCHEMA.  
  
-   I membri dei ruoli predefiniti del database db_owner and db_ddladmin possono creare, alterare ed eliminare oggetti sequenza.  
  
-   I membri dei ruoli predefiniti del database db_owner e db_datawriter possono aggiornare gli oggetti sequenza comportando la generazione di numeri.  
  
 Nell'esempio seguente viene concessa l'autorizzazione AdventureWorks\Larry per creare sequenze nello schema di test.  
  
```sql  
GRANT CREATE SEQUENCE ON SCHEMA::Test TO [AdventureWorks\Larry]  
```  
  
 È possibile trasferire la proprietà di un oggetto sequenza tramite l'istruzione **ALTER AUTHORIZATION**.  
  
 Se una sequenza utilizza un tipo di dati definito dall'utente, l'autore della sequenza deve disporre dell'autorizzazione REFERENCES per il tipo.  
  
### <a name="audit"></a>Audit  
 Per controllare **CREATE SEQUENCE**, monitorare **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Esempi  
 Per esempi relativi alla creazione di sequenze e all'uso della funzione **NEXT VALUE FOR** per generare numeri di sequenza, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 La maggior parte degli esempi seguenti riguarda la creazione di oggetti sequenza in un schema denominato test.  
  
 Per creare lo schema test, eseguire l'istruzione seguente.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
```  
  
### <a name="a-creating-a-sequence-that-increases-by-1"></a>R. Creazione di una sequenza che aumenta di 1  
 Nell'esempio seguente Thierry crea una sequenza denominata CountBy1 che aumenta di uno ogni volta che viene utilizzata.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="b-creating-a-sequence-that-decreases-by-1"></a>B. Creazione di una sequenza che diminuisce di 1  
 Nell'esempio seguente la sequenza inizia da 0 e aumenta di un numero negativo ogni volta che viene utilizzata.  
  
```sql  
CREATE SEQUENCE Test.CountByNeg1  
    START WITH 0  
    INCREMENT BY -1 ;  
GO  
```  
  
### <a name="c-creating-a-sequence-that-increases-by-5"></a>C. Creazione di una sequenza che aumenta di 5  
 Nell'esempio seguente viene creata una sequenza che aumenta di 5 ogni volta che viene utilizzata.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 5  
    INCREMENT BY 5 ;  
GO  
```  
  
### <a name="d-creating-a-sequence-that-starts-with-a-designated-number"></a>D. Creazione di una sequenza che inizia con un numero definito  
 Dopo avere importato una tabella, Thierry nota che il numero ID più elevato utilizzato è 24.328. Thierry necessita di una sequenza che genererà numeri che iniziano da 24.329. Nel codice seguente viene creata una sequenza che inizia da 24.329 e aumenta di 1.  
  
```sql  
CREATE SEQUENCE Test.ID_Seq  
    START WITH 24329  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="e-creating-a-sequence-using-default-values"></a>E. Creazione di una sequenza utilizzando valori predefiniti  
 Nell'esempio seguente viene creata una sequenza utilizzando i valori predefiniti.  
  
```sql  
CREATE SEQUENCE Test.TestSequence ;  
```  
  
 Eseguire l'istruzione seguente per visualizzare le proprietà della sequenza.  
  
```sql  
SELECT * FROM sys.sequences WHERE name = 'TestSequence' ;  
```  
  
 Un elenco parziale dell'output dimostra i valori predefiniti.  
  
| Output | Valore predefinito|  
|-|-|  
|`start_value`|`-9223372036854775808`|  
|`increment`|`1`|  
|`mimimum_value`|`-9223372036854775808`|  
|`maximum_value`|`9223372036854775807`|  
|`is_cycling`|`0`|  
|`is_cached`|`1`|  
|`current_value`|`-9223372036854775808`|  
  
### <a name="f-creating-a-sequence-with-a-specific-data-type"></a>F. Creazione di una sequenza con un tipo di dati specifico

Nell'esempio seguente viene creata una sequenza utilizzando il tipo di dati **smallint**, con un intervallo compreso tra -32.768 e 32.767.  
  
```sql  
CREATE SEQUENCE SmallSeq 
    AS smallint ;  
```  
  
### <a name="g-creating-a-sequence-using-all-arguments"></a>G. Creazione di una sequenza utilizzando tutti gli argomenti  
 Nell'esempio seguente viene creata una sequenza denominata DecSeq utilizzando il tipo di dati **decimale**, con un intervallo compreso tra 0 e 255. La sequenza inizia con 125 e aumenta di 25 ogni volta che viene generato un numero. Poiché la sequenza è configurata per riprendere quando il valore supera il valore massimo di 200, la sequenza riprende dal valore minimo di 100.  
  
```sql  
CREATE SEQUENCE Test.DecSeq  
    AS decimal(3,0)   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
```  
  
 Eseguire l'istruzione seguente per visualizzare il primo valore; l'opzione `START WITH` di 125.  
  
```sql  
SELECT NEXT VALUE FOR Test.DecSeq;  
```  
  
 Eseguire l'istruzione altre tre volte affinché vengano restituiti i numeri 150, 175 e 200.  
  
 Eseguire nuovamente l'istruzione per vedere come il valore iniziale torna nuovamente all'opzione `MINVALUE` con valore 100.  
  
 Eseguire il codice seguente per confermare la dimensione della cache e vedere il valore corrente.  
  
```sql  
SELECT cache_size, current_value   
FROM sys.sequences  
WHERE name = 'DecSeq' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
