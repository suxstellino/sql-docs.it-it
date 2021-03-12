---
description: Configurare la crittografia delle colonne usando Always Encrypted con PowerShell
title: Configurare la crittografia delle colonne usando Always Encrypted con PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 074c012b-cf14-4230-bf0d-55e23d24f9c8
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 286a5590fea6ccd96351f8d9ad558cb92a7317af
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622780"
---
# <a name="configure-column-encryption-using-always-encrypted-with-powershell"></a>Configurare la crittografia delle colonne usando Always Encrypted con PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive la procedura per la configurazione Always Encrypted di destinazione per le colonne del database tramite il cmdlet [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption) nel modulo di PowerShell *SqlServer* . Il cmdlet **Set-SqlColumnEncryption** modifica sia lo schema del database di destinazione che i dati archiviati nelle colonne selezionate. I dati archiviati in una colonna possono essere crittografati, crittografati nuovamente o decrittografati, a seconda delle impostazioni di crittografia di destinazione specificate per le colonne e la configurazione di crittografia corrente.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Se si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] e l'istanza di SQL Server è configurata con un'enclave sicura, è possibile eseguire le operazioni di crittografia sul posto, senza trasferire i dati all'esterno del database. Vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md). Tenere presente che PowerShell non supporta la crittografia sul posto.

::: moniker-end
Per altre informazioni sul supporto di Always Encrytped nel modulo di PowerShell SqlServer, vedere [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

## <a name="prerequisites"></a>Prerequisiti

Per impostare la configurazione di crittografia di destinazione, è necessario assicurarsi che:
- nel database sia configurata una chiave di crittografia della colonna (se si vuole crittografare o crittografare nuovamente una colonna). Per informazioni dettagliate, vedere [Configurare le chiavi Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md).
- sia possibile accedere alla chiave master di ogni colonna che si vuole crittografare, crittografare nuovamente o decrittografare dal computer che esegue i cmdlet di PowerShell. 

## <a name="performance-and-availability-considerations"></a>Considerazioni sulle prestazioni e sulla disponibilità

Per applicare le impostazioni di crittografia di destinazione specificate per il database, il cmdlet **Set-SqlColumnEncryption** scarica in modo trasparente tutti i dati dalle colonne contenenti le tabelle di destinazione, ricarica i dati in un set di tabelle temporanee (con le impostazioni di crittografia di destinazione) e infine sostituisce le tabelle originali con le nuove versioni delle tabelle. Il provider di dati .NET Framework per SQL Server sottostante crittografa e/o decrittografa i dati durante il download e/o il caricamento, a seconda della configurazione di crittografia corrente della colonna di destinazione e delle impostazioni di crittografia di destinazione specificate per le colonne di destinazione. L'operazione per spostare i dati potrebbe richiedere molto tempo, a seconda delle dimensioni dei dati nelle tabelle interessate e della larghezza di banda di rete.

Il cmdlet **Set-SqlColumnEncryption** supporta due approcci per l'impostazione della configurazione di crittografia di destinazione: approccio online e approccio offline.

Con l'approccio offline, le tabelle di destinazione (e tutte le tabelle correlate alle tabelle di destinazione, ad esempio, tutte le tabelle che presentano relazioni di chiave esterna con una tabella di destinazione) non sono disponibili per la scrittura di transazioni per tutta la durata dell'operazione. La semantica dei vincoli di chiave esterna (**CHECK** o **NOCHECK**) viene sempre mantenuta quando si usa l'approccio offline.

Con l'approccio online, per cui è richiesto il modulo PowerShell di SQL Server versione 21.x o successiva, l'operazione di copia e crittografia, decrittografia o ricrittografia dei dati viene eseguita in modo incrementale. Le applicazioni possono leggere e scrivere i dati da e verso le tabelle di destinazione per tutta la durata dell'operazione di spostamento dei dati, ad eccezione dell'ultima iterazione, la cui durata è limitata dal parametro **MaxDownTimeInSeconds**, che è possibile definire. Per rilevare ed elaborare le modifiche che le applicazioni possono eseguire durata la copia dei dati, il cmdlet abilita il [rilevamento delle modifiche](../../track-changes/enable-and-disable-change-tracking-sql-server.md) nel database di destinazione. Per questo motivo, è probabile che l'approccio online utilizzi più risorse sul lato server rispetto all'approccio offline. L'operazione potrebbe richiedere anche molto più tempo con l'approccio online, soprattutto se è in esecuzione un carico di lavoro con un'intensa attività di scrittura nel database. L'approccio online può essere usato per crittografare una tabella alla volta e la tabella deve avere una chiave primaria. Per impostazione predefinita, i vincoli di chiave esterna vengono ricreati con l'opzione **NOCHECK** per ridurre al minimo l'impatto sulle applicazioni. È possibile forzare il mantenimento della semantica dei vincoli di chiave esterna specificando l'opzione **KeepCheckForeignKeyConstraints**. 

Di seguito sono riportate le linee guida per la scelta tra gli approcci offline e online:

Usare l'approccio offline:
- Per ridurre al minimo la durata dell'operazione. 
- Per crittografare/decrittografare/ricrittografare le colonne in più tabelle contemporaneamente.
- Se la tabella di destinazione non ha una chiave primaria.

Usare l'approccio online:
- Per ridurre al minimo i tempi di inattività o indisponibilità del database per le applicazioni.

## <a name="security-considerations"></a>Considerazioni sulla sicurezza

Il cmdlet **Set-SqlColumnEncryption** , usato per configurare la crittografia per le colonne del database, gestisce sia le chiavi Always Encrypted che i dati archiviati nelle colonne del database. È quindi importante eseguire il cmdlet in un computer protetto. Se il database è in SQL Server, eseguire il cmdlet da un computer diverso da quello che ospita l'istanza di SQL Server. Poiché l'obiettivo principale di Always Encrypted è di garantire la sicurezza dei dati sensibili crittografati anche se il sistema di database viene compromesso, eseguire uno script di PowerShell che elabora le chiavi e/o i dati sensibili nei computer SQL Server può ridurre o annullare i vantaggi della funzionalità.

Attività  |Articolo  |Accede alle chiavi in testo non crittografato o all'archivio delle chiavi  |Accede al database   
---|---|---|---
Passaggio 1. Avviare un ambiente PowerShell e importare il modulo SqlServer. | [Importazione del modulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
Passaggio 2. Connettersi al server e al database | [Connessione a un database](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sì
Passaggio 3. Eseguire l'autenticazione ad Azure, se la chiave master della colonna, che protegge la chiave di crittografia della colonna da ruotare, è archiviata nell'insieme di credenziali delle chiavi di Azure | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sì | No
Passaggio 4. Costruire una matrice di oggetti SqlColumnEncryptionSettings, uno per ogni colonna del database che si vuole crittografare, crittografare nuovamente o decrittografare. In PowerShell SqlColumnMasterKeySettings è un oggetto presente in memoria che specifica lo schema di crittografia di destinazione per una colonna. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | No | No
Passaggio 5. Impostare la configurazione di crittografia desiderata, specificata nella matrice di oggetti SqlColumnMasterKeySettings creata nel passaggio precedente. Una colonna viene crittografata, crittografata nuovamente o decrittografata a seconda delle impostazioni di destinazione e la configurazione di crittografia corrente della colonna stessa.| [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Nota:** questo passaggio può richiedere molto tempo. Le applicazioni non potranno accedere alle tabelle durante l'intera operazione o una parte di essa, a seconda dell'approccio (online o offline) selezionato. | Sì | Sì

## <a name="encrypt-columns-using-offline-approach---example"></a>Crittografare colonne con l'approccio offline - Esempio

L'esempio riportato di seguito illustra l'impostazione della configurazione di crittografia di destinazione per due colonne. Se una colonna non è ancora crittografata, verrà crittografata. Se una colonna è già crittografata con una chiave e/o un tipo di crittografia diverso, verrà decrittografata e quindi crittografata nuovamente con la chiave o il tipo di destinazione specificato.


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -LogFileDirectory .
```
 
## <a name="encrypt-columns-using-online-approach---example"></a>Crittografare colonne con l'approccio online - Esempio

L'esempio riportato di seguito illustra l'impostazione della configurazione di crittografia di destinazione per due colonne usando l'approccio online. Se una colonna non è ancora crittografata, verrà crittografata. Se una colonna è già crittografata con una chiave e/o un tipo di crittografia diverso, verrà decrittografata e quindi crittografata nuovamente con la chiave o il tipo di destinazione specificato.

```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -UseOnlineApproach -MaxDowntimeInSeconds 180 -LogFileDirectory .
```
## <a name="decrypt-columns---example"></a>Decrittografare colonne - Esempio

L'esempio riportato di seguito illustra come decrittografare tutte le colonne crittografate in un database.


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Find all encrypted columns, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType "Plaintext" 
        }
    }
}

# Decrypt all columns.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -LogFileDirectory .
```
 
## <a name="next-steps"></a>Passaggi successivi
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Vedere anche  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
 - [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
 - [Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati](configure-always-encrypted-using-dacpac.md)