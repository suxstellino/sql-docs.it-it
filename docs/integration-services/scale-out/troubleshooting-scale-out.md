---
title: Risoluzione dei problemi relativi a SSIS Scale Out | Microsoft Docs
description: Risolvere i problemi comuni relativi a SSIS Scale Out come gli errori di configurazione e la mancanza di autorizzazioni di accesso.
ms.custom: performance
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 7d9c02b6912019304b37d3d536312c6a5707f769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337413"
---
# <a name="troubleshoot-scale-out"></a>Risolvere i problemi di Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SSIS Scale Out richiede la comunicazione tra il database del catalogo SSIS `SSISDB`, il servizio Scale Out Master e il servizio Scale Out Worker. In alcuni casi la comunicazione viene interrotta a causa di errori di configurazione, mancanza di autorizzazioni di accesso e per altri motivi. Questo articolo contiene le informazioni necessarie per risolvere i problemi relativi alla configurazione di Scale Out.

Per analizzare i sintomi riscontrati, seguire uno alla volta i passaggi illustrati di seguito fino alla risoluzione del problema.

## <a name="scale-out-master-fails"></a>Errore di Scale Out Master

### <a name="symptoms"></a>Sintomi 
-   Scale Out Master non si connette al database SSISDB. 

-   Non è possibile visualizzare le proprietà master in Scale Out Manager.

-   Le proprietà master non vengono popolate nella vista `[catalog].[master_properties]`.

### <a name="solution"></a>Soluzione
1.  Verificare che Scale Out sia abilitato.

    Fare clic con il pulsante destro del mouse su **SSISDB** in Esplora oggetti di SSMS e selezionare **La funzionalità Scale Out è abilitata**.

    ![Verificare che Scale Out sia abilitato](media/isenabled.PNG)

    Se il valore della proprietà è False, abilitare Scale Out chiamando la stored procedure `[catalog].[enable_scaleout]`.

2.  Verificare che il nome SQL Server specificato nel file di configurazione di Scale Out Master sia corretto e riavviare il servizio Scale Out Master.

## <a name="scale-out-worker-fails"></a>Errore di Scale Out Worker

### <a name="symptoms"></a>Sintomi 
-   Scale Out Worker non si connette a Scale Out Master.

-   Dopo l'aggiunta in Scale Out Manager, Scale Out Worker non viene visualizzato.

-   Scale Out Worker non viene visualizzato nella vista `[catalog].[worker_agents]`.

-   Il servizio Scale Out Worker è in esecuzione, ma Scale Out Worker è offline.

### <a name="solution"></a>Soluzione
Controllare i messaggi di errore nel log del servizio Scale Out Worker in `\<drive\>:\Users\\*[account running worker service]*\AppData\Local\SSIS\Cluster\Agent`.

## <a name="no-endpoint-listening"></a>Nessun endpoint in ascolto

### <a name="symptoms"></a>Sintomi

*"System.ServiceModel.EndpointNotFoundException: Non è disponibile alcun endpoint in ascolto su https://* [NomeComputer]:[Porta] */ClusterManagement/ in grado di accettare il messaggio."*

### <a name="solution"></a>Soluzione

1.  Verificare che il numero di porta specificato nel file di configurazione del servizio Scale Out Master sia corretto e riavviare il servizio Scale Out Master. 

2.  Verificare che l'endpoint master specificato nel file di configurazione del servizio Scale Out Worker sia corretto e riavviare il servizio Scale Out Worker.

3.  Verificare che la porta del firewall sia aperta nel nodo Scale Out Master.

4.  Risolvere altri eventuali problemi di connessione tra il nodo Scale Out Master e il nodo Scale Out Worker.

## <a name="could-not-establish-trust-relationship"></a>Impossibile stabilire una relazione di trust

### <a name="symptoms"></a>Sintomi
*""System.ServiceModel.Security.SecurityNegotiationException: Non è stato possibile stabilire la relazione di trust per il canale sicuro SSL/TLS con l'autorità '[Nome Computer]:[Porta]'."*

*"System.Net.WebException: Connessione sottostante chiusa: Impossibile stabilire una relazione di trust per il canale sicuro SSL/TLS."*

*"System.Security.Authentication.AuthenticationException: Il certificato remoto non è stato ritenuto valido dalla procedura di convalida."*

### <a name="solution"></a>Soluzione
1.  Installare il certificato di Scale Out Master nell'archivio radice dei certificati del computer locale nel nodo Scale Out Worker se il certificato non è ancora installato e riavviare il servizio Scale Out Worker.

2.  Verificare che il nome host nell'endpoint master sia incluso nei nomi comuni (CN) del certificato di Scale Out Master. In caso contrario, reimpostare l'endpoint master nel file di configurazione di Scale Out Worker e riavviare il servizio Scale Out Worker. 

    > [!NOTE]
    > Se non è possibile modificare il nome host dell'endpoint master a causa di impostazioni DNS, sarà necessario modificare il certificato di Scale Out Master. Vedere [Gestire i certificati per SSIS Scale Out](deal-with-certificates-in-ssis-scale-out.md).

3.  Verificare che l'identificazione personale master specificata nella configurazione di Scale Out Worker corrisponda all'identificazione personale del certificato di Scale Out Master. 

## <a name="could-not-establish-secure-channel"></a>Impossibile stabilire un canale sicuro

### <a name="symptoms"></a>Sintomi

*"System.ServiceModel.Security.SecurityNegotiationException: Impossibile stabilire un canale sicuro per SSL/TLS con l'autorità '[Nome Computer]:[Porta]'."*

*"System.Net.WebException: Richiesta annullata: Impossibile creare un canale sicuro SSL/TLS."*

### <a name="solution"></a>Soluzione
Verificare che l'account che esegue il servizio Scale Out Worker abbia accesso al certificato di Scale Out Worker usando il comando seguente:

```dos
winhttpcertcfg.exe -l -c LOCAL_MACHINE\MY -s {CN of the worker certificate}
```

Se l'account non ha accesso, concederlo usando il comando seguente e riavviare il servizio Scale Out Worker.

```dos
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the worker certificate} -a {the account running Scale Out Worker service}
```

## <a name="http-request-forbidden"></a>Richiesta HTTP non consentita

### <a name="symptoms"></a>Sintomi

*"System.ServiceModel.Security.MessageSecurityException: La richiesta HTTP non è consentita con lo schema di autenticazione client 'Anonimo'."*

*"System.Net.WebException: Errore del server remoto: (403) Accesso negato."*

### <a name="solution"></a>Soluzione
1.  Installare il certificato di Scale Out Worker nell'archivio radice dei certificati del computer locale nel nodo Scale Out Master se il certificato non è ancora installato e riavviare il servizio Scale Out Worker.

2.  Eseguire la pulizia dei certificati inutili nell'archivio radice dei certificati del computer locale nel nodo Scale Out Master.

3.  Configurare Schannel in modo da non inviare più l'elenco delle autorità di certificazione radice attendibili durante il processo di handshake TLS/SSL aggiungendo la voce del Registro di sistema seguente nel nodo Scale Out Master.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nome valore: **SendTrustedIssuerList** 

    Tipo valore: **REG_DWORD** 

    Dati valore: **0 (False)**

4.  Se non è possibile pulire tutti i certificati non autofirmati come descritto nel passaggio 2, impostare il valore della chiave del Registro di sistema seguente su 2.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nome valore: **ClientAuthTrustMode** 

    Tipo valore: **REG_DWORD** 

    Dati valore: **2**

    > [!NOTE]
    > In presenza di certificati non autofirmati nell'archivio certificati radice, l'autenticazione del certificato client non riesce. Per altre informazioni, vedere [Internet Information Services (IIS) 8 potrebbe rifiutare le richieste di certificati client con gli errori HTTP 403.7 o 403.16](https://support.microsoft.com/help/2802568/internet-information-services-iis-8-may-reject-client-certificate-requ).

## <a name="http-request-error"></a>Errore di richiesta HTTP

### <a name="symptoms"></a>Sintomi

*"System.ServiceModel.CommunicationException: Errore durante la richiesta HTTP a https://[Nome Computer]:[Porta]/ClusterManagement/. È possibile che il certificato server non sia configurato correttamente con HTTP.SYS nel caso HTTPS o che il binding di sicurezza tra il client e il server non corrisponda."*

### <a name="solution"></a>Soluzione
1.  Verificare che il certificato di Scale Out Master sia associato correttamente alla porta nell'endpoint master nel nodo master eseguendo il comando seguente:

    ```dos
    netsh http show sslcert ipport=0.0.0.0:{Master port}
    ```

    Verificare che l'hash del certificato visualizzato corrisponda all'identificazione personale del certificato di Scale Out Master. Se l'associazione non è corretta, reimpostarla eseguendo i comandi seguenti e riavviare il servizio Scale Out Worker.

    ```dos
    netsh http delete sslcert ipport=0.0.0.0:{Master port}
    netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={Master certificate thumbprint} certstorename=Root  appid={random guid}
    ```

## <a name="cannot-open-certificate-store"></a>Impossibile aprire l'archivio certificati

### <a name="symptoms"></a>Sintomi
Alla connessione di Scale Out Worker a Scale Out Master, la convalida in Scale Out Manager non è riuscita generando il messaggio di errore *"Non è possibile aprire l'archivio certificati nel computer".*

### <a name="solution"></a>Soluzione

1.  Eseguire Scale Out Manager come amministratore. Se si apre Scale Out Manager con SSMS, è necessario eseguire SSMS come amministratore.

2.  Avviare il servizio Registro di sistema remoto nel computer, se non è in esecuzione.

## <a name="execution-doesnt-start"></a>Avvio dell'esecuzione impossibile

### <a name="symptoms"></a>Sintomi
Non è possibile avviare l'esecuzione in Scale Out.

### <a name="solution"></a>Soluzione

Verificare lo stato dei computer selezionati per eseguire il pacchetto nella vista `[catalog].[worker_agents]`. È necessario che almeno un ruolo di lavoro sia online e abilitato.

## <a name="no-log"></a>Nessun log

### <a name="symptoms"></a>Sintomi 
I pacchetti vengono eseguiti correttamente, ma non vengono registrati messaggi.

### <a name="solution"></a>Soluzione

Verificare che l'autenticazione di SQL Server sia consentita dall'istanza di SQL Server che ospita il database SSISDB.

> [!NOTE]  
> Se è stato modificato l'account per la registrazione di Scale Out, vedere [Modificare l'account per la registrazione di Scale Out](change-logdb-account.md) e verificare la stringa di connessione usata per la registrazione.

## <a name="error-messages-arent-helpful"></a>I messaggi di errore non sono utili

### <a name="symptoms"></a>Sintomi
I messaggi di errore nel report di esecuzione del pacchetto non sono sufficienti per la risoluzione del problema.

### <a name="solution"></a>Soluzione
Sono disponibili più log di esecuzione in `TasksRootFolder` configurati in `WorkerSettings.config`. Per impostazione predefinita, la cartella è `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. *[account]* indica l'account che esegue il servizio Scale Out Worker con il valore predefinito `SSISScaleOutWorker140`.

Per individuare il log per l'esecuzione del pacchetto con *[execution ID]* , eseguire il comando Transact-SQL seguente per ottenere il valore di *[task ID*. Quindi, individuare il nome della sottocartella contenente *[task ID]* in `TasksRootFolder`.

```sql
SELECT [TaskId]
FROM [SSISDB].[internal].[tasks] tasks, [SSISDB].[internal].[executions] executions 
WHERE executions.execution_id = *Your Execution Id* AND tasks.JobId = executions.job_id
```

> [!WARNING]
> Questa query serve solo alla risoluzione dei problemi. Le viste interne a cui si fa riferimento nella query devono poi essere modificate. 

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni, vedere gli articoli seguenti relativi all'impostazione e alla configurazione di SSIS Scale Out:
-   [Introduzione a Integration Services Scale Out (SSIS) in un singolo computer](get-started-with-ssis-scale-out-onebox.md)
-   [Procedura dettagliata: Installare Integration Services (SSIS) Scale Out](walkthrough-set-up-integration-services-scale-out.md)
