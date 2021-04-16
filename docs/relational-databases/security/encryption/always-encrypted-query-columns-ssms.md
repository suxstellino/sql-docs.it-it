---
title: Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio | Microsoft Docs
description: Informazioni su come eseguire query sulle colonne in Always Encrypted usando SQL Server Management Studio. Recuperare testo crittografato o valori di testo archiviati nelle colonne crittografate.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7ad39aeedd62011c660ae2c7f67a385bc8c3710
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107560997"
---
# <a name="query-columns-using-always-encrypted-with-sql-server-management-studio"></a>Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive come eseguire query sulle colonne crittografate con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) usando [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md). Con SSMS è possibile:
- Recuperare i valori del testo crittografato archiviati nelle colonne crittografate. 
- Recuperare i valori del testo non crittografato archiviati nelle colonne crittografate.   
- Inviare i valori del testo non crittografato destinati alle colonne crittografate (ad esempio, nelle istruzioni `INSERT` o `UPDATE` e come parametri di ricerca delle clausole `WHERE` nelle istruzioni `SELECT`).

> [!NOTE]
> L'uso delle chiavi master della colonna archiviate in un [HSM](https://docs.microsoft.com/azure/key-vault/managed-hsm/overview) gestito in Azure Key Vault richiede SSMS 18.9 o versione successiva.

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Recupero dei valori del testo crittografato archiviati nelle colonne crittografate    
Per eseguire query SELECT che recuperano il testo crittografato di dati archiviati in colonne crittografate (senza decrittografare i dati) non è necessario avere accesso alle chiavi master di colonna che proteggono i dati. Per recuperare i valori da una colonna crittografata come testo crittografato in SSMS:

1. Verificare che sia possibile accedere ai metadati relativi alle chiavi che proteggono le colonne in cui si esegue la query. Anche se non è necessario poter accedere alle chiavi master di colonna, è necessario disporre delle autorizzazioni a livello di database per visualizzare gli oggetti metadati della chiave master di colonna e della chiave di crittografia della colonna nel database. Per informazioni dettagliate, vedere [Autorizzazioni per l'esecuzione di query su colonne crittografate](#permissions-for-querying-encrypted-columns) più avanti.
1. Assicurarsi di aver disabilitato Always Encrypted per la connessione di database per la finestra dell'editor di query, da cui si eseguirà una query `SELECT` che recupera valori in testo crittografato. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#en-dis) più avanti in questo articolo.     
1. Eseguire la query `SELECT`. I dati recuperati dalle colonne crittografate verranno restituiti come valori binari (crittografati).   

### <a name="example"></a>Esempio
Supponendo che `SSN` è una colonna crittografata nella tabella `Patients` , la query riportata di seguito recupererà i valori binari del testo crittografato, se Always Encrypted è disabilitato per la connessione di database.   

![Screenshot della query SELECT [SSN] FROM [dbo].[Patients] e i risultati della query visualizzati come valori di testo crittografato binario.](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Recupero dei valori del testo non crittografato archiviati nelle colonne crittografate    
Per recuperare i valori da una colonna crittografata come testo non crittografato (decrittografando i valori):   
1. Verificare che sia possibile accedere alle chiavi master di colonna e ai metadati relativi alle chiavi che proteggono le colonne in cui si esegue la query. Per informazioni dettagliate, vedere [Autorizzazioni per l'esecuzione di query su colonne crittografate](#permissions-for-querying-encrypted-columns) più avanti.
1.  Assicurarsi di aver abilitato Always Encrypted per la connessione di database per la finestra dell'editor di query, da cui si eseguirà una query `SELECT` che recupera e decrittografa i dati. Questo indicherà al provider di dati .NET Framework per SQL Server (usato da SSMS) di decrittografare le colonne crittografate nel set di risultati della query. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#en-dis) più avanti in questo articolo.
1.  Eseguire la query `SELECT`. Tutti i dati recuperati dalle colonne crittografate verranno restituiti come valori in testo non crittografato dei tipi di dati originali.
 
### <a name="example"></a>Esempio
Supponendo che SSN sia una colonna crittografata `char(11)` nella tabella `Patients` , la query mostrata di seguito restituirà i valori del testo non crittografato se Always Encrypted è abilitato per la connessione di database e se si ha accesso alla chiave master della colonna configurata per la colonna `SSN` .   

![Screenshot della query SELECT [SSN] FROM [Clinic].[dbo].[Patients] e i risultati della query visualizzati come valori di testo normale.](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Invio di valori del testo non crittografato destinati alle colonne crittografate       
Per eseguire una query che invia un valore destinato a una colonna crittografata, ad esempio una query che inserisce, aggiorna o applica un filtro in base a un valore archiviato in una colonna crittografata:

1. Verificare che sia possibile accedere alle chiavi master della colonna e ai metadati per le chiavi che proteggono le colonne su cui si esegue la query. Per altre informazioni, vedere [Autorizzazioni per l'esecuzione di query su colonne crittografate](#permissions-for-querying-encrypted-columns) più avanti.
1.  Assicurarsi di aver abilitato Always Encrypted per la connessione di database per la finestra dell'editor di query, da cui si eseguirà una query `SELECT` che recupera e decrittografa i dati. Questo indicherà al provider di dati .NET Framework per SQL Server (usato da SSMS) di decrittografare le colonne crittografate nel set di risultati della query. Vedere la sezione [Abilitazione e disabilitazione di Always Encrypted per una connessione di database](#en-dis) più avanti in questo articolo.
1. Assicurarsi che la funzionalità Parametrizzazione per Always Encrypted sia abilitata per la finestra dell'editor di query. Richiede almeno SSMS versione 17.0. Dichiarare una variabile Transact-SQL e inizializzarla con un valore da inviare al database (mediante le operazioni di inserimento, aggiornamento o applicazione del filtro). Per informazioni dettagliate, vedere la sezione [Parametrizzazione per Always Encrypted](#param) più avanti in questo articolo.

1. Eseguire la query inviando il valore della variabile Transact-SQL al database. SSMS convertirà la variabile in un parametro di query e crittograferà il relativo valore prima di inviarlo al database.   

### <a name="example"></a>Esempio
Supponendo che `SSN` sia una colonna crittografata `char(11)` nella tabella `Patients` , lo script seguente proverà a trovare una riga contenente `'795-73-9838'` nella colonna SSN e a restituire il valore della colonna `LastName` , a condizione che Always Encrypted sia abilitato per la connessione di database, che la funzionalità Parametrizzazione per Always Encrypted sia abilitata per la finestra dell'editor di query e che si abbia accesso alla chiave master della colonna configurata per la colonna `SSN` .   

![Screenshot della query DECLARE @SSN CHAR(11) = '795-73-9838' SELECT [LastName] FROM [dbo].[Patients] WHERE [SSN] = @SSN e dei risultati della query.](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Autorizzazioni per l'esecuzione di query su colonne crittografate

Per eseguire le query su colonne crittografate, incluse le query che recuperano i dati in testo crittografato, sono necessarie le autorizzazioni `VIEW ANY COLUMN MASTER KEY DEFINITION` e `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` nel database.

Oltre alle autorizzazioni precedenti, per decrittografare i risultati di una query o crittografare i parametri di query (prodotti dalla parametrizzazione delle variabili Transact-SQL), sono necessarie anche le autorizzazioni dell'archivio chiavi per accedere e usare la chiave master della colonna che protegge le colonne di destinazione. Per informazioni dettagliate sulle autorizzazioni per l'archivio chiavi, vedere Creare e archiviare chiavi master della colonna [per](create-and-store-column-master-keys-always-encrypted.md) Always Encrypted e trovare una sezione pertinente per l'archivio chiavi.

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a><a name="en-dis"></a> Abilitazione e disabilitazione di Always Encrypted per una connessione di database   
Quando ci si connette a un database in SSMS, è possibile abilitare o disabilitare Always Encrypted per la connessione di database. Per impostazione predefinita, Always Encrypted è disabilitato. 

Quando si abilita Always Encrypted per una connessione di database, si indica al provider di dati .NET Framework per SQL Server, usato da SQL Server Management Studio, di provare a eseguire le operazioni seguenti in modo trasparente:   
-   Decrittografare tutti i valori recuperati dalle colonne crittografate e restituiti nei risultati della query.   
-   Crittografare i valori delle variabili Transact-SQL con parametri destinati alle colonne di database crittografate.   

Se non si Abilita Always Encrypted per una connessione, il provider di dati .NET Framework per SQL Server, usato da SSMS, non proverà a crittografare i parametri di query o a decrittografare i risultati.

È possibile abilitare o disabilitare Always Encrypted quando si crea una nuova connessione oppure si modifica una connessione esistente usando la finestra di dialogo **Connetti al server**. 

Per abilitare o disabilitare Always Encrypted:
1. Aprire la finestra di dialogo **Connetti al server**. Per informazioni dettagliate, vedere [Connessione a un'istanza di SQL Server](../../../ssms/quickstarts/ssms-connect-query-sql-server.md#connect-to-a-sql-server-instance).
1. Fare clic su **Opzioni >>** .
1. Se si usa SSMS 18 o versione successiva:
    1. Selezionare la scheda **Always Encrypted**.
    1. Per abilitare Always Encrypted, selezionare **Abilita Always Encrypted (crittografia colonna)** . Per disabilitare Always Encrypted, verificare che **Abilita Always Encrypted (crittografia colonna)** non sia selezionato.
1. Se si usa SSMS 17 o versione precedente:
    1. Selezionare la scheda **Proprietà aggiuntive**.
    1. Per abilitare Always Encrypted, digitare `Column Encryption Setting = Enabled`. Per disabilitare Always Encrypted, specificare `Column Encryption Setting = Disabled` o rimuovere l'impostazione di **Crittografia di colonna** dalla scheda **Proprietà aggiuntive** (il valore predefinito è **Disabilitato**).   
 1. Fare clic su **Connetti**.

Per eseguire istruzioni che sfruttano un'enclave sicuro lato server quando si usa Always Encrypted con [enclave](always-encrypted-enclaves.md)sicuri, è necessario specificare un URL di attestazione dell'enclave, oltre ad abilitare Always Encrypted per la connessione. Per informazioni dettagliate, vedere Prerequisiti per l'esecuzione di istruzioni [T-SQL tramite enclave in SSMS.](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms)

> [!TIP]
> Per abilitare e disabilitare Always Encrypted per una finestra dell'editor di query esistente:   
> 1.    Fare clic con il pulsante destro del mouse in un punto qualsiasi all'interno della finestra dell'editor di query.
> 2.    Selezionare **Connessione** > **Cambia connessione**. Si aprirà la finestra di dialogo **Connetti al server** per la connessione corrente per la finestra dell'editor di query. 
> 2.    Abilitare o disabilitare Always Encrypted seguendo i passaggi descritti in precedenza e fare clic su **Connetti**.  
   
## <a name="parameterization-for-always-encrypted"></a><a name="param"></a>Parametrizzazione per Always Encrypted   
 
Parametrizzazione per Always Encrypted è una funzionalità di SQL Server Management Studio che converte automaticamente le variabili Transact-SQL in parametri di query (istanze della [classe SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter)). Richiede almeno SSMS versione 17.0. Ciò consente al provider di dati .NET Framework per SQL Server di rilevare i dati destinati alle colonne crittografate e di crittografare tali dati prima di inviarli al database. 
  
Senza parametrizzazione, il provider di dati .NET Framework passa ogni istruzione creata nell'editor di query come query senza parametri. Se la query contiene valori letterali o variabili Transact-SQL destinate a colonne crittografate, il provider di dati .NET Framework per SQL Server non riuscirà a rilevarle e crittografarle prima di inviare la query al database. Di conseguenza, la query avrà esito negativo a causa di mancata corrispondenza tra i tipi, ovvero tra la variabile Transact-SQL letterale non crittografata e la colonna crittografata. Ad esempio, la query seguente avrà esito negativo senza parametrizzazione, supponendo che la colonna `SSN` sia crittografata.   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Abilitazione e disabilitazione della funzionalità Parametrizzazione per Always Encrypted

La funzionalità Parametrizzazione per Always Encrypted è disabilitata per impostazione predefinita.

Per abilitare o disabilitare la funzionalità Parametrizzazione per Always Encrypted per la finestra dell'editor di query corrente:

1. Selezionare **Query** dal menu principale.
2. Selezionare **Opzioni query**.
3. Passare a **Esecuzione** > **Avanzata**.
4. Selezionare o deselezionare **Abilita parametrizzazione per Always Encrypted**.
5. Fare clic su **OK**.

Per abilitare/disabilitare la funzionalità Parametrizzazione per Always Encrypted per future finestre dell'editor di query:

1. Selezionare **Strumenti** dal menu principale.
2. Selezionare **Opzioni**.
3. Passare a **Esecuzione query** > **SQL Server** > **Avanzata**.
4. Selezionare o deselezionare **Abilita parametrizzazione per Always Encrypted**.
5. Fare clic su **OK**.

Se si esegue una query in una finestra dell'editor di query che usa una connessione di database con Always Encrypted abilitato, ma la parametrizzazione non è abilitata per la finestra dell'editor di query, verrà richiesto di abilitarla.

> [!NOTE]
> La funzionalità Parametrizzazione per Always Encrypted funziona solo nelle finestre dell'editor di query che usano connessioni di database con l'opzione Always Encrypted abilitata (vedere [Abilitazione e disabilitazione della funzionalità Parametrizzazione per Always Encrypted](#enabling-and-disabling-parameterization-for-always-encrypted)). Non verrà parametrizzata alcuna variabile Transact-SQL se la finestra dell'editor di query usa una connessione di database senza l'opzione Always Encrypted abilitata.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funzionamento di Parametrizzazione per Always Encrypted

Se nella connessione di database sono abilitati sia la funzionalità Parametrizzazione per Always Encrypted che il comportamento di Always Encrypted per una finestra dell'editor di query, SQL Server Management Studio proverà a parametrizzare le variabili Transact-SQL che soddisfano le condizioni necessarie seguenti:

- Le variabili devono essere dichiarate e inizializzate nella stessa istruzione (inizializzazione inline). Le variabili dichiarate con istruzioni `SET` separate non verranno parametrizzate.
- Le variabili devono essere inizializzate con un singolo valore letterale. Le variabili inizializzate con espressioni che comprendono operatori o funzioni non verranno parametrizzate.

Di seguito sono riportati alcuni esempi di variabili che verranno parametrizzate da SQL Server Management Studio.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Di seguito sono riportati invece alcuni esempi di variabili che non verranno parametrizzate da SQL Server Management Studio:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Per far sì che la parametrizzazione riesca:   
- Il tipo del valore letterale usato per l'inizializzazione della variabile da parametrizzare deve corrispondere al tipo nella dichiarazione di variabile.   
- Se il tipo dichiarato della variabile è un tipo data o ora, la variabile deve essere inizializzata usando una stringa con uno dei [formati conformi a ISO 8601](../../../t-sql/functions/cast-and-convert-transact-sql.md#date-and-time-styles) seguenti.    

Di seguito sono riportati alcuni esempi di dichiarazioni di variabili Transact-SQL che causano errori di parametrizzazione:   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio usa Intellisense per indicare le variabili che possono essere parametrizzate correttamente e quelle la cui parametrizzazione non riuscirà e il motivo.   

Una dichiarazione di una variabile che può essere parametrizzata correttamente è contrassegnata con una sottolineatura di avviso nell'editor di query. Se si passa il mouse su un'istruzione di dichiarazione contrassegnata con una sottolineatura di avviso, verranno visualizzati i risultati del processo di parametrizzazione, inclusi i valori delle proprietà chiave dell'oggetto [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) risultante (la variabile a cui viene eseguito il mapping): [SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), [Size](/dotnet/api/system.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/system.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/system.data.sqlclient.sqlparameter.scale), [SqlValue](/dotnet/api/system.data.sqlclient.sqlparameter.sqlvalue). È anche possibile visualizzare l'elenco completo di tutte le variabili che sono state parametrizzate correttamente nella scheda **Avviso** della vista **Elenco errori** . Per aprire la vista **Elenco errori** , selezionare **Vista** dal menu principale, quindi selezionare **Elenco errori**.    

Se SQL Server Management Studio ha provato a parametrizzare una variabile, ma la parametrizzazione non è riuscita, la dichiarazione della variabile verrà contrassegnata con una sottolineatura di errore. Se si passa il mouse sull'istruzione di dichiarazione contrassegnata con una sottolineatura di errore, verranno visualizzati i risultati relativi all'errore. È anche possibile visualizzare l'elenco completo degli errori di parametrizzazione per tutte le variabili nella scheda **Errore** della vista **Elenco errori** . Per aprire la vista **Elenco errori** , selezionare **Vista** dal menu principale, quindi selezionare **Elenco errori**.   

La schermata seguente mostra un esempio di sei dichiarazioni di variabili. SQL Server Management Studio ha parametrizzato correttamente le prime tre variabili. Le ultime tre variabili non hanno soddisfatto le condizioni necessarie per la parametrizzazione e pertanto SQL Server Management Studio non ha provato a parametrizzarle (le relative dichiarazioni non vengono contrassegnate in alcun modo).

![Screenshot che mostra un esempio di sei dichiarazioni di variabili di cui tre parametrizzate in modo corretto e tre in errore con i messaggi di avviso associati.](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
Un altro esempio riportato di seguito mostra due variabili che soddisfano le condizioni necessarie per la parametrizzazione, ma il tentativo di parametrizzazione non è riuscito perché le variabili sono state inizializzate in modo non corretto.    
 
![Screenshot che mostra un esempio di due dichiarazioni di variabili che alla fine hanno esito negativo con il messaggio di errore associato.](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
> [!NOTE]
> Poiché Always Encrypted supporta un subset limitato di conversioni di tipi, in molti casi è necessario che il tipo di dati di una variabile Transact-SQL sia lo stesso tipo della colonna di database di destinazione a cui è destinato. Ad esempio, supponendo che il tipo della colonna `SSN` nella tabella `Patients` è `char(11)`, la query sottostante avrà esito negativo perché il tipo della variabile `@SSN` , ovvero `nchar(11)`, non corrisponde al tipo della colonna.   

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
> Senza la parametrizzazione, l'intera query, incluse le conversioni dei tipi, viene elaborata all'interno di SQL Server o del database SQL di Azure. Con parametrizzazione abilitata, alcune conversioni dei tipi vengono eseguite da .NET Framework all'interno di SQL Server Management Studio. A causa delle differenze tra il sistema dei tipi .NET Framework e il sistema dei tipi SQL Server (ad esempio diversa precisione di alcuni tipi come float), una query eseguita con la parametrizzazione abilitata può produrre risultati diversi rispetto alla query eseguita senza la parametrizzazione abilitata. 

## <a name="next-steps"></a>Passaggi successivi
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)