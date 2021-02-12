---
title: Errore di accesso in modalità AD - Dominio non trusted
titleSuffix: SQL Server Big Data Cluster
description: Correzione del comportamento - I client non riescono a eseguire l'autenticazione quando le voci DNS degli endpoint vengono configurate come CNAME che punta a un nome di alias.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 05/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 41145555b16dc437a4cb8f47ec490077f8aa8db3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045551"
---
# <a name="symptom-ad-mode-login-fails---untrusted-domain-big-data-clusters"></a>Sintomo: Errore di accesso in modalità AD - Dominio non trusted (cluster Big Data)

In un cluster Big Data (BDC) di SQL Server in modalità Active Directory, un tentativo di connessione potrebbe non riuscire e il tentativo di connessione restituisce l'errore seguente:

`Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.`

Questa situazione può verificarsi quando vengono configurate voci DNS come CNAME che punta a un nome di alias del proxy inverso che distribuisce il traffico ai nodi Kubernetes.

## <a name="root-cause"></a>Causa radice

Quando gli endpoint vengono configurati con voci DNS con CNAME che punta a un nome di alias del proxy inverso che distribuisce il traffico ai nodi Kubernetes:

- Il processo di autenticazione Kerberos cerca un nome dell'entità servizio (SPN) corrispondente alla voce per CNAME; non si tratta del vero nome SPN registrato da BDC in Active Directory
- Errore di autenticazione 

## <a name="confirm-root-cause"></a>Confermare la causa radice

Dopo l'errore di autenticazione, controllare se sono presenti ticket Kerberos nella cache. 

Per controllare la cache dei ticket, usare il comando `klist`. 

Cercare un ticket con un nome SPN corrispondente all'endpoint a cui si è tentato di connettersi.

Il ticket previsto non è presente.

In questo esempio, un endpoint master, il record DNS `bdc-sql` è CNAME impostato sul proxy inverso denominato `ServerReverseProxy`

```PowerShell
Resolve-DnsName bdc-sql
```

La sezione seguente mostra i risultati del comando precedente.

```
Name                           Type   TTL   Section    NameHost
----                           ----   ---   -------    --------
bdc-sql.mydomain.com           CNAME  3600  Answer     ReverseProxyServer.mydomain.com

Name       : ReverseProxyServer.mydomain.com
QueryType  : A
TTL        : 3600
Section    : Answer
IP4Address : 193.168.5.10
```

>[!NOTE]
>La sezione seguente fa riferimento a [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html). `tshark` è un'utilità della riga di comando installata come parte dell'utilità di traccia di rete [Wireshark](https://www.wireshark.org/docs/)).

Per visualizzare il nome SPN richiesto da Active Directory, usare `tshark`. Il comando seguente limita l'acquisizione della traccia di rete alla comunicazione del protocollo Kerberos e visualizza solo messaggi `krb-error (30)`. Questi messaggi devono contenere messaggi per richieste SPN non riuscite.

```bash
tshark -Y "kerberos && kerberos.msg_type == 30" -T fields -e kerberos.error_code -e kerberos.SNameString
```

Da una shell dei comandi diversa, provare a connettersi al pod master:

```bash
klist purge

sqlcmd -S bdc-sql.mydomain.com,31433 -E
```

Vedere l'esempio di output seguente.

```bash
klist purge

Current LogonId is 0:0xf6b58
        Deleting all tickets:
        Ticket(s) purged!

sqlcmd -S bdc-sql.mydomain.com,31433 -E
sqlcmd: Error: Microsoft ODBC Driver 17 for SQL Server : Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.
```

Controllare l'output di `tshark`. 

```bash
Capturing on 'Ethernet 3'
25      krbtgt,RLAZURE.COM
7       MSSQLSvc,ReverseProxyServer.mydomain.com:31433
2 packets captured
```

Si notino le richieste del client `SPN MSSQLSvc,ReverseProxyServer.mydomain.com:31433` che non esistono. Il tentativo di connessione ha infine esito negativo con l'errore 7. L'errore 7 indica `KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN Server not found in Kerberos database`.

Nella configurazione corretta il client richiede il nome SPN registrato dal BDC. Nell'esempio il nome SPN corretto sarebbe stato `MSSQLSvc,bdc-sql.mydomain.com:31433`.

>[!NOTE]
>L'errore 25 indica `KDC_ERR_PREAUTH_REQUIRED` - necessaria pre-autenticazione aggiuntiva. Questo errore può essere ignorato. `KDC_ERR_PREAUTH_REQUIRED` viene restituito nella richiesta AD Kerberos iniziale. Per impostazione predefinita, il client Kerberos di Windows non include le informazioni di pre-autenticazione nella prima richiesta.

Per visualizzare l'elenco di SPN registrato da BDC per l'endpoint master, eseguire `setspn -L mssql-master`. 

Vedere l'esempio di output seguente:

```bash
Registered ServicePrincipalNames for CN=mssql-master,OU=bdc,DC=mydomain,DC=com:
        MSSQLSvc/bdc-sqlread.mydomain.com:31436
        MSSQLSvc/-sqlread:31436
        MSSQLSvc/bdc-sqlread.mydomain.com
        MSSQLSvc/bdc-sqlread
        MSSQLSvc/bdc-sql.mydomain.com:31433
        MSSQLSvc/bdc-sql:31433
        MSSQLSvc/bdc-sql.mydomain.com
        MSSQLSvc/bdc-sql
        MSSQLSvc/master-p-svc.mydomain.com:1533
        MSSQLSvc/master-p-svc:1533
        MSSQLSvc/master-p-svc.mydomain.com:1433
        MSSQLSvc/master-p-svc:1433
        MSSQLSvc/master-p-svc.mydomain.com
        MSSQLSvc/master-p-svc
        MSSQLSvc/master-svc.mydomain.com:1533
        MSSQLSvc/master-svc:1533
        MSSQLSvc/master-svc.mydomain.com:1433
        MSSQLSvc/master-svc:1433
        MSSQLSvc/master-svc.mydomain.com
        MSSQLSvc/master-svc
```

Nei risultati sopra riportati l'indirizzo del proxy inverso non deve essere registrato.

## <a name="resolve"></a>Risolvi

In questa sezione vengono illustrati due modi per risolvere il problema. Dopo avere apportato le modifiche appropriate, eseguire `ipconfig -flushdns` e `klist purge` nel client. Ritentare quindi la connessione.

### <a name="option-1"></a>Opzione 1

Rimuovere il record CNAME per ogni endpoint BDC in DNS e sostituire con più record `A` che puntano a ogni nodo Kubernetes o a ogni master Kubernetes in presenza di più di un master.

>[!TIP]
>Lo script descritto di seguito usa PowerShell. Per altre informazioni, vedere [Installazione di PowerShell in Linux](/powershell/scripting/install/installing-powershell-core-on-linux).

È possibile usare lo script di PowerShell seguente per aggiornare i record degli endpoint DNS. Eseguire lo script da qualsiasi computer connesso allo stesso dominio:

```powershell
#Specify the DNS server, example contoso.local
$Domain_DNS_name=mydomain.com'

#DNS records for bdc endpoints
$Controller_DNS_name = 'bdc-control'
$Managment_proxy_DNS_name= 'bdc-proxy'
$Master_Primary_DNS_name = 'bdc-sql'
$Master_Secondary_DNS_name = 'bdc-sqlread'
$Gateway_DNS_name = 'bdc-gateway'
$AppProxy_DNS_name = 'bdc-appproxy'

#Performing Endpoint DNS records Checks..

#Build array of endpoints 
$BdcEndpointsDns = New-Object System.Collections.ArrayList

[void]$BdcEndpointsDns.Add($Controller_DNS_name)
[void]$BdcEndpointsDns.Add($Managment_proxy_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Primary_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Secondary_DNS_name)
[void]$BdcEndpointsDns.Add($Gateway_DNS_name)
[void]$BdcEndpointsDns.Add($AppProxy_DNS_name)

#Build arrary for results 
$BdcEndpointsDns_Result = New-Object System.Collections.ArrayList

foreach ($DnsName in $BdcEndpointsDns) {
    try {
        $endpoint_DNS_record = Resolve-DnsName $DnsName -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop 
        foreach ($ip in $endpoint_DNS_record.IPAddress) {
            [void]$BdcEndpointsDns_Result.Add("OK - $DnsName is an A record with an IP $ip")
        }
    }
    catch {
        [void]$BdcEndpointsDns_Result.Add("MisConfiguration - $DnsName is not an A record or does not exists")
    }  
}

#show the results 
$BdcEndpointsDns_Result
```

### <a name="option-2"></a>Opzione 2

In alternativa, è possibile aggirare il problema modificando il CNAME in modo che punti all'indirizzo IP del proxy inverso anziché al nome del proxy inverso.

## <a name="confirm-resolution"></a>Confermare la risoluzione

Dopo aver corretto il problema con una delle opzioni precedenti, verificare la correzione connettendosi a un cluster Big Data con Active Directory.

## <a name="next-steps"></a>Passaggi successivi

[Verificare la voce DNS inversa (record PTR) per il controller di dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
