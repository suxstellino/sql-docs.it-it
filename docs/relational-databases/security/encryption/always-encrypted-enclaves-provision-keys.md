---
description: Effettuare il provisioning delle chiavi abilitate per l'enclave
title: Effettuare il provisioning delle chiavi abilitate per l'enclave | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 8a2039b4444c69a35793cb248ceb4553a7c698ce
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107560948"
---
# <a name="provision-enclave-enabled-keys"></a>Effettuare il provisioning delle chiavi abilitate per l'enclave

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Questo articolo descrive come effettuare il provisioning di chiavi abilitate per l'enclave che supportano i calcoli all'interno di enclave sicuri lato server usati per [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md). 

Quando si effettua il provisioning di chiavi abilitate per l'enclave, si applicano le linee guida e i processi generali per la [gestione delle chiavi Always Encrypted](overview-of-key-management-for-always-encrypted.md). Questo articolo illustra dettagli specifici di Always Encrypted con enclave sicuri.

Per effettuare il provisioning di una chiave master di colonna abilitata per l'enclave usando SQL Server Management Studio o PowerShell, verificare che la nuova chiave supporti i calcoli nell'enclave. In questo modo lo strumento (SSMS o PowerShell) genererà l'istruzione `CREATE COLUMN MASTER KEY` che imposta `ENCLAVE_COMPUTATIONS` nei metadati della chiave master delle colonne nel database. Per altre informazioni, vedere [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). 

Lo strumento firmerà inoltre digitalmente le proprietà master della colonna con la chiave master della colonna e archivierà la firma nei metadati del database. La firma impedisce la manomissione dell'impostazione `ENCLAVE_COMPUTATIONS`. I driver client SQL verificano le firme prima di consentire l'uso dell'enclave. In questo modo gli amministratori della sicurezza hanno il controllo sui dati di colonna che possono essere calcolati all'interno dell'enclave.

`ENCLAVE_COMPUTATIONS` non è modificabile, ovvero non è possibile cambiarlo dopo aver definito la chiave master di colonna nei metadati. Per abilitare i calcoli nell'enclave usando una chiave di crittografia di colonna, crittografata da una determinata chiave master di colonna, è necessario ruotare la chiave master di colonna e sostituirla con una chiave master di colonna abilitata per l'enclave. Vedere [Ruotare le chiavi abilitate per l'enclave](always-encrypted-enclaves-rotate-keys.md).

> [!NOTE]
> Attualmente sia SSMS che PowerShell supportano le chiavi master di colonna abilitate per l'enclave archiviate in Azure Key Vault o nell'archivio certificati Windows. I moduli di protezione hardware (che usano CNG o CAPI) non sono supportati.

Per creare una chiave di crittografia di colonna abilitata per l'enclave, è necessario assicurarsi di selezionare una chiave master di colonna abilitata per l'enclave per crittografare la nuova chiave. 

Le sezioni seguenti forniscono altri dettagli su come effettuare il provisioning di chiavi abilitate per l'enclave usando SSMS e PowerShell.

## <a name="provision-enclave-enabled-keys-using-sql-server-management-studio"></a>Effettuare il provisioning di chiavi abilitate per l'enclave con SQL Server Management Studio
In SQL Server Management Studio è possibile effettuare il provisioning di:
- Una chiave master di colonna abilitata per l'enclave usando la finestra di dialogo **Nuova chiave master della colonna**.
- Una chiave di crittografia di colonna abilitata per l'enclave usando la finestra di dialogo **Nuova chiave di crittografia della colonna**.

> [!NOTE]
> La [procedura guidata Always Encrypted](always-encrypted-wizard.md) attualmente non supporta la generazione di chiavi abilitate per l'enclave. È tuttavia possibile creare chiavi abilitate per l'enclave usando prima le finestre di dialogo precedenti e quindi, quando si esegue la procedura guidata, selezionare una crittografia di colonna abilitata per l'enclave già esistente per le colonne da crittografare.

Requisiti minimi della versione di SSMS:

- SSMS 18.3 quando si usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 quando si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="provision-enclave-enabled-column-master-keys-with-the-new-column-master-key-dialog"></a>Effettuare il provisioning di chiavi master di colonna abilitate per l'enclave con la finestra di dialogo Nuova chiave master della colonna
Per effettuare il provisioning di una chiave master di colonna abilitata per l'enclave, seguire i passaggi in [Effettuare il provisioning delle chiavi master di colonna con la finestra di dialogo Nuova chiave master della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog). Assicurarsi di selezionare **Consenti calcoli enclave**. Vedere lo screenshot di seguito:

![Consenti calcoli enclave](./media/always-encrypted-enclaves/allow-enclave-computations.png)

> [!NOTE]
> La casella di controllo **Consenti calcoli enclave** viene visualizzata solo se è configurata un'enclave sicura per il database. Se si usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Configurare l'enclave sicura in SQL Server](always-encrypted-enclaves-configure-enclave-type.md). Se si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], vedere [Abilitare Intel SGX per il database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).

> [!TIP]
> Per verificare se una chiave master di colonna è abilitata per l'enclave, fare clic con il pulsante destro del mouse su di essa in Esplora oggetti e scegliere **Proprietà**. Se la chiave è abilitata per l'enclave, **Calcoli dell'enclave: Consentiti** viene visualizzato nella finestra che mostra le proprietà della chiave. In alternativa, è possibile usare la vista [sys.column_master_keys (Transact-SQL)](../../system-catalog-views/sys-column-master-keys-transact-sql.md).

### <a name="provision-enclave-enabled-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Effettuare il provisioning di chiavi di crittografia di colonna abilitate per l'enclave con la finestra di dialogo Nuova chiave di crittografia della colonna
Per effettuare il provisioning di una chiave di crittografia di colonna abilitata per l'enclave, seguire i passaggi in [Effettuare il provisioning delle chiavi di crittografia di colonna con la finestra di dialogo Nuova chiave di crittografia della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). Quando si seleziona una chiave master di colonna, assicurarsi che sia abilitata per l'enclave.

> [!TIP]
> Per verificare se una chiave di crittografia di colonna è abilitata per l'enclave, fare clic con il pulsante destro del mouse su di essa in Esplora oggetti e scegliere **Proprietà**. Se la chiave è abilitata per l'enclave, **Calcoli dell'enclave: Consentiti** viene visualizzato nella finestra che mostra le proprietà della chiave.

## <a name="provision-enclave-enabled-keys-using-powershell"></a>Effettuare il provisioning delle chiavi abilitate per l'enclave con PowerShell
Per effettuare il provisioning di chiavi abilitate per l'enclave usando PowerShell, è necessario il modulo di PowerShell SqlServer versione 21.1.18179 o successiva.

In generale, i flussi di lavoro di provisioning delle chiavi di PowerShell (con e senza separazione dei ruoli) per Always Encrypted, descritti in [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md) si applicano anche alle chiavi abilitate per enclave. In questa sezione vengono fornite informazioni dettagliate sulle chiavi abilitate per l'enclave.

Il modulo di PowerShell SqlServer estende i cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) e [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) con il parametro `-AllowEnclaveComputations` per consentire di specificare una chiave master di colonna abilitata per l'enclave durante il processo di provisioning. Entrambi i cmdlet creano un oggetto locale contenente le proprietà di una chiave master di colonna (archiviata in Azure Key Vault o nell'archivio certificati Windows). Se specificata, la proprietà `-AllowEnclaveComputations` contrassegna la chiave come abilitata per l'enclave nell'oggetto locale e fa anche in modo che il cmdlet acceda alla chiave master di colonna a cui si fa riferimento (in Azure Key Vault o nell'archivio certificati Windows) per firmare digitalmente le proprietà della chiave. Dopo aver creato un oggetto impostazioni per una nuova chiave master di colonna abilitata per l'enclave, è possibile usarlo in una chiamata successiva del cmdlet [**New-SqlColumnMasterKey**](/powershell/module/sqlserver/new-sqlcolumnmasterkey) per creare un oggetto metadati che descrive la nuova chiave nel database.

Il provisioning delle chiavi di crittografia di colonna abilitate per l'enclave non è diverso dal provisioning delle chiavi di crittografia di colonna non abilitate per l'enclave. È sufficiente assicurarsi che una chiave master di colonna usata per crittografare la nuova chiave di crittografia di colonna sia abilitata per l'enclave.

> [!NOTE]
> Il modulo di PowerShell SqlServer non supporta attualmente il provisioning di chiavi abilitate per l'enclave archiviate in moduli di protezione hardware (tramite CNG o CAPI).

### <a name="example---provision-enclave-enabled-keys-using-windows-certificate-store"></a>Esempio - Effettuare il provisioning di chiavi abilitate per l'enclave usando l'archivio certificati Windows
L'esempio end-to-end seguente illustra come effettuare il provisioning di chiavi abilitate per l'enclave, archiviando la chiave master di colonna archiviata nell'archivio certificati Windows. Lo script è basato sull'esempio illustrato in [Archivio certificati Windows senza separazione dei ruoli (esempio)](configure-always-encrypted-keys-using-powershell.md#windows-certificate-store-without-role-separation-example). È importante notare l'uso del parametro `-AllowEnclaveComputations` nel cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings), che è l'unica differenza tra i flussi di lavoro nei due esempi.

```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

### <a name="example---provision-enclave-enabled-keys-using-azure-key-vault"></a>Esempio - Effettuare il provisioning di chiavi abilitate per l'enclave usando Azure Key Vault
L'esempio end-to-end seguente illustra come effettuare il provisioning delle chiavi abilitate per l'enclave, archiviando la chiave master della colonna in un insieme di credenziali delle chiavi in Azure Key Vault. Lo script è basato sull'esempio illustrato in [Azure Key Vault senza separazione dei ruoli (esempio)](configure-always-encrypted-keys-using-powershell.md#azure-key-vault-without-role-separation-example). Esistono due importanti differenze tra il flusso di lavoro per le chiavi abilitate per l'enclave e quello per le chiavi non abilitate per l'enclave. 
- Nello script seguente [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) usa il parametro `-AllowEnclaveComputations` per abilitare per l'enclave la nuova chiave master di colonna. 
- Lo script seguente chiama il cmdlet [**Add-SqlAzureAuthenticationContext**](/powershell/module/sqlserver/add-sqlazureauthenticationcontext) per accedere ad Azure prima di chiamare il cmdlet [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings). Per prima cosa è necessario accedere ad Azure, perché il parametro `-AllowEnclaveComputations` fa in modo che **New-SqlAzureKeyVaultColumnMasterKeySettings** chiami Azure Key Vault per firmare le proprietà della chiave master di colonna.

```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure - it is required before calling the next cmdlet.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md)
- [Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Vedere anche  
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)