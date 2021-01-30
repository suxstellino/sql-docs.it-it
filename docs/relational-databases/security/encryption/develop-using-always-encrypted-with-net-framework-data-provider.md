---
title: Always Encrypted con il provider di dati .NET Framework
description: Informazioni su come sviluppare applicazioni .NET usando la funzionalità Always Encrypted per SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 827e509e-3c4f-4820-aa37-cebf0f7bbf80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 26b598865668ce5fb10e34eb9558fe018a7c60a8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075893"
---
# <a name="using-always-encrypted-with-the-net-framework-data-provider-for-sql-server"></a>Using Always Encrypted with .NET Framework Data Provider for SQL Server (Uso di Always Encrypted con il provider di dati .NET Framework per SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo fornisce informazioni su come sviluppare applicazioni .NET usando [Always Encrypted](always-encrypted-database-engine.md) o [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md) e il [provider di dati .NET Framework per SQL Server](/dotnet/framework/data/adonet/sql/).

Always Encrypted consente alle applicazioni client di eseguire la crittografia dei dati sensibili senza mai rivelare i dati o le chiavi di crittografia a SQL Server o al database SQL di Azure. Un driver abilitato per Always Encrypted, come il provider di dati .NET Framework per SQL Server, fa tutto questo eseguendo in modo trasparente la crittografia e la decrittografia dei dati sensibili nell'applicazione client. Il driver determina automaticamente i parametri di query corrispondenti alle colonne di database con dati sensibili (protette mediante Always Encrypted) e crittografa i valori di tali parametri prima di passare i dati a SQL Server o al database SQL di Azure. Analogamente, il driver esegue in modo trasparente la decrittografia dei dati, recuperati dalle colonne di database crittografate nei risultati delle query. Per altre informazioni, vedere [Sviluppare applicazioni usando Always Encrypted con enclave sicure](always-encrypted-enclaves-client-development.md).

## <a name="prerequisites"></a>Prerequisiti

- Configurare Always Encrypted nel database. Ciò implica il provisioning di chiavi Always Encrypted e l'impostazione della crittografia per le colonne di database selezionate. Se non è presente un database in cui Always Encrypted è configurato, seguire le istruzioni fornite nel blog di [introduzione a Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted).
- Se si usa Always Encrypted con enclave sicure, vedere [Sviluppare applicazioni usando Always Encrypted con enclave sicure](always-encrypted-enclaves-client-development.md) per altri prerequisiti.
- Verificare che nel computer di sviluppo sia installato .NET Framework versione 4.6.1 o successiva. Per informazioni dettagliate, vedere [.NET Framework 4.6](/dotnet/framework/). È necessario anche assicurarsi che .NET Framework versione 4.6 o successiva sia configurato come versione di .NET Framework di destinazione nell'ambiente di sviluppo. Se si usa Visual Studio, vedere [Procedura: Assegnare come destinazione una versione di .NET Framework](/visualstudio/ide/how-to-target-a-version-of-the-dotnet-framework). 

> [!NOTE]
> Il livello di supporto per Always Encrypted varia in base alle specifiche versioni di .NET Framework. Per informazioni dettagliate, vedere la sezione Riferimento all'API di Always Encrypted riportata di seguito.

## <a name="enabling-always-encrypted-for-application-queries"></a>Abilitazione di Always Encrypted per le query dell'applicazione
Il modo più semplice per abilitare la crittografia dei parametri e la decrittografia dei risultati delle query per le colonne crittografate consiste nell'impostare il valore della parola chiave della stringa di connessione Impostazione di crittografia di colonna su **Abilitata**.

Di seguito è riportato un esempio di stringa di connessione che abilita la Crittografia sempre attiva:

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

Di seguito è riportato invece un esempio equivalente che usa la proprietà SqlConnectionStringBuilder.ColumnEncryptionSetting.

```cs
SqlConnectionStringBuilder strbldr = new SqlConnectionStringBuilder();
strbldr.DataSource = "server63";
strbldr.InitialCatalog = "Clinic";
strbldr.IntegratedSecurity = true;
strbldr.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(strbldr.ConnectionString);
```

Always Encrypted può anche essere abilitato per le singole query. Vedere la sezione **Controllo dell'impatto di Always Encrypted sulle prestazioni** di seguito.
Perché la crittografia o la decrittografia abbiano esito positivo, non è sufficiente l'abilitazione di Always Encrypted. È necessario anche assicurarsi che:
- L'applicazione abbia le autorizzazioni di database *VIEW ANY COLUMN MASTER KEY DEFINITION* e *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , necessarie per accedere ai metadati sulle chiavi Always Encrypted nel database. Per informazioni dettagliate, vedere la [sezione Autorizzazioni in Always Encrypted (motore di database)](./always-encrypted-database-engine.md#database-permissions).
- L'applicazione può accedere alla chiave master della colonna che protegge le chiavi di crittografia di colonna, crittografando le colonne di database sottoposte a query.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Abilitare Always Encrypted con enclave sicuri

A partire da .NET Framework versione 4.7.2, il driver supporta [Always Encrypted con enclavi sicuri](always-encrypted-enclaves.md). 

Per informazioni generali sul ruolo del driver del client nei calcoli e nell'attestazione dell'enclave, vedere [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md). 

Per configurare l'applicazione:

1. Abilitare Always Encrypted per le query dell'applicazione, come illustrato nella sezione precedente.
2. Integrare il pacchetto NuGet [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders) con l'applicazione. Il pacchetto NuGet è una libreria di provider di enclave che implementa la logica lato client per i protocolli di attestazione e per stabilire un canale sicuro con un'enclave sicura.  
3. Aggiornare la configurazione dell'applicazione (ad esempio, in web.config o app.config) per definire il mapping tra un tipo di enclave, configurato per il database, e un provider di enclave. 
    1. Se si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host, è necessario eseguire il mapping del tipo di enclave con sicurezza basata su virtualizzazione alla classe Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider dal pacchetto NuGet.
    2. Se si usa il [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] e l'attestazione di Microsoft Azure, è necessario eseguire il mapping del tipo di enclave SGX alla classe Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider dal pacchetto NuGet.

    Per istruzioni dettagliate su come modificare la configurazione dell'applicazione, vedere [Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicure](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md).

4. Impostare la parola chiave `Enclave Attestation URL` nella stringa di connessione di database su un URL di attestazione (un endpoint del servizio di attestazione). È necessario ottenere un URL di attestazione per l'ambiente dall'amministratore del servizio di attestazione.
   1. Se si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host (HGS), vedere [Determinare e condividere l'URL di attestazione HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
   2. Se si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] e il servizio Attestazione di Microsoft Azure, vedere [Determinare l'URL di attestazione per i criteri di attestazione](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation).

Per un'esercitazione dettagliata, vedere [Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recupero e modifica di dati nelle colonne crittografate

Dopo aver abilitato Always Encrypted per le query dell'applicazione, è possibile usare API ADO.NET standard (vedere [Recupero e modifica dei dati in ADO.NET](/dotnet/framework/data/adonet/retrieving-and-modifying-data)) o le API del [provider di dati .NET Framework per SQL Server](/dotnet/framework/data/adonet/sql/) , definite in [System.Data.SqlClient Namespace](/dotnet/api/system.data.sqlclient), per recuperare o modificare dati nelle colonne di database crittografate. Supponendo che l'applicazione abbia le autorizzazioni per il database necessarie e possa accedere alla chiave master della colonna, il provider di dati .NET Framework per SQL Server eseguirà la crittografia di eventuali parametri di query destinati a colonne crittografate ed eseguirà la decrittografia dei dati recuperati dalle colonne che restituiscono valori di testo non crittografato di tipi .NET, corrispondenti ai tipi di dati di SQL Server impostati per le colonne nello schema del database.
Se Always Encrypted non è abilitato, le query con parametri destinati alle colonne crittografate avranno esito negativo. Le query possono comunque recuperare dati dalle colonne crittografate, a condizione che non presentino parametri destinati alle colonne crittografate. Il provider di dati .NET Framework per SQL Server, tuttavia, non proverà a decrittografare tutti i valori recuperati dalle colonne crittografate e l'applicazione riceverà dati crittografati binari (come matrici di byte).

La tabella seguente riepiloga il comportamento delle query, a seconda che Always Encrypted sia abilitato o meno:

|Caratteristica della query | Always Encrypted è abilitato e l'applicazione può accedere alle chiavi e ai metadati delle chiavi|Always Encrypted è abilitato e l'applicazione non può accedere alle chiavi o ai metadati delle chiavi | Always Encrypted è disabilitato|
|:---|:---|:---|:---|
| Query con parametri destinati alle colonne crittografate. | I valori dei parametri vengono crittografati in modo trasparente. | Errore | Errore|
| Query che recuperano dati dalle colonne crittografate, senza parametri destinati alle colonne crittografate.| I risultati delle colonne vengono decrittografati in modo trasparente. L'applicazione riceve valori di testo non crittografato dei tipi di dati .NET corrispondenti ai tipi di SQL Server configurati per le colonne crittografate. | Errore | I risultati delle colonne crittografate non vengono decrittografati. L'applicazione riceve i valori crittografati come matrici di byte (byte[]). 

Gli esempi seguenti illustrano il recupero e la modifica dei dati nelle colonne crittografate. Negli esempi si presuppone la presenza della tabella di destinazione con lo schema seguente. Le colonne SSN e BirthDate sono crittografate.


```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1), 
 [SSN] [char](11) COLLATE Latin1_General_BIN2 
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC, 
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', 
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL, 
 [BirthDate] [date] 
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED, 
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', 
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="inserting-data-example"></a>Esempio di inserimento di dati

Questo esempio illustra come inserire una riga nella tabella Patients. Tenere presente quanto segue:
- Il codice di esempio non contiene alcun elemento specifico per la crittografia. Il provider di dati .NET Framework per SQL Server rileva automaticamente e crittografa i parametri *paramSSN* e *paramBirthdate* destinati alle colonne crittografate. In questo modo la crittografia diventa trasparente per l'applicazione. 
- I valori inseriti nelle colonne di database, incluse quelle crittografate, vengono passati come oggetti [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) . L'uso di **SqlParameter** è facoltativo quando si inviano i valori alle colonne non crittografate (è tuttavia consigliabile usarlo perché consente di impedire attacchi SQL injection), mentre è necessario per i valori destinati alle colonne crittografate. Se i valori inseriti nelle colonne SSN o BirthDate sono stati passati come valori letterali incorporati nell'istruzione della query, la query avrà esito negativo perché il provider di dati .NET Framework per SQL Server non riesce a determinare i valori delle colonne di destinazione crittografate e quindi non crittograferà i valori. Di conseguenza, il server li rifiuterà come incompatibili con le colonne crittografate.
- Il tipo di dati del parametro destinato alla colonna SSN è impostato su una stringa ANSI (non Unicode), che esegue il mapping al tipo di dati di SQL Server char/varchar. Se il tipo del parametro è stato impostato su una stringa Unicode (stringa), che esegue il mapping a nchar/nvarchar, la query avrà esito negativo perché Always Encrypted non supporta le conversioni da valori nchar/nvarchar crittografati a valori char/varchar crittografati. Vedere [Mapping dei tipi di dati SQL Server](/dotnet/framework/data/adonet/sql-server-data-type-mappings) per informazioni sui mapping dei tipi di dati.
- Il tipo di dati del parametro inserito nella colonna BirthDate è impostato in modo esplicito sul tipo di dati SQL Server di destinazione tramite la [proprietà SqlParameter.SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), anziché mediante il mapping implicito dei tipi .NET ai tipi di dati di SQL Server applicati quando si usa la [proprietà SqlParameter.DbType](/dotnet/api/system.data.sqlclient.sqlparameter.dbtype). Per impostazione predefinita, viene eseguito il mapping della [struttura DateTime](/dotnet/api/system.datetime) al tipo di dati datetime di SQL Server. Considerato che il tipo di dati della colonna BirthDate corrisponde alla data e Always Encrypted non supporta una conversione dei valori di data e ora crittografati in valori di data crittografati, l'uso del mapping predefinito causerà un errore. 

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
{
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

      SqlParameter paramSSN = cmd.CreateParameter();
      paramSSN.ParameterName = @"@SSN";
      paramSSN.DbType = DbType.AnsiStringFixedLength;
      paramSSN.Direction = ParameterDirection.Input;
      paramSSN.Value = "795-73-9838";
      paramSSN.Size = 11;
      cmd.Parameters.Add(paramSSN);

      SqlParameter paramFirstName = cmd.CreateParameter();
      paramFirstName.ParameterName = @"@FirstName";
      paramFirstName.DbType = DbType.String;
      paramFirstName.Direction = ParameterDirection.Input;
      paramFirstName.Value = "Catherine";
      paramFirstName.Size = 50;
      cmd.Parameters.Add(paramFirstName);

      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);

      SqlParameter paramBirthdate = cmd.CreateParameter();
      paramBirthdate.ParameterName = @"@BirthDate";
      paramBirthdate.SqlDbType = SqlDbType.Date;
      paramBirthdate.Direction = ParameterDirection.Input;
      paramBirthdate.Value = new DateTime(1996, 09, 10);
      cmd.Parameters.Add(paramBirthdate);

      cmd.ExecuteNonQuery();
   } 
}
```

### <a name="retrieving-plaintext-data-example"></a>Esempio di recupero di dati di testo non crittografato

L'esempio seguente illustra come filtrare i dati in base ai valori crittografati e recuperare i dati in testo non crittografato dalle colonne crittografate. Tenere presente quanto segue:
- Il valore usato nella clausola WHERE per filtrare la colonna SSN deve essere passato usando SqlParameter, in modo che il provider di dati .NET Framework per SQL Server possibile codificarli in modo trasparente prima dell'invio al database.
- Tutti i valori stampati dal programma saranno in testo non crittografato perché il provider di dati .NET Framework per SQL Server decrittografa in modo trasparente i dati recuperati dalle colonne SSN e BirthDate.


> [!NOTE]
> Le query possono eseguire confronti di uguaglianza nelle colonne se sono crittografate tramite crittografia deterministica. Per altre informazioni, vedere [Selezione della crittografia deterministica o casuale](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
    
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
 {
    using (SqlCommand cmd = connection.CreateCommand())
 {

 cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";
 SqlParameter paramSSN = cmd.CreateParameter();
 paramSSN.ParameterName = @"@SSN";
 paramSSN.DbType = DbType.AnsiStringFixedLength;
 paramSSN.Direction = ParameterDirection.Input;
 paramSSN.Value = "795-73-9838";
 paramSSN.Size = 11;
 cmd.Parameters.Add(paramSSN);
 using (SqlDataReader reader = cmd.ExecuteReader())
 {
   if (reader.HasRows)
 {
 while (reader.Read())
 {
    Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
 }
```

### <a name="retrieving-encrypted-data-example"></a>Esempio di recupero di dati crittografati

Se Always Encrypted non è abilitato, una query può comunque recuperare dati dalle colonne crittografate, a condizione che non presenti parametri destinati alle colonne crittografate.

L'esempio seguente illustra come recuperare i dati crittografati binari dalle colonne crittografate. Tenere presente quanto segue:

- Considerato che Always Encrypted non è abilitato nella stringa di connessione, la query restituirà i valori crittografati di SSN e BirthDate come matrici di byte (il programma converte i valori in stringhe).
- Una query che recupera dati dalle colonne crittografate con Always Encrypted disabilitato può avere parametri, a condizione che nessuno dei parametri sia destinato a una colonna crittografata. La query precedente filtra in base alla colonna LastName, non è crittografata nel database. Se la query avesse filtrato per SSN o data di nascita, avrebbe avuto esito negativo.


```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
                
using (SqlConnection connection = new SqlConnection(connectionString))
{
   connection.Open();
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";
      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);
      using (SqlDataReader reader = cmd.ExecuteReader())
      {
         if (reader.HasRows)
         {
            while (reader.Read())
         {
         Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
      }
   }
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Come evitare i problemi comuni quando si eseguono query su colonne crittografate

Questa sezione descrive le categorie di errori che si verificano con maggiore frequenza quando si eseguono query su colonne crittografate da applicazioni .NET e fornisce alcune linee guida su come evitare il problema.

### <a name="unsupported-data-type-conversion-errors"></a>Errori di conversione dei tipi di dati non supportati

Always Encrypted supporta alcune conversioni per i tipi di dati crittografati. Per l'elenco dettagliato delle conversioni di tipi supportate, vedere [Always Encrypted](always-encrypted-database-engine.md). Per evitare errori di conversione dei tipi di dati, eseguire le operazioni seguenti:

- Impostare i tipi di parametri destinati alle colonne crittografate in modo che il tipo di dati di SQL Server del parametro corrisponda esattamente al tipo della colonna di destinazione oppure che sia supportata una conversione del tipo di dati di SQL Server del parametro nel tipo di destinazione della colonna. È possibile applicare il mapping desiderato dei tipi di dati .NET a specifici tipi di dati di SQL Server usando la proprietà SqlParameter.SqlDbType.
- Verificare che la precisione e la scala dei parametri destinati alle colonne dei tipi di dati di SQL Server decimali e numerici siano uguali a quelle configurate per la colonna di destinazione.  
- Verificare che la precisione dei parametri destinati alle colonne dei tipi di dati di SQL Server datetime2, datetimeoffset o time non sia maggiore della precisione per la colonna di destinazione, nelle query che modificano i valori di tale colonna.  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errori causati dal passaggio di testo non crittografato anziché di valori crittografati

Qualsiasi valore destinato a una colonna crittografata deve essere crittografato all'interno dell'applicazione. Il tentativo di inserire, modificare o filtrare in base a un valore di testo non crittografato su una colonna crittografata comporterà un errore simile al seguente:


```cs
System.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Per evitare tali errori, verificare che:
- Always Encrypted sia abilitato per le query dell'applicazione destinate alle colonne crittografate (per la stringa di connessione o nell'oggetto [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) per una query specifica).
- Per inviare i dati destinati alle colonne crittografate venga usato SqlParameter. L'esempio seguente illustra una query che filtra in modo errato in base a un valore letterale o costante in una colonna crittografata (SSN), anziché passare il valore letterale all'interno di un oggetto SqlParameter. 


```cs
using (SqlCommand cmd = connection.CreateCommand())
{
   cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>Uso degli archivi delle chiavi master delle colonne

Per crittografare un valore di parametro o decrittografare i dati nei risultati della query, il provider di dati .NET Framework per SQL Server deve ottenere una chiave di crittografia di colonna che è configurata per la colonna di destinazione. Le chiavi di crittografia di colonna vengono archiviate in forma crittografata nei metadati del database. Ogni chiave di crittografia di colonna ha una chiave master corrispondente che è stata usata per crittografare la chiave di crittografia. I metadati del database non archiviano le chiavi master delle colonne, ma contengono solo le informazioni su un archivio delle chiavi contenente una specifica chiave master di colonna e il percorso della chiave nell'archivio.

Per ottenere un valore di testo non crittografato nella chiave di crittografia della colonna, il provider di dati .NET Framework per SQL Server ottiene prima di tutto i metadati relativi sia alla chiave di crittografia della colonna che alla chiave master della colonna corrispondente e quindi usa le informazioni nei metadati per contattare l'archivio delle chiavi, che contiene la chiave master della colonna, e per decrittografare la chiave di crittografia della colonna crittografata. Il provider di dati .NET Framework per SQL Server comunica con un archivio delle chiavi usando un provider di archivio delle chiavi master delle colonne, ossia un'istanza di una classe derivata dalla [classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider).


Il processo per ottenere la chiave di crittografia di una colonna consiste nelle fasi seguenti:

1.  Se Always Encrypted è abilitato per una query, il provider di dati .NET Framework per SQL Server chiama **sys.sp_describe_parameter_encryption** in modo trasparente per recuperare i metadati di crittografia per i parametri destinati alle colonne crittografate, se la query include parametri. Per i dati crittografati contenuti nei risultati di una query, SQL Server associa automaticamente i metadati di crittografia. Le informazioni sulla chiave master della colonna includono:
    - Il nome di un provider di archivio delle chiavi che incapsula un archivio contenente la chiave master della colonna. 
    - Il percorso che specifica la posizione della chiave master della colonna nell'archivio delle chiavi.
    
    Le informazioni sulla chiave di crittografia della colonna includono:

    - Il valore crittografato della chiave di crittografia della colonna.
    - Il nome dell'algoritmo usato per crittografare la chiave di crittografia della colonna.
2.  Il provider di dati .NET Framework per SQL Server usa il nome del provider di archivio delle chiavi master delle colonne per cercare l'oggetto provider (un'istanza di una classe derivata dalla classe SqlColumnEncryptionKeyStoreProvider) in una struttura di dati interna.
3.  Per decrittografare la chiave di crittografia della colonna, il provider di dati .NET Framework per SQL Server chiama il metodo SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey, passando il percorso della chiave master della colonna, il valore crittografato della chiave di crittografia della colonna e il nome dell'algoritmo di crittografia, usato per generare la chiave di crittografia della colonna crittografata.




### <a name="using-built-in-column-master-key-store-providers"></a>Uso dei provider predefiniti di archivio delle chiavi master delle colonne

Il provider di dati .NET Framework per SQL Server viene fornito con i seguenti provider predefiniti di archivio delle chiavi master delle colonne, che sono pre-registrati con i nomi di provider specifici (usati per cercare il provider).


| Classe | Descrizione | Nome del provider (ricerca) |
|:---|:---|:---|
|Classe SqlColumnEncryptionCertificateStoreProvider| Un provider per l'archivio certificati di Windows. | MSSQL_CERTIFICATE_STORE |
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) <br><br>**Nota:** questo provider è disponibile in .NET Framework 4.6.1 e versioni successive. |Un provider di un archivio delle chiavi che supporta l'[API Cryptography Next Generation (CNG) Microsoft](/windows/win32/seccng/cng-portal). In genere, un archivio di questo tipo è un modulo di protezione hardware, ovvero un dispositivo fisico che protegge e gestisce le chiavi digitali e fornisce l'elaborazione della crittografia.  | MSSQL_CNG_STORE|
| [Classe SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)<br><br>**Nota:** questo provider è disponibile in .NET Framework 4.6.1 o versioni successive.| Un provider di un archivio delle chiavi che supporta [Microsoft CryptoAPI (CAPI)](/previous-versions/visualstudio/aa266944(v=vs.60)). In genere, un archivio di questo tipo è un modulo di protezione hardware, ovvero un dispositivo fisico che protegge e gestisce le chiavi digitali e fornisce l'elaborazione della crittografia.| MSSQL_CSP_PROVIDER |
  
Per usare questi provider non è necessario apportare alcuna modifica del codice dell'applicazione, ma tenere presente quanto segue:

- È necessario che l'utente (o l'amministratore del database) verifichi che il nome del provider, configurato nei metadati della chiave master della colonna, sia corretto e che il percorso della chiave master sia conforme al formato del percorso della chiave valido per un determinato provider. È consigliabile configurare le chiavi usando strumenti come SQL Server Management Studio, che genera automaticamente i nomi di provider e i percorsi di chiave validi quando viene eseguita l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) . Per altre informazioni, vedere [Configurare Always Encrypted usando SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) e [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).
- Verificare che l'applicazione possa accedere alla chiave nell'archivio chiavi. Questa operazione potrebbe comportare l'accesso da parte dell'applicazione alla chiave e/o all'archivio delle chiavi, a seconda dell'archivio, o l'esecuzione di altri passaggi di configurazione specifici dell'archivio delle chiavi. Ad esempio, per accedere a un archivio delle chiavi che implementa CNG o CAPI (come un modulo di protezione hardware), è necessario assicurarsi che una libreria di implementazione di CNG o CAPI per l'archivio sia installata nel computer dell'applicazione. Per informazioni dettagliate, vedere [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault-provider"></a>Uso del provider dell'insieme di credenziali delle chiavi di Azure

L'insieme di credenziali delle chiavi di Azure rappresenta una scelta valida per archiviare e gestire le chiavi master delle colonne per Always Encrypted, soprattutto se le applicazioni sono ospitate in Azure. Il provider di dati .NET Framework per SQL Server non include un provider predefinito di archivio delle chiavi master delle colonne per Azure Key Vault, ma è disponibile come pacchetto NuGet, che è possibile integrare facilmente con l'applicazione. Per informazioni dettagliate, vedere [Always Encrypted: Proteggere i dati sensibili nel database SQL con la crittografia dei dati e archiviare le chiavi di crittografia nell'insieme di credenziali delle chiavi di Azure](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implementazione di un provider personalizzato di archivio delle chiavi master delle colonne

Se si vuole archiviare le chiavi master delle colonne in un archivio delle chiavi che non è supportato da un provider esistente, è possibile implementare un provider personalizzato estendendo la [classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) e registrando il provider con il metodo [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders) .


```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
    {
        public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
        {
            // Logic for encrypting a column encrypted key.
        }
        public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
        {
            // Logic for decrypting a column encrypted key.
        }
    }  
    class Program
    {
        static void Main(string[] args)
        {
            Dictionary\<string, SqlColumnEncryptionKeyStoreProvider> providers =
               new Dictionary\<string, SqlColumnEncryptionKeyStoreProvider>();
            providers.Add("MY_CUSTOM_STORE", customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
            providers.Add(SqlColumnEncryptionCertificateStoreProvider.ProviderName, customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers); 
       // ...
        }

    }
```
 
### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Uso di provider di archivio delle chiavi master delle colonne per il provisioning di chiavi a livello di codice

Quando si accede a colonne crittografate, il provider di dati .NET Framework per SQL Server trova in modo trasparente e chiama il provider di archivio delle chiavi master delle colonne per decrittografare le chiavi di crittografia delle colonne. In genere, il codice dell'applicazione normale non chiama direttamente i provider di archivio delle chiavi master delle colonne. È tuttavia possibile creare un'istanza e chiamare un provider in modo esplicito a livello di codice e gestire le chiavi Always Encrypted per generare la chiave di crittografia di una colonna crittografata e decrittografare la chiave di crittografia di una colonna (ad esempio nell'ambito della rotazione della chiave master della colonna). Per altre informazioni, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
L'implementazione di strumenti di gestione delle chiavi personali può essere necessaria solo se si usa un provider di archivio delle chiavi personalizzato. Quando si usano chiavi archiviate in archivi delle chiavi, per i quali esistono provider predefiniti, e/o nell'insieme di credenziali delle chiavi di Azure, è possibile usare gli strumenti esistenti, ad esempio SQL Server Management Studio o PowerShell per gestire le chiavi ed eseguirne il provisioning.
L'esempio seguente illustra la generazione della chiave di crittografia di una colonna e l'uso della [classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) per crittografare la chiave con un certificato.


```cs
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey(); 
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", "")); 
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey); 
}
```


## <a name="controlling-performance-impact-of-always-encrypted"></a>Controllo dell'impatto di Always Encrypted sulle prestazioni

Considerato che Always Encrypted è una tecnologia di crittografia lato client, la maggior parte del sovraccarico delle prestazioni si verifica sul lato client, non nel database. A parte il costo delle operazioni di crittografia e decrittografia, le altre origini di sovraccarico delle prestazioni sul lato client sono le seguenti:
- Round trip aggiuntivi al database per recuperare i metadati per i parametri di query.
- Chiamate a un archivio delle chiavi master delle colonne per accedere alla chiave master di una colonna.

Questa sezione descrive le ottimizzazioni delle prestazioni predefinite nel provider .NET Framework per SQL Server e come controllare l'impatto sulle prestazioni esercitato dai due fattori descritti.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controllo dei round trip per recuperare i metadati per i parametri di query

Se Always Encrypted è abilitato per una connessione, per impostazione predefinita il provider di dati .NET Framework per SQL Server chiamerà [sys.sp_describe_parameter_encryption](../../system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) per ogni query con parametri, passando l'istruzione di query (senza i valori dei parametri) a SQL Server. **sys.sp_describe_parameter_encryption** analizza l'istruzione di query per verificare se sono presenti parametri da crittografare e, in tal caso, restituisce le informazioni relative alla crittografia che consentiranno al provider di dati .NET Framework per SQL Server di crittografare i valori dei parametri. Il comportamento descritto garantisce all'applicazione client un elevato livello di trasparenza. Non è necessario che l'applicazione (e lo sviluppatore dell'applicazione) sappiano quali query accedono alle colonne crittografate, a condizione che i valori destinati alle colonne crittografate vengano passati al provider di dati .NET Framework per SQL Server negli oggetti SqlParameter.


### <a name="query-metadata-caching"></a>Memorizzazione nella cache dei metadati di query

In .NET Framework 4.6.2 e versioni successive, il provider di dati .NET Framework per SQL Server memorizza nella cache i risultati di **sys.sp_describe_parameter_encryption** per ogni istruzione di query. Di conseguenza, se la stessa istruzione di query viene eseguita più volte, il driver chiama **sys.sp_describe_parameter_encryption** una sola volta. La memorizzazione nella cache dei metadati di crittografia per le istruzioni di query consente di ridurre in modo sostanziale l'impatto sulle prestazioni del recupero dei metadati dal database. La memorizzazione nella cache è abilitata per impostazione predefinita. È possibile disabilitare la memorizzazione nella cache dei metadati dei parametri impostando su false la [proprietà SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled), ma questa operazione non è consigliabile tranne in rari casi, come quello descritto di seguito:

Si consideri un database che ha due schemi diversi: s1 e s2. Ogni schema contiene una tabella con lo stesso nome: t. Le definizioni delle tabelle s1.t e s2.t sono identiche, ad eccezione delle proprietà relative alla crittografia: una colonna, denominata c, in s1.t non è crittografata ed è crittografata in s2.t. Il database ha due utenti: u1 e u2. Lo schema predefinito per l'utente u1 è s1, mentre quello per l'utente u2 è s2. Un'applicazione .NET apre due connessioni al database, rappresentando l'utente u1 su una connessione e l'utente u2 su un'altra connessione. L'applicazione invia una query con un parametro destinato alla colonna c tramite la connessione per l'utente u1. La query non specifica lo schema e quindi si presuppone che venga usato lo schema utente predefinito. Successivamente, l'applicazione invia la stessa query tramite la connessione per l'utente u2. Se la memorizzazione nella cache dei metadati di query è abilitata, dopo la prima query la cache verrà popolata con metadati indicanti che la colonna c, contenente le destinazioni dei parametri di query, non è crittografata. Poiché la seconda query ha la stessa istruzione di query, verranno usate le informazioni memorizzate nella cache. Di conseguenza, il driver invierà la query senza crittografare il parametro (anche se questo non è corretto perché la colonna di destinazione, s2.t.c, è crittografata), passando al server il valore di testo non crittografato del parametro. Il server rileverà l'incompatibilità e forzerà l'aggiornamento della cache. Di conseguenza, l'applicazione invierà nuovamente la query in modo trasparente con il valore del parametro correttamente crittografato. In tal caso, la memorizzazione nella cache deve essere disabilitata per impedire che valori sensibili vengano passati al server in modalità non crittografata. 




### <a name="setting-always-encrypted-at-the-query-level"></a>Impostazione di Always Encrypted a livello di query

Per controllare l'impatto sulle prestazioni del recupero dei metadati di crittografia per le query con parametri, è possibile abilitare Always Encrypted per singole query, anziché l'impostarla per la connessione. In questo modo, è possibile assicurarsi che **sys.sp_describe_parameter_encryption** venga richiamato solo per le query con parametri destinati alle colonne crittografate. Si noti, tuttavia, che in tal modo si riduce la trasparenza della crittografia: se si modificano le proprietà di crittografia delle colonne di database, potrebbe essere necessario modificare il codice dell'applicazione per allinearlo alle modifiche dello schema.

> [!NOTE]
> L'impostazione di Always Encrypted a livello di query offre vantaggi limitati per le prestazioni in .NET 4.6.2 e versioni successive, che implementano la memorizzazione nella cache dei metadati di crittografia dei parametri.

Per controllare il comportamento di Always Encrypted per le singole query, è necessario usare questo costruttore di [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) e [SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting). Di seguito sono riportate alcune linee guida utili:
- Se la maggior parte delle query che un'applicazione client invia alla connessione del database accede alle colonne crittografate:
    - Impostare la parola chiave della stringa di connessione di **Impostazione di crittografia di colonna** su *Abilitata*.
    - Impostare **SqlCommandColumnEncryptionSetting.Disabled** per le singole query che non accedono alle colonne crittografate. In questo modo verranno disabilitati sia la chiamata di sys.sp_describe_parameter_encryption chiamante che il tentativo di decrittografare tutti i valori nel set di risultati.
    - Impostare **SqlCommandColumnEncryptionSetting.ResultSet** per le singole query che non presentano parametri da crittografare, ma che recuperano i dati dalle colonne crittografate. In questo modo verranno disabilitati la chiamata di sys.sp_describe_parameter_encryption e la crittografia dei parametri. La query potrà decrittografare i risultati delle colonne di crittografia.
- Se la maggior parte delle query che un'applicazione client invia alla connessione del database non accede alle colonne crittografate:
    - Impostare la parola chiave della stringa di connessione di **Impostazione di crittografia di colonna** su **Disabilitata**.
    - Impostare **SqlCommandColumnEncryptionSetting.Enabled** per le singole query che presentano parametri da crittografare. In questo modo verranno abilitate sia la chiamata di sys.sp_describe_parameter_encryption che la decrittografia dei risultati di query recuperati da colonne crittografate.
    - Impostare **SqlCommandColumnEncryptionSetting.ResultSet** per le query che non presentano parametri da crittografare, ma che recuperano i dati dalle colonne crittografate. In questo modo verranno disabilitati la chiamata di sys.sp_describe_parameter_encryption e la crittografia dei parametri. La query potrà decrittografare i risultati delle colonne di crittografia.

Nell'esempio seguente Always Encrypted è disabilitato per la connessione al database. La query eseguita dall'applicazione ha un parametro la cui destinazione è la colonna LastName non crittografata. La query recupera i dati dalle colonne SSN e BirthDate, che sono entrambe crittografate. In tal caso, la chiamata di sys.sp_describe_parameter_encryption per recuperare i metadati di crittografia non è necessaria. È tuttavia necessario abilitare la decrittografia dei risultati della query in modo che l'applicazione possa ricevere i valori di testo non crittografato dalle due colonne crittografate. A tale scopo, usare l'impostazione SqlCommandColumnEncryptionSetting.ResultSet.



```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
    {
        SqlParameter paramLastName = cmd.CreateParameter();
        paramLastName.ParameterName = @"@LastName";
        paramLastName.DbType = DbType.String;
        paramLastName.Direction = ParameterDirection.Input;
        paramLastName.Value = "Abel";
        paramLastName.Size = 50;
        cmd.Parameters.Add(paramLastName);
        using (SqlDataReader reader = cmd.ExecuteReader())
            {
               if (reader.HasRows)
               {
                  while (reader.Read())
                  {
                     Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
                  }
               }
            }
  } 
}
```


### <a name="column-encryption-key-caching"></a>Memorizzazione nella cache delle chiavi di crittografia della colonna

Per ridurre il numero di chiamate a un archivio chiavi master della colonna per decrittografare le chiavi di crittografia della colonna, il provider di dati .NET Framework per SQL Server memorizza nella cache le chiavi di crittografia della colonna di testo non crittografato. Dopo aver ricevuto il valore delle chiavi di crittografia della colonna crittografata dai metadati del database, il driver prova prima di tutto a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente al valore della chiave crittografata. Il driver chiama l'archivio delle chiavi contenente la chiave master della colonna solo se non trova nella cache il valore della chiave di crittografia della colonna crittografata.

> [!NOTE]
> In .NET Framework 4.6 e 4.6.1, le voci delle chiavi di crittografia delle colonne nella cache non vengono mai rimosse. Questo significa che per una determinata chiave di crittografia della colonna crittografata specificata, il driver contatta l'archivio chiavi una sola volta nel corso della durata dell'applicazione.

In .NET Framework 4.6.2 e versioni successive, le voci della cache vengono rimosse per motivi di sicurezza dopo un intervallo di durata (TTL) configurabile. Il valore predefinito di questo intervallo è 2 ore. Se si preferisce impostare requisiti di sicurezza più severi riguardo al tempo in cui le chiavi di crittografia delle colonne possono rimanere memorizzate nella cache in testo non crittografato nell'applicazione, è possibile modificare il valore della durata (TTL) tramite la [proprietà SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl). 


## <a name="enabling-additional-protection-for-a-compromised-sql-server"></a>Abilitazione della protezione aggiuntiva per un'istanza di SQL Server compromessa

Per impostazione predefinita, il *provider di dati .NET Framework per SQL Server* si basa sul sistema di database (SQL Server o database SQL di Azure) per fornire i metadati sulle colonne del database crittografate e sulla modalità di crittografia usata. I metadati di crittografia consentono al provider di dati .NET Framework per SQL Server di crittografare i parametri di query e decrittografare i risultati di query senza alcun intervento da parte dell'applicazione, riducendo così notevolmente il numero di modifiche necessarie nell'applicazione. Se tuttavia il processo di SQL Server è compromesso e un utente malintenzionato manomette i metadati inviati da SQL Server al provider di dati .NET Framework per SQL Server, tale utente potrebbe essere in grado di sottrarre informazioni sensibili. Questa sezione descrive le API che consentono di fornire un ulteriore livello di protezione da questo tipo di attacco, anche se a discapito della trasparenza. 

### <a name="forcing-parameter-encryption"></a>Forzare la crittografia dei parametri 

Prima di inviare una query con parametri a SQL Server, il provider di dati .NET Framework per SQL Server chiede a SQL Server (chiamando [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)) di analizzare l'istruzione di query e di fornire informazioni sui parametri della query da crittografare. Un'istanza di SQL Server compromessa può fuorviare il provider di dati .NET Framework per SQL Server inviando metadati indicanti che il parametro non è destinato a una colonna crittografata, anche se questa è crittografata nel database. Di conseguenza, il provider di dati .NET Framework per SQL Server non crittograferà il valore del parametro e lo invierà come testo non crittografato all'istanza di SQL Server compromessa.

Per evitare un attacco di questo tipo, un'applicazione può impostare su true la [proprietà SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption) del parametro. In questo modo, il provider di dati .NET Framework per SQL Server genererà un'eccezione, se i metadati ricevuti dal server indicano che il parametro non deve essere crittografato.

Sebbene l'uso della **proprietà SqlParameter.ForceColumnEncryption** consenta di migliorare la sicurezza, ha l'effetto di ridurre la trasparenza della crittografia per l'applicazione client. Se si aggiorna lo schema del database per modificare il set di colonne crittografate, può essere necessario apportare modifiche anche all'applicazione.

L'esempio di codice seguente illustra l'uso della **proprietà SqlParameter.ForceColumnEncryption** per impedire che i numeri SSN vengano inviati in testo non crittografato al database. 



```cs
SqlCommand cmd = _sqlconn.CreateCommand(); 

// Use parameterized queries to access Always Encrypted data. 
 
cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;"; 

SqlParameter paramSSN = cmd.CreateParameter(); 
paramSSN.ParameterName = @"@SSN"; 
paramSSN.DbType = DbType.AnsiStringFixedLength; 
paramSSN.Direction = ParameterDirection.Input; 
paramSSN.Value = ssn; 
paramSSN.Size = 11; 
paramSSN.ForceColumnEncryption = true; 
cmd.Parameters.Add(paramSSN); 

SqlDataReader reader = cmd.ExecuteReader();
```
 

### <a name="configuring-trusted-column-master-key-paths"></a>Configurazione di percorsi attendibili per le chiavi master delle colonne 

I metadati di crittografia, restituiti da SQL Server per i parametri di query destinati alle colonne crittografate e per i risultati recuperati dalle colonne di crittografia, includono il percorso della chiave master della colonna che identifica l'archivio delle chiavi e la posizione della chiave nell'archivio. Se l'istanza di SQL Server è compromessa, può inviare il percorso della chiave che indirizza il provider di dati .NET Framework per SQL Server verso la posizione controllata da un utente malintenzionato. Ciò può causare una perdita di credenziali dell'archivio delle chiavi, nel caso in cui l'archivio richieda l'autenticazione dell'applicazione. 

Per impedire attacchi di questo tipo, l'applicazione può specificare l'elenco dei percorsi di chiave attendibili per un determinato server usando la [proprietà SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths). Se il provider di dati .NET Framework per SQL Server riceve un percorso di chiave non incluso nell'elenco dei percorsi di chiave attendibili, verrà generata un'eccezione. 

Anche se l'impostazione di percorsi di chiave attendibili consente di migliorare la sicurezza dell'applicazione, è necessario modificare il codice o/e la configurazione dell'applicazione ogni volta che si ruota la chiave master della colonna, ossia ogni volta che il percorso della chiave master della colonna cambia. 

L'esempio seguente mostra come configurare i percorsi attendibili delle chiavi master delle colonne:


```cs
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance 
// First, create a list of trusted key paths for your server 
List<string> trustedKeyPathList = new List<string>(); 
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea"); 

// Register the trusted key path list for your server 

SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);
```
 



## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>Copia dei dati crittografati mediante SqlBulkCopy

Con SqlBulkCopy, è possibile copiare dati, che sono già crittografati e archiviati in una tabella, in un'altra tabella, senza la decrittografia dei dati. A tale scopo, eseguire queste operazioni:

- Assicurarsi che la configurazione di crittografia della tabella di destinazione sia identica alla configurazione della tabella di origine. In particolare, entrambe le tabelle devono avere le stesse colonne crittografate e le colonne devono essere crittografate usando gli stessi tipi di crittografia e le stesse chiavi di crittografia. Nota: se una o più colonne di destinazione sono crittografate in modo diverso dalla relativa colonna di origine corrispondente, non sarà possibile decrittografare i dati nella tabella di destinazione dopo l'operazione di copia. I dati risulteranno danneggiati.
- Configurare entrambe le connessioni di database, per la tabella di origine e la tabella di destinazione, senza Always Encrypted abilitato. 
- Impostare l'opzione AllowEncryptedValueModifications (vedere [SqlBulkCopyOptions](/dotnet/api/system.data.sqlclient.sqlbulkcopyoptions)). Nota: prestare attenzione quando si specifica AllowEncryptedValueModifications; questa operazione può causare, infatti, il danneggiamento del database perché il provider di dati .NET Framework per SQL Server non verifica se i dati vengono effettivamente crittografati o se vengono crittografati correttamente usando lo stesso tipo di crittografia, l'algoritmo e la chiave come colonna di destinazione.

L'opzione AllowEncryptedValueModifications è disponibile in .NET Framework 4.6.1 e versioni successive.

Di seguito è riportato un esempio che copia i dati da una tabella a altra. Si presuppone che le colonne SSN e BirthDate nascita siano crittografate.
        

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
   string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
   string targetConnectionString = "Data Source= server64; Initial Catalog=Clinic; Integrated Security=true";
   using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
   {
      connSource.Open();
      using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
      {
         using (SqlDataReader reader = cmd.ExecuteReader())
         {
            SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications);
            copy.EnableStreaming = true;
            copy.DestinationTableName = targetTable;
            copy.WriteToServer(reader);
         }
      }
}
```


## <a name="always-encrypted-api-reference"></a>Riferimento all'API di Always Encrypted

**Spazio dei nomi:** [System.Data.SqlClient](/dotnet/api/system.data.sqlclient)

**Assembly:** System.Data (in System.Data.dll)




|Nome|Descrizione|Introdotta nella versione di .NET
|:---|:---|:---
|[Classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|Un provider di archivi di chiavi per l'archivio certificati di Windows.|  4.6
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)|Un provider di archivi di chiavi per l'API di crittografia di Microsoft: Next Generation (CNG).|  4.6.1
|[Classe SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)|Un provider di archivio di chiavi CAPI di Microsoft sulla base dei provider di servizi di crittografia (CSP).|4.6.1  
|[classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|Classe base per tutti i provider di archivi di chiavi.|  4.6
|[Enumerazione SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting)|Impostazioni per abilitare la crittografia e la decrittografia per una connessione al database.|4.6  
|[Enumerazione SqlConnectionColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlconnectioncolumnencryptionsetting)|Impostazioni per controllare il comportamento della Crittografia sempre attiva per ogni singola query.|4.6  
| [Proprietà SqlConnectionStringBuilder.ColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|Ottiene e imposta Always Encrypted nella stringa di connessione.|4.6|
| [proprietà SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) | Abilita e disabilita la memorizzazione nella cache dei metadati di crittografia delle query. | 4.6.2
| [proprietà SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl) | Ottiene e imposta la durata (TTL) per le voci nella cache delle chiavi di crittografia delle colonne. | 4.6.2
|[proprietà SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|Consente di impostare un elenco di percorsi principali attendibili per un server di database. Se durante l'elaborazione di una query dell'applicazione il driver riceve un percorso chiave non presente nell'elenco, la query non riesce. Questa proprietà fornisce un’ulteriore protezione da attacchi alla sicurezza in cui un SQL Server compromesso fornisce falsi percorsi chiavi. Questo potrebbe causare la perdita delle credenziali dell’archivio di chiavi.|  4.6
|[Metodo SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|Consente di registrare provider personalizzati per gli archivi delle chiavi. È un dizionario che esegue il mapping dei nomi dei provider degli archivi delle chiavi per le implementazioni del provider dell’archivio delle chiavi.|  4.6
|[SqlCommand Constructor (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](https://msdn.microsoft.com/library/dn956511\(v=vs.110\).aspx)|Consente di controllare il comportamento della Crittografia sempre attiva per ogni singola query.|  4.6
|[proprietà SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption)|Applica la crittografia di un parametro. Se SQL Server indica al driver che il parametro non deve essere crittografato, la query che usa il parametro avrà esito negativo. Questa proprietà fornisce protezione aggiuntiva contro attacchi alla sicurezza in cui un SQL Server compromesso fornisce al client metadati di crittografia non corretti, con conseguente rischio di divulgazione dei dati.|4.6  
|Nuova parola chiave della [stringa di connessione](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring) : `Column Encryption Setting=enabled`|Abilita o disabilita la funzionalità Crittografia sempre attiva per la connessione.| 4.6 
  

## <a name="see-also"></a>Vedere anche

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Blog sulla Crittografia sempre attiva](/archive/blogs/sqlsecurity/getting-started-with-always-encrypted)
- [Esercitazione su database SQL: Proteggere i dati sensibili con Always Encrypted](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
