---
title: Creare e archiviare chiavi master della colonna per Always Encrypted
description: Informazioni su come selezionare un archivio chiavi e creare chiavi master della colonna per SQL Server Always Encrypted.
ms.custom: seo-lt-2019
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 856e8061-c604-4ce4-b89f-a11876dd6c88
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a3c59b19c952659e0630abb5ebbfa34f8a5dfbd3
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561121"
---
# <a name="create-and-store-column-master-keys-for-always-encrypted"></a>Creare e archiviare chiavi master della colonna per Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Le *chiavi master della colonna* proteggono le chiavi usate in Always Encrypted per crittografare le chiavi di crittografia della colonna. Le chiavi master della colonna devono essere archiviate in un archivio attendibile e devono essere accessibili alle applicazioni che le richiedono per crittografare o decrittografare i dati e agli strumenti per la configurazione di Always Encrypted e la gestione delle chiavi di Always Encrypted.

Questo articolo fornisce informazioni dettagliate per la selezione di un archivio chiavi e la creazione di chiavi master della colonna per Always Encrypted. Per una panoramica dettagliata, vedere [Overview of Key Management for Always Encrypted (Panoramica della gestione delle chiavi per Always Encrypted)](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

## <a name="selecting-a-key-store-for-your-column-master-key"></a>Scelta di un archivio chiavi per la chiave master della colonna

Always Encrypted supporta più archivi chiavi per l'archiviazione di chiavi master della colonna Always Encrypted. Gli archivi chiavi supportati variano a seconda dei driver e della versione in uso.

Esistono due categorie generali di archivi da considerare: gli *archivi chiavi locali* e gli *archivi chiavi centralizzati*.

###  <a name="local-or-centralized-key-store"></a>Archivio chiavi centralizzato o locale?

* Gli **archivi chiavi locali** possono essere usati solo dalle applicazioni nei computer che contengono l'archivio chiavi locale. In altre parole, è necessario replicare l'archivio chiavi e la chiave per ogni computer che esegue l'applicazione. Un esempio di archivio chiavi locale è l'archivio certificati Windows. Quando si usa un archivio chiavi locale, è necessario verificare che l'archivio chiavi esista in ogni computer che ospita l'applicazione e che il computer contenga le chiavi master della colonna richieste dall'applicazione per accedere ai dati protetti con Always Encrypted. Quando si esegue il provisioning di una chiave master della colonna per la prima volta o quando si modifica (ruota) la chiave, è necessario verificare che la chiave venga distribuita a tutti i computer che ospitano una o più applicazioni.

* Gli **archivi chiavi centralizzati** vengono usati dalle applicazioni in più computer. Un esempio di archivio chiavi centralizzato è un [insieme di credenziali delle chiavi di Azure](https://azure.microsoft.com/services/key-vault/). Un archivio chiavi centralizzato in genere facilita la gestione delle chiavi perché non è necessario avere più copie delle chiavi master della colonna in più computer. Verificare che le applicazioni siano configurate per la connessione all'archivio chiavi centralizzato.

### <a name="which-key-stores-are-supported-in-always-encrypted-enabled-client-drivers"></a>Quali sono gli archivi chiavi supportati nei driver dei client abilitati per Always Encrypted?

I driver dei client abilitati per Always Encrypted sono driver dei client di SQL Server con un supporto predefinito che consente di incorporare Always Encrypted nelle applicazioni client. I driver abilitati per Always Encrypted includono alcuni provider predefiniti per gli archivi chiavi più diffusi. Alcuni driver consentono anche di implementare e registrare un provider di archivio della chiave master della colonna personalizzato, in modo che sia possibile usare qualsiasi archivio chiavi, anche se non è disponibile un provider predefinito specifico. Quando si sceglie tra un provider predefinito e un provider personalizzato considerare che l'utilizzo un provider predefinito in genere implica meno modifiche alle applicazioni. In alcuni casi, è richiesta solo la modifica a una stringa di connessione del database.

I provider predefiniti disponibili dipendono dal driver, dalla versione del driver e dal sistema operativo selezionati.  Consultare la documentazione di Always Encrypted per il driver specifico per determinare quali archivi chiavi sono supportati in modo predefinito e se il driver supporta provider di archivi chiavi personalizzati - [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md).

### <a name="which-key-stores-are-supported-in-sql-tools"></a>Quali archivi chiavi sono supportati negli strumenti SQL?
SQL Server Management Studio, Azure Data Studio e il modulo PowerShell SqlServer supportano le chiavi master della colonna archiviate in:

- Insiemi di credenziali delle chiavi [e HMS gestiti](https://docs.microsoft.com/azure/key-vault/managed-hsm/overview) in Azure Key Vault.
  > [!NOTE]
  > I moduli di gestione dell'account utente gestiti richiedono SSMS 18.9 o versione successiva e il modulo SqlServer di PowerShell versione 21.1.18235 o successiva. Azure Data Studio attualmente non supporta i HMS gestiti.

- Archivio certificati Windows.
- Archivi chiavi, ad esempio il modulo di protezione hardware, che forniscono l'API CNG (Cryptography Next Generation) o l'API cryptography (CAPI).

## <a name="creating-column-master-keys-in-windows-certificate-store"></a>Creazione di chiavi master della colonna nell'archivio certificati Windows    

Una chiave master della colonna può essere un certificato archiviato nell'archivio certificati Windows. Un driver abilitato per Always Encrypted non verifica la data di scadenza o una catena di autorità di certificazione. Un certificato viene usato semplicemente come una coppia di chiavi composta da una chiave pubblica e da una privata.

Per essere una chiave master della colonna valida, un certificato deve:
* essere un certificato X.509.
* essere archiviato in uno dei percorsi dell'archivio certificati: *computer locale* o *utente corrente*. Per creare un certificato nel percorso dell'archivio certificati del computer locale, è necessario essere un amministratore nel computer di destinazione.
* contenere una chiave privata. La lunghezza consigliata delle chiavi nel certificato è di 2048 bit o superiore.
* essere creato per lo scambio di chiavi.


Esistono diversi modi per creare un certificato che corrisponda a una chiave master della colonna valida, ma l'opzione più semplice consiste nel creare un certificato autofirmato.


### <a name="create-a-self-signed-certificate-using-powershell"></a>Creare un certificato autofirmato usando PowerShell

Usare il cmdlet [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) per creare un certificato autofirmato. L'esempio seguente mostra come generare un certificato che può essere usato come chiave master della colonna per Always Encrypted.

```
# New-SelfSignedCertificate is a Windows PowerShell cmdlet that creates a self-signed certificate. The below examples show how to generate a certificate that can be used as a column master key for Always Encrypted.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048 

# To create a certificate in the local machine certificate store location you need to run the cmdlet as an administrator.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:LocalMachine\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048
```

### <a name="create-a-self-signed-certificate-using-sql-server-management-studio-ssms"></a>Creare un certificato autofirmato usando SQL Server Management Studio (SSMS)

Per informazioni dettagliate, vedere [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).
Per un'esercitazione dettagliata che usa SQL Server Management Studio e archivia le chiavi di Always Encrypted nell'archivio certificati Windows, vedere l' [esercitazione guidata su Always Encrypted (archivio certificati Windows)](/azure/azure-sql/database/always-encrypted-certificate-store-configure).


### <a name="making-certificates-available-to-applications-and-users"></a>Rendere disponibili i certificati a utenti e applicazioni

Se la chiave master della colonna è un certificato archiviato nel percorso dell'archivio certificati del *computer locale* , è necessario esportare il certificato con la chiave privata e importarlo in tutti i computer che ospitano le applicazioni che si prevede eseguiranno la crittografia o la decrittografia dei dati archiviati nelle colonne crittografate oppure negli strumenti per la configurazione di Always Encrypted e per la gestione delle chiavi di Always Encrypted. Inoltre, a ogni utente deve essere concessa un'autorizzazione di lettura per il certificato archiviato nel percorso dell'archivio certificati del computer locale per poter usare il certificato come una chiave master della colonna.

Se la chiave master della colonna è un certificato archiviato nel percorso dell'archivio certificati dell' *utente corrente* , è necessario esportare il certificato con la chiave privata e importarlo nel percorso dell'archivio certificati dell'utente corrente di tutti gli account che eseguono le applicazioni che si prevede eseguiranno la crittografia o la decrittografia dei dati archiviati nelle colonne crittografate oppure negli strumenti per la configurazione di Always Encrypted e per la gestione delle chiavi di Always Encrypted, in tutti i computer che contengono tali applicazioni/strumenti. Non è necessaria la configurazione dell'autorizzazione: dopo l'accesso a un computer, un utente può accedere a tutti i certificati nel percorso dell'archivio certificati dell'utente corrente.

#### <a name="using-powershell"></a>Utilizzo di PowerShell
Usare i cmdlet [Import-PfxCertificate](/powershell/module/pkiclient/import-pfxcertificate) ed [Export-PfxCertificate](/powershell/module/pkiclient/export-pfxcertificate) per importare ed esportare un certificato.

#### <a name="using-microsoft-management-console"></a>Uso di Microsoft Management Console 

Per concedere all'utente l'autorizzazione di *lettura* per un certificato archiviato nel percorso dell'archivio certificati del computer locale, seguire questi passaggi:

1.  Aprire un prompt dei comandi e digitare **mmc**.
2.  Nella console MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.
3.  Nella finestra di dialogo **Aggiungi/Rimuovi snap-in** fare clic su **Aggiungi**.
4.  Nella finestra di dialogo **Aggiungi snap-in autonomo** fare clic su **Certificati** e quindi su **Aggiungi**.
5.  Nella finestra **di dialogo** Snap-in certificati fare clic su **Account computer** e quindi su **Fine.**
6.  Nella finestra di dialogo **Aggiungi/Rimuovi snap-in**, fare clic su **Chiudi**.
7.  Nella finestra di dialogo **Aggiungi/Rimuovi snap-in** fare clic su **OK**.
8.  In **Snap-in certificati** trovare il certificato nella cartella **Certificati > Personale**, fare clic con il pulsante destro del mouse sul certificato, scegliere **Tutte le attività** e quindi fare clic su **Gestisci chiavi private**.
9.  Nella finestra di dialogo **Sicurezza** aggiungere le autorizzazioni di lettura per un account utente, se necessario.

## <a name="creating-column-master-keys-in-azure-key-vault"></a>Creazione di chiavi master della colonna nell'insieme di credenziali delle chiavi di Azure

Azure Key Vault protegge le chiavi crittografiche e i segreti ed è un'opzione utile per archiviare le chiavi master della colonna per Always Encrypted, soprattutto se le applicazioni sono ospitate in Azure. Per creare una chiave nell' [insieme di credenziali delle chiavi di Azure](/azure/key-vault/general/overview), è necessario una [sottoscrizione di Azure](https://azure.microsoft.com/free/) e un insieme di credenziali delle chiavi di Azure. Una chiave può essere archiviata in un insieme di credenziali delle chiavi o in [un modulo di protezione HSM gestito.](https://docs.microsoft.com/azure/key-vault/managed-hsm/overview) Per essere una chiave master della colonna valida, la chiave gestita in Azure Key Vault deve essere una chiave RSA.

### <a name="using-azure-cli-portal-or-powershell"></a>Uso dell'interfaccia della riga di comando di Azure, del portale o di PowerShell

Per informazioni su come creare una chiave in un insieme di credenziali delle chiavi, vedere:
- [Avvio rapido: Impostare e recuperare una chiave da Azure Key Vault con l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/key-vault/keys/quick-create-cli)
- [Avvio rapido: Impostare e recuperare una chiave da Azure Key Vault con Azure PowerShell](https://docs.microsoft.com/azure/key-vault/keys/quick-create-powershell)
- [Avvio rapido: Impostare e recuperare una chiave da Azure Key Vault con il portale di Azure](https://docs.microsoft.com/azure/key-vault/keys/quick-create-portal)

Per informazioni su come creare una chiave in un HSM gestito, vedere:
- [Gestire un modulo di protezione hardware gestito con l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/key-vault/managed-hsm/key-management)

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Per informazioni dettagliate su come creare una chiave master della colonna in un insieme di credenziali delle chiavi o un modulo di protezione HSM gestito in Azure Key Vault con SSMS, vedere Effettuare il provisioning di chiavi Always Encrypted usando [SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).
Per un'esercitazione dettagliata che usa SSMS e archivia le chiavi Always Encrypted in un insieme di credenziali delle chiavi, vedere l'esercitazione sulla procedura guidata di Always Encrypted [(Azure Key Vault).](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)

### <a name="making-azure-key-vault-keys-available-to-applications-and-users"></a>Rendere disponibili le chiavi dell'insieme di credenziali delle chiavi di Azure ad applicazioni e utenti

Per accedere a una colonna crittografata, l'applicazione deve essere in grado di accedere Azure Key Vault e deve anche avere autorizzazioni specifiche sulla chiave master della colonna per decrittografare la chiave di crittografia della colonna che protegge la colonna.

Per gestire le chiavi Always Encrypted, sono necessarie le autorizzazioni per elencare e creare chiavi master della colonna in Azure Key Vault ed eseguire operazioni di crittografia usando le chiavi.

#### <a name="key-vaults"></a>Insiemi di credenziali delle chiavi

Se si archiviano le chiavi master della colonna in un insieme di credenziali delle chiavi e si usano le autorizzazioni del ruolo per l'autorizzazione:

* L'identità dell'applicazione deve essere un membro dei ruoli che consentono le azioni del piano dati seguenti nell'insieme di credenziali delle chiavi: 

  - Microsoft.KeyVault/vaults/keys/decrypt/action
  - Microsoft.KeyVault/vaults/keys/read
  - Microsoft.KeyVault/vaults/keys/verify/action 
  
  Il modo più semplice per concedere all'applicazione l'autorizzazione necessaria è aggiungere la propria identità al ruolo Key Vault [Utente crypto.](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#key-vault-crypto-user) È anche possibile creare un ruolo personalizzato con le autorizzazioni necessarie.
* Un utente che gestisce le chiavi Always Encrypted deve essere un membro o ruoli che consentano le azioni del piano dati seguenti nell'insieme di credenziali delle chiavi: 
  - Microsoft.KeyVault/vaults/keys/create/action
  - Microsoft.KeyVault/vaults/keys/decrypt/action
  - Microsoft.KeyVault/vaults/keys/encrypt/action
  - Microsoft.KeyVault/vaults/keys/read
  - Microsoft.KeyVault/vaults/keys/sign/action
  - Microsoft.KeyVault/vaults/keys/verify/action
  
  Il modo più semplice per concedere all'utente l'autorizzazione necessaria è aggiungere l'utente al ruolo Key Vault [Crypto User.](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#key-vault-crypto-user)  È anche possibile creare un ruolo personalizzato con le autorizzazioni necessarie.

Se si archiviano le chiavi master della colonna in un insieme di credenziali delle chiavi e si usano i criteri di accesso per l'autorizzazione:

* L'identità dell'applicazione richiede le autorizzazioni seguenti per i criteri di accesso nell'insieme di credenziali delle chiavi: *get*, *unwrapKey* e *verify*.
* Un utente che gestisce le chiavi per Always Encrypted richiede le autorizzazioni seguenti per i criteri di accesso nell'insieme di credenziali delle chiavi: *create*, *get*, *list*, *sign*, *unwrapKey*, *wrapKey*, *verify*.

Per informazioni generali su come configurare l'autenticazione e l'autorizzazione per gli insiemi di credenziali delle chiavi, vedere [Autorizzare](https://docs.microsoft.com/azure/key-vault/general/authentication#authorize-a-security-principal-to-access-key-vault)un'entità di sicurezza ad accedere Key Vault .

#### <a name="managed-hsms"></a>HMS gestiti

L'identità dell'applicazione deve essere un membro dei ruoli che consentono le azioni del piano dati seguenti nel HSM gestito: 

- Microsoft.KeyVault/managedHsm/keys/decrypt/action
- Microsoft.KeyVault/managedHsm/keys/read/action
- Microsoft.KeyVault/managedHsm/keys/verify/action

Microsoft consiglia di creare un ruolo personalizzato contenente solo le autorizzazioni precedenti.

Un utente che gestisce le chiavi Always Encrypted deve essere un membro o ruoli che consentano le azioni del piano dati seguenti sulla chiave: 

- Microsoft.KeyVault/managedHsm/keys/create/action
- Microsoft.KeyVault/managedHsm/keys/decrypt/action
- Microsoft.KeyVault/managedHsm/keys/encrypt/action
- Microsoft.KeyVault/managedHsm/keys/read
- icrosoft. KeyVault/managedHsm/keys/sign/action
- Microsoft.KeyVault/managedHsm/keys/verify/action

Il modo più semplice per concedere all'utente le autorizzazioni precedenti è aggiungere l'utente al ruolo Managed HSM Crypto User. È anche possibile creare un ruolo personalizzato con le autorizzazioni necessarie.

Per altre informazioni sul controllo di accesso per i HMS gestiti, vedere:

- [Controllo di accesso per il modulo di protezione hardware gestito](https://docs.microsoft.com/azure/key-vault/managed-hsm/access-control)
- [Ruoli predefiniti predefiniti del controllo degli accessi in base al ruolo locale del servizio HSM gestito.](https://docs.microsoft.com/azure/key-vault/managed-hsm/built-in-roles#permitted-operations)

## <a name="creating-column-master-keys-in-hardware-security-modules-using-cng"></a>Creazione di chiavi master della colonna nei moduli di protezione hardware con CNG

Una chiave master della colonna per Always Encrypted può essere archiviata in un archivio chiavi che implementa l'API Cryptography Next Generation (CNG). In genere, questo tipo di archivio è un modulo di protezione hardware (HSM). Un modulo di protezione hardware (HSM) è un dispositivo fisico che protegge e gestisce le chiavi digitali e fornisce l'elaborazione della crittografia. I moduli di protezione hardware vengono generalmente forniti come schede plug-in o dispositivi esterni collegati direttamente a un computer (HSM locale) o a un server di rete.

Per rendere disponibile un modulo di protezione hardware alle applicazioni di un determinato computer, è necessario installare e configurare un provider di archiviazione chiavi (KSP) che implementa CNG nel computer. Un driver del client Always Encrypted, ovvero un provider di archivio della chiave master della colonna interno al driver, usa il provider di archiviazione chiavi per crittografare e decrittografare le chiavi di crittografia della colonna, protette con la chiave master della colonna archiviata nell'archivio chiavi.

Windows include il provider di archiviazione chiavi del software Microsoft, un provider di archiviazione chiavi basato su software che è possibile usare per i test. Vedere [CNG Key Storage Providers (Provider di archiviazione chiavi CNG)](/windows/desktop/SecCertEnroll/cng-key-storage-providers).

### <a name="creating-column-master-keys-in-a-key-store-using-cngksp"></a>Creazione di chiavi master della colonna in un archivio chiavi con CNG/KSP

Una chiave master della colonna deve essere una chiave asimmetrica, ovvero una coppia di chiavi pubblica/privata, che usa l'algoritmo RSA. La lunghezza della chiave consigliata è di almeno 2048.

#### <a name="using-hsm-specific-tools"></a>Utilizzo degli strumenti specifici per il modulo di protezione hardware
Vedere la documentazione per il modulo di protezione hardware.

#### <a name="using-powershell"></a>Utilizzo di PowerShell

È possibile usare le API .NET per creare una chiave in un archivio chiavi usando CNG in PowerShell.


```
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
```

#### <a name="using-sql-server-management-studio"></a>Uso di SQL Server Management Studio

Vedere [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).

### <a name="making-cng-keys-available-to-applications-and-users"></a>Rendere disponibili le chiavi CNG a utenti e applicazioni

Vedere la documentazione per il modulo di protezione hardware e il provider di archiviazione chiavi per informazioni su come configurare il provider di archiviazione chiavi in un computer e su come concedere l'accesso al modulo di protezione hardware a utenti e applicazioni.

## <a name="creating-column-master-keys-in-hardware-security-modules-using-capi"></a>Creazione di chiavi master della colonna nei moduli di protezione hardware con CAPI

Una chiave master della colonna per Always Encrypted può essere archiviata in un archivio chiavi che implementa l'API Cryptography (CAPI). In genere, questo tipo di archivio è un modulo di protezione hardware (HSM), ovvero un dispositivo fisico che protegge e gestisce le chiavi digitali e fornisce l'elaborazione della crittografia. I moduli di protezione hardware vengono generalmente forniti come schede plug-in o dispositivi esterni collegati direttamente a un computer (HSM locale) o a un server di rete.

Per rendere disponibile un modulo di protezione hardware alle applicazioni di un determinato computer, è necessario installare e configurare un provider del servizio di crittografia (CSP) che implementa CAPI nel computer. Un driver del client Always Encrypted, ovvero un provider di archivio della chiave master della colonna interno al driver, usa il provider del servizio di crittografia per crittografare e decrittografare le chiavi di crittografia della colonna, protette con la chiave master della colonna archiviata nell'archivio chiavi. 

> [!NOTE]
> CAPI è un'API legacy deprecata. Se è disponibile un provider di archiviazione chiavi per il modulo di protezione hardware, è necessario usare quello invece di CSP/CAPI.

Un provider del servizio di crittografia deve supportare l'algoritmo RSA da usare con Always Encrypted.

Windows include i provider del servizio di crittografia seguenti basati su software, ovvero non supportati da un modulo di protezione hardware, che supportano RSA e che possono essere usati per i test: Microsoft Enhanced RSA e AES Cryptographic Provider.

### <a name="creating-column-master-keys-in-a-key-store-using-capicsp"></a>Creazione di chiavi master della colonna in un archivio chiavi con CAPI/CSP

Una chiave master della colonna deve essere una chiave asimmetrica, ovvero una coppia di chiavi pubblica/privata, che usa l'algoritmo RSA. La lunghezza della chiave consigliata è di almeno 2048.

#### <a name="using-hsm-specific-tools"></a>Utilizzo degli strumenti specifici per il modulo di protezione hardware
Vedere la documentazione per il modulo di protezione hardware.

#### <a name="using-sql-server-management-studio-ssms"></a>Utilizzo di SQL Server Management Studio (SSMS)
Vedere [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).

### <a name="making-cng-keys-available-to-applications-and-users"></a>Rendere disponibili le chiavi CNG a utenti e applicazioni
Vedere la documentazione per il modulo di protezione hardware e il provider del servizio di crittografia per informazioni su come configurare il provider del servizio di crittografia in un computer e su come concedere l'accesso al modulo di protezione hardware a utenti e applicazioni.
 
## <a name="next-steps"></a>Passaggi successivi  
- [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)
  
## <a name="see-also"></a>Vedere anche 
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)