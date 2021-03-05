---
title: Aggiornare AZDATA_PASSWORD
description: Aggiornare manualmente `AZDATA_PASSWORD`
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/03/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a77adecbe4a5498c80e0f88c169fae1bced1a8ba
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186064"
---
# <a name="manually-update-azdata_password"></a>Aggiornare `AZDATA_PASSWORD` manualmente

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Se il funziona o meno [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] con Active Directory integrazione, `AZDATA_PASSWORD` viene impostato durante la distribuzione. Offre un'autenticazione di base per il controller del cluster e l'istanza master. In questo documento viene descritto come aggiornare manualmente `AZDATA_PASSWORD`.

## <a name="change-azdata_password-for-controller"></a>Modificare `AZDATA_PASSWORD` per il controller

Se il cluster funziona in modalità non Active Directory, aggiornare la password del gateway Apache Knox seguendo questa procedura:

1. Ottenere le credenziali del controller eseguendo [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] i comandi seguenti:

   a. Eseguire questo comando come amministratore di Kubernetes:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Decodificare in Base64 il segreto:

   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. In una finestra di comando separata esporre la porta del server di database del controller:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Usare la password dell'amministratore di sistema appena ottenuta per connettersi al server di database del controller da uno strumento client SQL.

1. Generare una nuova password complessa per `AZDATA_USERNAME` per sostituire la `AZDATA_PASSWORD` esistente.

   Per semplificare l'esempio, i passaggi successivi usano "newPassword" perché la password generata è "newPassword". 

1. Ottenere `hexsalt` dalla tabella users:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` restituisce una stringa esadecimale casuale, ad esempio `64FC59DF31244FFEE02F457BC0750226`.

1. Crittografare la nuova password complessa usando `hexsalt`:

   Per praticità, viene messo a disposizione uno strumento predefinito `pbkdf2` per crittografare la password. Scaricare l'app .NET Core appropriata per la piattaforma per [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries) .

   L'app è indipendente e non richiede prerequisiti, ad esempio i runtime .NET. Per crittografare la password, eseguire:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Aggiornare la password nella tabella users:

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Modificare `AZDATA_PASSWORD` nell'istanza master di SQL Server

1. Connettersi all'endpoint SQL master con un utente amministratore.

1. Per modificare la password per le credenziali di accesso definite durante la distribuzione nel parametro `AZDATA_USERNAME`, eseguire il comando TSQL seguente:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>Aggiornamento manuale della password per Grafana e Kibana

Dopo aver seguito i passaggi per aggiornare AZDATA_PASSWORD, si noterà che [Grafana](app-monitor.md) e [Kibana](cluster-logging-kibana.md) accettano ancora la vecchia password. Questo perché Grafana e Kibana non hanno visibilità sul nuovo segreto Kubernetes. È necessario aggiornare manualmente la password per Grafana e Kibana separatamente.

## <a name="update-grafana-password"></a>Aggiornare la password di Grafana

Seguire queste opzioni per aggiornare manualmente la password per [Grafana](app-monitor.md).

1. L'utilità htpasswd è obbligatoria. È possibile installarlo in qualsiasi computer client.
  
### <a name="for-ubuntu"></a>[Per Ubuntu](#tab/for-ubuntu)
In Ubuntu Linux è possibile utilizzare quanto segue:
```bash
sudo apt install apache2-utils
```
### <a name="for-rhel"></a>[Per RHEL](#tab/for-rhel)
In Red Hat Enterprise Linux è possibile utilizzare quanto segue:
```bash
sudo yum install httpd-tools
```
---

2. Genera la nuova password. 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    Sostituire i valori per/ <username/> ,/ <password/> ,/ <secret/> in base alle esigenze, ad esempio:
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. Codificare ora la password:
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    Mantenere la stringa Base64 di output per un momento successivo.
    
4. Modificare quindi mgmtproxy-Secret:
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. Aggiornare il controller-login-htpasswd con la nuova stringa Base64 della password codificata generata in precedenza:
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. Identificare ed eliminare il Pod mgmtproxy. 
     
    Se necessario, identificare il nome del mgmtproxy prod.
    
    ### <a name="for-windows"></a>[Per Windows](#tab/for-windows)
     In un server Windows è possibile utilizzare quanto segue:
    ```bash
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    ### <a name="for-linux"></a>[Per Linux](#tab/for-linux)
     In Linux è possibile usare gli elementi seguenti:
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    ---

     Rimuovere il Pod mgmtproxy:
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. Attendere che il Pod mgmtproxy torni online e che il dashboard di Grafana venga avviato.  
 
    L'attesa non è significativa e il pod dovrebbe essere online entro pochi secondi. Per controllare lo stato del Pod è possibile usare lo stesso `get pods` comando usato nel passaggio precedente. 

    Se viene visualizzato lo stato pronto per il Pod mgmtproxy, usare kubectl per descrivere il pod: 

    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```   

    Per la risoluzione dei problemi e l'ulteriore raccolta dei log, usare il comando dell'interfaccia della riga di comando di Azure `[azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md)`   

    
8. A questo punto, accedere a Grafana con la nuova password. 


## <a name="update-the-kibana-password"></a>Aggiornare la password di Kibana

Seguire queste opzioni per aggiornare manualmente la password per [Kibana](cluster-logging-kibana.md).

> [!NOTE]
> Il browser Microsoft Edge precedente non è compatibile con Kibana, è necessario usare il browser basato su cromo perimetrale per visualizzare correttamente il dashboard. Quando si caricano i dashboard usando un browser non supportato, viene visualizzata una pagina vuota. vedere [browser supportati per Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

1. Aprire l'URL Kibana.
    
    È possibile trovare l'URL dell'endpoint del servizio Kibana dall'interno di [Azure Data Studio](manage-with-controller-dashboard.md#controller-dashboard)oppure usare il comando **azdata** seguente:
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    ad esempio https://11.111.111.111:30777/kibana/app/kibana#/discover

2. Nel riquadro sinistro fare clic sull'opzione **sicurezza** .
    
    ![Screenshot del menu nel riquadro a sinistra di Kibana, con l'opzione di sicurezza scelta](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. Nella pagina sicurezza fare clic su **database utente interno** sotto i backend di autenticazione dell'intestazione.

    ![Screenshot della pagina sicurezza con la casella database utente interno scelta.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. A questo punto, verrà visualizzato l'elenco degli utenti nel database degli utenti interni dell'intestazione. Utilizzare questa pagina per aggiungere, modificare e rimuovere gli utenti per l'accesso all'endpoint Kibana. Per l'utente che necessita della password aggiornata, fare clic sul pulsante **modifica** nella parte destra dell'utente.

    ![Screenshot della pagina del database utente interno. Nell'elenco di utenti, per l'utente KubeAdmin, viene scelto il pulsante modifica.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. Immettere la nuova password due volte e fare clic su **Submit (Invia**):

    ![Screenshot del modulo di modifica interno dell'utente. È stata immessa una nuova password nei campi password e Ripeti password.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. Chiudere il browser e riconnettersi all'URL Kibana usando la password aggiornata.

> [!Note]
> Dopo aver eseguito l'accesso con la nuova password, se vengono visualizzate pagine vuote in Kibana, disconnettersi manualmente usando l'opzione Disconnetti nell'angolo superiore destro e accedere di nuovo.

## <a name="see-also"></a>Vedi anche

* [azdata BDC (interfaccia della riga di comando di Azure Data)](../azdata/reference/reference-azdata-bdc.md)  
* [Monitorare le applicazioni con azdata e il dashboard Grafana](app-monitor.md)   
* [Estrarre i log del cluster con il dashboard di Kibana](cluster-logging-kibana.md)   