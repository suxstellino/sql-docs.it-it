---
title: Eseguire istruzioni Transact-SQL con enclave sicure
description: Eseguire istruzioni DDL (Data Definition Language) per configurare la crittografia per la colonna o gestire gli indici nelle colonne crittografate ed eseguire query su colonne crittografate
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 130d2a9f2d92a77ab2d2e033f8e5f4fb9f88ef9f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837664"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>Eseguire istruzioni Transact-SQL con enclave sicure

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclave sicure](always-encrypted-enclaves.md) consente ad alcune istruzioni Transact-SQL (T-SQL) di eseguire calcoli riservati sulle colonne di database crittografate in un'enclave sicura sul lato server.

## <a name="statements-using-secure-enclaves"></a>Istruzioni che usano le enclave sicure

I tipi di istruzioni T-SQL seguenti usano le enclave sicure.

### <a name="ddl-statements-using-secure-enclaves"></a>Istruzioni DDL che usano enclave sicure

I tipi seguenti di istruzioni [DDL (Data Definition Language)](../../../t-sql/statements/statements.md#data-definition-language) richiedono enclave sicure.

- Istruzioni [ALTER TABLE column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md) che attivano operazioni di crittografia sul posto usando chiavi abilitate per l'enclave. Per altre informazioni, vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md).
- Istruzioni [CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md) e [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md) che creano o modificano gli indici per le colonne abilitate per l'enclave usando la crittografia casuale. Per altre informazioni, vedere [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md).
  
### <a name="dml-statements-using-secure-enclaves"></a>Istruzioni DML che usano enclave sicure

Le istruzioni [DML (Data Manipulation Language)](../../../t-sql/statements/statements.md#data-manipulation-language) seguenti o le query sulle colonne abilitate per l'enclave che usano la crittografia casuale richiedono le enclave sicure:

- Le query che usano uno o più degli operatori Transact-SQL seguenti sono supportate all'interno di enclave sicure:
  - [Operatori di confronto](../../../mdx/comparison-operators.md)
  - [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - [Join](../../performance/joins.md) - [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] supporta solo i join a cicli annidati. [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] supporta join a cicli annidati, hash join e merge join
  - [Clausola SELECT - ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md). Supportata in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. Proprietà non supportata in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
  - [Clausola SELECT - GROUP BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md). Supportata in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. Proprietà non supportata in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
- Query che inseriscono, aggiornano o eliminano righe, che a loro volta attivano l'inserimento e/o la rimozione di una chiave di indice da e verso un indice in una colonna abilitata per l'enclave. Per altre informazioni, vedere [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Le operazioni sugli indici e le query DML riservate che usano le enclave sono supportate solo nelle colonne abilitate per l'enclave che usano la crittografia casuale. La crittografia deterministica non è supportata.
>
> In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], le query riservate che usano le enclave su colonne di stringhe di caratteri (`char`, `nchar`) richiedono la configurazione delle regole di confronto binary2 (BIN2) per l'ordinamento per la colonna. In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], è necessario usare le regole di confronto BIN2 o UTF-8.

### <a name="dbcc-commands-using-secure-enclaves"></a>Comandi DBCC che usano enclave sicure

Anche i comandi amministrativi [DBCC (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md) che comportano il controllo dell'integrità degli indici potrebbero richiedere enclave sicure, se il database contiene indici su colonne abilitate per l'enclave che usano la crittografia casuale. Ad esempio, [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) e [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>Prerequisiti per l'esecuzione di istruzioni con enclave sicure

L'ambiente deve soddisfare i requisiti seguenti per supportare l'esecuzione di istruzioni che usano un'enclave sicura.

- L'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o il database e il server in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] devono essere configurati correttamente per supportare le enclave e l'attestazione. Per altre informazioni, vedere [Configurare l'enclave sicura e l'attestazione](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- È necessario ottenere un URL di attestazione dall'ambiente dall'amministratore del servizio di attestazione.

  - Se si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host (HGS), vedere [Determinare e condividere l'URL di attestazione HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Se si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] e il servizio Attestazione di Microsoft Azure, vedere [Determinare l'URL di attestazione per i criteri di attestazione](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- Se ci si connette al database usando l'applicazione, è necessario usare un driver client che supporti Always Encrypted con enclave sicure. L'applicazione deve connettersi al database con la funzionalità Always Encrypted abilitata per la connessione al database e il protocollo di attestazione e l'URL di attestazione configurati correttamente. Per informazioni dettagliate, vedere [Sviluppare applicazioni usando Always Encrypted con enclave sicure](always-encrypted-enclaves-client-development.md).
- Se si usa SQL Server Management Studio (SSMS) o Azure SQL Data Studio, è necessario abilitare Always Encrypted e configurare il protocollo di attestazione e l'URL di attestazione durante la connessione al database. Per altre informazioni, vedere le sezioni seguenti.

> [!NOTE]
> La connessione al database con Always Encrypted e l'attestazione configurata non è necessaria per le operazioni seguenti se si usano chiavi di crittografia della colonna memorizzate nella cache: Query DDL che creano o modificano indici, query DML che aggiornano gli indici e comandi DBCC che controllano l'integrità degli indici. Per altre informazioni, vedere [Richiamare operazioni di indicizzazione mediante chiavi di crittografia di colonna memorizzate nella cache](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>Prerequisiti per l'esecuzione di istruzioni T-SQL usando le enclave in SSMS

Requisiti minimi della versione per SQL Server Management Studio:

- SSMS 18.3 quando si usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 quando si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Assicurarsi di eseguire le istruzioni da una finestra di query che usa una connessione per cui sono configurati Always Encrypted e l'URL di attestazione corretto.

1. Nella finestra di dialogo **Connetti al server** specificare il nome del server, selezionare un metodo di autenticazione e specificare le credenziali.
2. Fare clic su **Opzioni >>** e selezionare la scheda **Always Encrypted**.
3. Selezionare la casella di controllo **Abilita Always Encrypted (crittografia di colonna)** e specificare l'URL di attestazione dell'enclave. Ad esempio, `https://hgs.bastion.local/Attestation` o `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`.

    ![Connettersi al server con l'attestazione tramite SSMS](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. Selezionare **Connetti**.
5. Se viene richiesto di abilitare la parametrizzazione per le query Always Encrypted, selezionare **Abilita** se si prevede di eseguire query DML con parametri. Per altre informazioni, vedere [Parametrizzazione per Always Encrypted](always-encrypted-query-columns-ssms.md#param).

Per informazioni dettagliate, vedere [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](always-encrypted-query-columns-ssms.md#en-dis).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>Prerequisiti per l'esecuzione di istruzioni T-SQL usando le enclave in Azure Data Studio

È consigliabile usare la versione minima consigliata **1.23** o versione successiva.

Assicurarsi di eseguire le istruzioni da una finestra di query che usa una connessione con la funzionalità Always Encrypted abilitata e con il protocollo di attestazione e l'URL di attestazione corretti configurati.

1. Nella finestra di dialogo **Connessione** fare clic su **Avanzate...** .
2. Per abilitare Always Encrypted per la connessione, impostare il campo **Always Encrypted** su **Attivato**.
3. Specificare il protocollo di attestazione e l'URL di attestazione.
    - Se si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] impostare **Protocollo di attestazione** su **Servizio Sorveglianza host** e immettere l'URL di attestazione del servizio Sorveglianza host nel campo **URL di attestazione enclave**.
    - Se si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], impostare **Protocollo di attestazione** su **Attestazione di Azure** e immettere l'URL di attestazione che fa riferimento ai criteri nel servizio Attestazione di Microsoft Azure nel campo **URL di attestazione enclave**.

    ![Connettersi al server con l'attestazione tramite Azure Data Studio](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. Fare clic su **OK** per chiudere le **Proprietà avanzate**.

Per informazioni dettagliate, vedere [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection).

Se si prevede di eseguire query DML con parametri, è necessario abilitare anche la [parametrizzazione per Always Encrypted](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted).

## <a name="examples"></a>Esempi

Questa sezione include esempi di query DML che usano le enclave. 

Negli esempi viene usato lo schema seguente.

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>Ricerca esatta delle corrispondenze

La query seguente esegue una ricerca esatta delle corrispondenze per la colonna di tipo stringa `SSN` crittografata.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>Ricerca con criteri di ricerca

La query seguente esegue una ricerca con criteri di ricerca sulla colonna di tipo stringa `SSN` crittografata, cercando i dipendenti con l'ultima cifra specificata per il numero di previdenza sociale.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>Confronto di intervalli

La query seguente esegue un confronto di intervalli sulla colonna `Salary` crittografata, cercando i dipendenti con stipendi compresi nell'intervallo specificato.

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>Join

La query seguente crea un join tra le tabelle `Employees` e `Jobs` usando la colonna `Salary` crittografata. La query recupera i dipendenti con stipendi non compresi nell'intervallo di stipendio per l'incarico del dipendente.

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>Ordinamento

La query seguente ordina i record dei dipendenti in base alla colonna `Salary` crittografata, recuperando i 10 dipendenti con gli stipendi più elevati.
> [!NOTE]
> L'ordinamento delle colonne crittografate è supportato in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], ma non in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>Passaggi successivi

- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Vedere anche

- [Risolvere i problemi comuni per Always Encrypted con enclave sicure](always-encrypted-enclaves-troubleshooting.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md)
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md)