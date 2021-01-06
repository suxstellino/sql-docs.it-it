---
description: "Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri"
title: "Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri | Microsoft Docs"
ms.custom: ''
ms.date: 07/09/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 59c377d5055e8eb3858e1005c50d9c4dfb8334ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771552"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Questa esercitazione illustra come sviluppare una semplice applicazione che esegue query di database che usano un'enclave sicuro lato server per [Always Encrypted con enclave sicuri](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted con enclave sicuri è supportato solo in Windows.

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione è la continuazione di [Esercitazione: Introduzione ad Always Encrypted con enclave sicuri tramite SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Assicurarsi di averla completata prima eseguire le procedure successive.

È necessario anche Visual Studio (è consigliata la versione 2019), che può essere scaricato da [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). L'ambiente di sviluppo dell'applicazione deve usare .NET Framework 4.6 o versione successiva o .NET Core 2.1 o versione successiva.

## <a name="step-1-set-up-your-visual-studio-project"></a>Passaggio 1: Configurare il progetto di Visual Studio

Per usare Always Encrypted con enclave sicuri in un'applicazione .NET Framework è necessario verificare che l'applicazione abbia come destinazione .NET Framework 4.6 o versione successiva. Per usare Always Encrypted con enclave sicuri in un'applicazione .NET Core è necessario verificare che l'applicazione abbia come destinazione .NET Core 2.1 o versione successiva.

Se si archivia la chiave master della colonna in Azure Key Vault, è anche necessario integrare l'applicazione con [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Aprire Visual Studio.

2. Creare un nuovo progetto di app console C\# (.NET Framework/Core).

3. Assicurarsi che il progetto sia destinato almeno a .NET Framework 4.6 o .NET Core 2.1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere Proprietà e impostare Framework di destinazione.

4. Installare il pacchetto NuGet seguente passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Se si usa Azure Key Vault pe l'archiviazione delle chiavi master della colonna, installare i pacchetti NuGet seguenti passando a **Strumenti** (menu principale) > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Eseguire il codice seguente nella Console di Gestione pacchetti.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Specificare `Attestation Protocol` e `Enclave Attestation Url` nella stringa di connessione, che verrà usata nell'applicazione per comunicare con SQL Server.

  ```cs
   Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Column Encryption Setting = Enabled
   ```

## <a name="step-2-implement-your-application-logic"></a>Passaggio 2: Implementare la logica dell'applicazione

L'applicazione si connetterà al database **ContosoHR** creato in [Esercitazione: Introduzione ad Always Encrypted con enclave sicuri tramite SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) ed eseguirà una query che contiene il predicato `LIKE` sulla colonna **SSN** e un confronto di intervalli sulla colonna **Salary**.

1. Sostituire il contenuto del file Program.cs (generato da Visual Studio) con il codice seguente. Aggiornare la stringa di connessione di database con il nome del server e l'URL di attestazione dell'enclave per l'ambiente. È anche possibile aggiornare le impostazioni di autenticazione del database.

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

                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Compilare ed eseguire l'applicazione.

## <a name="see-also"></a>Vedi anche

- [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](sqlclient-support-always-encrypted.md)
- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted con enclave sicuri](azure-key-vault-enclave-example.md)
