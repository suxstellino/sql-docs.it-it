---
title: Registrare il computer per il servizio Sorveglianza host
description: Registrare il computer SQL Server per il servizio Sorveglianza host per Always Encrypted con enclave sicure.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5864ec2b5bda5febc27bbb15606452befe7e293f
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534780"
---
# <a name="register-computer-with-host-guardian-service"></a>Registrare il computer per il servizio Sorveglianza host

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Questo articolo descrive come registrare computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per l'attestazione con il servizio Sorveglianza host (HGS).

> [!NOTE]
> Il processo di registrazione di un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in HGS richiede la collaborazione tra l'amministratore di HGS e l'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Vedere [Ruoli e responsabilità per la configurazione dell'attestazione con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Prima di iniziare, verificare di avere distribuito almeno un computer HGS e di aver configurato il servizio di attestazione HGS.
Per altre informazioni, vedere [Distribuire il servizio Sorveglianza host per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md).

## <a name="step-1-install-the-attestation-client-components"></a>Passaggio 1: Installare i componenti client di attestazione

> [!NOTE]
> Questo passaggio deve essere eseguito dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Per consentire a un client SQL di verificare che stia comunicando con un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] attendibile, il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deve completare l'attestazione per il servizio Sorveglianza host.
Il processo di attestazione è gestito da un componente Windows facoltativo denominato client HGS.
La procedura seguente consente di installare questo componente e iniziare l'attestazione.

1. Verificare che il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] soddisfi i [prerequisiti descritti nella documentazione per la pianificazione di HGS](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites).

2. Eseguire il comando seguente in una console di PowerShell con privilegi elevati per installare la funzionalità Supporto per Sorveglianza host per Hyper-V e i componenti di attestazione.

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. Riavviare il sistema per completare l'installazione.

## <a name="step-2-verify-virtualization-based-security-is-running"></a>Passaggio 2: Verificare che sia in esecuzione la sicurezza basata sulla virtualizzazione

> [!NOTE]
> Questo passaggio deve essere eseguito dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Quando si installa la funzionalità Supporto per Sorveglianza host per Hyper-V, viene automaticamente configurata e abilitata la funzionalità di sicurezza basata sulla virtualizzazione (VBS).
Le enclavi per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted sono protette da ed eseguite all'interno dell'ambiente VBS.
VBS potrebbe non essere avviato se nel computer non è installato e abilitato un dispositivo IOMMU.
Per verificare se VBS è in esecuzione, aprire lo strumento Informazioni di sistema eseguendo `msinfo32.exe` e individuare gli elementi `Virtualization-based security` nella parte inferiore del riepilogo di sistema.

![Screenshot delle informazioni di sistema che mostra lo stato e la configurazione della sicurezza basata sulla virtualizzazione](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

Il primo elemento da controllare è `Virtualization-based security`, che può avere i tre valori seguenti:

- `Running` indica che VBS è configurato correttamente ed è stato possibile avviarlo correttamente. Se il computer mostra questo stato, è possibile procedere al passaggio 3.
- `Enabled but not running` indica che VBS è configurato per l'esecuzione, ma l'hardware non ha i requisiti di sicurezza minimi per eseguire VBS. Potrebbe essere necessario modificare la configurazione dell'hardware nel BIOS o UEFI per abilitare le funzionalità facoltative del processore come un'unità IOMMU o, se l'hardware non supporta effettivamente le funzionalità richieste, potrebbe essere necessario ridurre i requisiti di sicurezza di VBS. Per altre informazioni, continuare a leggere questa sezione.
- `Not enabled` indica che VBS non è configurato per l'esecuzione. La funzionalità Supporto per Sorveglianza host per Hyper-V abilita automaticamente VBS, quindi è consigliabile ripetere il passaggio 1 se viene visualizzato questo stato.

Se VBS non è in esecuzione nel computer, controllare le proprietà di `Virtualization-based security`. Confrontare i valori nell'elemento `Required Security Properties` con i valori nell'elemento `Available Security Properties`.
Le proprietà richieste devono essere uguali o un subset delle proprietà di sicurezza disponibili per l'esecuzione di VBS.

Nel contesto dell'attestazione delle enclave di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], le proprietà di sicurezza hanno la priorità seguente:

- `Base virtualization support` è sempre obbligatorio, poiché rappresenta le funzionalità hardware minime necessarie per eseguire un hypervisor.
- `Secure Boot` è consigliato ma non obbligatorio per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. L'avvio protetto protegge da rootkit richiedendo l'esecuzione di un bootloader firmato da Microsoft subito dopo il completamento dell'inizializzazione UEFI. Se si usa l'attestazione TPM (Trusted Platform Module), l'abilitazione dell'avvio protetto verrà misurata e applicata indipendentemente dal fatto che VBS sia configurato per richiedere l'avvio protetto.
- `DMA Protection` è consigliato ma non obbligatorio per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. La protezione DMA usa un'unità IOMMU per proteggere VBS e la memoria dell'enclave da attacchi di accesso diretto alla memoria. In un ambiente di produzione è sempre necessario usare computer con protezione DMA. In un ambiente di sviluppo/test, è accettabile rimuovere il requisito per la protezione DMA. Se l'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] è virtualizzata, probabilmente non sarà disponibile la protezione DMA e sarà necessario rimuovere il requisito per l'esecuzione di VBS. Vedere il [modello di attendibilità](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model) per informazioni sulle garanzie di sicurezza inferiori durante l'esecuzione in una macchina virtuale.

Prima di ridurre le funzionalità di sicurezza necessarie per VBS, rivolgersi all'OEM o al provider di servizi cloud per verificare se esiste un modo per abilitare i requisiti della piattaforma mancanti in UEFI o BIOS (ad esempio, abilitazione dell'avvio protetto, Intel VT-d o AMD IOV).

Per modificare le funzionalità di sicurezza della piattaforma necessarie per VBS, eseguire il comando seguente in una console di PowerShell con privilegi elevati:

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

Dopo aver modificato il Registro di sistema, riavviare il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e verificare se VBS è di nuovo in esecuzione.

Se il computer è gestito dall'azienda, è possibile che le modifiche apportate a queste chiavi del Registro di sistema vengano sostituite da Criteri di gruppo o Microsoft Endpoint Manager dopo il riavvio.
Contattare l'help desk IT per verificare se vengono distribuiti criteri per la gestione della configurazione di VBS.

## <a name="step-3-configure-the-attestation-url"></a>Passaggio 3: Configurare l'URL di attestazione

> [!NOTE]
> Questo passaggio deve essere eseguito dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] verrà quindi configurato con l'URL del servizio di attestazione HGS, ottenuto dall'amministratore di HGS.

In una console di PowerShell con privilegi elevati, aggiornare ed eseguire il comando seguente per configurare l'URL di attestazione.

- Sostituire `hgs.bastion.local` con il nome del cluster HGS.
- È possibile eseguire `Get-HgsServer` in qualsiasi computer HGS per ottenere il nome del cluster
- L'URL di attestazione deve sempre terminare con `/Attestation`
- SQL Server non sfrutta le funzionalità di protezione delle chiavi di HGS, quindi specificare qualsiasi URL fittizio, ad esempio `http://localhost`, per `-KeyProtectionServerUrl`

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

A meno che il computer non sia stato registrato in HGS in precedenza, il comando segnala un errore di attestazione. Questo risultato è normale.

Il campo `AttestationMode` nell'output del cmdlet indica la modalità di attestazione usata da HGS.

Procedere al [passaggio 4A](#step-4a-register-a-computer-in-tpm-mode) per registrare il computer in modalità TPM o al [passaggio 4B](#step-4b-register-a-computer-in-host-key-mode) per registrare il computer nella modalità con chiave host.

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>Passaggio 4A: Registrare un computer in modalità TPM

> [!NOTE]
> Questo passaggio viene eseguito congiuntamente dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e dall'amministratore di HGS. Per informazioni dettagliate, vedere le note seguenti.

### <a name="prepare"></a>Preparare

> [!NOTE]
> Questa azione deve essere eseguita dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

In questo passaggio si vedrà come raccogliere informazioni sullo stato del TPM del computer e registrarlo per HGS.

Se il servizio di attestazione HGS è configurato per l'uso della modalità con chiave host, procedere al [passaggio 4B](#step-4b-register-a-computer-in-host-key-mode).

Prima di iniziare a raccogliere le misurazioni del TPM, assicurarsi di usare una configurazione valida conosciuta del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Nel computer devono essere installati tutti i componenti hardware necessari ed è necessario che siano stati applicati gli aggiornamenti più recenti per software e firmware.
HGS misura i computer rispetto a questa baseline in fase di attestazione, quindi è importante che lo stato sia il più sicuro e previsto possibile quando si raccolgono le misurazioni del TPM.

Per l'attestazione TPM vengono raccolti tre file di dati, alcuni dei quali possono essere riutilizzati in presenza di computer con configurazione identica.

| Artefatto di attestazione | Cosa misura | Univocità |
| -------------------- | ---------------- | ---------- |
| Identificatore di piattaforma  | Chiave di verifica dell'autenticità pubblica nel TPM del computer e certificato della chiave di verifica dell'autenticità del produttore del TPM. | 1 per ogni computer |
| Baseline TPM | Registri di controllo della piattaforma (PCR) nel TPM che misurano la configurazione del firmware e del sistema operativo caricati durante il processo di avvio. Ad esempio, lo stato di avvio protetto e l'eventuale crittografia dei dump di arresto anomalo del sistema. | Una baseline per ogni configurazione di computer univoca (hardware e software identici possono usare la stessa baseline) |
| Criterio di integrità del codice | I criteri di [Controllo delle applicazioni di Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) considerati attendibili per proteggere i computer | Uno per ogni criterio di integrità del codice univoco distribuito nei computer. |

È possibile configurare più di un artefatto di attestazione in HGS per supportare combinazioni miste di hardware e software.
HGS richiede solo che un computer che esegue l'attestazione corrisponda a un solo criterio di ogni categoria di criteri.
Ad esempio, se sono disponibili tre baseline TPM registrate in HGS, le misurazioni del computer possono corrispondere a una qualsiasi di queste baseline per soddisfare i requisiti dei criteri.

### <a name="configure-a-code-integrity-policy"></a>Configurare un criterio di integrità del codice

> [!NOTE]
> Questa procedura deve essere eseguita dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

HGS richiede che a ogni computer che esegue l'attestazione in modalità TPM sia applicato di un criterio di controllo delle applicazioni di Windows Defender (WDAC).
I criteri di integrità del codice WDAC limitano il software che può essere eseguito in un computer controllando ogni processo che tenta di eseguire il codice in base a un elenco di autori attendibili e hash di file.
Per il caso d'uso [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], le enclave sono protette dalla sicurezza basata sulla virtualizzazione e non possono essere modificate dal sistema operativo host, quindi la rigidità dei criteri WDAC non influisce sulla sicurezza delle query crittografate.
Di conseguenza, è consigliabile distribuire un criterio in modalità di controllo ai computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per soddisfare i requisiti di attestazione senza imporre ulteriori restrizioni al sistema.

Se si usa già un criterio di integrità del codice WDAC personalizzato nei computer per rafforzare la configurazione del sistema operativo, è possibile passare a [Raccogliere informazioni per l'attestazione TPM](#collect-tpm-attestation-information).

1. Sono disponibili criteri di esempio predefiniti in ogni sistema operativo Windows Server 2019, Windows 10 versione 1809 e successive. Il criterio `AllowAll` consente l'esecuzione di qualsiasi software nel computer senza restrizioni. Convertire il criterio in un formato binario riconosciuto dal sistema operativo e da HGS per usarlo. In una console di PowerShell con privilegi elevati, eseguire i comandi seguenti per compilare i criteri `AllowAll`:

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. Seguire le istruzioni riportate nella [guida alla distribuzione di Controllo applicazioni di Microsoft Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide) per distribuire il file `allowall_cipolicy.bin` nei computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] usando [Criteri di gruppo](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy). Per i computer del gruppo di lavoro, seguire lo stesso processo usando Editor Criteri di gruppo locali (`gpedit.msc`).

3. Eseguire `gpupdate /force` nei computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per configurare i nuovi criteri di integrità del codice, quindi riavviare i computer per applicare i criteri.

### <a name="collect-tpm-attestation-information"></a>Raccogliere informazioni per l'attestazione TPM

> [!NOTE]
> Questa procedura deve essere eseguita dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Ripetere i passaggi seguenti per ogni computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] che eseguirà l'attestazione per HGS:

1. Con il computer in uno stato valido conosciuto, eseguire i comandi seguenti in PowerShell per raccogliere le informazioni per l'attestazione TPM:

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. Condividere i tre file di attestazione con l'amministratore di HGS. 

### <a name="register-the-sql-server-computer-with-hgs"></a>Registrare il computer SQL Server in HGS

> [!NOTE]
> Questa procedura deve essere eseguita dall'amministratore di HGS.

Ripetere i passaggi seguenti per ogni computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] che eseguirà l'attestazione per HGS:

1. Copiare i file di attestazione ottenuti dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] nel server HGS. 
2. Nel server HGS eseguire i comandi seguenti in una console di PowerShell con privilegi elevati per registrare il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > Se si verifica un errore durante il tentativo di registrare l'identificatore univoco del TPM, assicurarsi di [importare i certificati intermedi e radice del TPM](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation) nel computer HGS in uso.

Oltre all'identificatore della piattaforma, alla baseline del TPM e ai criteri di integrità del codice, è possibile che HGS configuri e applichi criteri predefiniti, che potrebbe essere necessario modificare.
Questi criteri predefiniti vengono misurati dalla baseline TPM raccolta dal server e rappresentano un'ampia gamma di impostazioni di sicurezza che devono essere abilitate per proteggere il computer.
In presenza di computer senza un'unità IOMMU per la protezione da attacchi DMA (ad esempio, una macchina virtuale), sarà necessario disabilitare il criterio IOMMU.

Per disabilitare il requisito per IOMMU, eseguire il comando seguente nel server HGS:

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> Se si disabilita il criterio IOMMU, non saranno richieste unità IOMMU per alcun computer che esegue l'attestazione per HGS.
> Non è possibile disabilitare i criteri IOMMU per un solo computer.

È possibile esaminare l'elenco dei criteri e degli host TPM registrati con i comandi seguenti di PowerShell:

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>Passaggio 4B: Registrare un computer in modalità con chiave host

> [!NOTE]
> Questo passaggio viene eseguito congiuntamente dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e dall'amministratore di HGS. Per informazioni dettagliate, vedere le note seguenti.

Questo passaggio descrive il processo di generazione di una chiave univoca per l'host e la registrazione di tale chiave in HGS.
Se il servizio di attestazione HGS è configurato per l'uso della modalità TPM, seguire le istruzioni nel [passaggio 4A](#step-4a-register-a-computer-in-tpm-mode).

### <a name="generate-a-key-for-a-ssnoversion-md-computer"></a>Generare una chiave per un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

> [!NOTE]
> Questa parte deve essere eseguita congiuntamente dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

L'attestazione con chiave host funziona generando una coppia di chiavi asimmetriche nel computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e fornendo a HGS la metà pubblica di tale chiave.

Ripetere i passaggi seguenti per ogni computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] che eseguirà l'attestazione per HGS:

1. Per generare la coppia di chiavi, eseguire il comando seguente in una console di PowerShell con privilegi elevati:

    ```powershell
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

    Se è già stata creata una chiave host e si vuole generare una nuova coppia di chiavi, usare invece i comandi seguenti:

    ```powershell
    Remove-HgsClientHostKey
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

2. Condividere il file del certificato con l'amministratore di HGS.

### <a name="register-the-sql-server-computer-with-hgs"></a>Registrare il computer SQL Server in HGS

> [!NOTE]
> Questa procedura deve essere eseguita dall'amministratore di HGS.

Ripetere i passaggi seguenti per ogni computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] che eseguirà l'attestazione per HGS:

1. Copiare il file di certificato ottenuto dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] in un server HGS.
2. Eseguire il comando seguente in una console di PowerShell con privilegi elevati per registrare il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

    ```powershell
    Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
   ```

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>Passaggio 5: Verificare che l'host possa essere eseguire l'attestazione correttamente

> [!NOTE]
> Questo passaggio deve essere eseguito dall'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Dopo aver registrato il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per HGS ([passaggio 4A](#step-4a-register-a-computer-in-tpm-mode) per la modalità TPM, [passaggio 4B](#step-4b-register-a-computer-in-host-key-mode) per la modalità con chiave host), è necessario verificare che sia in grado di eseguire correttamente l'attestazione.

È possibile controllare la configurazione del client di attestazione HGS ed eseguire un tentativo di attestazione in qualsiasi momento con [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration?view=win10-ps&preserve-view=true).

L'output del comando sarà simile al seguente:

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

I due campi più importanti nell'output sono `AttestationStatus`, che indica se il computer ha superato l'attestazione e `AttestationSubStatus`, in cui vengono indicati i criteri che il computer non ha soddisfatto in caso di esito negativo dell'attestazione.

Di seguito sono illustrati i valori più comuni che possono essere visualizzati in `AttestationStatus`:

| `AttestationStatus` | Spiegazione |
| ----------------- | ----------- |
| Scaduto | L'host ha superato l'attestazione in precedenza, ma il certificato di integrità emesso è scaduto. Verificare che l'ora dell'host e di HGS siano sincronizzate. |
| `InsecureHostConfiguration` | Il computer non soddisfa uno o più criteri di attestazione configurati nel server HGS. Per altre informazioni, vedere `AttestationSubStatus`. |
| NotConfigured | Il computer non è configurato con un URL di attestazione. [Configurare l'URL di attestazione](#step-3-configure-the-attestation-url) |
| Passed | Il computer ha superato l'attestazione ed è considerato attendibile per l'esecuzione di enclave di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |
| `TransientError` | Il tentativo di attestazione non è riuscito a causa di un errore temporaneo. Questo errore indica in genere che si è verificato un problema durante il tentativo di contattare HGS in rete. Controllare la connessione di rete e assicurarsi che il computer sia in grado di risolvere il computer e indirizzarlo al nome del servizio HGS. |
| `TpmError` | Il dispositivo TPM del computer ha segnalato un errore durante il tentativo di attestazione. Per altre informazioni esaminare i log degli errori. La cancellazione del TPM può risolvere il problema, ma fare attenzione a sospendere BitLocker e altri servizi che si basano sul TPM prima di cancellarlo. |
| `UnauthorizedHost` | La chiave host non è nota a HGS. Seguire le istruzioni riportate nel [passaggio 4B](#step-4b-register-a-computer-in-host-key-mode) per registrare il computer per HGS. |

Quando `AttestationStatus` indica `InsecureHostConfiguration`, il campo `AttestationSubStatus` verrà popolato con uno o più nomi dei criteri in errore.
La tabella seguente illustra i valori più comuni e indica come correggere gli errori.

| AttestationSubStatus | Cosa significa e cosa fare |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | Il criterio di integrità del codice nel computer non è registrato in HGS *o* il computer non sta attualmente usando un criterio di integrità del codice. Vedere [Configurare un criterio di integrità del codice](#configure-a-code-integrity-policy) per istruzioni. |
| DumpsEnabled | Il computer è configurato per consentire i dump di arresto anomalo del sistema, ma il criterio Hgs_DumpsEnabled non consente i dump. Disabilitare i dump nel computer o disabilitare il criterio Hgs_DumpsEnabled per continuare. |
| FullBoot | Ripresa del computer da uno stato di sospensione o ibernazione, con conseguenti modifiche delle misurazioni del TPM. Riavviare il computer per generare misurazioni TPM pulite. |
| HibernationEnabled | Il computer è configurato per consentire l'ibernazione con file di ibernazione non crittografati. Disabilitare l'ibernazione nel computer per risolvere il problema. |
| HypervisorEnforcedCodeIntegrityPolicy | Il computer non è configurato per l'uso di un criterio di integrità del codice. Controllare Criteri di gruppo o Criteri di gruppo locali > Configurazione computer > Modelli amministrativi > Sistema > Device Guard > Attiva sicurezza basata su virtualizzazione > Protezione basata su virtualizzazione dell'integrità del codice. Questo criterio deve essere "Abilitato senza blocco". |
| Iommu | Per questo computer non è abilitato un dispositivo IOMMU. Se si tratta di un computer fisico, abilitare IOMMU nel menu di configurazione di UEFI. Se si tratta di una macchina virtuale e non è disponibile un'unità IOMMU, eseguire `Disable-HgsAttestationPolicy Hgs_IommuEnabled` nel server HGS. |
| SecureBoot | Avvio protetto non è abilitato nel computer. Per risolvere l'errore, abilitare l'avvio protetto nel menu di configurazione di UEFI. |
| VirtualSecureMode | La sicurezza basata sulla virtualizzazione non è in esecuzione nel computer. Seguire le istruzioni riportate nel [passaggio 2: Verificare che VBS sia in esecuzione nel computer](#step-2-verify-virtualization-based-security-is-running). |

## <a name="next-steps"></a>Passaggi successivi

- [Configurare l'enclave sicuro in SQL Server](always-encrypted-enclaves-configure-enclave-type.md)