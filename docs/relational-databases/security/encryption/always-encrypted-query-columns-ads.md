---
description: Eseguire query sulle colonne usando Always Encrypted con Azure Data Studio
title: Eseguire query sulle colonne usando Always Encrypted con Azure Data Studio | Microsoft Docs
ms.custom: ''
ms.date: 5/19/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0a61514ff76bbae9e403683529d017f6fcb1c079
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101879"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>Eseguire query sulle colonne usando Always Encrypted con Azure Data Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive come eseguire query sulle colonne crittografate con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) usando [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md). Con Azure Data Studio è possibile:
- Recuperare i valori del testo crittografato archiviati nelle colonne crittografate. 
- Recuperare i valori del testo non crittografato archiviati nelle colonne crittografate.  
- Inviare i valori del testo non crittografato destinati alle colonne crittografate (ad esempio, nelle istruzioni `INSERT` o `UPDATE` e come parametri di ricerca delle clausole `WHERE` nelle istruzioni `SELECT`). 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Recupero dei valori del testo crittografato archiviati nelle colonne crittografate    
Questa sezione descrive come recuperare i dati archiviati come testo crittografato nelle colonne crittografate.

### <a name="steps"></a>Passaggi
1. Assicurarsi di aver disabilitato Always Encrypted per la connessione di database per la finestra di query, da cui si eseguirà una query `SELECT` che recupera valori in testo crittografato. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#enabling-and-disabling-always-encrypted-for-a-database-connection) più avanti in questo articolo.     
1. Eseguire la query `SELECT`. I dati recuperati dalle colonne crittografate verranno restituiti come valori binari (crittografati).   

### <a name="example"></a>Esempio
Supponendo che `SSN` è una colonna crittografata nella tabella `Patients` , la query riportata di seguito recupererà i valori binari del testo crittografato, se Always Encrypted è disabilitato per la connessione di database.   

![Screenshot della query SELECT * FROM [dbo].[Patients] e i risultati della query visualizzati come valori di testo crittografato binario.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Recupero dei valori del testo non crittografato archiviati nelle colonne crittografate    
Questa sezione descrive come recuperare i dati archiviati come testo crittografato nelle colonne crittografate.

### <a name="prerequisites"></a>Prerequisiti
- Azure Data Studio versione 17.1 o successiva.
- È necessario avere accesso alle chiavi master di colonna e ai metadati relativi alle chiavi che proteggono le colonne su cui si esegue la query. Per informazioni dettagliate, vedere [Autorizzazioni per l'esecuzione di query su colonne crittografate](#permissions-for-querying-encrypted-columns) più avanti.
- Le chiavi master della colonna devono essere archiviate in Azure Key Vault o nell'archivio certificati Windows. Azure Data Studio non supporta altri archivi di chiavi.

### <a name="steps"></a>Passaggi
1.  Abilitare Always Encrypted per la connessione di database per la finestra di query, da cui si eseguirà una query `SELECT` che recupera e decrittografa i dati. Questo indicherà al [provider di dati Microsoft .NET Framework per SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), usato da Azure Data Studio, di decrittografare le colonne crittografate nel set di risultati della query. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#enabling-and-disabling-always-encrypted-for-a-database-connection) più avanti in questo articolo.
1.  Eseguire la query `SELECT`. Tutti i dati recuperati dalle colonne crittografate verranno restituiti come valori in testo non crittografato dei tipi di dati originali.
 
### <a name="example"></a>Esempio
Supponendo che SSN sia una colonna crittografata nella tabella `Patients`, la query visualizzata di seguito restituirà i valori del testo non crittografato se Always Encrypted è abilitato per la connessione di database e se si ha accesso alla chiave master della colonna configurata per la colonna `SSN`.   

![Screenshot della query SELECT * FROM [dbo].[Patients] e i risultati della query visualizzati come valori di testo normale.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Invio di valori del testo non crittografato destinati alle colonne crittografate       
In questa sezione viene descritto come eseguire una query che invia valori destinati a una colonna crittografata. Ad esempio, una query che inserisce, aggiorna o filtra in base a un valore archiviato in una colonna crittografata:

### <a name="prerequisites"></a>Prerequisiti
- Azure Data Studio versione 18.1 o successiva.
- È necessario avere accesso alle chiavi master di colonna e ai metadati relativi alle chiavi che proteggono le colonne su cui si esegue la query. Per informazioni dettagliate, vedere [Autorizzazioni per l'esecuzione di query su colonne crittografate](#permissions-for-querying-encrypted-columns) più avanti.
- Le chiavi master della colonna devono essere archiviate in Azure Key Vault o nell'archivio certificati Windows. Azure Data Studio non supporta altri archivi di chiavi.

### <a name="steps"></a>Passaggi
1. Abilitare Always Encrypted per la connessione di database per la finestra di query, da cui si eseguirà una query `SELECT` che recupera e decrittografa i dati. Questa operazione indicherà al [provider di dati Microsoft .NET per SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), usato da Azure Data Studio, di crittografare i parametri di query destinati alle colonne crittografate e decrittografare i risultati recuperati dalle colonne crittografate. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#enabling-and-disabling-always-encrypted-for-a-database-connection) più avanti in questo articolo. 
1. Abilitare la parametrizzazione per Always Encrypted per la finestra di query. Per informazioni dettagliate, vedere la sezione [Parametrizzazione per Always Encrypted](#parameterization-for-always-encrypted) più avanti in questo articolo.
1. Dichiarare una variabile Transact-SQL e inizializzarla con un valore da inviare al database mediante le operazioni di inserimento, aggiornamento o applicazione del filtro. 
1. Eseguire la query inviando il valore della variabile Transact-SQL al database. Azure Data Studio convertirà la variabile in un parametro di query e crittograferà il relativo valore prima di inviarlo al database.   

### <a name="example"></a>Esempio
Supponendo che `SSN` sia una colonna `char(11)` crittografata nella tabella `Patients`, lo script seguente tenterà di trovare una riga contenente `'795-73-9838'` nella colonna SSN. I risultati vengono restituiti se Always Encrypted è abilitato per la connessione al database, la parametrizzazione per Always Encrypted è abilitata per la finestra di query ed è possibile accedere alla chiave master della colonna configurata per la colonna `SSN`.   

![Screenshot della query DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN e dei risultati della query.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Autorizzazioni per l'esecuzione di query su colonne crittografate

Per eseguire query sulle colonne crittografate, comprese query che recuperano dati in testo crittografato, sono necessarie le autorizzazioni **VIEW ANY COLUMN MASTER KEY DEFINITION** e **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** nel database.

Oltre a queste autorizzazioni, per decrittografare i risultati delle query o per crittografare i parametri di query (generati dalla parametrizzazione delle variabili Transact-SQL), è necessario anche accedere alla chiave master della colonna proteggendo le colonne di destinazione:

- **Archivio certificati: computer locale:** è necessario avere accesso in **Lettura** al certificato usato come chiave master della colonna o essere l'amministratore del computer.   
- **Azure Key Vault:** sono necessarie le autorizzazioni **get**, **unwrapKey** e **verify** per l'insieme di credenziali contenente la chiave master della colonna.

Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>Abilitazione e disabilitazione di Always Encrypted per una connessione di database   
Quando ci si connette a un database in Azure Data Studio, è possibile abilitare o disabilitare Always Encrypted per la connessione di database. Per impostazione predefinita, Always Encrypted è disabilitato. 

Quando si abilita Always Encrypted per una connessione di database, si indica al [provider di dati Microsoft .NET per SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), usato da Azure Data Studio, di provare a eseguire le operazioni seguenti in modo trasparente:   
-   Decrittografare tutti i valori recuperati dalle colonne crittografate e restituiti nei risultati della query.   
-   Crittografare i valori delle variabili Transact-SQL con parametri destinati alle colonne di database crittografate.   

Se non si abilita Always Encrypted per una connessione, il provider di dati Microsoft .NET per SQL Server non proverà a crittografare i parametri di query o a decrittografare i risultati.

È possibile abilitare o disabilitare Always Encrypted quando ci si connette a un database. Per informazioni generali su come connettersi a un database, vedere:
- [Avvio rapido: Connettersi ed eseguire query in SQL Server usando Azure Data Studio](../../../azure-data-studio/quickstart-sql-server.md)
- [Avvio rapido: Usare Azure Data Studio per connettersi ed eseguire query nel database SQL di Azure](../../../azure-data-studio/quickstart-sql-database.md)

Per abilitare o disabilitare Always Encrypted:
1. Nella finestra di dialogo **Connessione** fare clic su **Avanzate...** .
2. Per abilitare Always Encrypted per la connessione, impostare il campo **Always Encrypted** su **Attivato**. Per disabilitare Always Encrypted, lasciare vuoto il valore del campo **Always Encrypted** o impostarlo su **Disattivato**.
3. Se si usa [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] e l'istanza di SQL Server è configurata con un'enclave sicuro, è possibile specificare un protocollo di enclave e un URL di attestazione dell'enclave. Se l'istanza di SQL Server non usa un'enclave sicuro, assicurarsi di lasciare vuoti i campi **Attestation Protocol** (Protocollo di attestazione) e **URL di attestazione enclave**. Per altre informazioni, vedere [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md).
4. Fare clic su **OK** per chiudere le **Proprietà avanzate**.

![Breve video che illustra i passaggi per abilitare Always Encrypted per la connessione.](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

> [!TIP]
> Per abilitare e disabilitare Always Encrypted per una finestra di query, fare clic su **Disconnetti** e quindi fare clic su **Connetti** e completare i passaggi precedenti per riconnettersi al database con i valori desiderati nel campo **Always Encrypted**. 

> [!NOTE] 
> Il pulsante **Cambia connessione** in una finestra di query attualmente non supporta l'abilitazione e la disabilitazione di Always Encrypted.

## <a name="parameterization-for-always-encrypted"></a>Parametrizzazione per Always Encrypted

La parametrizzazione per Always Encrypted è una funzionalità di Azure Data Studio 18.1 e versioni successive che converte automaticamente le variabili Transact-SQL in parametri di query (istanze della [classe SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter)). Ciò consente al [provider di dati Microsoft .NET per SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) di rilevare i dati destinati alle colonne crittografate e di crittografare tali dati prima di inviarli al database.
  
Senza la parametrizzazione, il provider di dati Microsoft .NET per SQL Server passa ogni istruzione creata nella finestra di query come query senza parametri. Se la query contiene valori letterali o variabili Transact-SQL destinate a colonne crittografate, il provider di dati .NET Framework per SQL Server non riuscirà a rilevarle e crittografarle prima di inviare la query al database. Di conseguenza, la query avrà esito negativo a causa di mancata corrispondenza tra i tipi, ovvero tra la variabile Transact-SQL letterale non crittografata e la colonna crittografata. Ad esempio, la query seguente avrà esito negativo senza parametrizzazione, supponendo che la colonna `SSN` sia crittografata.   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Abilitazione e disabilitazione della funzionalità Parametrizzazione per Always Encrypted

La funzionalità Parametrizzazione per Always Encrypted è disabilitata per impostazione predefinita.

Per abilitare e disabilitare la parametrizzazione per Always Encrypted:

1. Selezionare **File** > **Preferenze** > **Impostazioni** (**Code** > **Preferences**  > **Settings** (Codice - Preferenze - Impostazioni) su Mac).
2. Passare a **Dati** > **Microsoft SQL Server**.
3. Selezionare o deselezionare **Abilita parametrizzazione per Always Encrypted**.
4. Chiudere la finestra **Impostazioni**.

![Breve video che illustra come abilitare o disabilitare la parametrizzazione per Always Encrypted.](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> La parametrizzazione per Always Encrypted funziona solo in una query che usa connessioni di database con l'opzione Always Encrypted abilitata (vedere [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#enabling-and-disabling-always-encrypted-for-a-database-connection)). Non verrà parametrizzata alcuna variabile Transact-SQL se la finestra di query usa una connessione di database senza l'opzione Always Encrypted abilitata.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funzionamento di Parametrizzazione per Always Encrypted

Se nella finestra di query sono abilitati sia la parametrizzazione per Always Encrypted che Always Encrypted, Azure Data Studio proverà a parametrizzare le variabili Transact-SQL che soddisfano i prerequisiti seguenti:

- Le variabili devono essere dichiarate e inizializzate nella stessa istruzione (inizializzazione inline). Le variabili dichiarate con istruzioni `SET` separate non verranno parametrizzate.
- Le variabili devono essere inizializzate con un singolo valore letterale. Le variabili inizializzate con espressioni che comprendono operatori o funzioni non verranno parametrizzate.

Di seguito sono riportati alcuni esempi di variabili che verranno parametrizzate da Azure Data Studio.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Questi sono esempi di variabili che invece non verranno parametrizzate da Azure Data Studio:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Per far sì che la parametrizzazione riesca:   
- Il tipo del valore letterale usato per l'inizializzazione della variabile da parametrizzare deve corrispondere al tipo nella dichiarazione di variabile.   
- Se il tipo dichiarato della variabile è un tipo data o ora, la variabile deve essere inizializzata usando una stringa con uno dei formati conformi a ISO 8601 seguenti.   

Di seguito sono riportati alcuni esempi di dichiarazioni di variabili Transact-SQL che causano errori di parametrizzazione:

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio usa Intellisense per indicare le variabili che possono essere parametrizzate correttamente e quelle la cui parametrizzazione non riuscirà e il motivo.   

Una dichiarazione di una variabile che può essere parametrizzata correttamente è contrassegnata con un messaggio informativo nella finestra di query. Se si passa il mouse su un'istruzione di dichiarazione contrassegnata con una sottolineatura informativa, verrà visualizzato il messaggio contenente i risultati del processo di parametrizzazione, inclusi i valori delle proprietà chiave dell'oggetto della [classe SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) risultante. Viene eseguito il mapping della variabile a: [SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype), [Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale), [SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue)). È anche possibile visualizzare l'elenco completo di tutte le variabili che sono state parametrizzate correttamente nella scheda **Problemi**. Per aprire la vista **Problemi**, selezionare **Visualizza** > **Problemi**.    



Se Azure Data Studio ha provato a parametrizzare una variabile, ma la parametrizzazione non è riuscita, la dichiarazione della variabile verrà contrassegnata con una sottolineatura di errore. Se si passa il mouse sull'istruzione di dichiarazione contrassegnata con una sottolineatura di errore, verranno visualizzati i risultati relativi all'errore. È anche possibile visualizzare l'elenco completo degli errori di parametrizzazione per tutte le variabili nella vista **Problemi**.

 
> [!NOTE]
> Poiché Always Encrypted supporta un subset limitato di conversioni di tipi, in molti casi è necessario che il tipo di dati di una variabile Transact-SQL sia lo stesso tipo della colonna di database di destinazione a cui la variabile è destinata. Ad esempio, supponendo che il tipo della colonna `SSN` nella tabella `Patients` sia `char(11)`, la query sottostante avrà esito negativo perché il tipo della variabile `@SSN`, ovvero `nchar(11)`, non corrisponde al tipo della colonna.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> Senza la parametrizzazione, l'intera query, incluse le conversioni dei tipi, viene elaborata all'interno di SQL Server o del database SQL di Azure. Con la parametrizzazione abilitata, alcune conversioni di tipi vengono eseguite dal provider di dati Microsoft .NET per SQL Server in Azure Data Studio. A causa delle differenze tra il sistema dei tipi Microsoft .NET e il sistema dei tipi SQL Server (ad esempio diversa precisione di alcuni tipi come ad esempio float), una query eseguita con la parametrizzazione abilitata può produrre risultati diversi rispetto alla query eseguita senza la parametrizzazione abilitata. 

## <a name="next-steps"></a>Passaggi successivi
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)