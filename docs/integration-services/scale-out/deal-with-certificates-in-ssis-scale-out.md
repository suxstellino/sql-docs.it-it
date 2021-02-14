---
title: Gestire i certificati in SQL Server Integration Services Scale Out | Microsoft Docs
description: Questo articolo descrive come gestire i certificati per proteggere le comunicazioni tra SSIS Scale Out Master e le istanze di Scale Out Worker.
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.custom: performance
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 0577e6f0e0d1bd78b71b2945977c313ff6a36afc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347435"
---
# <a name="manage-certificates-for-sql-server-integration-services-scale-out"></a>Gestire i certificati in SQL Server Integration Services Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Per proteggere la comunicazione tra Scale Out Master e istanze di Scale Out Worker, SSIS Scale Out usa due certificati, uno per Scale Out Master e uno per le istanze di Scale Out Worker. 

## <a name="scale-out-master-certificate"></a>Certificato di Scale Out Master

Nella maggior parte dei casi, il certificato di Scale Out Master viene configurato durante l'installazione di Scale Out Master.

Nella pagina **Configurazione di Integration Services Scale Out - Nodo master** dell'Installazione guidata di SQL Server, è possibile scegliere di creare un nuovo certificato TLS/SSL autofirmato oppure usare un certificato TLS/SSL esistente.

![Configurazione Master](media/master-config.PNG)

**Nuovo certificato**. Se non si hanno requisiti speciali per i certificati, è possibile scegliere di creare un nuovo certificato TLS/SSL autofirmato. In questo certificato è anche possibile specificare i nomi comuni (CN). Verificare che il nome host dell'endpoint master che sarà poi usato dalle istanze di Scale Out Worker sia incluso nei nomi comuni. Per impostazione predefinita, sono inclusi il nome del computer e l'indirizzo IP del nodo master. 

**Certificato esistente**. Se si sceglie di usare un certificato esistente, fare clic su **Sfoglia** per selezionare un certificato TLS/SSL dall'archivio certificati **Radice** del computer locale.

### <a name="change-the-scale-out-master-certificate"></a>Modificare il certificato di Scale Out Master

Potrebbe essere necessario modificare il certificato di Scale Out Master perché scaduto o per altri motivi. Per modificare il certificato di Scale Out Master, eseguire le operazioni seguenti:

#### <a name="1-create-a-tlsssl-certificate"></a>1. Creare un certificato TLS/SLL.
Creare e installare un nuovo certificato TLS/SLL nel nodo master con il comando seguente:

```dos
MakeCert.exe -n CN={master endpoint host} SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```
Ad esempio:

```dos
MakeCert.exe -n CN=MasterMachine SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```

#### <a name="2-bind-the-certificate-to-the-master-port"></a>2. Associare il certificato alla porta master
Controllare l'associazione originale con il comando seguente:

```dos
netsh http show sslcert ipport=0.0.0.0:{Master port}
```

Ad esempio:

```dos
netsh http show sslcert ipport=0.0.0.0:8391
```

Eliminare l'associazione originale e configurare la nuova associazione con i comandi seguenti:

```dos
netsh http delete sslcert ipport=0.0.0.0:{Master port}
netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={TLS/SSL Certificate Thumbprint} certstorename=Root appid={original appid}
```

Ad esempio:

```dos
netsh http delete sslcert ipport=0.0.0.0:8391
netsh http add sslcert ipport=0.0.0.0:8391 certhash=01d207b300ca662f479beb884efe6ce328f77d53 certstorename=Root appid={a1f96506-93e0-4c91-9171-05a2f6739e13}
```

#### <a name="3-update-the-scale-out-master-service-configuration-file"></a>3. Aggiornare il file di configurazione del servizio Scale Out Master
Aggiornare il file di configurazione del servizio Scale Out Master `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config` nel nodo master. Aggiornare **SSLCertThumbprint** all'identificazione personale del nuovo certificato TLS/SSL.

#### <a name="4-restart-the-scale-out-master-service"></a>4. Riavviare il servizio Scale Out Master

#### <a name="5-reconnect-scale-out-workers-to-scale-out-master"></a>5. Ricollegare le istanze di Scale Out Worker a Scale Out Master
Per ogni istanza di Scale Out Worker, eliminare il ruolo di lavoro e aggiungerlo di nuovo usando [Scale Out Manager](integration-services-ssis-scale-out-manager.md) oppure eseguire le operazioni seguenti:

a.  Installare il certificato TLS/SSL client nell'archivio Radice del computer locale nel nodo di lavoro.

b.  Aggiornare il file di configurazione del servizio Scale Out Worker.

Aggiornare il file di configurazione del servizio Scale Out Worker `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config` nel nodo di lavoro. Aggiornare **MasterHttpsCertThumbprint** all'identificazione personale del nuovo certificato TLS/SSL.

c.  Riavviare il servizio Scale Out Worker.

## <a name="scale-out-worker-certificate"></a>Certificato di Scale Out Worker

Il certificato di Scale Out Worker viene generato automaticamente durante l'installazione di Scale Out Worker. 

### <a name="change-the-scale-out-worker-certificate"></a>Modificare il certificato di Scale Out Worker

Se si vuole modificare il certificato di Scale Out Worker, eseguire le operazioni seguenti:

#### <a name="1-create-a-certificate"></a>1. Creare un certificato
Creare e installare un certificato con il comando seguente:

```dos
MakeCert.exe -n CN={worker machine name};CN={worker machine ip} SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

Ad esempio:

```dos
MakeCert.exe -n CN=WorkerMachine;CN=10.0.2.8 SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

#### <a name="2-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-worker-node"></a>2. Installare il certificato client nell'archivio Radice del computer locale nel nodo di lavoro

#### <a name="3-grant-service-access-to-the-certificate"></a>3. Consentire al servizio l'accesso al certificato
Eliminare il certificato precedente e consentire al servizio Scale Out Worker l'accesso al nuovo certificato con il comando seguente:

```dos
certmgr.exe /del /c /s /r localmachine My /n {CN of the old certificate}
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the new certificate} -a {the account running Scale Out Worker service}
```

Ad esempio:

```dos
certmgr.exe /del /c /s /r localmachine My /n WorkerMachine
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s WorkerMachine -a SSISScaleOutWorker140
```

#### <a name="4-update-the-scale-out-worker-service-configuration-file"></a>4. Aggiornare il file di configurazione del servizio Scale Out Worker
Aggiornare il file di configurazione del servizio Scale Out Worker `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config` nel nodo di lavoro. Aggiornare **WorkerHttpsCertThumbprint** all'identificazione personale del nuovo certificato.

#### <a name="5-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-master-node"></a>5. Installare il certificato client nell'archivio Radice del computer locale nel nodo master

#### <a name="6-restart-the-scale-out-worker-service"></a>6. Riavviare il servizio Scale Out Worker

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni, vedere gli articoli seguenti:
-   [Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
