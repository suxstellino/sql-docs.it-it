---
description: Ruotare le chiavi Always Encrypted con PowerShell
title: Ruotare le chiavi Always Encrypted con PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 646f225c1534ea06bd04bd5c452ab5e7e2858f51
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210950"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>Ruotare le chiavi Always Encrypted con PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

In questo articolo sono descritti i passaggi per ruotare le chiavi per Always Encrypted usando il modulo PowerShell SqlServer. Per informazioni su come usare il modulo PowerShell SqlServer per Always Encrypted, vedere [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

La rotazione delle chiavi Always Encrypted è il processo di sostituzione di una chiave esistente con una nuova. Potrebbe essere necessario ruotare una chiave se questa è stata compromessa oppure per conformità ai criteri e alle normative dell'organizzazione che impongono la rotazione a intervalli regolari delle chiavi crittografiche. 

Always Encrypted usa due tipi di chiavi, quindi ci sono due flussi di lavoro principali per la rotazione delle chiavi: rotazione delle chiavi master della colonna e rotazione delle chiavi di crittografia della colonna.

* **Rotazione della chiave di crittografia della colonna** : comporta la decrittografia dei dati crittografati con la chiave corrente e la nuova crittografia dei dati tramite la nuova chiave di crittografia della colonna. Dal momento che la rotazione di una chiave di crittografia della colonna richiede l'accesso alle chiavi e al database, questa può solo essere eseguita senza la separazione dei ruoli.
* **Rotazione delle chiavi master della colonna** : comporta la decrittografia delle chiavi di crittografia della colonna che sono protette dalla chiave master corrente della colonna, la nuova crittografia tramite la nuova chiave master della colonna e l'aggiornamento dei metadati per entrambi i tipi di chiavi. La rotazione delle chiavi master della colonna può essere completata con o senza la separazione dei ruoli (quando si usa il modulo PowerShell SqlServer).

## <a name="column-master-key-rotation-without-role-separation"></a>Rotazione delle chiavi master della colonna senza la separazione dei ruoli

Il metodo di rotazione di una chiave master della colonna descritta in questa sezione non supporta la separazione dei ruoli tra un amministratore della sicurezza e un amministratore di database. I passaggi seguenti combinano operazioni sulle chiavi fisiche con operazioni sui metadati della chiave, quindi questo flusso di lavoro è consigliato per le organizzazioni che usano il modello DevOps oppure se il database è ospitato nel cloud e l'obiettivo principale consiste nel limitare l'accesso ai dati sensibili agli amministratori del cloud, escludendo gli amministratori di database locale. Questo metodo non è consigliato nel caso in cui eventuali concorrenti includano amministratori di database oppure se gli amministratori di database non devono avere accesso ai dati sensibili.  


| Attività | Articolo | Accede alle chiavi di testo non crittografato o all'archivio chiavi| Accede al database
|:---|:---|:---|:---
|Passaggio 1. Creare una nuova chiave master della colonna in un archivio chiavi.<br><br>**Nota:** il modulo PowerShell SqlServer non supporta questo passaggio. Per eseguire questa operazione dalla riga di comando, è necessario usare gli strumenti specifici per l'archivio chiavi. | [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Sì | No
|Passaggio 2. Avviare un ambiente PowerShell e importare il modulo SqlServer | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Passaggio 3. Connettersi al server e al database. | [Connessione a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
|Passaggio 4. Creare un oggetto SqlColumnMasterKeySettings che includa informazioni sul percorso della nuova chiave master della colonna. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria Per crearlo, è necessario usare il cmdlet specifico dell'archivio chiavi. |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | No | No
|Passaggio 5. Creare i metadati relativi alla nuova chiave master della colonna nel database. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Nota:** questo cmdlet genera automaticamente l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) per creare i metadati della chiave. | No | Sì
|Passaggio 6. Eseguire l'autenticazione in Azure, se la chiave master corrente o nuova della colonna è archiviata nell'insieme di credenziali delle chiavi di Azure | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sì | No
|Passaggio 7. Avviare la rotazione, crittografando ognuna delle chiavi di crittografia della colonna, che è attualmente protetta con la chiave master della colonna precedente, usando la nuova chiave master della colonna. Dopo questo passaggio ciascuna chiave di crittografia della colonna interessata (associata con la chiave master precedente della colonna durante la rotazione) è crittografata con la chiave master precedente e nuova della colonna e ha due valori crittografati nei metadati del database.| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | Sì | Sì
|Passaggio 8. Chiedere agli amministratori di tutte le applicazioni di eseguire query sulle colonne crittografate del database (protette con la chiave master precedente della colonna) per assicurarsi che possano accedere alla nuova chiave master della colonna.| [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Sì | No
|Passaggio 9. Completare la rotazione<br><br>**Nota:** prima di eseguire questo passaggio, assicurarsi che tutte le applicazioni che eseguono query sulle colonne crittografate protette con la chiave master precedente della colonna, siano state configurate per usare la nuova chiave master della colonna. Se non si esegue questo passaggio in maniera completa, alcune applicazioni potrebbero non essere in grado di decrittografare i dati. Completare la rotazione rimuovendo i valori crittografati dai database creati con la chiave master precedente della colonna. Questa operazione rimuove l'associazione tra la chiave master precedente della colonna e le chiavi di crittografia della colonna da essa protetti. |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| No | Sì
|Passaggio 10. Rimuovere i metadati dalla chiave master precedente della colonna. |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| No | Sì

> [!NOTE]
> Si consiglia di non eliminare definitivamente la chiave master precedente della colonna dopo la rotazione. È opportuno invece conservare la chiave master precedente della colonna nell'archivio chiavi corrente o archiviarla in un altro posto sicuro. Se si ripristina il database da un file di backup creato *prima* che la nuova chiave master della colonna sia stata configurata, è necessaria la chiave precedente per accedere ai dati.

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>Rotazione di una chiave master della colonna senza la separazione dei ruoli (esempio di certificato Windows)

Lo script seguente è un esempio end-to-end che sostituisce una chiave master esistente della colonna (CMK1) con una nuova chiave master della colonna (CMK2).

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>Rotazione delle chiavi master della colonna con la separazione dei ruoli

Il flusso di lavoro della rotazione delle chiavi master della colonna descritto in questa sezione assicura la separazione dei ruoli tra un amministratore della sicurezza e un amministratore di database.

> [!IMPORTANT]
> Prima di eseguire passaggi in cui *Accede a chiavi di testo non crittografato/archivio chiavi*=**Sì** (passaggi che accedono a chiavi di testo non crittografato/archivio chiavi), assicurarsi che l'ambiente PowerShell venga eseguito su un computer protetto diverso da un computer che ospita il database. Per altre informazioni, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

### <a name="part-1-dba"></a>Parte 1: Amministratore di database

Un amministratore di database recupera metadati sulla chiave master della colonna da ruotare e sulle relative chiavi di crittografia associate alla chiave master della colonna corrente. L'amministratore di database condivide tutte queste informazioni con un amministratore della sicurezza.


| Attività | Articolo | Accede alle chiavi di testo non crittografato o all'archivio chiavi| Accede al database
|:---|:---|:---|:---
|Passaggio 1. Avviare un ambiente PowerShell e importare il modulo SqlServer. | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | nessuno
|Passaggio 2. Connettersi al server e a un database. | [Connettersi a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
|Passaggio 3. Recuperare i metadati sulla chiave master precedente della colonna.| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | No | Sì
|Passaggio 4. Recuperare i metadati sulle chiavi di crittografia di colonna, protetti dalla chiave master precedente della colonna, inclusi i relativi valori crittografati. | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | No | Sì
|Passaggio 5. Condividere il percorso della chiave master della colonna (nome del provider e percorso della chiave master della colonna) e i valori crittografati delle chiavi di crittografia della colonna corrispondenti, protetti con la chiave master della colonna precedente.| Vedere gli esempi seguenti. | No | No

### <a name="part-2-security-administrator"></a>Parte 2: Amministratore della sicurezza

L'amministratore della sicurezza genera una nuova chiave master della colonna, riesegue la crittografia delle chiavi di crittografia delle colonne interessate e condivide con l'amministratore di database le informazioni sulla nuova chiave master della colonna, nonché il set dei valori nuovi crittografati per le chiavi di crittografia delle colonne interessate.

| Attività | Articolo | Accede alle chiavi di testo non crittografato o all'archivio chiavi| Accede al database
|:---|:---|:---|:---
|Passaggio 1. Ottenere dall'amministratore del database il percorso della chiave master precedente della colonna e i valori crittografati delle chiavi di crittografia corrispondenti della colonna, protette con la chiave master precedente della colonna.|N/D<br>Vedere gli esempi seguenti.|No| No
|Passaggio 2. Creare una nuova chiave master della colonna in un archivio chiavi.<br><br>**Nota:** il modulo SqlServer non supporta questo passaggio. Per eseguire questa attività da una riga di comando, è necessario usare gli strumenti specifici del tipo di archivio chiavi.|[Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Sì | No
|Passaggio 3. Avviare un ambiente PowerShell e importare il modulo SqlServer. | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Passaggio 4. Creare un oggetto SqlColumnMasterKeySettings che includa informazioni sul percorso della chiave master **precedente** della colonna. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria |New-SqlColumnMasterKeySettings| No | No
|Passaggio 5. Creare un oggetto SqlColumnMasterKeySettings che includa informazioni sul percorso della **nuova** chiave master della colonna. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria Per crearlo, è necessario usare il cmdlet specifico dell'archivio chiavi. | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| No | No
|Passaggio 6. Eseguire l'autenticazione in Azure, se la chiave master precedente (corrente) o nuova della colonna è archiviata nell'insieme di credenziali delle chiavi di Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sì | No
|Passaggio 7. Crittografare nuovamente ogni valore della chiave di crittografia della colonna, attualmente protetta con la chiave master precedente della colonna, usando la nuova chiave master della colonna. | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**Nota:** quando si esegue questo cmdlet, è possibile eseguire il passaggio degli oggetti SqlColumnMasterKeySettings per la chiave master precedente e nuova della colonna, insieme a un valore della chiave di crittografia della colonna da crittografare di nuovo.|Sì|No
|Passaggio 8. Condividere il percorso della nuova chiave master della colonna (nome del provider e percorso della chiave master della colonna) e il set di nuovi valori crittografati delle chiavi di crittografia della colonna con l'amministratore del database.| Vedere gli esempi seguenti. | No | No

> [!NOTE]
> Si consiglia di non eliminare definitivamente la chiave master precedente della colonna dopo la rotazione. È opportuno invece conservare la chiave master precedente della colonna nell'archivio chiavi corrente o archiviarla in un altro posto sicuro. Se si ripristina il database da un file di backup creato *prima* che la nuova chiave master della colonna sia stata configurata, è necessaria la chiave precedente per accedere ai dati.

### <a name="part-3-dba"></a>Parte 3: Amministratore di database

L'amministratore di database crea metadati per la nuova chiave master della colonna e aggiorna i metadati delle chiavi di crittografia delle colonne interessate per aggiungere il set nuovo di valori crittografati. In questo passaggio, l'amministratore di database interagisce anche con gli amministratori delle applicazioni che eseguono query sulle colonne di crittografia, i quali assicurano che l'applicazione può accedere alla nuova chiave master della colonna. Dopo che tutte le applicazioni sono configurate per usare la nuova chiave master della colonna, l'amministratore di database rimuove il set precedente di valori crittografati e i metadati della chiave master precedente della colonna.

| Attività | Articolo | Accede alle chiavi di testo non crittografato o all'archivio chiavi| Accede al database
|:---|:---|:---|:---
|Passaggio 1. Ottenere dall'amministratore del database il percorso della nuova chiave master della colonna e il set nuovo di valori crittografati delle corrispondenti chiavi di crittografia della colonna, protette con la chiave master precedente della colonna.| Vedere gli esempi seguenti. | No | No
|Passaggio 2. Avviare un ambiente PowerShell e importare il modulo SqlServer. | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Passaggio 3. Connettersi al server e a un database. | [Connessione a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
|Passaggio 4. Creare un oggetto SqlColumnMasterKeySettings che includa informazioni sul percorso della nuova chiave master della colonna. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria |New-SqlColumnMasterKeySettings| No| No
|Passaggio 5. Creare i metadati relativi alla nuova chiave master della colonna nel database.|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Nota:** questo cmdlet genera automaticamente l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) per creare i metadati della chiave. | No | Sì
|Passaggio 6. Recuperare i metadati sulle chiavi di crittografia della colonna, protetti dalla chiave master precedente della colonna.| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| No | Sì
|Passaggio 7. Aggiungere un valore crittografato nuovo (creato tramite la nuova chiave master della colonna) ai metadati di ogni chiave di crittografia della colonna interessata.|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|No|Sì
|Passaggio 8. Chiedere agli amministratori di tutte le applicazioni di eseguire query sulle colonne crittografate del database (protette con la chiave master precedente della colonna) per assicurarsi che possano accedere alla nuova chiave master della colonna.|[Creazione e archiviazione di chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| No|No
|Passaggio 9. Completare la rotazione rimuovendo dal database i valori crittografati associati con la chiave master precedente della colonna.<br><br>**Nota:** prima di eseguire questo passaggio, assicurarsi che tutte le applicazioni che eseguono query sulle colonne crittografate protette con la chiave master precedente della colonna siano state configurate per usare la nuova chiave master della colonna. Se non si esegue questo passaggio in maniera completa, alcune applicazioni potrebbero non essere in grado di decrittografare i dati.<br><br>Questo passaggio rimuove l'associazione tra la chiave master precedente della colonna e le chiavi di crittografia della colonna da essa protetti. | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>In alternativa, è possibile usare [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue) | No|Sì
|Passaggio 10. Rimuovere i metadati dalla chiave master precedente della colonna dal database| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| No|Sì

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>Rotazione di una chiave master della colonna con la separazione dei ruoli (esempio di certificato Windows)

Lo script seguente è un esempio end-to-end per la generazione di una nuova chiave master della colonna (certificato nell'archivio certificati di Windows), il quale ruota la chiave master esistente (corrente) della colonna per sostituirla con la nuova chiave master della colonna. Questo script presuppone che il database di destinazione contenga la chiave master della colonna, denominata CMK1 (da ruotare), che crittografa alcune chiavi di crittografia della colonna.

Parte 1: Amministratore di database

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


Parte 2: Amministratore della sicurezza

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


Parte 3: Amministratore di database

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>Rotazione di una chiave di crittografia della colonna

La rotazione di una chiave di crittografia della colonna comporta la decrittografia dei dati in tutte le colonne crittografate con la chiave da ruotare e la nuova crittografia dei dati che usano la nuova chiave di crittografia della colonna. Questo flusso di lavoro di rotazione richiede accesso alle chiavi e al database e quindi non può essere eseguito con la separazione dei ruoli. La rotazione di una chiave di crittografia della colonna può richiedere molto tempo, se le tabelle contenenti le colonne crittografate con la chiave da ruotare sono di grandi dimensioni. Pertanto, l'organizzazione deve pianificare con attenzione una rotazione della chiave di crittografia della colonna.

È possibile ruotare una chiave di crittografia della colonna usando un approccio online o offline. Il primo metodo è probabilmente il più veloce, ma le applicazioni non possono scrivere nelle tabelle interessate. Il secondo approccio richiederà probabilmente più tempo, ma è possibile limitare l'intervallo di tempo durante il quale le tabelle interessate non sono disponibili per le applicazioni. Per altre informazioni, vedere [Configurare la crittografia delle colonne usando Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md) e [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/).

| Attività | Articolo | Accede alle chiavi di testo non crittografato o all'archivio chiavi| Accede al database
|:---|:---|:---|:---
|Passaggio 1. Avviare un ambiente PowerShell e importare il modulo SqlServer. | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Passaggio 2. Connettersi al server e a un database. | [Connessione a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
|Passaggio 3. Eseguire l'autenticazione in Azure, se la chiave master della colonna che protegge la chiave di crittografia della colonna da ruotare è archiviata nell'insieme di credenziali delle chiavi di Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sì | No
|Passaggio 4. Generare una nuova chiave di crittografia di colonna, crittografarla con la chiave master della colonna e creare i metadati della chiave di crittografia della colonna nel database.  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Nota:** usare una variante del cmdlet che genera internamente e crittografa una chiave di crittografia della colonna.<br>Questo cmdlet genera automaticamente l'istruzione [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) per creare i metadati della chiave. | Sì | Sì
|Passaggio 5. Trovare tutte le colonne crittografate con la chiave di crittografia precedente della colonna. | [Guida alla programmazione di SMO (SQL Server Management Objects)](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | No | Sì
|Passaggio 6. Creare un oggetto *SqlColumnEncryptionSettings* per ogni colonna interessata.  SqlColumnEncryptionSettings è un oggetto presente in memoria (in PowerShell). che specifica lo schema di crittografia di destinazione per una colonna. In questo caso, l'oggetto deve specificare che la colonna interessata deve essere crittografata con la nuova chiave di crittografia della colonna. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | No | No
|Passaggio 7. Crittografare nuovamente le colonne, identificate nel passaggio 5, usando la nuova chiave di crittografia della colonna. | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Nota:** questo passaggio può richiedere molto tempo. Le applicazioni non potranno accedere alle tabelle durante l'intera operazione o una parte di essa, a seconda dell'approccio (online o offline) selezionato. | Sì | Sì
|Passaggio 8. Rimuovere i metadati per la chiave di crittografia precedente della colonna. | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | No | Sì

### <a name="example---rotating-a-column-encryption-key"></a>Esempio: rotazione di una chiave di crittografia della colonna

Lo script seguente illustra la rotazione di una chiave di crittografia della colonna.  Questo script presuppone che il database di destinazione contenga alcune colonne crittografate con una chiave di crittografia della colonna denominata CEK1 (da ruotare), che è protetta da una chiave master della colonna, denominata CMK1 (la chiave master della colonna non è archiviata in Azure Key Vault). 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)
  
## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configurare Always Encrypted tramite PowerShell](configure-always-encrypted-using-powershell.md)
- [Ruotare chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
