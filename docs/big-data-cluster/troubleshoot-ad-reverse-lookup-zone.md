---
title: Distribuzione in modalità Active Directory arrestata - voce della zona di ricerca inversa mancante per il controller di dominio
titleSuffix: SQL Server Big Data Cluster
description: La distribuzione di BDC con la modalità AD è bloccata a causa di una voce di zona di ricerca inversa mancante per il controller di dominio nel server DNS del controller di dominio.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02276cee62edd604df5e018bc22cf86cddaaec03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043951"
---
# <a name="ad-mode-deployment-stopped---missing-reverse-lookup-zone-entry-for-dc"></a>Distribuzione in modalità Active Directory arrestata - voce della zona di ricerca inversa mancante per il controller di dominio

La distribuzione in modalità Active Directory (AD) si blocca. Controllare i sintomi per verificare se il server DNS del controller di dominio non è presente nella voce della zona di ricerca inversa. 

## <a name="symptom"></a>Sintomo

È stata avviata la distribuzione di BDC con la modalità AD, tuttavia la distribuzione è bloccata e non va avanti.

L'esempio seguente illustra i risultati della distribuzione in una shell Bash.

```
The privacy statement can be viewed at:
https://go.microsoft.com/fwlink/?LinkId=853010
 
The license terms for SQL Server Big Data Cluster can be viewed at:
Enterprise: https://go.microsoft.com/fwlink/?linkid=2104292
Standard: https://go.microsoft.com/fwlink/?linkid=2104294
Developer: https://go.microsoft.com/fwlink/?linkid=2104079
 
Cluster deployment documentation can be viewed at:
https://aka.ms/bdc-deploy
 
NOTE: Cluster creation can take a significant amount of time depending on
configuration, network speed, and the number of nodes in the cluster.
 
Starting cluster deployment.
Cluster controller endpoint is available at bdc-control.contoso.com:30080, 193.168.5.14:30080.
Waiting for control plane to be ready after 5 minutes.
Waiting for control plane to be ready after 10 minutes.
Waiting for control plane to be ready after 15 minutes.
Waiting for control plane to be ready after 20 minutes.
Waiting for control plane to be ready after 25 minutes.
```

Controllare i pod distribuiti correnti.

```bash
kubectl get pods -n mssql-cluster
```

I risultati riportati di seguito indicano che sono stati distribuiti solo i pod appartenenti al controller. I pod per il calcolo, i dati o l'archiviazione non vengono creati.

```
NAME              READY   STATUS    RESTARTS   AGE
control-rts5t     3/3     Running   0          18m
controldb-0       2/2     Running   0          18m
controlwd-csgst   1/1     Running   0          16m
dns-7kfnz         2/2     Running   0          16m
logsdb-0          1/1     Running   0          16m
logsui-2pc29      1/1     Running   0          16m
metricsdb-0       1/1     Running   0          16m
metricsdc-4rtm4   1/1     Running   0          16m
metricsdc-6lr2t   1/1     Running   0          16m
metricsdc-ftx9m   1/1     Running   0          16m
metricsdc-h59jb   1/1     Running   0          16m
metricsui-lvdpt   1/1     Running   0          16m
mgmtproxy-mkmxp   2/2     Running   0          16m
```

Esaminare i log del contenitore del supporto per la sicurezza. Cercare gli errori LDAP. 

## <a name="check-security-support-container"></a>Controllare il contenitore del supporto per la sicurezza 

Esaminare i log del contenitore del supporto per la sicurezza.

Il comando seguente raccoglie i log di supporto per la sicurezza in un cluster nello spazio dei nomi `mssql-cluster`.

```bash
azdata bdc debug copy-logs -n mssql-cluster -c security-support
```

Estrarre i log e individuare `\mssql-cluster\control-<identifier>\controller\control-rts5t-controller-stdout.log`.

> [!TIP]
> Esistono diversi modi per raccogliere i log. Anziché copiare i log con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], è possibile usare un notebook in Azure Data Studio.
> In Azure Data Studio connettersi al cluster Kubernetes ed eseguire un notebook appropriato per la risoluzione dei problemi. Di seguito sono elencati alcuni esempi di notebook.
>
> - TSG027 - Osservare la distribuzione del cluster
> - TSG061 - Ottenere la parte finale di tutti i log dei contenitori per i pod nello spazio dei nomi BDC
> - TSG001 - Eseguire `azdata copy-logs`
>

## <a name="inspect-the-logs"></a>Esaminare i log

Individuare il log. L'esempio seguente fa riferimento a un log di distribuzione del controller. 

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`"

```
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'cntrl-controller'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=cntrl-controller,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'ldap-user'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=ldap-user,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'nginx-mgmtproxy'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=nginx-mgmtproxy,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
```

## <a name="cause"></a>Causa

La voce della zona di ricerca inversa per il controller di dominio nella voce del DNS del controller di dominio è mancante. 

## <a name="resolution"></a>Risoluzione

Eseguire lo script di PowerShell seguente per verificare se è stata configurata una voce DNS inversa (record PTR).

```powershell
#Domain Controller FQDN 'DCserver01.contoso.local'
$Domain_controller_FQDN = 'DCserver01.contoso.local'

#Performing Domain Controller DNS record, reverse PTR Checks...
$DcControllerDnsPtr_Result = New-Object System.Collections.ArrayList
try {
    $Domain_controller_DNS_Record = Resolve-DnsName $Domain_controller_FQDN -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop
    foreach ($ip in $Domain_controller_DNS_Record.IPAddress) {
        #resolving hostname by IP address to make sure we have reverse PTR record 
        if ((Resolve-DnsName $ip).NameHost -eq $Domain_controller_FQDN) {
            [void]$DcControllerDnsPtr_Result.add("OK - $Domain_controller_FQDN has an A record with an IP $ip, Reverse PTR record is in place") 
        }
        else {
            [void]$DcControllerDnsPtr_Result.add("Missing - $Domain_controller_FQDN has an A record with an IP $ip, But no reverse PTR record was found for the host")
        }
    }
}
catch {
    [void]$DcControllerDnsPtr_Result.add("Error - " + $_.exception.message)
}

#show the results 
$DcControllerDnsPtr_Result
```

## <a name="next-steps"></a>Passaggi successivi

[Verificare la voce DNS inversa (record PTR) per il controller di dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
