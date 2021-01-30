---
description: rowversion (Transact-SQL)
title: rowversion (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- timestamp_TSQL
- timestamp
dev_langs:
- TSQL
helpviewer_keywords:
- rowversion data type
- size [SQL Server], rowversion
- row version-stamping [SQL Server]
- rowversion columns
- version-stamping table rows
- unique rowversion numbers
- unique timestamp numbers
- timestamp data type
- timestamp columns
- size [SQL Server], timestamp
ms.assetid: 65c9cf0e-3e8a-45f8-87b3-3460d96afb0b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 47117ba5b13d7b494174576d76cca9a5472f0c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187833"
---
# <a name="rowversion-transact-sql"></a>rowversion (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tipo di dati che espone numeri binari univoci generati automaticamente all'interno di un database. **rowversion** viene in genere usato come meccanismo per contrassegnare le righe di tabella con il numero della versione. Le dimensioni di archiviazione sono 8 byte. Il tipo di dati **rowversion** rappresenta un numero incrementale e non mantiene una data o un orario. Per registrare una data o un orario, usare il tipo di dati **datetime2**.
  
## <a name="remarks"></a>Osservazioni  
Ogni database include un contatore che viene incrementato a ogni operazione di inserimento o aggiornamento eseguita su una tabella contenente una colonna di tipo **rowversion** all'interno del database. Questo contatore è il valore rowversion relativo al database che tiene traccia del tempo relativo all'interno di un database e non del tempo effettivo che può essere associato a un orologio. Ogni tabella può includere una sola colonna di tipo **rowversion**. A ogni modifica o inserimento di una riga in una colonna di tipo **rowversion**, il valore rowversion incrementato relativo al database viene inserito nella colonna di tipo **rowversion**. Questa caratteristica rende la colonna **rowversion** non adatta per le chiavi, in particolare per le chiavi primarie. Gli aggiornamenti eseguiti sulla riga modificano il valore rowversion, con la conseguente modifica del valore della chiave. Se la colonna è una chiave primaria, il valore di chiave precedente non è più valido, come non sono più valide le chiavi esterne che fanno riferimento al valore precedente. Se un cursore dinamico include riferimenti alla tabella, tutti gli aggiornamenti modificano la posizione delle righe all'interno del cursore. Se la colonna è una chiave indice, tutti gli aggiornamenti alle righe di dati comportano l'aggiornamento dell'indice.  Il valore di tipo **rowversion** viene incrementato con qualsiasi istruzione di aggiornamento, anche se i valori di riga non vengono modificati. Ad esempio, se un valore di colonna è 5 e un'istruzione di aggiornamento imposta il valore su 5, questa azione viene considerata un aggiornamento anche se non è presente alcuna modifica e **rowversion** viene incrementato.
  
**timestamp** è il sinonimo del tipo di dati **rowversion** ed è conforme al comportamento dei sinonimi dei tipi di dati. Nelle istruzioni DDL usare **rowversion** anziché **timestamp** laddove possibile. Per altre informazioni, vedere [Sinonimi dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-synonyms-transact-sql.md).
  
Il tipo di dati  **timestamp** di [!INCLUDE[tsql](../../includes/tsql-md.md)] è diverso dal tipo di dati **timestamp** definito nello standard ISO.
  
> [!NOTE]  
>  La sintassi di **timestamp** è deprecata. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
In un'istruzione CREATE TABLE o ALTER TABLE non è necessario specificare un nome di colonna per il tipo di dati **timestamp**, ad esempio:
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, timestamp);  
```  
  
Se non si specifica un nome di colonna, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] genera il nome di colonna **timestamp**. Il sinonimo **rowversion** non è tuttavia caratterizzato dalla stessa funzionalità. Quando si usa **rowversion**, è necessario specificare un nome di colonna, ad esempio:
  
```sql
CREATE TABLE ExampleTable2 (PriKey int PRIMARY KEY, VerCol rowversion) ;  
```  
  
> [!NOTE]  
>  Per generare valori **rowversion** duplicati, usare l'istruzione SELECT INTO nella quale una colonna di tipo **rowversion** è inclusa nell'elenco SELECT. Non è consigliabile usare **rowversion** in questo modo.  
  
Una colonna di tipo **rowversion** che non ammette valori Null equivale dal punto di vista semantico a una colonna di tipo **binary(8)**. Una colonna di tipo **rowversion** che non ammette valori Null equivale dal punto di vista semantico a una colonna di tipo **varbinary(8)**.
  
È possibile usare la colonna di tipo **rowversion** di una riga per stabilire con facilità se per la riga è stata eseguita un'istruzione di aggiornamento dall'ultima volta in cui è stata letta. Se viene eseguita un'istruzione di aggiornamento per la riga, il valore rowversion viene aggiornato. Se invece non viene eseguita alcuna istruzione di aggiornamento per la riga, il valore rowversion rimane invariato rispetto alla lettura precedente. Per restituire il valore rowversion corrente per un database, usare [@@DBTS](../../t-sql/functions/dbts-transact-sql.md).
  
È possibile aggiungere una colonna **rowversion** a una tabella per consentire la gestione dell'integrità del database quando più utenti eseguono contemporaneamente l'aggiornamento di righe. Potrebbe essere necessario, inoltre, conoscere le righe aggiornate e il relativo numero senza eseguire nuovamente una query nella tabella.
  
Si supponga ad esempio di avere creato una tabella denominata `MyTest` e di averla popolata mediante le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti.
  
```sql
CREATE TABLE MyTest (myKey int PRIMARY KEY  
    ,myValue int, RV rowversion);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (2, 0);  
GO  
```  
  
È possibile utilizzare le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di esempio seguenti per implementare il controllo della concorrenza ottimistico nella tabella `MyTest` durante l'aggiornamento. Lo script usa `<myRv>` per rappresentare il valore **rowversion** dall'ultima lettura della riga. Sostituire il valore con il valore **rowversion** effettivo. Un esempio di un valore **rowversion** effettivo è `0x00000000000007D3`.
  
```sql
DECLARE @t TABLE (myKey int);  
UPDATE MyTest  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND RV = <myRv>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  


È possibile inoltre inserire le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di esempio in una transazione. Eseguendo una query sulla variabile `@t` nell'ambito della transazione, è possibile recuperare la colonna aggiornata `myKey` della tabella senza rieseguire una query sulla tabella `MyTest`.

Di seguito viene riportato lo stesso esempio che usa la sintassi **timestamp**. Sostituire `<myTS>` con un **timestamp** effettivo.

```sql
CREATE TABLE MyTest2 (myKey int PRIMARY KEY  
    ,myValue int, TS timestamp);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (2, 0);  
GO  
DECLARE @t TABLE (myKey int);  
UPDATE MyTest2  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND TS = <myTS>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION &#40;Transact-SQL&#41;](../../t-sql/functions/min-active-rowversion-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
  
  
