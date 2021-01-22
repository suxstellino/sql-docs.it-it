---
description: Configurare la crittografia delle colonne sul posto con Transact-SQL
title: Configurare la crittografia delle colonne sul posto con Transact-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: ab59eec637bd5afc127227b09445417ffa1fe4eb
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534850"
---
# <a name="configure-column-encryption-in-place-with-transact-sql"></a>Configurare la crittografia delle colonne sul posto con Transact-SQL

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Questo articolo descrive come eseguire operazioni di crittografia sul posto sulle colonne che usano Always Encrypted con enclave sicuri con l'[istruzione ALTER TABLE](../../../odbc/microsoft/alter-table-statement.md)/`ALTER COLUMN`. Per informazioni di base sulla crittografia sul posto e sui prerequisiti generali, vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md).

Con l'istruzione `ALTER TABLE` o `ALTER COLUMN`, è possibile impostare la configurazione della crittografia di destinazione per una colonna. Quando si esegue l'istruzione, l'enclave sicuro lato server crittografa, crittografa nuovamente o decrittografa i dati archiviati nella colonna, a seconda della configurazione della crittografia corrente e di destinazione, specificata nella definizione della colonna nell'istruzione. 
- Se la colonna non è attualmente crittografata, verrà crittografata se si specifica la clausola `ENCRYPTED WITH` nella definizione della colonna.
- Se la colonna è attualmente crittografata, verrà decrittografata (convertita in una colonna di testo non crittografato), se non si specifica la clausola `ENCRYPTED WITH` nella definizione della colonna.
- Se la colonna è attualmente crittografata, verrà crittografata nuovamente se si specifica la clausola `ENCRYPTED WITH` e il tipo di crittografia di colonna specificato o la chiave di crittografia di colonna è diversa dal tipo di crittografia o dalla chiave di crittografia di colonna attualmente usata. 

> [!NOTE]
> Non è possibile combinare operazioni di crittografia con altre modifiche in una singola istruzione `ALTER TABLE`/`ALTER COLUMN`, tranne l'impostazione della colonna su `NULL` o `NOT NULL` oppure la modifica di una regola di confronto. Ad esempio, non è possibile crittografare una colonna E modificare un tipo di dati della colonna in un'unica istruzione Transact-SQL `ALTER TABLE`/`ALTER COLUMN`. Usare due istruzioni separate.

Come qualsiasi query che usa un enclave sicuro lato server, un'istruzione `ALTER TABLE`/`ALTER COLUMN` che attiva la crittografia sul posto deve essere inviata tramite una connessione con Always Encrypted e i calcoli dell'enclave abilitati. 

Nel resto dell'articolo viene descritto come attivare la crittografia sul posto usando l'istruzione `ALTER TABLE`/`ALTER COLUMN` da SQL Server Management Studio. In alternativa, è possibile eseguire `ALTER TABLE`/`ALTER COLUMN` da Azure Data Studio o dall'applicazione. 

> [!NOTE]
> Attualmente, il cmdlet [Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd) nel modulo di PowerShell SqlServer e [sqlcmd](../../../tools/sqlcmd-utility.md) non supportano l'uso di `ALTER TABLE`/`ALTER COLUMN` per le operazioni di crittografia sul posto.

## <a name="perform-in-place-encryption-with-transact-sql-in-ssms"></a>Eseguire la crittografia sul posto con Transact-SQL in SSMS
### <a name="pre-requisites"></a>Prerequisiti
- Prerequisiti descritti in [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md).
- SQL Server Management Studio 18.3 o versione successiva quando si usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SQL Server Management Studio 18.8 o versione successiva quando si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="steps"></a>Passaggi
1. Aprire una finestra di query con Always Encrypted e i calcoli dell'enclave abilitati nella connessione di database. Per informazioni dettagliate, vedere [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](always-encrypted-query-columns-ssms.md#en-dis).
2. Nella finestra di query eseguire l'istruzione `ALTER TABLE`/`ALTER COLUMN` specificando la configurazione di crittografia di destinazione per una colonna da crittografare, decrittografare o crittografare di nuovo. Per crittografare o crittografare nuovamente la colonna, usare la clausola `ENCRYPTED WITH`. Se la colonna è una colonna di tipo stringa (ad esempio, `char`, `varchar`, `nchar`, `nvarchar`), potrebbe anche essere necessario modificare le regole di confronto impostando regole di confronto BIN2. 
    
    > [!NOTE]
    > Se la chiave master della colonna è archiviata in Azure Key Vault, potrebbe essere richiesto di accedere ad Azure.

3. Cancellare la cache dei piani per tutti i batch e le stored procedure che accedono alla tabella per aggiornare le informazioni di crittografia dei parametri. 
 
    ```sql
    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```
    > [!NOTE]
    > Se non si rimuove il piano per le query interessate dalla cache, la prima esecuzione della query dopo la crittografia potrebbe non riuscire.

    > [!NOTE]
    > Usare `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` o `DBCC FREEPROCCACHE` per cancellare la cache dei piani con attenzione, perché ciò potrebbe comportare una temporanea riduzione del livello delle prestazioni delle query. Per ridurre al minimo l'impatto negativo della cancellazione della cache, è possibile rimuovere selettivamente i piani solo per le query interessate.

4.  Chiamare [sp_refresh_parameter_encryption](../../system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) per aggiornare i metadati per i parametri di ogni modulo (stored procedure, funzione, vista, trigger) persistente in [sys.parameters](../..//system-catalog-views/sys-parameters-transact-sql.md) e che potrebbe risultare invalidato in seguito alla crittografia delle colonne.

### <a name="examples"></a>Esempi
#### <a name="encrypting-a-column-in-place"></a>Crittografia di una colonna sul posto
L'esempio seguente presuppone che:
- `CEK1` sia una chiave di crittografia di colonna abilitata per l'enclave.
- La colonna `SSN` sia di testo non crittografato e usi attualmente le regole di confronto del database predefinite, come Latin1 non BIN2 (ad esempio, `Latin1_General_CI_AI_KS_WS`).

L'istruzione crittografa la colonna `SSN` usando la crittografia casuale e la chiave di crittografia di colonna abilitata per l'enclave sul posto. Sovrascrive anche le regole di confronto del database predefinite con regole di confronto BIN2 corrispondenti (nella stessa tabella codici).

L'operazione viene eseguita online (`ONLINE = ON`). Si noti anche la chiamata a `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE`, che ricrea i piani delle query interessate dalla modifica dello schema di tabella.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char] COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
WITH
(ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-change-encryption-type"></a>Crittografare nuovamente una colonna sul posto per modificare il tipo di crittografia
L'esempio seguente presuppone che:
- La colonna `SSN` sia crittografata con la crittografia deterministica e una chiave di crittografia di colonna abilitata per l'enclave, `CEK1`.
- Le regole di confronto correnti, impostate a livello di colonna, siano `Latin1_General_BIN2`.

L'istruzione seguente crittografa nuovamente la colonna usando la crittografia casuale e la stessa chiave (`CEK1`)

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-rotate-a-column-encryption-key"></a>Crittografare nuovamente una colonna sul posto per ruotare una chiave di crittografia di colonna
L'esempio seguente presuppone che:
- La colonna `SSN` venga crittografata usando la crittografia casuale e una chiave di crittografia di colonna abilitata per l'enclave, `CEK1`.
- `CEK2` sia una chiave di crittografia di colonna abilitata per l'enclave (diversa da `CEK1`).
- Le regole di confronto correnti, impostate a livello di colonna, siano `Latin1_General_BIN2`.

L'istruzione seguente crittografa nuovamente la colonna con `CEK2`.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK2]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```
#### <a name="decrypt-a-column-in-place"></a>Decrittografare un colonna sul posto
L'esempio seguente presuppone che:
- La colonna `SSN` venga crittografata con una chiave di crittografia di colonna abilitata per l'enclave.
- Le regole di confronto correnti, impostate a livello di colonna, siano `Latin1_General_BIN2`.

L'istruzione seguente decrittografa la colonna e mantiene invariate le regole di confronto. In alternativa, è possibile scegliere di modificare le regole di confronto. Sostituire ad esempio le regole di confronto con regole di confronto non BIN2 nella stessa istruzione.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
WITH (ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Vedere anche  
- [Risolvere i problemi comuni per Always Encrypted con enclave sicure](always-encrypted-enclaves-troubleshooting.md)
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md)
- [Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)