---
description: "Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri"
title: "Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri | Microsoft Docs"
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 177737bd2927583bdfda1c9b36904faf4ed6023d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534700"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Questa esercitazione illustra come sviluppare un'applicazione che esegue query di database che usano un'enclave sicura sul lato server per [Always Encrypted con enclave sicure](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted con enclave sicuri è supportato solo in Windows.

## <a name="prerequisites"></a>Prerequisiti

Assicurarsi di aver completato una delle esercitazioni riportate di seguito prima di seguire le procedure in questa esercitazione:

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

È necessario anche Visual Studio (è consigliata la versione 2019), che può essere scaricato da [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). L'ambiente di sviluppo dell'applicazione deve usare .NET Framework 4.6 o versione successiva o .NET Core 2.1 o versione successiva.

## <a name="step-1-set-up-your-visual-studio-project"></a>Passaggio 1: Configurare il progetto di Visual Studio

Per usare Always Encrypted con enclave sicuri in un'applicazione .NET Framework è necessario verificare che l'applicazione abbia come destinazione .NET Framework 4.6 o versione successiva. Per usare Always Encrypted con enclave sicuri in un'applicazione .NET Core è necessario verificare che l'applicazione abbia come destinazione .NET Core 2.1 o versione successiva.

Se si archivia la chiave master della colonna in Azure Key Vault, è anche necessario integrare l'applicazione con [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Aprire Visual Studio.

2. Creare un nuovo progetto di app console C\# (.NET Framework/Core).

3. Assicurarsi che il progetto sia destinato almeno a .NET Framework 4.6 o .NET Core 2.1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, selezionare **Proprietà** e impostare il Framework di destinazione.

4. Installare il pacchetto NuGet seguente passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Se si usa Azure Key Vault pe l'archiviazione delle chiavi master della colonna, installare i pacchetti NuGet seguenti passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="step-2-implement-your-application-logic"></a>Passaggio 2: Implementare la logica dell'applicazione

L'applicazione si connetterà al database di **ContosoHR** da [esercitazione: Introduzione a always Encrypted con enclave sicure con SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) o- [esercitazione: introduzione always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) e verrà eseguita una query contenente il `LIKE` predicato nella colonna **SSN** e un confronto tra intervalli nella colonna **Salary** .

1. Sostituire il contenuto del file Program.cs (generato da Visual Studio) con il codice seguente. 

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                // Connection string for SQL Server
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                // Connection string for Azure SQL Database
                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = AAS; Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

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

2. Aggiornare la stringa di connessione del database.
    1. Impostare il nome del server e le impostazioni di autenticazione del database validi.
    2. Impostare il valore della `Attestation Protocol` parola chiave su:
       - `HGS` -Se si usa [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] e il servizio sorveglianza host (HGS).
       - `AAS` -Se si sta usando [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] e Microsoft Azure attestazione.
    3. Impostare `Enclave Attestation URL` su un URL di attestazione per l'ambiente.

3. Compilare ed eseguire l'applicazione.

## <a name="see-also"></a>Vedi anche

- [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](sqlclient-support-always-encrypted.md)
- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted con enclave sicuri](azure-key-vault-enclave-example.md)
