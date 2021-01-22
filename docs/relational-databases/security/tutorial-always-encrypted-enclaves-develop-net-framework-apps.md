---
description: "Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri"
title: "Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri | Microsoft Docs"
ms.custom: ''
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
ms.openlocfilehash: 092220a2ef2715b8d5cd414ab5a4bc3e3493acb5
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534740"
---
# <a name="tutorial-develop-a-net-framework-application-using-always-encrypted-with-secure-enclaves"></a>Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Questa esercitazione illustra come sviluppare un'applicazione che esegue query di database che usano un'enclave sicura sul lato server per [Always Encrypted con enclave sicure](encryption/always-encrypted-enclaves.md). 

## <a name="prerequisites"></a>Prerequisiti
Assicurarsi di aver completato una delle esercitazioni riportate di seguito prima di seguire le procedure in questa esercitazione:

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Sarà necessario anche Visual Studio (versione 2019 consigliata) che può essere scaricato da [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Il computer di sviluppo dell'applicazione deve eseguire.NET Framework 4.7.2 o versione successiva.

## <a name="step-1-set-up-your-visual-studio-project"></a>Passaggio 1: Configurare il progetto di Visual Studio

Per usare Always Encrypted con enclave sicuri in un'applicazione .NET Framework, è necessario assicurarsi che l'applicazione sia sviluppata con .NET Framework 4.7.2 e integrata con il [pacchetto NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders). Inoltre, se si archivia la chiave master della colonna in Azure Key Vault, è anche necessario integrare l'applicazione con il [pacchetto NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider), versione 2.2.0 o successiva. 

1. Aprire Visual Studio.

2. Creare un nuovo progetto di app console C\# (.NET Framework).

3. Assicurarsi che il progetto sia destinato almeno a .NET Framework 4.7.2. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere **Proprietà** e impostare **Framework di destinazione** su.NET Framework 4.7.2.

4. Installare il pacchetto NuGet seguente passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders -IncludePrerelease
   ```

5. Se si usa Azure Key Vault pe l'archiviazione delle chiavi master della colonna, installare i pacchetti NuGet seguenti passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -IncludePrerelease -Version 2.2.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Aprire il file App.config del progetto.

7. Individuare la sezione `<configuration>` e aggiungere o aggiornare le sezioni `<configSections>`.

   1. Se la sezione `<configuration>` **non** contiene la sezione `<configSections>`, aggiungere il contenuto seguente immediatamente sotto `<configuration>`.

      ```xml
      <configSections>
        <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </configSections>
      ```

   2. Se la sezione `<configuration>` contiene già la sezione `<configSections>`, aggiungere la riga seguente all'interno della sezione `<configSections>`:

      ```xml
      <section name="SqlColumnEncryptionEnclaveProviders"  type="System.   Data.SqlClient.   SqlColumnEncryptionEnclaveProviderConfigurationSection, System.   Data,  Version=4.0.0.0, Culture=neutral,    PublicKeyToken=b77a5c561934e089" />
      ```

8. Nella sezione `<configuration>`, sotto `</configSections>`, aggiungere una nuova sezione, che specifica un provider di enclave da usare per l'attestazione e l'interazione con l'enclave sicura lato server.

   1. Se si usa [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host (HGS) (si sta usando il database da [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)), aggiungere la sezione riportata di seguito.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Di seguito è riportato un esempio completo di un file app.config per una semplice applicazione console.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

   1. Se si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e il servizio Attestazione di Microsoft Azure (si usa il database da [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)), aggiungere la sezione riportata di seguito.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Di seguito è riportato un esempio completo di un file app.config per una semplice applicazione console.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

## <a name="step-2-implement-your-application-logic"></a>Passaggio 2: Implementare la logica dell'applicazione

L'applicazione si connetterà al database **ContosoHR** creato in [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md) o [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) ed eseguirà una query che contiene il predicato `LIKE` sulla colonna **SSN** e un confronto di intervalli sulla colonna **Salary**.

1. Sostituire il contenuto del file Program.cs (generato da Visual Studio) con il codice seguente. Aggiornare la stringa di connessione di database con il nome del server, le impostazioni di autenticazione del database e l'URL di attestazione dell'enclave per l'ambiente.

    ```cs
    using System;
    using System.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {
    
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();
    
                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }   
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Compilare ed eseguire l'applicazione.  

## <a name="see-also"></a>Vedere anche

- [Using Always Encrypted with .NET Framework Data Provider for SQL Server (Uso di Always Encrypted con il provider di dati .NET Framework per SQL Server)](encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
