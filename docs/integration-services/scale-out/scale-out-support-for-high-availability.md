---
title: Supporto della disponibilità elevata in Scale Out| Microsoft Docs
description: Informazioni su come configurare il lato SQL Server Integration Services (SSIS) Scale Out Master per la disponibilità elevata.
ms.custom: performance
ms.date: 05/23/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: dd53bf6f045687f7d79f6c0e836490adb60cda44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348859"
---
# <a name="scale-out-support-for-high-availability"></a>Supporto della disponibilità elevata in Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



In SSIS Scale Out la disponibilità elevata sul lato Scale Out Worker viene assicurata attraverso l'esecuzione di pacchetti con più istanze di Scale Out Worker.

La disponibilità elevata sul lato Scale Out Master si ottiene con [Always On per il catalogo SSIS](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) e il clustering di failover Windows. In questa soluzione vengono ospitate più istanze di Scale Out Master nel cluster di failover di Windows. Quando il servizio Scale Out Master o il database SSISDB è inattivo nel nodo primario, il servizio o il database SSISDB nel nodo secondario continua ad accettare le richieste degli utenti e a comunicare con le istanze di Scale Out Worker.

In alternativa, la disponibilità elevata sul lato Scale Out Master può essere ottenuta con l'istanza del cluster di failover di SQL Server. Vedere [Supporto di Scale Out per disponibilità elevata tramite istanza del cluster di failover di SQL Server](scale-out-failover-cluster-instance.md).

Per configurare la disponibilità elevata sul lato Scale Out Master con Always On per il catalogo SSIS, eseguire le operazioni seguenti:

## <a name="1-prerequisites"></a>1. Prerequisiti
Configurare un cluster di failover di Windows Vedere il post di blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Installazione della funzionalità di clustering di failover e degli strumenti per Windows Server 2012) per le istruzioni. Installare la funzionalità e gli strumenti in tutti i nodi del cluster.

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2. Installare Scale Out Master nel nodo primario
Installare i servizi del motore di database di SQL Server, Integration Services e Scale Out Master nel nodo primario per Scale Out Master. 

Durante l'installazione, eseguire le operazioni seguenti:

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1 Impostare l'account che esegue il servizio Scale Out Master su un account di dominio
Questo account dovrà poi poter accedere al database SSISDB nel nodo secondario nel cluster di failover di Windows. Poiché il failover del servizio Scale Out Master e del database SSISDB può essere eseguito separatamente, questi due componenti possono anche non trovarsi nello stesso nodo dopo il failover.

![Configurazione del server a disponibilità elevata](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2 Includere il nome host DNS del servizio Scale Out Master nei nomi comuni (CN) del certificato di Scale Out Master

Questo nome host è l'endpoint di Scale Out Master, che viene creato come un servizio generico nel cluster di failover (vedere il passaggio 7).   (Fornire un nome host DNS e non un nome del server.)

![Configurazione del master a disponibilità elevata](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3. Installare Scale Out Master nel nodo secondario
Installare i servizi del motore di database di SQL Server, Integration Services e Scale Out Master nel nodo secondario per Scale Out Master. 

Usare lo stesso certificato di Scale Out Master applicato nel nodo primario. Esportare il certificato TLS/SSL di Scale Out Master presente nel nodo primario con una chiave privata e installarlo nell'archivio certificati radice del computer locale nel nodo secondario. Selezionare questo certificato durante l'installazione di Scale Out Master nel nodo secondario.

![Configurazione del master a disponibilità elevata 2](media/ha-master-config2.PNG)

> [!NOTE]
> È possibile configurare più istanze di backup di Scale Out Master ripetendo queste operazioni per Scale Out Master in altri nodi secondari.

## <a name="4-set-up-and-configure-ssisdb-support-for-always-on"></a>4. Impostare e configurare il supporto SSIS per Always On

Seguire le istruzioni per impostare e configurare il supporto SSISDB per Always On in [Always On per il catalogo SSIS (SSISDB)](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb).

È anche necessario creare un listener del gruppo di disponibilità per il gruppo di disponibilità in cui viene aggiunto il database SSISDB. Vedere [Creare o configurare un listener del gruppo di disponibilità](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Aggiornare il file di configurazione del servizio Scale Out Master
Aggiornare il file di configurazione del servizio Scale Out Master `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config` nel nodo master e secondario. Aggiornare **SqlServerName** in *[Nome DNS del listener del gruppo di disponibilità],[Porta]* .

## <a name="6-enable-package-execution-logging"></a>6. Abilitare la registrazione dell'esecuzione dei pacchetti

La registrazione nel database SSISDB viene eseguita dall'account di accesso **##MS_SSISLogDBWorkerAgentLogin##** la cui password viene generata automaticamente. Per il corretto funzionamento della registrazione per tutte le repliche del database SSISDB, eseguire le operazioni seguenti

### <a name="61-change-the-password-of-ms_ssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1 Modificare la password di **##MS_SSISLogDBWorkerAgentLogin##** nell'istanza di SQL Server primaria

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2 Aggiungere l'account di accesso all'istanza di SQL Server secondaria

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3 Aggiornare la stringa di connessione usata per la registrazione.
Chiamare la stored procedure `[catalog].[update_logdb_info]` usando i valori di parametro seguenti:

-   `@server_name = '[Availability Group Listener DNS name],[Port]'`

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster"></a>7. Configurare il ruolo del servizio Scale Out Master del cluster di failover di Windows Server

1.  In Gestione cluster di failover connettersi al cluster per Scale Out. Selezionare il cluster. Selezionare **Azione** dal menu e poi **Configura ruolo**.

2.  Nella finestra di dialogo **Configurazione guidata disponibilità elevata** selezionare **Servizio generico** nella pagina **Selezione ruolo**. Selezionare SQL Server Integration Services Scale Out Master 14.0 nella pagina **Seleziona servizio**.

3.  Nella pagina **Punto di accesso client** immettere il nome host DNS del servizio Scale Out Master.

    ![Configurazione guidata disponibilità elevata 1](media/ha-wizard1.PNG)

4.  Completare la procedura guidata.

Nelle macchine virtuali di Azure questa procedura di configurazione richiede passaggi aggiuntivi. Una spiegazione completa di questi concetti e passaggi esula dall'ambito di questo articolo.

1.  È necessario configurare un dominio di Azure. Windows Server Failover Clustering richiede che tutti i computer del cluster siano membri dello stesso dominio. Per altre informazioni, vedere [Abilitare Azure Active Directory Domain Services tramite il portale di Azure](/azure/active-directory-domain-services/create-instance).

2. È necessario configurare un servizio di bilanciamento del carico di Azure. Questa operazione è un requisito per il listener del gruppo di disponibilità. Per altre informazioni, vedere [Esercitazione: Bilanciare il carico del traffico interno con un servizio di bilanciamento del carico Basic nel portale di Azure](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal).

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8. Aggiornare l'indirizzo di Scale Out Master nel database SSISDB

Nell'istanza di SQL Server primaria eseguire la stored procedure `[catalog].[update_master_address]` con il valore di parametro `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'`. 

## <a name="9-add-the-scale-out-workers"></a>9. Aggiungere le istanze di Scale Out Worker

A questo punto è possibile aggiungere le istanze di Scale Out Worker usando [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md). Immettere `[SQL Server Availability Group Listener DNS name],[Port]` nella pagina di connessione.

## <a name="upgrade-scale-out-in-high-availability-environment"></a>Aggiornare Scale Out in un ambiente a disponibilità elevata
Per aggiornare Scale Out in un ambiente a disponibilità elevata seguire la [procedura di aggiornamento di Always On for SSIS Catalog](../catalog/ssis-catalog.md#Upgrade), aggiornare Scale Out Master e Scale Out Worker in ogni computer, quindi ricreare il ruolo cluster di failover Windows Server nel passaggio 7 precedente con la nuova versione del servizio Scale Out Master.

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni, vedere gli articoli seguenti:
-   [Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)