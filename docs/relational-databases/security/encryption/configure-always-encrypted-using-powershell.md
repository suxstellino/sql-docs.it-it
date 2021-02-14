---
title: Configurare Always Encrypted tramite PowerShell | Microsoft Docs
description: Informazioni su come importare il modulo PowerShell SqlServer, che include i cmdlet per la configurazione di Always Encrypted nel database SQL di Azure e in SQL Server.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 12f2bde5-e100-41fa-b474-2d2332fc7650
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5768357f83252c4d71ac1aeff064364a63367b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345400"
---
# <a name="configure-always-encrypted-using-powershell"></a>Configure Always Encrypted using PowerShell (Configurare Always Encrypted usando PowerShell)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Il modulo PowerShell SqlServer include i cmdlet per la configurazione di [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) in [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

## <a name="security-considerations-when-using-powershell-to-configure-always-encrypted"></a>Considerazioni sulla sicurezza quando si usa PowerShell per configurare Always Encrypted

Poiché l'obiettivo principale di Always Encrypted è garantire la sicurezza dei dati sensibili crittografati anche se il sistema di database viene compromesso, l'esecuzione di uno script di PowerShell che elabora chiavi o dati sensibili nel computer SQL Server può ridurre o annullare i vantaggi della funzionalità. Per altre indicazioni relative alla sicurezza, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

È possibile usare PowerShell per gestire chiavi Always Encrypted con e senza separazione dei ruoli offrendo il controllo su coloro che hanno accesso alle chiavi di crittografia effettive nell'archivio chiavi e coloro che hanno accesso al database.

 Per altre indicazioni, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

## <a name="prerequisites"></a>Prerequisiti

Installare il [modulo SqlServer](/powershell/sqlserver/sqlserver/vlatest/sqlserver) in un computer protetto che NON sia il computer che ospita l'istanza di SQL Server. Il modulo può essere installato direttamente da PowerShell Gallery.  Per altri dettagli, vedere le istruzioni di [download](../../../powershell/download-sql-server-ps-module.md).


## <a name="importing-the-sqlserver-module"></a><a name="importsqlservermodule"></a> Importazione del modulo SqlServer 

Per caricare il modulo SqlServer:

1.  Usare il cmdlet **Set-ExecutionPolicy** per impostare i criteri di esecuzione degli script appropriati.
2.  Usare il cmdlet **Import-Module** per importare il modulo SqlServer.

Questo esempio carica il modulo SqlServer.

```
# Import the SQL Server Module.  
Import-Module "SqlServer" 
```

## <a name="connecting-to-a-database"></a><a name="connectingtodatabase"></a> Connessione a un database

Alcuni dei cmdlet Always Encrypted possono essere usati con dati o metadati del database e richiedono la connessione al database. Quando si configura Always Encrypted con il modulo SqlServer è consigliabile connettersi al database con i due metodi seguenti: 
1. Connettersi tramite il cmdlet **Get-SqlDatabase**.
2. Connettersi tramite il provider SQL Server PowerShell.

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="using-get-sqldatabase"></a>Tramite Get-SqlDatabase
Il cmdlet **Get-SqlDatabase** consente di connettersi a un database in SQL Server o nel database SQL di Azure. Restituisce un oggetto di database, che può essere passato usando il parametro **InputObject** di un cmdlet che si connette al database. 

### <a name="using-sql-server-powershell"></a>Utilizzo di SQL Server PowerShell

```
# Import the SqlServer module
Import-Module "SqlServer"  

# Connect to your database
# Set the valid server name, database name and authentication keywords in the connection string
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# List column master keys for the specified database.
Get-SqlColumnMasterKey -InputObject $database
```

In alternativa, è possibile usare il reindirizzamento:


```
$database | Get-SqlColumnMasterKey
```

### <a name="using-sql-server-powershell-provider"></a>Tramite il provider SQL Server PowerShell
Il [provider SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md) espone la gerarchia degli oggetti SQL Server in percorsi simili ai percorsi del file system. Con SQL Server PowerShell è possibile spostarsi tra i percorsi usando alias di Windows PowerShell simili ai comandi normalmente usati per spostarsi tra i percorsi del file system. Dopo essere passati all'istanza di destinazione e al database, i cmdlet successivi vengono eseguiti nel database come illustrato nell'esempio seguente. 

> [!NOTE]
> Questo metodo di connessione a un database si applica solo a SQL Server e non è supportato nel database SQL di Azure.

```
# Import the SqlServer module.
Import-Module "SqlServer"
# Navigate to the database in the remote instance.
cd SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
# List column master keys in the above database.
Get-SqlColumnMasterKey
```

 
In alternativa, è possibile specificare un percorso di database usando il parametro generico **Path** anziché passare al database.


```
# Import the SqlServer module.
Import-Module "SqlServer" 
# List column master keys for the specified database.
Get-SqlColumnMasterKey -Path SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
```
 
## <a name="always-encrypted-tasks-using-powershell"></a>Attività Always Encrypted tramite PowerShell

- [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [Ruotare le chiavi Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Crittografare, crittografare nuovamente o decrittografare colonne con Always Encrypted tramite PowerShell](configure-column-encryption-using-powershell.md)


##  <a name="always-encrypted-cmdlet-reference"></a><a name="aecmdletreference"></a> Riferimento dei cmdlet di Always Encrypted

Per Always Encrypted sono disponibili i cmdlet di PowerShell seguenti:

|CMDLET |Descrizione
|:---|:---
|**[Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)** |Esegue l'autenticazione in Azure e acquisisce un token di autenticazione.
|**[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)** |Aggiunge un nuovo valore crittografato per un oggetto chiave di crittografia della colonna esistente nel database.
|**[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)** |Completa la rotazione di una chiave master della colonna.
|**[Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)**   |Restituisce tutti gli oggetti chiave di crittografia della colonna definiti nel database oppure un solo oggetto chiave di crittografia della colonna con il nome specificato.
|**[Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey)**   |Restituisce tutti gli oggetti chiave master della colonna definiti nel database oppure un solo oggetto chiave master della colonna con il nome specificato.
|**[Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation)** |Avvia la rotazione di una chiave master della colonna.
|**[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)** |Crea un oggetto SqlColumnMasterKeySettings che descrive una chiave asimmetrica archiviata nell'insieme di credenziali delle chiavi di Azure.
|**[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)** |Crea un oggetto SqlColumnMasterKeySettings che descrive una chiave asimmetrica archiviata in un archivio chiavi che supporta l'API CNG (Cryptography Next Generation).
|**[New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)**   |Crea un oggetto chiave di crittografia della colonna nel database.
|**[New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)**   |Produce un valore crittografato di una chiave di crittografia della colonna.
|**[New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings)** |Crea un oggetto SqlColumnEncryptionSettings che incapsula le informazioni sulla crittografia di una singola colonna, tra cui la chiave di crittografia della colonna e il tipo di crittografia.
|**[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)**   |Crea un oggetto chiave master della colonna nel database.
|**[New-SqlColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkeysettings)**|Crea un oggetto SqlColumnMasterKeySettings per una chiave master della colonna con il provider specificato e il percorso della chiave.
|**[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)** |Crea un oggetto SqlColumnMasterKeySettings che descrive una chiave asimmetrica archiviata in un archivio chiavi con un CSP (Cryptography Service Provider) che supporta l'API Cryptography (CAPI).
|**[Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey)** |Rimuove l'oggetto chiave di crittografia della colonna dal database.
|**[Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)**   |Rimuove un valore crittografato da un oggetto chiave di crittografia della colonna esistente nel database.
|**[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)** |Rimuove l'oggetto chiave master della colonna dal database.
|**[Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)** |Crittografa, decrittografa o ricrittografa le colonne specificate nel database.



## <a name="see-also"></a>Vedere anche

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurare Always Encrypted usando SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)