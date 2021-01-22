---
title: Distribuire il servizio Sorveglianza host
description: Distribuire il servizio sorveglianza host per Always Encrypted con enclave sicure.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 88e79166a8b44139f58192feece211bc3b3d2db3
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534790"
---
# <a name="deploy-the-host-guardian-service-for-ssnoversion-md"></a>Distribuire il servizio Sorveglianza host per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Questo articolo descrive come distribuire il servizio Sorveglianza host (HGS) come servizio di attestazione per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Prima di iniziare, leggere l'articolo[Pianificare l'attestazione del servizio Sorveglianza host](./always-encrypted-enclaves-host-guardian-service-plan.md) per un elenco completo dei prerequisiti e delle linee guida per l'architettura.

> [!NOTE]
> L'amministratore di HGS è responsabile dell'esecuzione di tutti i passaggi descritti in questo articolo. Vedere [Ruoli e responsabilità per la configurazione dell'attestazione con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

## <a name="step-1-set-up-the-first-hgs-computer"></a>Passaggio 1: Configurare il primo computer HGS

Il servizio Sorveglianza host (HGS) viene eseguito come servizio cluster in uno o più computer.
In questo passaggio verrà configurato un nuovo cluster HGS nel primo computer.
Se è già disponibile un cluster HGS e si aggiungono altri computer per la disponibilità elevata, procedere al [Passaggio 2: Aggiungere altri computer HGS al cluster](#step-2-add-more-hgs-computers-to-the-cluster).

Prima di iniziare, verificare che nel computer in uso sia in esecuzione Windows Server 2019 Standard o Datacenter Edition, che siano disponibili privilegi di amministratore locale e che il computer non sia già stato aggiunto a un dominio di Active Directory.

1. Accedere al primo computer HGS come amministratore locale e aprire una console di Windows PowerShell con privilegi elevati. Eseguire il comando seguente per installare il ruolo del servizio Sorveglianza host. Il computer verrà riavviato automaticamente per applicare le modifiche.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Dopo il riavvio del computer HGS, eseguire i comandi seguenti in una console di Windows PowerShell con privilegi elevati per installare la nuova foresta di Active Directory:

    ```powershell
    # Select the name for your new Active Directory root domain.
    # Make sure the name does not conflict with, and is not subordinate to, any existing domains on your network.
    $HGSDomainName = 'bastion.local'

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

    Il computer HGS verrà riavviato di nuovo per completare la configurazione della foresta di Active Directory. Al successivo accesso, l'account amministratore sarà un account amministratore di dominio. Per altre informazioni sulla gestione e la protezione della nuova foresta, è consigliabile vedere la [documentazione per le operazioni di Active Directory Domain Services](/windows-server/identity/ad-ds/manage/component-updates/ad-ds-operations).

3. Si procederà quindi alla configurazione del cluster HGS e all'installazione del servizio di attestazione eseguendo il comando seguente in una console di Windows PowerShell con privilegi elevati:

    ```powershell
    # Note: the name you provide here will be shared by all HGS nodes and used to point your SQL Server computers to the HGS cluster.
    # For example, if you provide "attsvc" here, a DNS record for "attsvc.yourdomain.com" will be created for every HGS computer.
    Initialize-HgsAttestation -HgsServiceName 'hgs'
    ```

## <a name="step-2-add-more-hgs-computers-to-the-cluster"></a>Passaggio 2: Aggiungere altri computer HGS al cluster

Dopo aver configurato il primo computer HGS e il cluster, è possibile aggiungere altri server HGS per garantire disponibilità elevata.
Se si sta configurando un solo server HGS (ad esempio, in un ambiente di sviluppo/test), è possibile procedere al passaggio 3.

Come per il primo computer HGS, verificare che nel computer che verrà aggiunto al cluster sia in esecuzione Windows Server 2019 Standard o Datacenter Edition, che siano disponibili privilegi di amministratore locale e che il computer non sia già stato aggiunto a un dominio di Active Directory.

1. Accedere al computer come amministratore locale e aprire una console di Windows PowerShell con privilegi elevati. Eseguire il comando seguente per installare il ruolo del servizio Sorveglianza host. Il computer verrà riavviato automaticamente per applicare le modifiche.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Controllare la configurazione del client DNS nel computer per assicurarsi che sia in grado di risolvere il dominio HGS. Il comando seguente dovrebbe restituire un indirizzo IP per il server HGS. Se non è possibile risolvere il dominio HGS, potrebbe essere necessario aggiornare le informazioni sul server DNS nella scheda di rete per usare il server DNS HGS per la risoluzione dei nomi.

    ```powershell
    # Change 'bastion.local' to the domain name you specified in Step 1.2
    nslookup bastion.local

    # If it fails, use sconfig.exe, option 8, to set the first HGS computer as your preferred DNS server.
    ```

3. Dopo il riavvio del computer, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevati per aggiungere il computer al dominio di Active Directory creato dal primo server HGS. Per eseguire questo comando, sono necessarie le credenziali di amministratore di dominio per il dominio di Active Directory. Al termine del comando, il computer verrà riavviato e il computer diventerà un controller di dominio Active Directory per il dominio HGS.

    ```powershell
    # Provide the fully qualified HGS domain name
    $HGSDomainName = 'bastion.local'

    # Provide a domain administrator's credential for the HGS domain
    $DomainAdminCred = Get-Credential

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $DomainAdminCred -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

4. Accedere con le credenziali di amministratore di dominio dopo il riavvio del computer. Aprire una console di Windows PowerShell con privilegi elevati ed eseguire i comandi seguenti per configurare il servizio di attestazione. Poiché il servizio di attestazione è compatibile con cluster, eseguirà la replica della relativa configurazione da altri membri del cluster. Le modifiche apportate ai criteri di attestazione in qualsiasi nodo HGS verranno applicate a tutti gli altri nodi.

    ```powershell
    # Provide the IP address of an existing, initialized HGS server
    # If you are using separate networks for cluster and application traffic, choose an IP address on the cluster network.
    # You can find the IP address of your HGS server by signing in and running "ipconfig /all"
    Initialize-HgsAttestation -HgsServerIPAddress '172.16.10.20'
    ```

5. Ripetere il passaggio 2 per ogni computer che si vuole aggiungere al cluster HGS.

## <a name="step-3-configure-a-dns-forwarder-to-your-hgs-cluster"></a>Passaggio 3: Configurare un server d'inoltro DNS per il cluster HGS

HGS esegue il proprio server DNS, che contiene i record dei nomi necessari per risolvere il servizio di attestazione.
I computer SQL Server non saranno in grado di risolvere questi record fino a quando non si configura il server DNS della rete per l'inoltro delle richieste ai server DNS HGS.

Il processo per la configurazione dei server d'inoltro DNS è specifico del fornitore, quindi è consigliabile contattare l'amministratore di rete per ottenere le istruzioni corrette per la rete specifica.

Se si usa il ruolo Server DNS di Windows Server per la rete aziendale, l'amministratore DNS può creare un server d'inoltro condizionale nel dominio HGS in modo che vengano inoltrate solo le richieste per il dominio HGS.
Ad esempio, se i server HGS usano il nome di dominio "bastion.local" e hanno gli indirizzi IP 172.16.10.20, 172.16.10.21 e 172.16.10.22, è possibile eseguire il comando seguente nel server DNS aziendale per configurare un server d'inoltro condizionale:

```powershell
# Tip: make sure to provide every HGS server's IP address
# If you use separate NICs for cluster and application traffic, use the application traffic NIC IP addresses here
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers "172.16.10.20", "172.16.10.21", "172.16.10.22"
```

## <a name="step-4-configure-the-attestation-service"></a>Passaggio 4: Configurare il servizio di attestazione

HGS supporta due modalità di attestazione: Attestazione TPM per la verifica crittografica dell'integrità e dell'identità di ogni computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e attestazione con chiave host per una semplice verifica dell'identità del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Se non è già stata selezionata una modalità di attestazione, consultare le informazioni sull'attestazione nella [guida alla pianificazione](./always-encrypted-enclaves-host-guardian-service-plan.md#attestation-modes) per ulteriori dettagli sulle garanzie di sicurezza e i casi d'uso per ogni modalità.

Nei passaggi di questa sezione verranno configurati i criteri di attestazione di base per una particolare modalità di attestazione.
Nel passaggio 4 verranno registrate le informazioni specifiche dell'host.
Se è necessario modificare la modalità di attestazione in futuro, ripetere i passaggi 3 e 4 usando la modalità di attestazione desiderata.

### <a name="switch-to-tpm-attestation"></a>Passare alla modalità di attestazione TPM

Per configurare l'attestazione TPM in HGS, è necessario un computer con accesso a Internet e almeno un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con un chip TPM 2.0 rev 1.16.

Per configurare HGS per l'uso della modalità TPM, aprire una console di PowerShell con privilegi elevati ed eseguire il comando seguente:

```powershell
Set-HgsServer -TrustTpm
```

Tutti i computer HGS nel cluster useranno ora la modalità TPM quando un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tenta l'attestazione.

Prima di poter registrare le informazioni TPM dai computer SQL Server con HGS, è necessario installare i certificati radice di verifica dell'autenticità (EK) dai fornitori del TPM.
Ogni TPM fisico è configurato in fabbrica con una chiave di verifica dell'autenticità univoca accompagnata da un certificato della chiave di verifica dell'autenticità che identifica il produttore.
Questo certificato garantisce che il TPM sia autentico.
HGS verifica il certificato della chiave di verifica dell'autenticità quando si registra un nuovo TPM in HGS confrontando la catena di certificati con un elenco di certificati radice attendibili.

Microsoft pubblica un elenco di certificati radice del fornitore TPM validi conosciuti che è possibile importare nell'archivio dei certificati radice TPM attendibili HGS.
Se i computer SQL Server sono virtualizzati, sarà necessario contattare il provider di servizi cloud o il fornitore della piattaforma di virtualizzazione per informazioni su come ottenere la catena di certificati per la chiave di verifica dell'autenticità del TPM virtuale.

Per scaricare il pacchetto dei certificati radice TPM attendibili da Microsoft per TPM fisici, seguire questa procedura:

1. In un computer con accesso a Internet, scaricare il pacchetto di certificati radice TPM più recente da [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925)

2. Verificare la firma del file CAB per assicurarsi che sia autentica.

    ```powershell
    # Note: replace the path below with the correct one to the file you downloaded in step 1
    Get-AuthenticodeSignature ".\TrustedTpm.cab"
    ```

    > [!WARNING]
    > Non continuare se la firma non è valida e contattare il supporto tecnico Microsoft per assistenza.

3. Espandere il file CAB in una nuova directory.

    ```powershell
    mkdir .\TrustedTpmCertificates
    expand.exe -F:* ".\TrustedTpm.cab" ".\TrustedTpmCertificates"
    ```

4. Nella nuova directory verranno visualizzate le directory per ogni fornitore di TPM. È possibile eliminare le directory per i fornitori non usati.

5. Copiare l'intera directory "TrustedTpmCertificates" nel server HGS.

6. Aprire una console di PowerShell con privilegi elevati nel server HGS ed eseguire il comando seguente per importare tutti i certificati radice e intermedi del TPM:

    ```powershell
    # Note: replace the path below with the correct location of the TrustedTpmCertificates folder on your HGS computer
    cd "C:\scratch\TrustedTpmCertificates"
    .\setup.cmd
    ```

7. Ripetere i passaggi 5 e 6 per ogni computer HGS.

Se sono stati ottenuti certificati CA intermedi e radice dall'OEM, dal provider di servizi cloud o dal fornitore della piattaforma di virtualizzazione, è possibile importare direttamente i certificati nel rispettivo archivio certificati del computer locale: `TrustedHgs_RootCA` o `TrustedHgs_IntermediateCA`. Ad esempio, in PowerShell:

```powershell
# Imports MyCustomTpmVendor_Root.cer to the local machine's "TrustedHgs_RootCA" store
Import-Certificate -FilePath ".\MyCustomTpmVendor_Root.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedHgs_RootCA"
```

### <a name="switch-to-host-key-attestation"></a>Passare alla modalità di attestazione con chiave host

Per usare l'attestazione con chiave host, eseguire il comando seguente in un server HGS in una console di PowerShell con privilegi elevati:

```powershell
Set-HgsServer -TrustHostKey
```

Tutti i computer HGS nel cluster useranno ora la modalità con chiave host quando un computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tenta l'attestazione.

## <a name="step-5-configure-the-hgs-https-binding"></a>Passaggio 5: Configurare il binding HTTPS HGS

In un'installazione predefinita, HGS espone solo un binding HTTP (porta 80).
È possibile configurare un binding HTTPS (porta 443) per crittografare tutte le comunicazioni tra computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] e HGS.
È consigliabile che tutte le istanze di produzione di HGS usino un binding HTTPS.

1. Ottenere un certificato TLS dall'autorità di certificazione, usando il nome completo del servizio HGS dal passaggio 1.3 come nome del soggetto. Se non si conosce il nome del servizio, è possibile trovarlo eseguendo `Get-HgsServer` in qualsiasi computer HGS. È possibile aggiungere nomi DNS alternativi all'elenco dei nomi soggetto alternativi se i computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] usano un nome DNS diverso per raggiungere il cluster HGS, ad esempio se HGS è dietro un servizio di bilanciamento del carico di rete con un indirizzo diverso.

2. Nel computer HGS usare [Set-HgsServer](/powershell/module/hgsserver/set-hgsserver) per abilitare il binding HTTPS e specificare il certificato TLS ottenuto nel passaggio precedente. Se il certificato è già installato nel computer nell'archivio certificati locale, usare il comando seguente per registrarlo in HGS:

    ```powershell
    # Note: you'll need to know the thumbprint for your certificate to configure HGS this way
    Set-HgsServer -Http -Https -HttpsCertificateThumbprint "54A043386555EB5118DB367CFE38776F82F4A181"
    ```

    Se il certificato è stato esportato (con una chiave privata) in un file PFX protetto da password, è possibile registrarlo con HGS eseguendo il comando seguente:

    ```powershell
    $PFXPassword = Read-Host -AsSecureString -Prompt "PFX Password"
    Set-HgsServer -Http -Https -HttpsCertificatePath "C:\path\to\hgs_tls.pfx" -HttpsCertificatePassword $PFXPassword
    ```

3. Ripetere i passaggi 1 e 2 per ogni computer HGS nel cluster. I certificati TLS non vengono replicati automaticamente tra i nodi HGS. Ogni computer HGS, inoltre, può avere un proprio certificato TLS univoco purché il soggetto corrisponda al nome del servizio HGS.

## <a name="step-6-determine-and-share-the-hgs-attestation-url"></a>Passaggio 6: Determinare e condividere l'URL di attestazione HGS

L'amministratore di HGS deve condividere l'URL di attestazione di HGS sia con gli amministratori del computer SQL Server che con gli amministratori delle applicazioni nell'organizzazione. Gli amministratori dei computer SQL Server necessitano dell'URL di attestazione per verificare che i computer SQL Server possano completare l'attestazione in HGS. Gli amministratori delle applicazioni dovranno usare l'URL di attestazione per configurare la modalità di connessione delle app a SQL Server.

Per determinare l'URL di attestazione, eseguire il cmdlet seguente.

```powershell
Get-HGSServer
```
L'output del comando sarà simile al seguente:

```
Name                           Value                                                                         
----                           -----                                                                         
AttestationOperationMode       HostKey                                                                       
AttestationUrl                 {http://hgs.bastion.local/Attestation}                                        
KeyProtectionUrl               {}         
```

L'URL di attestazione per HGS è il valore della proprietà AttestationUrl.

## <a name="next-steps"></a>Passaggi successivi

- [Registrare i computer per il servizio Sorveglianza host](./always-encrypted-enclaves-host-guardian-service-register.md)