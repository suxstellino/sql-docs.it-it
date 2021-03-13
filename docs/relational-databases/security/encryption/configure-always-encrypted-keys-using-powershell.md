---
title: Effettuare il provisioning di chiavi Always Encrypted con PowerShell | Microsoft Docs
description: Informazioni su come effettuare il provisioning delle chiavi per Always Encrypted usando il modulo PowerShell SqlServer per offrire il controllo dell'accesso alle chiavi di crittografia e al database.
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 3bdf8629-738c-489f-959b-2f5afdaf7d61
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4bb1e019f6f14d0a8525bb669cfa2502f870d254
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231529"
---
# <a name="provision-always-encrypted-keys-using-powershell"></a>Effettuare il provisioning di chiavi Always Encrypted con PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

    
In questo articolo sono descritti i passaggi per effettuare il provisioning delle chiavi per Always Encrypted usando il [modulo PowerShell SqlServer](../../../powershell/sql-server-powershell-provider.md). È possibile usare PowerShell per effettuare il provisioning di chiavi Always Encrypted [con e senza separazione dei ruoli](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#KeyManagementRoles)offrendo il controllo su coloro che hanno accesso alle chiavi di crittografia effettive nell'archivio chiavi e coloro che hanno accesso al database. 

Per una panoramica della gestione delle chiavi Always Encrypted, incluse alcune procedure consigliate, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
Per informazioni su come usare il modulo PowerShell SqlServer per Always Encrypted, vedere [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).


## <a name="key-provisioning-without-role-separation"></a><a name="KeyProvisionWithoutRoles"></a> Provisioning delle chiavi senza separazione dei ruoli

Il metodo di provisioning delle chiavi descritto in questa sezione non supporta la separazione dei ruoli tra amministratori della sicurezza e amministratori di database. Alcuni dei passaggi che seguono includono operazioni sulle chiavi fisiche e operazioni sui metadati delle chiavi. Questo metodo di provisioning delle chiavi è quindi consigliato per le organizzazioni che usano il modello DevOps oppure se il database è ospitato nel cloud e l'obiettivo principale consiste nel limitare l'accesso ai dati sensibili agli amministratori del cloud escludendo gli amministratori di database locali. Questo metodo non è consigliato nel caso in cui eventuali concorrenti includano amministratori di database oppure se gli amministratori di database non devono avere accesso ai dati sensibili.

Prima di eseguire passaggi che implicano l'accesso a chiavi di testo non crittografato o all'archivio chiavi (indicati nella colonna **Accede a chiavi di testo non crittografato/archivio chiavi** della tabella che segue), assicurarsi che l'ambiente PowerShell venga eseguito in un computer protetto diverso dal computer che ospita il database. Per altre informazioni, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).


Attività  |Articolo  |Accede alle chiavi in testo non crittografato o all'archivio delle chiavi  |Accede al database   
---------|---------|---------|---------
Passaggio 1. Creare una chiave master della colonna in un archivio chiavi.<br><br>**Nota:** il modulo PowerShell SqlServer non supporta questo passaggio. Per eseguire questa attività da una riga di comando, usare gli strumenti specifici dell'archivio chiavi selezionato. |[Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Sì | No     
Passaggio 2.  Avviare un ambiente PowerShell e importare il modulo PowerShell SqlServer.  |   [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   |    No    | No         
Passaggio 3.  Connettersi al server e al database.     |     [Connettersi a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase)    |    No     | Sì         
Passaggio 4.  Creare un oggetto *SqlColumnMasterKeySettings* che includa informazioni sul percorso della chiave master della colonna. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria Usare il cmdlet specifico dell'archivio chiavi.   |     [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)        |   No      | No         
Passaggio 5.  Creare i metadati relativi alla chiave master della colonna nel database.      |    [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Nota:** il cmdlet genera l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) per creare i metadati della chiave.|    No     |    Sì
Passaggio 6.  Eseguire l'autenticazione in Azure, se la chiave master della colonna è archiviata nell'insieme di credenziali delle chiavi di Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |  Sì   | No         
Passaggio 7.  Generare una nuova chiave di crittografia di colonna, crittografarla con la chiave master della colonna e creare i metadati della chiave di crittografia della colonna nel database.     |    [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Nota:** usare una variante del cmdlet che genera internamente e crittografa una chiave di crittografia della colonna.<br><br>**Nota:** il cmdlet genera l'istruzione [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) per creare i metadati della chiave.  | Sì | Sì
  

## <a name="windows-certificate-store-without-role-separation-example"></a>Archivio certificati Windows senza separazione dei ruoli (esempio)

Questo script è un esempio end-to-end di generazione di una chiave master della colonna che è un certificato nell'archivio certificati di Windows, di generazione e crittografia di una chiave di crittografia della colonna e di creazione dei metadati della chiave in un database di SQL Server. 


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

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings


# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="azure-key-vault-without-role-separation-example"></a>Insieme di credenziali delle chiavi di Azure senza separazione dei ruoli (esempio)

Questo script è un esempio end-to-end di provisioning e configurazione di un insieme di credenziali delle chiavi di Azure, di generazione di una chiave master della colonna nell'insieme di credenziali, di generazione e crittografia di una chiave di crittografia della colonna e di creazione dei metadati della chiave in un database SQL di Azure.


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
$akvKey = Add-AzKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database (Azure SQL database).
Import-Module "SqlServer"
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.Key.Kid

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Authenticate to Azure
Add-SqlAzureAuthenticationContext -Interactive

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="cngksp-without-role-separation-example"></a>CNG/Provider di archiviazione chiavi senza separazione dei ruoli (esempio)

Lo script che segue è un esempio end-to-end di generazione di una chiave master della colonna in un archivio chiavi che implementa l'API Cryptography Next Generation (CNG) generando e crittografando una chiave di crittografia della colonna e creando i metadati della chiave in un database di SQL Server.

L'esempio sfrutta l'archivio chiavi che usa il provider di archiviazione chiavi del software Microsoft. È possibile scegliere di modificare l'esempio per usare un altro archivio, ad esempio il modulo di protezione hardware. A tale scopo, è necessario assicurarsi che il provider di archiviazione chiavi (KSP) che implementa CNG per il dispositivo sia installato correttamente nel computer. Sostituire quindi `Microsoft Software Key Storage Provider` con il nome KSP del dispositivo.


```powershell
# Create a column master key in a key store that has a CNG provider, a.k.a key store provider (KSP).
$cngProviderName = "Microsoft Software Key Storage Provider" # If you have an HSM, you can use a KSP for your HSM instead of a Microsoft KSP
$cngAlgorithmName = "RSA"
$cngKeySize = 2048 # Recommended key size for Always Encrypted column master keys
$cngKeyName = "AlwaysEncryptedKey" # Name identifying your new key in the KSP
$cngProvider = New-Object System.Security.Cryptography.CngProvider($cngProviderName)
$cngKeyParams = New-Object System.Security.Cryptography.CngKeyCreationParameters
$cngKeyParams.provider = $cngProvider
$cngKeyParams.KeyCreationOptions = [System.Security.Cryptography.CngKeyCreationOptions]::OverwriteExistingKey
$keySizeProperty = New-Object System.Security.Cryptography.CngProperty("Length", [System.BitConverter]::GetBytes($cngKeySize), [System.Security.Cryptography.CngPropertyOptions]::None);
$cngKeyParams.Parameters.Add($keySizeProperty)
$cngAlgorithm = New-Object System.Security.Cryptography.CngAlgorithm($cngAlgorithmName)
$cngKey = [System.Security.Cryptography.CngKey]::Create($cngAlgorithm, $cngKeyName, $cngKeyParams)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCngColumnMasterKeySettings -CngProviderName $cngProviderName -KeyName $cngKeyName

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="key-provisioning-with-role-separation"></a><a name="KeyProvisionWithRoles"></a> Provisioning delle chiavi con separazione dei ruoli

In questa sezione sono elencati i passaggi per la configurazione della crittografia per i casi in cui gli amministratori della sicurezza non hanno accesso al database e gli amministratori di database non hanno accesso all'archivio chiavi o alle chiavi di testo non crittografato.


### <a name="security-administrator"></a>Amministratore della sicurezza

Prima di eseguire i passaggi che prevedono l'accesso alle chiavi di testo non crittografato o all'archivio chiavi (come indicato nella colonna **Accede alle chiavi di testo non crittografato o all'archivio chiavi** nella tabella che segue), assicurarsi che:
1.  l'ambiente di PowerShell venga eseguito in un computer protetto diverso da un computer che ospita il database.
2.  gli amministratori di database dell'organizzazione non abbiano accesso al computer (cosa che vanificherebbe lo scopo della separazione dei ruoli).

Per altre informazioni, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).


Attività  |Articolo  |Accede alle chiavi in testo non crittografato o all'archivio delle chiavi  |Accede al database  
---------|---------|---------|---------
Passaggio 1. Creare una chiave master della colonna in un archivio chiavi.<br><br>**Nota:** il modulo SqlServer non supporta questo passaggio. Per eseguire questa attività da una riga di comando, è necessario usare gli strumenti specifici del tipo di archivio chiavi.     | [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)  |    Sì    | No 
Passaggio 2.  Avviare una sessione di PowerShell e importare il modulo SqlServer.      |     [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule)     | No | No         
Passaggio 3.  Creare un oggetto *SqlColumnMasterKeySettings* che includa informazioni sul percorso della chiave master della colonna. *SqlColumnMasterKeySettings* è un oggetto presente in memoria (PowerShell). Usare il cmdlet specifico dell'archivio chiavi. |      [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)   | No         | No         
Passaggio 4.  Eseguire l'autenticazione in Azure, se la chiave master della colonna è archiviata nell'insieme di credenziali delle chiavi di Azure |    [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |Sì|No         
Passaggio 5.  Generare una chiave di crittografia della colonna, crittografarla con la chiave master della colonna per generare un valore crittografato della chiave di crittografia della colonna.     |   [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)     |    Sì    | No        
Passaggio 6.  Fornire il percorso della chiave master della colonna (nome del provider e percorso della chiave master della colonna) e un valore crittografato della chiave di crittografia della colonna all'amministratore di database.  | Vedere gli esempi seguenti.        |   No      | No         

### <a name="dba"></a>DBA 

Gli amministratori di database usano le informazioni ricevute dall'amministratore della sicurezza (passaggio 6 precedente) per creare e gestire i metadati della chiave Always Encrypted nel database.

Attività  |Articolo  |Accede alle chiavi di testo non crittografato  |Accede al database   
---------|---------|---------|---------
Passaggio 1.  Ottenere il percorso della chiave master della colonna e un valore crittografato della chiave di crittografia della colonna dall'amministratore della sicurezza. |Vedere gli esempi seguenti. | No | No
Passaggio 2.  Avviare un ambiente PowerShell e importare il modulo SqlServer.  | [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)  | No | No
Passaggio 3.  Connettersi al server e a un database. | [Connettersi a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
Passaggio 4.  Creare un oggetto SqlColumnMasterKeySettings che includa informazioni sul percorso della chiave master della colonna. SqlColumnMasterKeySettings è un oggetto presente in memoria. | New-SqlColumnMasterKeySettings | No | No
Passaggio 5. Creare i metadati relativi alla chiave master della colonna nel database | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br>**Nota:** il cmdlet genera l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) per creare i metadati della chiave master della colonna. | No | Sì
Passaggio 6. Creare i metadati della chiave di crittografia della colonna nel database. | New-SqlColumnEncryptionKey<br>**Nota:** gli amministratori di database usano una variazione del cmdlet che crea soltanto i metadati della chiave di crittografia della colonna.<br>Il cmdlet genera l'istruzione [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) per creare i metadati della chiave di crittografia della colonna. | No | Sì
  
## <a name="windows-certificate-store-with-role-separation-example"></a>Archivio certificati Windows con separazione dei ruoli (esempio)

### <a name="security-administrator"></a>Amministratore della sicurezza


```powershell
# Create a column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Generate a column encryption key, encrypt it with the column master key to produce an encrypted value of the column encryption key.
$encryptedValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $cmkSettings

# Share the location of the column master key and an encrypted value of the column encryption key with a DBA, via a CSV file on a share drive
$keyDataFile = "Z:\keydata.txt"
"KeyStoreProviderName, KeyPath, EncryptedValue" > $keyDataFile
$cmkSettings.KeyStoreProviderName + ", " + $cmkSettings.KeyPath + ", " + $encryptedValue >> $keyDataFile

# Read the key data back to verify
$keyData = Import-Csv $keyDataFile
$keyData.KeyStoreProviderName
$keyData.KeyPath
$keyData.EncryptedValue 
```

### <a name="dba"></a>DBA

```powershell
# Obtain the location of the column master key and the encrypted value of the column encryption key from your Security Administrator, via a CSV file on a share drive.
$keyDataFile = "Z:\keydata.txt"
$keyData = Import-Csv $keyDataFile

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $keyData.KeyStoreProviderName -KeyPath $keyData.KeyPath

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a  column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName -EncryptedValue $keyData.EncryptedValue
```  
 
## <a name="next-steps"></a>Passaggi successivi    
- [Configurare la crittografia delle colonne usando Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md)  
- [Ruotare le chiavi Always Encrypted con PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Vedere anche    
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)    
- [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
