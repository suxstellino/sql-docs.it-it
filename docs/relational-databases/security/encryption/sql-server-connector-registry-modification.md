---
title: Registrazione di errori e informazioni per il Connettore SQL Server
description: Questo articolo descrive l'abilitazione degli errori e della registrazione per il Connettore SQL Server tramite la modifica di voci del Registro di sistema
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 3a674994d7028a8f4455c26d6f705b9da7b5ee08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018684"
---
# <a name="sql-server-connector-error-and-information-logging"></a>Registrazione di errori e informazioni per il Connettore SQL Server

Questo articolo descrive la modifica di voci del Registro di sistema per abilitare la registrazione di errori e informazioni per il Connettore SQL Server.

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>Connettore SQL Server per Insieme credenziali chiavi Microsoft

Il [Connettore SQL Server per Microsoft Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45344) consente di usare Microsoft Azure Key Vault come provider EKM (Extensible Key Management) per la crittografia di SQL Server per proteggere le chiavi di crittografia.

Il [download](https://www.microsoft.com/download/details.aspx?id=45344) è costituito dal Connettore SQL Server e dagli script di esempio per consentire a un amministratore di SQL Server di apprendere come configurare il Connettore SQL Server e abilitare gli scenari di crittografia di SQL Server. Per altre informazioni, vedere [Extensible Key Management tramite Key Vault (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md).

Per porre domande, condividere informazioni dettagliate e discutere del Connettore SQL Server, usare il [forum di Azure Key Vault](https://social.msdn.microsoft.com/Forums/AzureKeyVault).

> [!NOTE]
> Durante la normale esecuzione, la DLL del Connettore SQL Server creerà dinamicamente le voci del Registro di sistema per stabilire la connettività ad Azure Key Vault, per creare la chiave `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]`. L'account di avvio di SQL Server deve essere un amministratore locale o l'account del servizio deve essere **NT SERVICE\MSSQLSERVER**

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>Aggiornare il Connettore SQL Server alla versione più recente

Per aggiornare il Connettore SQL Server (versione 1.0.5.0 con data di settembre 2020) alla versione più recente del provider di crittografia DLL, seguire questa procedura.

### <a name="upgrade"></a>Aggiornamento

1. Arrestare il servizio SQL Server usando Gestione configurazione SQL Server
1. Disinstallare la versione precedente usando **Pannello di controllo\Programmi\Programmi e funzionalità**
    1. Nome applicazione: Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.300.96
    1. Data del file DLL: 30/01/2018 15:00
1. Installare (aggiornare) il nuovo Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.2000.367
    1. Data del file DLL: 11/09/2020 ‏‎5:17
1. Avviare il servizio SQL Server
1. Verificare che i database crittografati siano accessibili

### <a name="rollback"></a>Rollback

1. Arrestare il servizio SQL Server usando Gestione configurazione SQL Server
1. Disinstallare la nuova versione usando **Pannello di controllo\Programmi\Programmi e funzionalità**
    1. Nome applicazione: Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.2000.367
    1. Data del file DLL: 11/09/2020 ‏‎5:17
1. Installare la versione precedente del Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.300.96
    1. Data del file DLL: 30/01/2018 15:00
1. Avviare il servizio SQL Server
1. Verificare che i database crittografati siano accessibili

> [!NOTE]
> - Le versioni 1.0.0.440 e precedenti del Connettore SQL Server sono state sostituite e non sono più supportate negli ambienti di produzione. Per altre informazioni sulla risoluzione dei problemi del Connettore SQL Server, vedere [Manutenzione e risoluzione dei problemi del Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).
> - A partire dalla versione 1.0.3.0, il Connettore SQL Server segnala i messaggi di errore rilevanti ai registri eventi di Windows per la risoluzione dei problemi.
> - A partire dalla versione [1.0.4.0: (versione 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi), è disponibile il supporto per i cloud privati di Azure, tra cui Azure Cina, Azure Germania e Azure per enti pubblici.
> - La versione 1.0.5.0 presenta una modifica che causa un'interruzione associata all'algoritmo di identificazione personale. Dopo l'aggiornamento alla versione 1.0.5.0 può verificarsi un errore di ripristino del database. Per altre informazioni, vedere l'[articolo della Knowledge Base 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - **A partire dalla versione 1.0.5.0 (con data del file settembre 2020), il Connettore SQL Server supporta il filtro dei messaggi e la logica di ripetizione dei tentativi di richiesta di rete**.
> - *La versione precedente del Connettore SQL Server è anche la versione: [1.0.5.0 (versione 15.0.300.96) – Data del file gennaio 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)* . Se si verificano problemi, eseguire l'aggiornamento al Connettore SQL Server più recente.

**Requisiti di sistema** - Versioni di SQL Server supportate:

- SQL Server 2019 RTM Enterprise a 64 bit
- SQL Server 2017 RTM Enterprise a 64 bit
- SQL Server 2016 RTM Enterprise a 64 bit
- SQL Server 2014 RTM Enterprise a 64 bit
- SQL Server 2012 SP2 Enterprise a 64 bit
- SQL Server 2012 SP1 CU6 Enterprise a 64 bit
- SQL Server 2008 R2 SP2 CU8 Enterprise a 64 bit

Nelle versioni di SQL Server 2008 e 2012 precedenti a quelle elencate in precedenza, è necessario installare la patch specificata nell'articolo della Knowledge Base seguente: [https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713).

Il Connettore SQL Server per Microsoft Azure Key Vault richiede anche .NET versione 4.5.1 nella macchina virtuale di Microsoft SQL Server in Azure. Questa libreria deve essere installata prima di installare il Connettore SQL Server.

Verificare che sia installata la versione appropriata di Visual Studio C++ Redistributable nella versione di SQL Server in esecuzione:

- Per SQL Server versioni 2008, 2008 R2, 2012 e 2014, installare Visual C++ Redistributable versione 2013.

- Per SQL Server 2016, installare Visual C++ Redistributable versione 2015.

## <a name="modify-windows-registry-steps"></a>Procedura per modificare il valore del Registro di sistema di Windows

Modificare le voci del Registro di sistema per abilitare la registrazione degli eventi di errore e informativi del Connettore SQL Server nel log eventi dell'applicazione di Windows.

> [!CAUTION]
> Seguire attentamente la procedura descritta in questa sezione e a proprio rischio. L'errata modifica del Registro di sistema può causare problemi gravi. Prima di modificare il Registro di sistema, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso si verifichino problemi.

1. Esistono due modi per aprire l'editor del Registro di sistema in Windows 10:
    - Nella casella di ricerca sulla barra delle applicazioni digitare **regedit**. Selezionare quindi il primo risultato per Editor del Registro di sistema (app desktop).

    ![ekm regedit open](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "ekm regedit open")
    - Premere e tenere premuto oppure fare clic con il pulsante destro del mouse sul pulsante Start e quindi scegliere Esegui. Immettere **regedit** nella finestra di dialogo e selezionare **OK**.

   ![ekm regedit start](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "ekm regedit start")

1. Passare a questa chiave del Registro di sistema:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\**

    ![ekm regedit akv](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "ekm regedit akv")  

1. Aggiungere una nuova chiave in **Azure Key Vault** denominata `Log`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv log](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "ekm regedit akv log.png")  

1. Sotto la chiave **Log** aggiungere un valore DWORD (32 bit) denominato `Level`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv log dword](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "ekm regedit akv log dword")  

1. Impostare il valore di DWORD su un livello di log appropriato (0, 1, 2):
   1. 0 (informazioni) - **Impostazione predefinita**
   1. 1 (errore)
   1. 2 (nessun log)

   ![ekm regedit akv log level](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "ekm regedit akv log level")  

Le voci del Registro di sistema descritte in questo articolo sono disponibili in questa chiave:

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

Facoltativamente, è possibile usare la riga di comando per generare la chiave:

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

Anche le voci del log eventi dell'applicazione per cui mancano messaggi possono essere corrette usando una voce del Registro di sistema. Il log eventi può contenere il messaggio `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`.  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>Articoli correlati

- Per script di esempio aggiuntivi, vedere il blog in [Transparent Data Encryption e Extensible Key Management di SQL Server con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
- [Extensible Key Management (EKM)](extensible-key-management-ekm.md)  
- [Extensible Key Management con Azure Key Vault](extensible-key-management-using-azure-key-vault-sql-server.md)
- [Manutenzione e risoluzione dei problemi di Connettore SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)