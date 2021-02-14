---
title: Configurare Extensible Key Management TDE (Transparent Data Encryption) con Azure Key Vault
description: Installare e configurare il Connettore SQL Server per Azure Key Vault.
ms.custom: seo-lt-2019
ms.date: 11/25/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
- TDE, AKV, EKM
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
author: Rupp29
ms.author: arupp
ms.openlocfilehash: 83a1b1c7e923362678a013de24b211a9da70be0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345387"
---
# <a name="set-up-sql-server-tde-extensible-key-management-by-using-azure-key-vault"></a>Configurare Extensible Key Management TDE di SQL Server usando Azure Key Vault

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In questo articolo si procederà all'installazione e alla configurazione del Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per Azure Key Vault.  
  
## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare a usare Azure Key Vault con l'istanza di SQL Server, assicurarsi che siano soddisfatti i prerequisiti seguenti:  
  
- È necessario disporre di una sottoscrizione di Azure.
  
- Installare [Azure PowerShell versione 5.2.0 o successiva](/powershell/azure/).  

- Creare un'istanza di Azure Active Directory (Azure AD).

- Per acquisire familiarità con i principi di archiviazione EKM (Extensible Key Management) con Azure Key Vault, vedere [EKM con Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  

- Installare la versione di Visual Studio C++ Redistributable basata sulla versione di SQL Server in esecuzione:
  
  Versione di SQL Server  | Versione di Visual Studio C++ Redistributable
  ---------|---------
  2008, 2008 R2, 2012, 2014 | [Pacchetti Visual C++ Redistributable per Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
  2016 | [Visual C++ Redistributable per Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

## <a name="step-1-set-up-an-azure-ad-service-principal"></a>Passaggio 1: Configurare un'entità servizio di Azure AD

Per concedere all'istanza di SQL Server le autorizzazioni di accesso per Azure Key Vault, è necessario un account dell'entità servizio in Azure AD.  
  
1. Accedere al [portale di Azure](https://ms.portal.azure.com/) ed eseguire una delle operazioni seguenti:

    - Selezionare il pulsante **Azure Active Directory**.

      ![Screenshot del riquadro dei servizi di Azure](../../../relational-databases/security/encryption/media/ekm/ekm-part1-login-portal.png)

    - Selezionare **Altri servizi** e digitare **Azure Active Directory** nella casella **Tutti i servizi**.

      ![Screenshot del riquadro di tutti i servizi di Azure](../../../relational-databases/security/encryption/media/ekm/ekm-part1-select-aad.png)  

1. Registrare un'applicazione con Azure Active Directory usando la procedura seguente. Per istruzioni dettagliate, vedere la sezione su come ottenere un'identità per l'applicazione del [post di blog di Azure Key Vault](/archive/blogs/kv/azure-key-vault-step-by-step).

    a. Nel riquadro **Panoramica di Azure Active Directory** selezionare **Registrazioni app**.

    ![Screenshot del riquadro "Panoramica di Azure Active Directory"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-app-register.png)

    b. Nel riquadro **Registrazioni app** selezionare **Nuova registrazione**.

    ![Screenshot del riquadro "Registrazioni app"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-registration.png)  

    c. Nel riquadro **Registra un'applicazione** immettere il nome dell'app visualizzato agli utenti e quindi selezionare **Registra**.

    ![Screenshot del riquadro "Registra un'applicazione"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-register.png)

    d. Nel riquadro sinistro selezionare **Certificati e segreti** e quindi selezionare **Nuovo segreto client**.

    ![Screenshot del riquadro "Certificati e segreti"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-certs-secrets.png)  

    e. In **Aggiungi un segreto client** immettere una descrizione e una scadenza appropriata e quindi selezionare **Aggiungi**.

    ![Screenshot della sezione "Aggiungi un segreto client"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-add-secret.png)  

    f. Nel riquadro **Certificati e segreti** in **"Valore"** selezionare il pulsante **Copia** accanto al valore del segreto client da usare per la creazione di una chiave asimmetrica in SQL Server.

    ![Screenshot del valore del segreto](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-secret.png)  

    g. Nel riquadro sinistro selezionare **Panoramica** e quindi nella casella **ID applicazione (client)** copiare il valore da usare per la creazione di una chiave asimmetrica in SQL Server.

    ![Screenshot del valore "ID applicazione (client)" nel riquadro Panoramica](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-appid.png)  

## <a name="step-2-create-a-key-vault"></a>Passaggio 2: Creare un insieme di credenziali delle chiavi

Selezionare il metodo che si vuole usare per creare un insieme di credenziali delle chiavi.

## <a name="azure-portal"></a>[Azure portal](#tab/portal)

### <a name="create-a-key-vault-by-using-the-azure-portal"></a>Creare un insieme di credenziali delle chiavi usando il portale di Azure

È possibile usare il portale di Azure per creare l'insieme di credenziali delle chiavi e quindi aggiungere un'entità di sicurezza di Azure AD.

1. Creare un gruppo di risorse.

   Tutte le risorse di Azure create tramite il portale di Azure devono essere contenute in un gruppo di risorse che viene creato per ospitare l'insieme di credenziali delle chiavi. Il nome della risorsa in questo esempio è *ContosoDevRG*. Scegliere un nome per il gruppo di risorse e l'insieme di credenziali delle chiavi poiché tutti i nomi degli insiemi di credenziali delle chiavi devono essere univoci a livello globale.

   Nel riquadro **Crea un gruppo di risorse** in **Dettagli del progetto** immettere i valori e quindi selezionare **Rivedi e crea**.

      ![Screenshot del riquadro "Crea un gruppo di risorse"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-resource-group.png)  

1. Creare un insieme di credenziali delle chiavi.

    Nel riquadro **Crea un insieme di credenziali delle chiavi** selezionare la scheda **Generale**, immettere i valori appropriati e quindi selezionare **Rivedi e crea**.

    ![Screenshot del riquadro "Crea un insieme di credenziali delle chiavi"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-key-vault.png)  

1. Nel riquadro **Criteri di accesso** selezionare **Aggiungi un criterio di accesso**.

    ![Screenshot del collegamento "Aggiungi un criterio di accesso" nel riquadro "Criteri di accesso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-access-policy.png)  

1. Nel riquadro **Aggiungi un criterio di accesso** eseguire le operazioni seguenti:
  
    a. Nell'elenco a discesa **Configura dal modello (facoltativo)** selezionare **Gestione delle chiavi**.

    b. Nel riquadro sinistro selezionare la scheda **Autorizzazioni delle chiavi** e quindi verificare che le caselle **Recupera**, **Elenco**, **Annulla il wrapping della chiave** ed **Esegui il wrapping della chiave** siano selezionate.

    c. Selezionare **Aggiungi**.

    ![Screenshot del riquadro "Aggiungi un criterio di accesso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-access-policy-permission.png)

1. Nel riquadro sinistro selezionare la scheda **Selezionare un'entità** e quindi eseguire queste operazioni:

    a. Nel riquadro **Entità di protezione** in **Seleziona** iniziare a digitare il nome dell'applicazione Azure AD e quindi nell'elenco dei risultati selezionare l'applicazione che si vuole aggiungere.

    ![Screenshot della casella di ricerca dell'applicazione nel riquadro Entità di protezione](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal.png)  

    b. Scegliere il pulsante **Seleziona** per aggiungere l'entità di sicurezza all'insieme di credenziali delle chiavi.

    ![Screenshot del pulsante Seleziona nel riquadro Entità di protezione](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-principal.png)

    c. In basso a sinistra selezionare **Aggiungi** per salvare le modifiche.

    ![Screenshot del pulsante Aggiungi nel riquadro "Aggiungi un criterio di accesso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal-new.png)

1. Nel riquadro **Key Vault** selezionare **Chiavi** e immettere un nome per l'insieme di credenziali delle chiavi. Usare il tipo di chiave **RSA** e le dimensioni della chiave RSA **2048**. Impostare le date di attivazione e di scadenza in base alle esigenze e impostare **Abilitato?** su **Sì**.

   ![Schermata del riquadro "Crea chiave"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-key-vault-key.png)  

1. Nel riquadro **Criteri di accesso** selezionare **Salva**.
  
   ![Screenshot del pulsante Salva nel riquadro "Aggiungi un criterio di accesso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-access-policy.png)  

## <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="create-a-key-vault-and-key-by-using-powershell"></a>Creare un insieme di credenziali delle chiavi e una chiave tramite PowerShell

L'insieme di credenziali delle chiavi e la chiave creati vengono usati dal motore di database di SQL Server per la protezione della chiave di crittografia.  
  
> [!IMPORTANT]
> La sottoscrizione in cui viene creato l'insieme di credenziali delle chiavi deve trovarsi nella stessa istanza di Azure AD predefinita in cui è stata creata l'entità servizio di Azure AD. Se si vuole usare un'istanza di Azure AD diversa da quella predefinita per la creazione di un'entità servizio per il Connettore SQL Server, è necessario modificare l'istanza di Azure AD predefinita nell'account di Azure prima di creare l'insieme di credenziali delle chiavi. Per informazioni su come modificare l'istanza di Azure AD predefinita nell'istanza da usare, vedere la sezione "Domande frequenti" in [Manutenzione e risoluzione dei problemi del Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Installare e accedere ad [Azure PowerShell 5.2.0 o versione successiva](/powershell/azure/) usando il comando seguente:  
  
    ```powershell  
    Connect-AzAccount  
    ```  
  
    L'istruzione restituisce:  
  
    ```console  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    > Se si hanno più sottoscrizioni e si vuole specificare la sottoscrizione da usare per l'insieme di credenziali, eseguire `Get-AzSubscription` per visualizzare le sottoscrizioni e `Select-AzSubscription` per scegliere la sottoscrizione appropriata. In caso contrario, PowerShell ne selezionerà automaticamente una per impostazione predefinita.  
  
1. Creare un gruppo di risorse.

    Tutte le risorse di Azure create tramite PowerShell devono essere contenute in un gruppo di risorse che viene creato per ospitare l'insieme di credenziali delle chiavi. Il nome della risorsa in questo esempio è *ContosoDevRG*. Scegliere un nome per il gruppo di risorse e l'insieme di credenziali delle chiavi poiché tutti i nomi degli insiemi di credenziali delle chiavi devono essere univoci a livello globale.  
  
    ```powershell  
    New-AzResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
    L'istruzione restituisce:  
  
    ```console
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]
    > Per il parametro `-Location` usare il comando `Get-AzureLocation` per identificare come specificare un percorso diverso dal percorso di questo esempio. Per altre informazioni, digitare **Get-Help Get-AzureLocation**.  
  
1. Creare un insieme di credenziali delle chiavi.

    Il cmdlet `New-AzKeyVault` richiede il nome di un gruppo di risorse, un nome dell'insieme di credenziali delle chiavi e una posizione geografica. Ad esempio, per un insieme di credenziali delle chiavi denominato `ContosoEKMKeyVault`, eseguire:  
  
    ```powershell  
    New-AzKeyVault -VaultName 'ContosoEKMKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
    Registrare il nome dell'insieme di credenziali delle chiavi.  
  
    L'istruzione restituisce:

    ```console
    Vault Name                       : ContosoEKMKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoEKMKeyVault  
    Vault URI: https://ContosoEKMKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
1. Concedere le autorizzazioni per consentire all'entità servizio di Azure AD di accedere ad Azure Key Vault.  
  
    È possibile autorizzare altri utenti e applicazioni a usare l'insieme di credenziali delle chiavi.  Per questo esempio, si userà l'entità servizio creata in [ Passaggio 1: Configurare un'entità servizio di Azure AD](#step-1-set-up-an-azure-ad-service-principal) per autorizzare l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    > [!IMPORTANT]
    > L'entità servizio di Azure AD deve avere almeno le autorizzazioni *get*, *list*,*wrapKey* e *unwrapKey* per l'insieme di credenziali delle chiavi.  
  
    Come illustrato nel comando seguente, usare l'**ID app (client)** del [Passaggio 1: Configurare un'entità servizio di Azure AD](#step-1-set-up-an-azure-ad-service-principal) per il parametro `ServicePrincipalName`. `Set-AzKeyVaultAccessPolicy` viene eseguito automaticamente e non produce output se eseguito correttamente.  
  
    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoEKMKeyVault' `  
      -ServicePrincipalName 9A57CBC5-4C4C-40E2-B517-EA677 `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
    Chiamare il cmdlet `Get-AzKeyVault` per verificare le autorizzazioni. Nell'output dell'istruzione in `Access Policies` viene visualizzato il nome dell'applicazione Azure AD elencato come un altro tenant che ha accesso a questo insieme di credenziali delle chiavi.  
  
1. Generare una chiave asimmetrica nell'insieme di credenziali delle chiavi. Questa operazione può essere eseguita in uno dei due modi seguenti: importare una chiave esistente o creare una nuova chiave.  

     > [!NOTE]
     > SQL Server supporta solo chiavi RSA a 2048 e 3072 bit e chiavi RSA-HSM a 2048 e 3072 bit.

### <a name="best-practices"></a>Procedure consigliate

Per garantire un ripristino rapido della chiave e accedere ai dati all'esterno di Azure, eseguire le procedure consigliate seguenti:

- Creare la chiave di crittografia in locale in un dispositivo del modulo di protezione hardware (HSM) locale. Assicurarsi di usare una chiave RSA 2048 o 3072 asimmetrica, in modo che sia supportata da SQL Server.
- Importare la chiave di crittografia in Azure Key Vault. Questo processo è descritto nelle sezioni seguenti.
- Prima di usare la chiave in Azure Key Vault per la prima volta, eseguire un backup di Azure Key Vault. Per altre informazioni, vedere il comando [Backup-AzureKeyVaultKey]().
- Ogni volta che vengono apportate modifiche alla chiave, ad esempio aggiungendo elenchi di controllo di accesso, tag o attributi chiave, assicurarsi di eseguire un altro backup della chiave di Azure Key Vault.

  > [!NOTE]
  > Il backup di una chiave è un'operazione relativa alla chiave di Azure Key Vault che restituisce un file che può essere salvato in qualsiasi posizione.

### <a name="types-of-keys"></a>Tipi di chiavi

È possibile generare quattro tipi di chiavi in Azure Key Vault che funzioneranno con SQL Server. Chiavi RSA a 2048 e 3072 bit e chiavi RSA-HSM a 2048 e 3072 bit asimmetriche.
  
- **Protetta da software**: elaborata nel software e crittografata quando inattiva. Le operazioni sulle chiavi protette da software vengono eseguite in macchine virtuali di Azure. Questo tipo è consigliato per le chiavi che non vengono usate in una distribuzione di produzione.  

- **Con protezione HSM**: creata e protetta da un modulo di protezione hardware (HSM) per una maggiore sicurezza. Il costo è di circa 1 USD per ogni versione della chiave.  
  
    > [!IMPORTANT]
    > Per il Connettore SQL Server, usare solo i caratteri a-z, A-Z, 0-9 e i trattini (-), con un limite di 26 caratteri.
    > Versioni diverse della chiave con lo stesso nome di chiave in Azure Key Vault non funzionano con il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per ruotare una chiave di Azure Key Vault usata da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere i passaggi relativi al rollover della chiave nella sezione "A. Istruzioni di manutenzione per il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]" in [Manutenzione e risoluzione dei problemi del Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  

### <a name="import-an-existing-key"></a>Importare una chiave esistente
  
Se è presente una chiave protetta da software RSA a 2048 bit, è possibile caricarla in Azure Key Vault. Ad esempio, se è presente un file PFX salvato nell'unità `C:\\` in un file denominato *softkey.pfx* che si vuole caricare in Azure Key Vault, eseguire il comando seguente per impostare la variabile `securepfxpwd` per una password di `12987553` per il file PFX:  
  
``` powershell  
$securepfxpwd = ConvertTo-SecureString -String '12987553' `  
  -AsPlainText -Force  
```  

È quindi possibile eseguire il comando seguente per importare la chiave dal file PFX, che protegge la chiave con l'hardware (consigliato) nel servizio Key Vault:  
  
``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
      -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
      -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
```  

> [!CAUTION]
> L'importazione della chiave asimmetrica è consigliata per gli scenari di produzione in quanto consente all'amministratore di depositare la chiave in un sistema di deposito delle chiavi. Se la chiave asimmetrica viene creata nell'insieme di credenziali, non potrà essere depositata perché la chiave privata non può mai lasciare l'insieme di credenziali. È consigliabile depositare le chiavi usate per proteggere i dati critici. La perdita di una chiave asimmetrica causa una perdita dei dati permanente.  

### <a name="create-a-new-key"></a>Creare una nuova chiave

In alternativa, è possibile creare una nuova chiave di crittografia direttamente in Azure Key Vault e proteggerla con il software o con il modulo di protezione hardware (HSM).  In questo esempio viene creata una chiave protetta da software con il cmdlet `Add-AzureKeyVaultKey`:  

``` powershell  
Add-AzureKeyVaultKey -VaultName 'ContosoEKMKeyVault' `  
  -Name 'ContosoRSAKey0' -Destination 'Software'  
```  
  
L'istruzione restituisce:  
  
```console
Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
Key        :  {"kid":"https:contosoekmkeyvault.azure.net/keys/  
                ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
VaultName  : contosodevkeyvault  
Name       : contosoRSAKey0  
Version    : <guid>  
Id         : https://contosoekmkeyvault.vault.azure.net:443/  
              keys/ContosoRSAKey0/<guid>  
```  

> [!IMPORTANT]
> L'insieme di credenziali delle chiavi supporta più versioni della stessa chiave denominata, ma le chiavi da usare con il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non devono essere con controllo delle versioni o rollback. Se l'amministratore vuole eseguire il rollback della chiave usata per la crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], sarà necessario creare una nuova chiave con un nome diverso nell'insieme di credenziali e usarla per crittografare la chiave DEK.  

---

## <a name="step-3-install-the-ssnoversion-connector"></a>Passaggio 3: Installare il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

È possibile scaricare il Connettore SQL Server dall' [Area download Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=521700). Il download deve essere eseguito dall'amministratore del computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

> [!NOTE]
> - Le versioni 1.0.0.440 e precedenti del Connettore SQL Server sono state sostituite e non sono più supportate negli ambienti di produzione. Attenersi alle istruzioni nella pagina [Manutenzione e risoluzione dei problemi del Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) in [Aggiornamento del Connettore SQL Server](sql-server-connector-maintenance-troubleshooting.md#upgrade-of--connector).
> - A partire dalla versione 1.0.3.0, il Connettore SQL Server segnala i messaggi di errore rilevanti ai registri eventi di Windows per la risoluzione dei problemi.
> - A partire dalla versione 1.0.4.0, è disponibile il supporto per i cloud privati di Azure, tra cui Azure Cina, Azure Germania e Azure per enti pubblici.
> - La versione 1.0.5.0 presenta una modifica che causa un'interruzione associata all'algoritmo di identificazione personale. Dopo l'aggiornamento alla versione 1.0.5.0 può verificarsi un errore di ripristino del database. Per altre informazioni, vedere l'[articolo della Knowledge Base 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - A partire dalla versione 1.0.5.0 (timestamp settembre 2020, il Connettore SQL Server supporta il filtro dei messaggi e la logica di ripetizione dei tentativi di richiesta di rete.
> - **A partire dalla versione aggiornata 1.0.5.0 (timestamp: novembre 2020), il Connettore SQL Server supporta le chiavi RSA 2048, RSA 3072, RSA-HSM 2048 e RSA-HSM 3072.**
  
  ![Screenshot dell'installazione guidata del Connettore SQL Server](../../../relational-databases/security/encryption/media/ekm/ekm-connector-install.png)  
  
Per impostazione predefinita, il connettore viene installato in C:\Programmi\Connettore SQL Server per Microsoft Azure Key Vault. Questo percorso può essere modificato durante l'installazione. Se si modifica il percorso, modificare gli script nella sezione seguente.  
  
Non è disponibile alcuna interfaccia per il connettore, ma se l'installazione è stata eseguita correttamente, nel computer viene installata *Microsoft.AzureKeyVaultService.EKM.dll*. Questo assembly è la DLL del provider di crittografia EKM che deve essere registrata con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando l'istruzione `CREATE CRYPTOGRAPHIC PROVIDER`.  
  
L'installazione del Connettore SQL Server consente anche di scaricare facoltativamente gli script di esempio per la crittografia di SQL Server.  
  
Per la spiegazione dei codici di errore, le impostazioni di configurazione o le attività di manutenzione per il Connettore SQL Server, vedere:  
  
- [A. Istruzioni di manutenzione per il Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
- [C. Spiegazione dei codici di errore per il Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
## <a name="step-4-configure-ssnoversion"></a>Passaggio 4: Configurare[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Per visualizzare una nota sui livelli minimi di autorizzazione necessari per ogni azione in questa sezione, vedere [B. Domande frequenti](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Eseguire *sqlcmd.exe* o aprire [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio.  
  
1. Configurare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per usare EKM eseguendo lo script [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente:  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  

    EXEC sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  

    -- Enable EKM provider  
    EXEC sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
1. Registrare il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] come provider EKM con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Creare un provider di crittografia usando il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che rappresenta un provider EKM per Azure Key Vault.
    In questo esempio il nome del provider è `AzureKeyVault_EKM`.  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  

    > [!NOTE]
    > La lunghezza del percorso file non può superare i 256 caratteri.  
  
1. Configurare le credenziali [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di un account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per l'uso dell'insieme di credenziali delle chiavi.  
  
    È necessario aggiungere le credenziali a ogni account di accesso che esegue la crittografia usando una chiave dall'insieme di credenziali delle chiavi. Ciò potrebbe includere:  
  
    - L'account di accesso dell'amministratore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che usa l'insieme di credenziali delle chiavi per configurare e gestire gli scenari di crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    - Altri account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che possono abilitare Transparent Data Encryption o altre funzionalità di crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Esiste un mapping uno-a-uno tra le credenziali e gli account di accesso. In altre parole, ogni account di accesso deve avere credenziali univoche.  
  
    Modificare lo script [!INCLUDE[tsql](../../../includes/tsql-md.md)] nei modi seguenti:  
  
    - Modificare l'argomento `IDENTITY` (`ContosoEKMKeyVault`) in modo che punti ad Azure Key Vault.
      - Se si usa *Azure globale*, sostituire l'argomento `IDENTITY` con il nome di Azure Key Vault specificato in [Passaggio 2: Creare un insieme di credenziali delle chiavi](#step-2-create-a-key-vault).
      - Se si usa un *cloud Azure privato* (ad esempio, Azure per enti pubblici, Azure China 21Vianet o Azure Germania), sostituire l'argomento `IDENTITY` con l'URI dell'insieme di credenziali delle chiavi restituito nel passaggio 3 della sezione [Creare un insieme di credenziali delle chiavi e una chiave tramite PowerShell](#create-a-key-vault-and-key-by-using-powershell). Non includere "https://" nell'URI dell'insieme di credenziali.
    - Sostituire la prima parte dell'argomento `SECRET` con l'ID client di Azure Active Directory specificato in [Passaggio 1: Configurare un'entità servizio di Azure AD](#step-1-set-up-an-azure-ad-service-principal). In questo esempio l'**ID client** è `9A57CBC54C4C40E2B517EA677E0EFA00`.  
  
      > [!IMPORTANT]
      > Assicurarsi di rimuovere i trattini dall'ID app (client).  
  
    - Completare la seconda parte dell'argomento `SECRET` con il **segreto client** del [Passaggio 1: Configurare un'entità servizio di Azure AD](#step-1-set-up-an-azure-ad-service-principal).  In questo esempio il segreto client è `08:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m`. La stringa finale dell'argomento `SECRET` sarà una lunga sequenza di lettere e numeri senza trattini.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred
        WITH IDENTITY = 'ContosoEKMKeyVault',                            -- for public Azure
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.azure.cn',          -- for Azure China 21Vianet
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.microsoftazure.de', -- for Azure Germany
               --<----Application (Client) ID ---><--Azure AD app (Client) ID secret-->
        SECRET = '9A57CBC54C4C40E2B517EA677E0EFA0008:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM;  
  
    -- Add the credential to the SQL Server administrator's domain login
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
    Per un esempio di utilizzo delle variabili per l'argomento `CREATE CREDENTIAL` e di rimozione dei trattini dall'ID client a livello di codice, vedere [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
1. Aprire la chiave di Azure Key Vault nell'istanza di SQL Server.  

    Se è stata creata una nuova chiave o è stata importata una chiave asimmetrica come descritto in [Passaggio 2: Creare un insieme di credenziali delle chiavi](#step-2-create-a-key-vault), sarà necessario aprire la chiave. Aprire la chiave specificando il nome della chiave nello script [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente.  
  
    - Sostituire `EKMSampleASYKey` con il nome che si vuole assegnare alla chiave in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    - Sostituire `ContosoRSAKey0` con il nome della chiave in Azure Key Vault.  
  
    ```sql  
    CREATE ASYMMETRIC KEY EKMSampleASYKey
    FROM PROVIDER [AzureKeyVault_EKM]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  

1. Creare un nuovo account di accesso usando la chiave asimmetrica in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] creata nel passaggio precedente.

     ```sql  
    --Create a Login that will associate the asymmetric key to this login
    CREATE LOGIN TDE_Login
    FROM ASYMMETRIC KEY EKMSampleASYKey;
    ```  

1. Creare un nuovo account di accesso in base alla chiave asimmetrica in SQL Server. Eliminare il mapping delle credenziali del Passaggio 4: Configurare SQL Server in modo che sia possibile eseguire il mapping delle credenziali al nuovo account di accesso.

     ```sql  
    --Now drop the credential mapping from the original association
    ALTER LOGIN [<domain>\<login>]
    DROP CREDENTIAL sysadmin_ekm_cred;
    ```

1. Modificare il nuovo account di accesso ed eseguire il mapping delle credenziali EKM al nuovo account di accesso.

     ```sql  
    --Now add the credential mapping to the new Login
    ALTER LOGIN TDE_Login
    ADD CREDENTIAL sysadmin_ekm_cred;
    ```  

1. Creare un database di test che verrà crittografato con la chiave di Azure Key Vault.

     ```sql
    --Create a test database that will be encrypted with the Azure key vault key
    CREATE DATABASE TestTDE
    ```  

1. Creare una chiave di crittografia del database usando ASYMMETRIC KEY (EKMSampleASYKey).

    ```sql  
    --Create an ENCRYPTION KEY using the ASYMMETRIC KEY (EKMSampleASYKey)
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY EKMSampleASYKey;
    ```
  
1. Crittografare il database di test. Abilitare Transparent Data Encryption impostando ENCRYPTION ON.

     ```sql  
    --Enable TDE by setting ENCRYPTION ON
    ALTER DATABASE TestTDE
    SET ENCRYPTION ON;  
     ```  

1. Pulire gli oggetti di test. Eliminare tutti gli oggetti creati in questo script di test.

    ```sql  
    -- CLEAN UP
    USE Master
    ALTER DATABASE [TestTDE] SET SINGLE_USER WITH ROLLBACK IMMEDIATE
    DROP DATABASE [TestTDE]

    ALTER LOGIN [TDE_Login] DROP CREDENTIAL [sysadmin_ekm_cred]
    DROP LOGIN [TDE_Login]

    DROP CREDENTIAL [sysadmin_ekm_cred]  

    USE MASTER
    DROP ASYMMETRIC KEY [EKMSampleASYKey]
    DROP CRYPTOGRAPHIC PROVIDER [AzureKeyVault_EKM]
    ```  

Per gli script di esempio, vedere il blog in [Transparent Data Encryption e Extensible Key Management di SQL Server con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).

## <a name="next-steps"></a>Passaggi successivi  
  
Dopo aver completato la configurazione di base, vedere [Usare il Connettore SQL Server con le funzionalità di crittografia SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).
  
## <a name="see-also"></a>Vedere anche  

- [Extensible Key Management con Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)
- [Manutenzione e risoluzione dei problemi del Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)
