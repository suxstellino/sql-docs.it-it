---
title: Indici sulle colonne abilitate per l'enclave con la crittografia casuale (esercitazione)
description: Questa esercitazione illustra come creare e usare gli indici sulle colonne abilitate per l'enclave tramite la crittografia casuale supportata in Always Encrypted con enclave sicure per SQL Server e il database SQL di Azure.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 5e08a42f9112379951c9fcff27693ab3700b69fd
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534820"
---
# <a name="tutorial-create-and-use-indexes-on-enclave-enabled-columns-using-randomized-encryption"></a>Esercitazione: Creare e usare indici sulle colonne abilitate per l'enclave tramite la crittografia casuale

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In questa esercitazione viene illustrato come creare e usare gli indici sulle colonne abilitate per l'enclave tramite la crittografia casuale supportata in [Always Encrypted con enclave sicuri](encryption/always-encrypted-enclaves.md). L'esercitazione spiega:

> [!div class="checklist"]
> - Come creare un indice quando si ha accesso alle chiavi (la chiave master della colonna e la chiave di crittografia della colonna) che proteggono la colonna.
> - Come creare un indice quando non si ha accesso alle chiavi che proteggono la colonna.

## <a name="prerequisites"></a>Prerequisiti

Assicurarsi di aver completato una delle esercitazioni riportate di seguito prima di seguire le procedure in questa esercitazione:

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="step-1-enable-accelerated-database-recovery-adr-in-your-database"></a>Passaggio 1: Abilitare il ripristino accelerato del database nel database

> [!NOTE]
> Questo passaggio si applica solo a [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]. Se si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], ignorare questo passaggio. Il ripristino accelerato del database viene abilitato automaticamente in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e la disabilitazione non è supportata.

Microsoft consiglia di abilitare il ripristino accelerato del database nel database prima di creare il primo indice su una colonna abilitata per l'enclave con la crittografia casuale. Vedere la sezione [Ripristino del database](./encryption/always-encrypted-enclaves.md#database-recovery) in [Always Encrypted con enclave sicuri](./encryption/always-encrypted-enclaves.md).



1. Chiudere tutte le istanze di SSMS che sono state usate nell'esercitazione precedente. Verranno chiuse le connessioni di database che sono state aperte, come richiesto per abilitare il ripristino accelerato del database.
1. Aprire una nuova istanza di SSMS e connettersi all'istanza di [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] come sysadmin **senza** Always Encrypted abilitato per la connessione di database.
    1. Avviare SSMS.
    1. Nella finestra di dialogo **Connetti al server** specificare il nome del server, selezionare un metodo di autenticazione e specificare le credenziali.
    1. Fare clic su **Opzioni >>** e selezionare la scheda **Always Encrypted**.
    1. Verificare che la casella di controllo **Abilita Always Encrypted (crittografia colonna)** **non** sia selezionata.
    1. Selezionare **Connetti**.
1. Aprire una nuova finestra di query ed eseguire l'istruzione seguente per abilitare il ripristino accelerato del database.

   ```sql
   ALTER DATABASE ContosoHR SET ACCELERATED_DATABASE_RECOVERY = ON;
   ```

## <a name="step-2-create-and-test-an-index-without-role-separation"></a>Passaggio 2: Creare e testare un indice senza la separazione dei ruoli

In questo passaggio verrà creato e testato un indice su una colonna crittografata. Si opererà come un singolo utente che ricopre sia il ruolo di amministratore di database, che gestisce il database, che di proprietario dei dati, che ha accesso alle chiavi che proteggono i dati.

1. Aprire una nuova istanza di SSMS e connettersi all'istanza di [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] **con** Always Encrypted abilitato per la connessione di database.
   1. Avviare una nuova istanza di SSMS.
   1. Nella finestra di dialogo **Connetti al server** specificare il nome del server, selezionare un metodo di autenticazione e specificare le credenziali.
   1. Fare clic su **Opzioni >>** e selezionare la scheda **Always Encrypted**.
   1. Selezionare la casella di controllo **Abilita Always Encrypted (crittografia colonna)** e specificare l'URL di attestazione dell'enclave, ad esempio `http://hgs.bastion.local/Attestation` o `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`.
   1. Selezionare **Connetti**.
   1. Se viene richiesto di abilitare la parametrizzazione per le query Always Encrypted, fare clic su **Abilita**.
1. Se non viene richiesto di abilitare la parametrizzazione per Always Encrypted, verificare che sia abilitata.
   1. Selezionare **Strumenti** dal menu principale di SSMS.
   2. Selezionare **Opzioni**.
   3. Passare a **Esecuzione query** > **SQL Server** > **Avanzata**.
   4. Assicurarsi che l'opzione **Abilita parametrizzazione per Always Encrypted** sia selezionata.
   5. Selezionare **OK**.
1. Aprire una finestra di query ed eseguire le istruzioni seguenti per crittografare la colonna **LastName** nella tabella **Employees**. Verrà creato e usato un indice su tale colonna nei passaggi successivi.

   ```sql  
   ALTER TABLE [HR].[Employees]
   ALTER COLUMN [LastName] [nvarchar](50) COLLATE Latin1_General_BIN2 
   ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
   GO   
   ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
   GO
   ```

1. Creare un indice sulla colonna **LastName**. Poiché si è connessi al database con Always Encrypted abilitato, il driver client all'interno di SSMS fornisce in modo trasparente **CEK1** (la chiave di crittografia della colonna, che protegge la colonna **LastName**) per l'enclave, necessaria per creare l'indice.

   ```sql
   CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
   INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
   GO
   ```

1. Eseguire una query avanzata sulla colonna **LastName** e verificare che SQL Server usi l'indice quando si esegue la query.
   1. Nella stessa finestra di query o in una nuova verificare che il pulsante **Includi statistiche query dinamiche** sulla barra degli strumenti sia attivato.
   1. Eseguire la query seguente.

       ```sql
       DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
       SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
       GO
      ```

   1. Nella scheda **Statistiche query dinamiche** (nella parte inferiore della finestra di query) è possibile osservare che la query usa l'indice.

## <a name="step-3-create-an-index-with-role-separation"></a>Passaggio 3: Creare un indice con la separazione dei ruoli

In questo passaggio verrà creato un indice su una colonna crittografata, operando come due utenti diversi. Un utente è un amministratore di database, che deve creare un indice, ma non ha accesso alle chiavi. L'altro utente è un proprietario dei dati, che può accedere alle chiavi.

1. Usando l'istanza di SSMS **senza** Always Encrypted abilitato, eseguire l'istruzione seguente per eliminare l'indice della colonna **LastName**.

   ```sql
   DROP INDEX IX_LastName ON [HR].[Employees]; 
   GO
   ```

1. Operando come il proprietario dei dati (o un'applicazione che dispone dell'accesso alle chiavi), popolare la cache all'interno dell'enclave con **CEK1**.

   > [!NOTE]
   > A meno che non sia stata riavviata l'istanza di SQL Server dopo il **Passaggio 2: Creare e testare un indice senza la separazione dei ruoli**, questo passaggio è ridondante dal momento che **CEK1** è già presente nella cache. È stato aggiunto per illustrare come un proprietario dei dati può fornire una chiave per l'enclave, se non è già presente nell'enclave.

   1. Nell'istanza di SSMS **con** Always Encrypted abilitato eseguire le istruzioni seguenti in una finestra di query. L'istruzione invia all'enclave tutte le chiavi di crittografia delle colonne abilitate per l'enclave. Per informazioni dettagliate, vedere [sp_enclave_send_keys](../system-stored-procedures/sp-enclave-send-keys-sql.md).

        ```sql
        EXEC sp_enclave_send_keys;
        GO
        ```

   1. Come alternativa all'esecuzione della stored procedure precedente, è possibile eseguire una query DML che usa l'enclave sulla colonna **LastName**. Questa operazione inserirà nell'enclave solo **CEK1**.

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

1. Operando come amministratore di database, creare l'indice.
    1. Nell'istanza di SSMS **senza** Always Encrypted abilitato eseguire le istruzioni seguenti in una finestra di query.

        ```sql
        CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
        INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
        GO
        ```

1. Come proprietario dei dati, eseguire una query avanzata sulla colonna **LastName** e verificare che SQL Server usi l'indice quando si esegue la query.
   1. Nell'istanza di SSMS **con** Always Encrypted abilitato selezionare una finestra di query esistente o aprirne una nuova e verificare che il pulsante **Includi statistiche query dinamiche** sulla barra degli strumenti sia attivato.
   1. Eseguire la query seguente. 

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

   1. Nella scheda **Statistiche query dinamiche** (nella parte inferiore della finestra di query) è possibile osservare che la query usa l'indice.

## <a name="next-steps"></a>Passaggi successivi
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Vedere anche
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](encryption/always-encrypted-enclaves-create-use-indexes.md)
