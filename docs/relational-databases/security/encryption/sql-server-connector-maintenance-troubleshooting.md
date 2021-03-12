---
title: Manutenzione e risoluzione dei problemi del Connettore SQL Server
description: Informazioni sulle istruzioni di manutenzione e i passaggi comuni per la risoluzione dei problemi per il Connettore SQL Server.
ms.custom: seo-lt-2019
ms.date: 10/08/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7bbd8aee97ccd31649661032c76729ad89b7fe5a
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622747"
---
# <a name="sql-server-connector-maintenance--troubleshooting"></a>Manutenzione e risoluzione dei problemi del Connettore SQL Server

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Informazioni supplementari sul Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sono disponibili in questo argomento. Per altre informazioni sul Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Procedura di installazione di Extensible Key Management con l'insieme di credenziali delle chiavi di Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) e [Usare Connettore SQL Server con le funzionalità di crittografia SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
##  <a name="a-maintenance-instructions-for-ssnoversion-connector"></a><a name="AppendixA"></a> A. Istruzioni di manutenzione per Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
### <a name="key-rollover"></a>Rollover della chiave  
  
> [!IMPORTANT]  
> Il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] richiede che nel nome della chiave vengano usati solo i caratteri "a-z", "A-Z", "0-9" e "-", con un limite di 26 caratteri.
> Versioni diverse della chiave con lo stesso nome di chiave nell'insieme di credenziali delle chiavi di Azure non funzioneranno con il Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per ruotare una chiave di Azure Key Vault usata da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], è necessario creare una nuova chiave con un nuovo nome.  
  
 In genere, il controllo delle versioni delle chiavi asimmetriche del server per la crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve essere eseguito ogni 1-2 anni. È importante notare che, anche se l'insieme di credenziali delle chiavi consente il controllo delle versioni delle chiavi, i clienti non dovrebbero usare questa funzionalità per implementare il controllo delle versioni. Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non può gestire le modifiche alla versione della chiave nell'insieme di credenziali delle chiavi. Per implementare il controllo delle versioni delle chiavi, creare una nuova chiave nell'insieme di credenziali delle chiavi e quindi crittografare di nuovo la chiave di crittografia dei dati in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 Ecco come si ottiene questo risultato per TDE:  
  
- **In PowerShell:** creare una nuova chiave asimmetrica (con un nome diverso dalla chiave asimmetrica TDE corrente) nell'insieme di credenziali delle chiavi.  
  
    ```powershell  
    Add-AzKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
- **Con [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] o sqlcmd.exe:** usare le istruzioni seguenti, come illustrato nella sezione 3 del passaggio 3.  
  
     Importare la nuova chiave asimmetrica.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]
    FROM PROVIDER [EKM]
    WITH PROVIDER_KEY_NAME = 'Key2',
    CREATION_DISPOSITION = OPEN_EXISTING
    GO  
    ```  
  
     Creare un nuovo account di accesso da associare alla nuova chiave asimmetrica (come illustrato nelle istruzioni relative a TDE).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Creare nuove credenziali per il mapping all'account di accesso.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789='
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Scegliere il database per cui si vuole crittografare di nuovo la chiave di crittografia del database.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Crittografare di nuovo la chiave di crittografia del database.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-ssnoversion-connector"></a>Aggiornamento del Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Le versioni 1.0.0.440 e precedenti sono state sostituite e non sono più supportate negli ambienti di produzione. Le versioni 1.0.1.0 e successive sono supportate negli ambienti di produzione. Usare le istruzioni seguenti per eseguire l'aggiornamento alla versione più recente disponibile nell'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=45344).

### <a name="upgrade"></a>Aggiornamento

1. Arrestare il servizio SQL Server usando Gestione configurazione SQL Server
1. Disinstallare la versione precedente usando Pannello di controllo\Programmi\Programmi e funzionalità
    1. Nome applicazione: Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.300.96 (o versione precedente)
    1. Data del file DLL: 30/01/2018 15:00 (o precedente)
1. Installare (aggiornare) il nuovo Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.2000.367
    1. Data del file DLL: 11/09/2020 ‏‎5:17
1. Avviare il servizio SQL Server
1. Verificare che i database crittografati siano accessibili

### <a name="rollback"></a>Rollback

1. Arrestare il servizio SQL Server usando Gestione configurazione SQL Server

1. Disinstallare la nuova versione usando Pannello di controllo\Programmi\Programmi e funzionalità
    1. Nome applicazione: Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.2000.367
    1. Data del file DLL: 11/09/2020 ‏‎5:17

1. Installare la versione precedente del Connettore SQL Server per Microsoft Azure Key Vault
    1. Versione: 15.0.300.96
    1. Data del file DLL: 30/01/2018 15:00
1. Avviare il servizio SQL Server

1. Verificare che i database che usano TDE siano accessibili.  
  
1. Dopo aver verificato il corretto funzionamento dell'aggiornamento, è possibile eliminare la cartella precedente di Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se si è scelto di rinominarla invece di disinstallarla nel passaggio 3.

### <a name="older-versions-of-the-sql-server-connector"></a>Versioni precedenti del Connettore SQL Server
  
Collegamenti diretti a versioni precedenti del Connettore SQL Server

- Corrente: [1.0.5.0 (versione 15.0.2000.367) - Data del file 11 settembre 2020](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/1033_15.0.2000.367/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.5.0 (versione 15.0.300.96) - Data del file 30 gennaio 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.4.0: (versione 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)

### <a name="rolling-the-ssnoversion-service-principal"></a>Rollover dell'entità servizio di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa le entità servizio create in Azure Active Directory come credenziali per accedere all'insieme di credenziali delle chiavi. L'entità servizio ha un ID client e una chiave di autenticazione. Le credenziali di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono configurate con il **nome dell'insieme di credenziali**, l' **ID client** e la **chiave di autenticazione**. La **Chiave di autenticazione** è valida per un determinato periodo di tempo (uno o due anni). Prima della scadenza è necessario generare una nuova chiave in Azure AD per l'entità servizio. Successivamente è necessario cambiare le credenziali in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] mantiene una cache per le credenziali nella sessione corrente, per cui, quando vengono modificate le credenziali, [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] deve essere riavviato.  
  
### <a name="key-backup-and-recovery"></a>Backup e ripristino delle chiavi

È importante eseguire regolarmente il backup dell'insieme di credenziali delle chiavi. In caso di perdita di una chiave asimmetrica nell'insieme di credenziali, è possibile ripristinarla dal backup. La chiave deve essere ripristinata usando lo stesso nome precedente con il comando Restore di PowerShell (vedere i passaggi successivi).  
In caso di perdita dell'insieme di credenziali, è necessario ricreare un insieme di credenziali e ripristinare la chiave asimmetrica al suo interno usando lo stesso nome assegnato in precedenza. Il nome dell'insieme di credenziali può essere diverso o uguale rispetto a prima. Impostare le autorizzazioni di accesso nel nuovo insieme di credenziali per concedere all'entità servizio di SQL Server l'accesso richiesto per gli scenari di crittografia di SQL Server e quindi modificare le credenziali di SQL Server in modo che includano il nuovo nome dell'insieme di credenziali.

In sintesi, ecco i passaggi necessari:  
  
- Eseguire il backup della chiave dell'insieme di credenziali usando il cmdlet di PowerShell Backup-AzureKeyVaultKey.  
- In caso di errore dell'insieme di credenziali, crearne uno nuovo nella stessa area geografica. L'utente che crea l'insieme di credenziali deve trovarsi nella stessa directory predefinita dell'entità servizio configurata per SQL Server.  
- Ripristinare la chiave nel nuovo insieme di credenziali usando il cmdlet di PowerShell Restore-AzureKeyVaultKey, che consente di ripristinare la chiave usando lo stesso nome che aveva in precedenza. Se esiste già una chiave con lo stesso nome, il ripristino non riesce.  
- Concedere le autorizzazioni all'entità servizio di SQL Server per l'uso di questo nuovo insieme di credenziali.
- Modificare le credenziali di SQL Server usate dal motore di database in modo da riflettere il nuovo nome dell'insieme di credenziali, se necessario.  
  
I backup delle chiavi possono essere ripristinati in aree diverse di Azure, purché rimangano nella stessa area geografica o nello stesso cloud nazionale: Stati Uniti, Canada, Giappone, Australia, India, Asia Pacifico, Europa, Brasile, Cina, US Government o Germania.  
  
##  <a name="b-frequently-asked-questions"></a><a name="AppendixB"></a> B. Domande frequenti

### <a name="on-azure-key-vault"></a>Informazioni sull'insieme di credenziali delle chiavi di Azure
  
**Come funzionano le operazioni relative alla chiave nell'insieme di credenziali delle chiavi di Azure?**  
 la chiave asimmetrica nell'insieme di credenziali delle chiavi viene usata per proteggere le chiavi di crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Solo la parte pubblica della chiave asimmetrica lascia sempre l'insieme di credenziali. La parte privata non viene mai esportata dall'insieme di credenziali. Tutte le operazioni di crittografia che usano la chiave asimmetrica vengono eseguite all'interno del servizio Azure Key Vault e sono protette dal sistema di sicurezza del servizio.  
  
 **Che cos'è un URI della chiave?**  
 Ogni chiave nell'insieme di credenziali delle chiavi di Azure ha un URI (Uniform Resource Identifier) che può essere usato per fare riferimento alla chiave dell'applicazione. Usare il formato `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` per ottenere la versione corrente e usare il formato `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` per ottenere una versione specifica.  
  
### <a name="on-configuring-ssnoversion"></a>Informazioni sulla configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**Quali sono gli endpoint a cui deve accedere Connettore SQL Server?**
Il connettore comunica con due endpoint che devono essere consentiti. L'unica porta necessaria per la comunicazione in uscita a questi altri servizi è 443 per Https:

- login.microsoftonline.com/*:443
- *.vault.azure.net/* :443

**Come si esegue la connessione ad Azure Key Vault usando un server proxy HTTP(S)?**
Il Connettore usa le impostazioni di configurazione del proxy di Internet Explorer. Queste impostazioni possono essere controllate usando [Criteri di gruppo](/archive/blogs/askie/how-to-configure-proxy-settings-for-ie10-and-ie11-as-iem-is-not-available) o il Registro di sistema, ma è importante tenere presente che non sono impostazioni a livello di sistema e dovranno essere indirizzate all'account del servizio che esegue l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se un amministratore del database visualizza o modifica le impostazioni in Internet Explorer, queste avranno effetto solo sull'account dell'amministratore del database anziché sul motore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'accesso interattivo al server usando l'account del servizio non è consigliato e viene bloccato in molti ambienti protetti. Per rendere effettive le modifiche apportate alle impostazioni proxy configurate, può essere necessario riavviare l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] perché vengono memorizzate nella cache quando il Connettore tenta per la prima volta di connettersi a un insieme di credenziali delle chiavi.

**Quali dimensioni delle chiavi Azure Key Vault sono supportate dall'Connettore SQL Server?**
La build più recente di Connettore SQL Server supporta Azure Key Vault chiavi di dimensioni 2048 e 3072.
  
 > [!NOTE] 
 > La visualizzazione "sys.asymmetric_keys" riporta le dimensioni della chiave come 2048 anche se viene usata la dimensione della chiave 3072. Si tratta di un gap noto in questa visualizzazione e il team del prodotto SQL Server lo risolverà in una versione futura.

**Quali sono i livelli minimi di autorizzazione necessari per ogni passaggio di configurazione in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]?**  
 Anche se è possibile eseguire tutti i passaggi di configurazione come membro del ruolo predefinito del server sysadmin, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] consiglia di ridurre al minimo le autorizzazioni. L'elenco seguente indica il livello di autorizzazione minima per ogni azione.  
  
- Per creare un provider di crittografia è necessaria l'autorizzazione `CONTROL SERVER` o l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
- Per modificare un'opzione di configurazione ed eseguire l'istruzione `RECONFIGURE` , è necessaria l'autorizzazione a livello di server `ALTER SETTINGS` . L'autorizzazione `ALTER SETTINGS` è assegnata implicitamente ai ruoli predefiniti del server sysadmin e **serveradmin** .  
  
- Per creare le credenziali è necessaria l'autorizzazione `ALTER ANY CREDENTIAL` .  
  
- Per aggiungere le credenziali a un account di accesso è necessaria l'autorizzazione `ALTER ANY LOGIN` .  
  
- Per creare una chiave asimmetrica è necessaria l'autorizzazione `CREATE ASYMMETRIC KEY` .  

**Come è possibile cambiare l'istanza predefinita di Active Directory in modo che l'insieme di credenziali delle chiavi venga creato nella stessa sottoscrizione dell'entità servizio di Active Directory creata per il Connettore [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] ?**

![aad change default directory helpsteps](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Andare al portale di Azure classico: [https://manage.windowsazure.com](https://manage.windowsazure.com)  
2. Selezionare **Impostazioni** nel menu a sinistra.
3. Selezionare la sottoscrizione di Azure attualmente in uso e fare clic su **Modifica directory** nei comandi nella parte inferiore della schermata.
4. Nella finestra popup usare il menu a discesa **Directory** per selezionare l'istanza di Active Directory che si vuole usare. L'istanza verrà impostata come directory predefinita.
5. Assicurarsi di essere l'amministratore globale della nuova istanza di Active Directory selezionata. Se non si è l'amministratore globale, si potrebbero perdere le autorizzazioni di gestione a causa della modifica della directory.
6. Dopo la chiusura della finestra popup, se non è visualizzata alcuna sottoscrizione, potrebbe essere necessario aggiornare il filtro **Filtra per directory** in **Sottoscrizioni** nel menu in alto a destra della schermata per visualizzare le sottoscrizioni che usano l'istanza di Active Directory appena aggiornata.

    > [!NOTE] 
    > Si potrebbe non disporre delle autorizzazioni per modificare la directory predefinita nella sottoscrizione di Azure. In questo caso, creare l'entità servizio AAD nella directory predefinita, in modo che si trovi nella stessa directory dell'insieme di credenziali delle chiavi di Azure usato successivamente.

Per altre informazioni su Active Directory, leggere [Associare le sottoscrizioni di Azure ad Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).
  
##  <a name="c-error-code-explanations-for-ssnoversion-connector"></a><a name="AppendixC"></a> C. Spiegazioni dei codici di errore per Connettore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

**Codici di errore del provider:**  
  
Codice di errore  |Simbolo  |Descrizione
---------|---------|---------  
0 | scp_err_Success | L'operazione è stata completata.
1 | scp_err_Failure | L'operazione non è riuscita.
2 | scp_err_InsufficientBuffer | Questo errore indica al motore di allocare altra memoria per il buffer.
3 | scp_err_NotSupported | L'operazione non è supportata. Ad esempio, il tipo di chiave o l'algoritmo specificato non è supportato dal provider EKM.
4 | scp_err_NotFound | Il provider EKM non ha trovato la chiave o l'algoritmo specificato.
5 | scp_err_AuthFailure | L'autenticazione con il provider EKM non è riuscita.
6 | scp_err_InvalidArgument | L'argomento specificato non è valido.
7 | scp_err_ProviderError | Si è verificato un errore non specificato nel provider EKM rilevato dal motore SQL.
401 | acquireToken | Il server ha restituito 401 per la richiesta. Verificare che l'ID client e il segreto siano corretti e che la stringa delle credenziali sia una concatenazione dell'ID client e del segreto di AAD senza trattini.
404 | getKeyByName | Il server ha risposto 404, perché il nome della chiave non è stato trovato. Assicurarsi che il nome della chiave sia presente nell'insieme di credenziali.
2049 | scp_err_KeyNameDoesNotFitThumbprint | Il nome della chiave è troppo lungo per il sistema di identificazione personale del motore SQL. Il nome della chiave non deve superare i 26 caratteri.
2050 | scp_err_PasswordTooShort | La stringa del segreto, che rappresenta la concatenazione dell'ID client e del segreto di AAD, contiene un numero di caratteri inferiore a 32.
2051 | scp_err_OutOfMemory | La memoria del motore SQL è insufficiente e non è stato possibile allocare memoria per il provider EKM.
2052 | scp_err_ConvertKeyNameToThumbprint | La conversione del nome della chiave in identificazione personale non è riuscita.
2053 | scp_err_ConvertThumbprintToKeyName|  La conversione dell'identificazione personale nel nome della chiave non è riuscita.
2058 | scp_err_FailureInRegistry|  Non è stato possibile eseguire l'operazione nel Registro di sistema. L'account del servizio SQL Server non ha l'autorizzazione per creare la chiave del Registro di sistema.
3000 | ErrorSuccess | L'operazione AKV è stata completata.
3001 | ErrorUnknown | L'operazione AKV non è riuscita con un errore non specificato.
3002 | ErrorHttpCreateHttpClientOutOfMemory | Non è possibile creare HttpClient per un'operazione AKV perché la memoria è insufficiente.
3003 | ErrorHttpOpenSession | Impossibile aprire una sessione HTTP a causa di un errore di rete.
3004 | ErrorHttpConnectSession | Impossibile connettere una sessione HTTP a causa di un errore di rete.
3005 | ErrorHttpAttemptConnect | Impossibile tentare la connessione a causa di un errore di rete.
3006 | ErrorHttpOpenRequest | Non è possibile aprire una richiesta a causa di un errore di rete.
3007 | ErrorHttpAddRequestHeader | Non è possibile aggiungere l'intestazione della richiesta.
3008 | ErrorHttpSendRequest | Non è possibile inviare una richiesta a causa di un errore di rete.
3009 | ErrorHttpGetResponseCode | Non è possibile ottenere un codice di risposta a causa di un errore di rete.
3010 | ErrorHttpResponseCodeUnauthorized | Il server ha restituito 401 per la richiesta.
3011 | ErrorHttpResponseCodeThrottled | Il server ha limitato la richiesta.
3012 | ErrorHttpResponseCodeClientError | La richiesta inviata dal connettore non è valida. Questo significa in genere che il nome della chiave non è valido o contiene caratteri non validi.
3013 | ErrorHttpResponseCodeServerError | Il server ha restituito un codice di risposta compreso tra 500 e 600.
3014 | ErrorHttpQueryHeader | Non è possibile eseguire la query per un'intestazione della risposta.
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | Non è possibile copiare l'intestazione della risposta a causa della memoria insufficiente.
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | Non è possibile eseguire la query sull'intestazione della risposta a causa della memoria insufficiente durante la riallocazione di un buffer.
3017 | ErrorHttpQueryHeaderNotFound | Non è possibile trovare l'intestazione della query nella risposta.
3018 | ErrorHttpQueryHeaderUpdateBufferLength | Non è possibile aggiornare la lunghezza del buffer quando si eseguono query sull'intestazione della risposta.
3019 | ErrorHttpReadData | Non è possibile leggere i dati della risposta a causa di un errore di rete.
3076 | ErrorHttpResourceNotFound | Il server ha risposto 404, perché il nome della chiave non è stato trovato. Assicurarsi che il nome della chiave sia presente nell'insieme di credenziali.
3077 | ErrorHttpOperationForbidden | Il server ha restituito l'errore 403 perché l'utente non ha le autorizzazioni appropriate per eseguire l'azione. Assicurarsi di avere le autorizzazioni per l'operazione specificata. Per il corretto funzionamento, il connettore richiede almeno le autorizzazioni "get, list, wrapKey, unwrapKey".
3100 | ErrorHttpCreateHttpClientOutOfMemory               | Non è possibile creare HttpClient per un'operazione AKV perché la memoria è insufficiente.
3101 | ErrorHttpOpenSession                               | Non è possibile aprire una sessione Http a causa di un errore di rete.
3102 | ErrorHttpConnectSession                            | Non è possibile connettere una sessione Http a causa di un errore di rete.
3103 | ErrorHttpAttemptConnect                            | Un tentativo di connessione non è riuscito a causa di un errore di rete.
3104 | ErrorHttpOpenRequest                               | Non è possibile aprire una richiesta a causa di un errore di rete.
3105 | ErrorHttpAddRequestHeader                          | Non è possibile aggiungere l'intestazione della richiesta.
3106 | ErrorHttpSendRequest                               | Non è possibile inviare una richiesta a causa di un errore di rete.
3107 | ErrorHttpGetResponseCode                           | Non è possibile ottenere un codice di risposta a causa di un errore di rete.
3108 | ErrorHttpResponseCodeUnauthorized                  | Il server ha restituito 401 per la richiesta. Verificare che l'ID client e il segreto siano corretti e che la stringa delle credenziali sia una concatenazione dell'ID client e del segreto di AAD senza trattini.
3109 | ErrorHttpResponseCodeThrottled                     | Il server ha limitato la richiesta.
3110 | ErrorHttpResponseCodeClientError                    | La richiesta non è valida. Questo significa in genere che il nome della chiave non è valido o contiene caratteri non validi.
3111 | ErrorHttpResponseCodeServerError                   | Il server ha restituito un codice di risposta compreso tra 500 e 600.
3112 | ErrorHttpResourceNotFound                          | Il server ha risposto 404, perché il nome della chiave non è stato trovato. Assicurarsi che il nome della chiave sia presente nell'insieme di credenziali.
3113 | ErrorHttpOperationForbidden                         | Il server ha restituito l'errore 403 perché l'utente non ha le autorizzazioni appropriate per eseguire l'azione. Assicurarsi di avere le autorizzazioni per l'operazione specificata. Sono richieste almeno le autorizzazioni "get, wrapKey, unwrapKey".
3114 | ErrorHttpQueryHeader                               | Non è possibile eseguire la query per un'intestazione della risposta.
3115 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader          | Non è possibile copiare l'intestazione della risposta a causa della memoria insufficiente.
3116 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer       | Non è possibile eseguire la query sull'intestazione della risposta a causa della memoria insufficiente durante la riallocazione di un buffer.
3117 | ErrorHttpQueryHeaderNotFound                       | Non è possibile trovare l'intestazione della query nella risposta.
3118 | ErrorHttpQueryHeaderUpdateBufferLength             | Non è possibile aggiornare la lunghezza del buffer quando si eseguono query sull'intestazione della risposta.
3119 | ErrorHttpReadData                                  | Non è possibile leggere i dati della risposta a causa di un errore di rete.
3120 | ErrorHttpGetResponseOutOfMemoryCreateTempBuffer    | Non è possibile ottenere il corpo della risposta a causa di memoria insufficiente durante la creazione di un buffer temporaneo.
3121 | ErrorHttpGetResponseOutOfMemoryGetResultString     | Non è possibile ottenere il corpo della risposta a causa di memoria insufficiente quando si ottiene la stringa di risultato.
3122 | ErrorHttpGetResponseOutOfMemoryAppendResponse      | Non è possibile ottenere il corpo della risposta a causa di memoria insufficiente durante l'accodamento della risposta.
3200 | ErrorGetAADValuesOutOfMemoryConcatPath | Non è possibile ottenere i valori dell'intestazione della verifica Azure Active Directory a causa di memoria insufficiente durante la concatenazione del percorso.
3201 | ErrorGetAADDomainUrlStartPosition | Non è possibile trovare la posizione iniziale per l'URL del dominio Azure Active Directory nell'intestazione della richiesta di verifica per la risposta con formattazione errata.
3202 | ErrorGetAADDomainUrlStopPosition | Non è possibile trovare la posizione finale per Azure Active Directory URL del dominio nell'intestazione della richiesta di verifica per la risposta con formattazione errata.
3203 | ErrorGetAADDomainUrlMalformatted | La formattazione dell'intestazione della richiesta di verifica per la risposta di Azure Active Directory è errata e non contiene l'URL del dominio AAD.
3204 | ErrorGetAADDomainUrlOutOfMemoryAlloc | Memoria insufficiente durante l'allocazione del buffer per l'URL del dominio Azure Active Directory.
3205 | ErrorGetAADTenantIdOutOfMemoryAlloc | Memoria insufficiente durante l'allocazione del buffer per l'ID tenant di Azure Active Directory.
3206 | ErrorGetAKVResourceUrlStartPosition | Non è possibile trovare la posizione iniziale per l'URL della risorsa Azure Key Vault nell'intestazione della richiesta di verifica per la risposta con formattazione errata.
3207 | ErrorGetAKVResourceUrlStopPosition | Non è possibile trovare la posizione finale per l'URL della risorsa Azure Key Vault nell'intestazione della richiesta di verifica per la risposta con formattazione errata.
3208 | ErrorGetAKVResourceUrlOutOfMemoryAlloc | Memoria insufficiente durante l'allocazione del buffer per l'URL della risorsa Azure Key Vault.
3300 | ErrorGetTokenOutOfMemoryConcatPath | Non è possibile ottenere il token a causa di memoria insufficiente durante la concatenazione del percorso della richiesta.
3301 | ErrorGetTokenOutOfMemoryConcatBody | Non è possibile ottenere il token a causa di memoria insufficiente durante la concatenazione del corpo della risposta.
3302 | ErrorGetTokenOutOfMemoryConvertResponseString | Non è possibile ottenere il token a causa di memoria insufficiente durante la conversione della stringa della risposta.
3303 | ErrorGetTokenBadCredentials | Non è possibile ottenere il token a causa di credenziali non corrette. Verificare che la stringa delle credenziali o il certificato sia valido.
3304 | ErrorGetTokenFailedToGetToken | Le credenziali sono corrette, ma l'operazione non è riuscita a ottenere un token valido.
3305 | ErrorGetTokenRejected | Il token è valido, ma viene rifiutato dal server.
3306 | ErrorGetTokenNotFound | Token non trovato nella risposta.
3307 | ErrorGetTokenJsonParser | Non è possibile analizzare la risposta JSON del server.
3308 | ErrorGetTokenExtractToken | Non è possibile estrarre il token dalla risposta JSON.
3400 | ErrorGetKeyByNameOutOfMemoryConvertResponseString | Non è possibile ottenere la chiave in base al nome a causa di memoria insufficiente per la conversione della stringa di risposta.
3401 | ErrorGetKeyByNameOutOfMemoryConcatPath | Non è possibile ottenere la chiave in base al nome a causa di memoria insufficiente durante la concatenazione del percorso.
3402 | ErrorGetKeyByNameOutOfMemoryConcatHeader | Non è possibile ottenere la chiave in base al nome a causa di memoria insufficiente durante la concatenazione dell'intestazione.
3403 | ErrorGetKeyByNameNoResponse | Non è possibile ottenere la chiave in base al nome perché il server non risponde.
3404 | ErrorGetKeyByNameJsonParser | Non è possibile ottenere la chiave in base al nome perché non è stato possibile analizzare la risposta JSON.
3405 | ErrorGetKeyByNameExtractKeyNode | Non è possibile ottenere la chiave in base al nome perché non è stato possibile estrarre il nodo chiave dalla risposta.
3406 | ErrorGetKeyByNameExtractKeyId | Non è possibile ottenere la chiave in base al nome perché non è stato possibile estrarre l'ID chiave dalla risposta.
3407 | ErrorGetKeyByNameExtractKeyType | Non è possibile ottenere la chiave in base al nome perché non è stato possibile estrarre il tipo di chiave dalla risposta.
3408 | ErrorGetKeyByNameExtractKeyN | Non è possibile ottenere la chiave in base al nome perché non è stato possibile estrarre la chiave N dalla risposta.
3409 | ErrorGetKeyByNameBase64DecodeN | Non è possibile ottenere la chiave in base al nome perché non è stato possibile decodificare N con Base64.
3410 | ErrorGetKeyByNameExtractKeyE | Non è possibile ottenere la chiave in base al nome perché non è stato possibile estrarre la chiave E dalla risposta.
3411 | ErrorGetKeyByNameBase64DecodeE | Non è possibile ottenere la chiave in base al nome perché non è stato possibile decodificare E con Base64.
3412 | ErrorGetKeyByNameExtractKeyUri | Non è possibile estrarre l'URI della chiave dalla risposta.
3500 | ErrorBackupKeyOutOfMemoryConvertResponseString | Non è possibile eseguire il backup della chiave a causa di memoria insufficiente durante la conversione della stringa di risposta.
3501 | ErrorBackupKeyOutOfMemoryConcatPath | Non è possibile eseguire il backup della chiave a causa di memoria insufficiente durante la concatenazione del percorso.
3502 | ErrorBackupKeyOutOfMemoryConcatHeader | Non è possibile eseguire il backup della chiave a causa di memoria insufficiente durante la concatenazione dell'intestazione della richiesta.
3503 | ErrorBackupKeyNoResponse | Non è possibile eseguire il backup della chiave perché il server non risponde.
3504 | ErrorBackupKeyJsonParser | Non è possibile eseguire il backup della chiave perché non è stato possibile analizzare la risposta JSON.
3505 | ErrorBackupKeyExtractValue | Non è possibile eseguire il backup della chiave perché l'estrazione del valore dalla risposta JSON non è riuscita.
3506 | ErrorBackupKeyBase64DecodeValue | Non è possibile eseguire il backup della chiave perché la decodifica con Base64 del campo del valore non è riuscita.
3600 | ErrorWrapKeyOutOfMemoryConvertResponseString | Non è possibile eseguire il wrapping della chiave a causa di memoria insufficiente durante la conversione della stringa di risposta.
3601 | ErrorWrapKeyOutOfMemoryConcatPath | Non è possibile eseguire il wrapping della chiave a causa di memoria insufficiente durante la concatenazione del percorso.
3602 | ErrorWrapKeyOutOfMemoryConcatHeader | Non è possibile eseguire il wrapping della chiave a causa di memoria insufficiente durante la concatenazione dell'intestazione.
3603 | ErrorWrapKeyOutOfMemoryConcatBody | Non è possibile eseguire il wrapping della chiave a causa di memoria insufficiente durante la concatenazione del corpo.
3604 | ErrorWrapKeyOutOfMemoryConvertEncodedBody | Non è possibile eseguire il wrapping della chiave a causa di memoria insufficiente durante la conversione del corpo codificato.
3605 | ErrorWrapKeyBase64EncodeKey | Non è possibile eseguire il wrapping della chiave perché la codifica Base64 della chiave non è riuscita.
3606 | ErrorWrapKeyBase64DecodeValue | Non è possibile eseguire il wrapping della chiave perché la decodifica Base64 del valore della risposta non è riuscita.
3607 | ErrorWrapKeyJsonParser | Non è possibile eseguire il wrapping della chiave perché l'analisi della risposta JSON non è riuscita.
3608 | ErrorWrapKeyExtractValue | Non è possibile eseguire il wrapping della chiave perché non è stato possibile estrarre il valore dalla risposta.
3609 | ErrorWrapKeyNoResponse | Non è possibile eseguire il wrapping della chiave perché il server non risponde.
3700 | ErrorUnwrapKeyOutOfMemoryConvertResponseString | Non è possibile annullare il wrapping della chiave a causa di memoria insufficiente durante la conversione della stringa di risposta.
3701 | ErrorUnwrapKeyOutOfMemoryConcatPath | Non è possibile annullare il wrapping della chiave a causa di memoria insufficiente durante la concatenazione del percorso.
3702 | ErrorUnwrapKeyOutOfMemoryConcatHeader | Non è possibile annullare il wrapping della chiave a causa di memoria insufficiente durante la concatenazione dell'intestazione.
3703 | ErrorUnwrapKeyOutOfMemoryConcatBody | Non è possibile annullare il wrapping della chiave a causa di memoria insufficiente durante la concatenazione del corpo.
3704 | ErrorUnwrapKeyOutOfMemoryConvertEncodedBody | Non è possibile annullare il wrapping della chiave a causa di memoria insufficiente durante la conversione del corpo codificato.
3705 | ErrorUnwrapKeyBase64EncodeKey | Non è possibile annullare il wrapping della chiave perché la codifica Base64 della chiave non è riuscita.
3706 | ErrorUnwrapKeyBase64DecodeValue | Non è possibile annullare il wrapping della chiave perché la decodifica Base64 del valore della risposta non è riuscita.
3707 | ErrorUnwrapKeyJsonParser | Non è possibile annullare il wrapping della chiave perché l'estrazione del valore dalla risposta non è riuscita.
3708 | ErrorUnwrapKeyExtractValue | Non è possibile annullare il wrapping della chiave perché l'estrazione del valore dalla risposta non è riuscita.
3709 | ErrorUnwrapKeyNoResponse | Non è possibile annullare il wrapping della chiave perché il server non risponde.
3800 | ErrorSecretAuthParamsGetRequestBody | Errore nella creazione del corpo della richiesta usando il clientID e il segreto di Azure Active Directory.
3801 | ErrorJWTTokenCreateHeader | Errore durante la creazione dell'intestazione del token JWT per l'autenticazione con AAD.
3802 | ErrorJWTTokenCreatePayloadGUID | Errore durante la creazione del GUID per il payload del token JWT per l'autenticazione con AAD.
3803 | ErrorJWTTokenCreatePayload | Errore durante la creazione del payload del token JWT per l'autenticazione con AAD.
3804 | ErrorJWTTokenCreateSignature | Errore durante la creazione della firma del token JWT per l'autenticazione con AAD.
3805 | ErrorJWTTokenSignatureHashAlg | Errore durante il recupero dell'algoritmo hash SHA256 per l'autenticazione con AAD.
3806 | ErrorJWTTokenSignatureHash | Errore durante la creazione dell'hash SHA256 per l'autenticazione del token JWT con AAD.
3807 | ErrorJWTTokenSignatureSignHash | Errore durante la firma dell'hash del token JWT per l'autenticazione con AAD.
3808 | ErrorJWTTokenCreateToken | Errore durante la creazione del token JWT per l'autenticazione con AAD.
3809 | ErrorPfxCertAuthParamsImportPfx | Errore durante l'importazione del certificato PFX per l'autenticazione con AAD.
3810 | ErrorPfxCertAuthParamsGetThumbprint | Errore durante il recupero dell'identificazione personale dal certificato PFX per l'autenticazione con AAD.
3811 | ErrorPfxCertAuthParamsGetPrivateKey | Errore durante il recupero della chiave privata dal certificato PFX per l'autenticazione con AAD.
3812 | ErrorPfxCertAuthParamsSignAlg | Errore durante il recupero dell'algoritmo di firma RSA per l'autenticazione del certificato PFX con AAD.
3813 | ErrorPfxCertAuthParamsImportForSign | Errore durante l'importazione della chiave privata PFX per la firma RSA per l'autenticazione con AAD.
3814 | ErrorPfxCertAuthParamsCreateRequestBody | Errore durante la creazione del corpo della richiesta dal certificato PFX per l'autenticazione con AAD.
3815 | ErrorPEMCertAuthParamsGetThumbprint | Errore di decodifica dell'identificazione digitale Base64 per l'autenticazione con AAD.
3816 | ErrorPEMCertAuthParamsGetPrivateKey | Errore nel recupero della chiave privata RSA da PEM per l'autenticazione con AAD.
3817 | ErrorPEMCertAuthParamsSignAlg | Errore nel recupero dell'algoritmo di firma RSA per l'autenticazione con chiave privata PEM con AAD.
3818 | ErrorPEMCertAuthParamsImportForSign | Errore nell'importazione della chiave privata PEM per la firma RSA per l'autenticazione con AAD.
3819 | ErrorPEMCertAuthParamsCreateRequestBody | Errore nella creazione del corpo della richiesta dalla chiave privata PEM per l'autenticazione con AAD.
3820 | ErrorLegacyPrivateKeyAuthParamsSignAlg | Errore nel recupero dell'algoritmo di firma RSA per l'autenticazione con chiave privata legacy con AAD.
3821 | ErrorLegacyPrivateKeyAuthParamsImportForSign | Errore nell'importazione della chiave privata Legacy per la firma RSA per l'autenticazione con AAD.
3822 | ErrorLegacyPrivateKeyAuthParamsCreateRequestBody        | Errore nella creazione del corpo della richiesta dalla chiave privata Legacy per l'autenticazione con AAD.
3900 | ErrorAKVDoesNotExist | Il nome Internet non è stato risolto. Questo indica in genere che Azure Key Vault è stato eliminato.
4000 | ErrorCreateKeyVaultRetryManagerOutOfMemory | Non è possibile creare RetryManager per un'operazione AKV a causa di memoria insufficiente.

Se il codice di errore non è presente in questa tabella, di seguito sono riportate alcune altre condizioni che potrebbero essere causa dell'errore:
  
- Non si ha accesso a Internet, quindi non è possibile accedere ad Azure Key Vault. Verificare la connessione Internet.  
  
- Il servizio dell'insieme di credenziali delle chiavi potrebbe non essere attivo. Riprovare in un altro momento.  
  
- La chiave asimmetrica dell'insieme di credenziali delle chiavi di Azure o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]potrebbe essere stata eliminata. Ripristinare la chiave.  
  
- Se viene visualizzato l'errore "Impossibile caricare la libreria", verificare che sia installata la versione di Visual Studio C++ Redistributable appropriata in base alla versione di SQL Server in esecuzione. La tabella seguente specifica la versione da installare dall'Area download Microsoft.

Nel registro eventi di Windows vengono inoltre registrati gli errori associati al Connettore SQL Server e questo può essere utile per avere maggiori informazioni sul motivo per cui si sta verificando l'errore. L'origine nel log eventi dell'applicazione di Windows sarà "Connettore SQL Server per Azure Key Vault".
  
Versione di SQL Server  |Collegamento di installazione ridistribuibile
---------|---------
2008, 2008 R2, 2012, 2014 | [Visual C++ Redistributable Package per Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
2016 | [Visual C++ Redistributable per Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)
  
## <a name="additional-references"></a>Riferimenti aggiuntivi

 Altre informazioni su Extensible Key Management:  
  
- [Extensible Key Management &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 Crittografie di SQL che supportano EKM:  
  
- [Abilitare TDE in SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
- [Crittografia dei backup](../../../relational-databases/backup-restore/backup-encryption.md)  
  
- [Creare un backup crittografato](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Comandi di [!INCLUDE[tsql](../../../includes/tsql-md.md)] correlati:  
  
- [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
- [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
- [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
- [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Documentazione dell'insieme di credenziali delle chiavi di Azure:  
  
- [Cos'è l'insieme di credenziali chiave di Azure?](/azure/key-vault/general/basic-concepts)  
  
- [Introduzione all'insieme di credenziali delle chiavi di Azure](/azure/key-vault/general/overview)  
  
- Riferimento di PowerShell [Cmdlet per l'insieme di credenziali delle chiavi di Azure](/powershell/module/azurerm.keyvault/)  
  
## <a name="see-also"></a>Vedere anche

 [Extensible Key Management tramite l'insieme di credenziali delle chiavi di Azure](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Usare Connettore SQL Server con le funzionalità di crittografia SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)  
 [Opzione di configurazione del server EKM provider enabled](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
 [Procedura di installazione di Extensible Key Management con l'insieme di credenziali delle chiavi di Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)

Per script di esempio aggiuntivi, vedere il blog in [Transparent Data Encryption e Extensible Key Management di SQL Server con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
