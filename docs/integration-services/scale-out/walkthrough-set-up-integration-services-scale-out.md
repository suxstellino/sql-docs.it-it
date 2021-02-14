---
title: 'Procedura dettagliata: Configurare SSIS Scale Out | Microsoft Docs'
description: Informazioni dettagliate sull'installazione e la configurazione di SQL Server Integration Services (SSIS) Scale Out Master.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: d25471ef16b88ee0280ecb7dd349762b72e35312
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337539"
---
# <a name="walkthrough-set-up-integration-services-ssis-scale-out"></a>Procedura dettagliata: Installare Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Installare [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) Scale Out completando le attività seguenti. 

> [!TIP]
> Se si installa Scale Out in un solo computer, installare contemporaneamente le funzionalità Scale Out Master e Scale Out Worker. Quando si installano le due funzionalità nello stesso momento, l'endpoint viene generato automaticamente per la connessione al master di scalabilità orizzontale. 

* [Installare il master di scalabilità orizzontale](#InstallMaster)

* [Installare il ruolo di lavoro di scalabilità orizzontale](#InstallWorker)

* [Installare il certificato client del ruolo di lavoro di scalabilità orizzontale](#InstallCert)

* [Aprire la porta del firewall](#Firewall)

* [Avviare i servizi Master di scalabilità orizzontale e Ruolo di lavoro di scalabilità orizzontale di SQL Server](#Start)

* [Abilitare il master di scalabilità orizzontale](#EnableMaster)

* [Abilitare la modalità di autenticazione di SQL Server](#EnableAuth)

* [Abilitare il ruolo di lavoro di scalabilità orizzontale](#EnableWorker)

## <a name="install-scale-out-master"></a><a name="InstallMaster"></a> Installare il master di scalabilità orizzontale

Per impostare Scale Out Master, è necessario installare i servizi del motore di database, [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)], e la funzionalità Scale Out Master di SSIS durante l'installazione di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. 

Per informazioni su come installare i servizi del motore di database e [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)], vedere [Installare il motore di database di SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md) e [Installare Integration Services](../install-windows/install-integration-services.md).

> [!NOTE]
> Per usare l'account di autenticazione SQL Server predefinito per la registrazione di Scale Out, selezionare Modalità mista come modalità di autenticazione nella pagina **Configurazione del motore di database** durante l'installazione del motore di database. Per altre informazioni, vedere [Modificare l'account per la registrazione di Scale Out](change-logdb-account.md).

Per installare la funzionalità Scale Out Master, usare l'installazione guidata di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] o il prompt dei comandi.

### <a name="install-scale-out-master-with-the-sql-server-installation-wizard"></a>Installare Scale Out Master con l'installazione guidata di SQL Server
1.  Nella pagina **Selezione funzionalità** selezionare la funzionalità **Scale Out Master** elencata sotto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].   
  
    ![Selezione della funzionalità Master](media/feature-select-master.PNG)
  
2.  Nella pagina **Configurazione server** selezionare l'account per l'esecuzione del **servizio Master di scalabilità orizzontale di SQL Server Integration Services** e selezionare **Tipo di avvio**.  
    ![Configurazione server](media/server-config.PNG)

3.  Nella pagina **Configurazione del master di scalabilità orizzontale di Integration Services** specificare il numero di porta che verrà usato dal master per comunicare con il ruolo di lavoro. Il numero di porta predefinito è 8391.  

    ![Configurazione Master](media/master-config.PNG "Configurazione Master")

4.  Specificare il certificato TLS/SSL usato per proteggere la comunicazione tra Scale Out Master e Scale Out Worker eseguendo una delle operazioni seguenti.
    * Fare clic su **Crea un nuovo certificato SSL** per creare automaticamente un certificato TLS/SSL autofirmato predefinito.  Il certificato predefinito viene installato in Autorità di certificazione radice attendibili, Computer locale. In questo certificato è possibile specificare i nomi comuni (CN). Il nome host dell'endpoint master deve essere incluso nei nomi comuni. Per impostazione predefinita, sono inclusi il nome del computer e l'IP del nodo master.
    * Selezionare un certificato TLS/SSL esistente nel computer locale facendo clic su **Usa un certificato SSL esistente** e poi su **Sfoglia** per selezionare un certificato. Nella casella di testo viene visualizzata l'identificazione personale del certificato. Facendo clic su **Sfoglia** vengono visualizzati i certificati archiviati in Autorità di certificazione radice attendibili, Computer locale. Il certificato selezionato deve essere archiviato in questo percorso.       

    ![Configurazione Master 2](media/master-config-2.PNG "Configurazione Master 2")
  
5.  Completare l'Installazione guidata di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-master-from-the-command-prompt"></a>Installare Scale Out Master al prompt dei comandi

Seguire le istruzioni riportate in [Installazione di SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Impostare i parametri relativi a Scale Out Master nel modo seguente:
 
1.  Aggiungere `IS_Master` al parametro `/FEATURES`

2.  Configurare Scale Out Master specificando i parametri seguenti e i relativi valori:
    -   `/ISMASTERSVCACCOUNT`
    -   `/ISMASTERSVCPASSWORD`
    -   `/ISMASTERSVCSTARTUPTYPE`
    -   `/ISMASTERSVCPORT`
    -   `/ISMasterSVCSSLCertCN` (facoltativo)
    -   `/ISMASTERSVCTHUMBPRINT` (facoltativo)

    > [!NOTE]
    > Se Scale Out Master non è installato insieme al motore di database e l'istanza del motore di database è un'istanza denominata, è necessario configurare `SqlServerName` nel file di configurazione del servizio di Scale Out Master dopo l'installazione. Per altre informazioni, vedere [Scale Out Master](integration-services-ssis-scale-out-master.md).

## <a name="install-scale-out-worker"></a><a name="InstallWorker"></a> Installare il ruolo di lavoro di scalabilità orizzontale
 
Per impostare Scale Out Worker, è necessario installare [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] e la funzionalità Scale Out Worker durante la procedura di installazione di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

Per installare la funzionalità Scale Out Worker, usare l'installazione guidata di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] o il prompt dei comandi.

### <a name="install-scale-out-worker-with-the-sql-server-installation-wizard"></a>Installare Scale Out Worker con l'installazione guidata di SQL Server

1.  Nella pagina **Selezione funzionalità** selezionare la funzionalità **Scale Out Worker** elencata sotto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

    ![Selezione della funzionalità Worker](media/feature-select-worker.PNG)

2.  Nella pagina **Configurazione server** selezionare l'account per l'esecuzione del **servizio Ruolo di lavoro di scalabilità orizzontale di SQL Server Integration Services** e selezionare **Tipo di avvio**.

    ![Configurazione Server 2](media/server-config-2.PNG "Configurazione Server 2")

3.  Nella pagina **Integration Services Scale Out Worker Configuration** specificare l'endpoint per la connessione al servizio Scale Out Worker. 

    - Per un ambiente con **computer singolo**, l'endpoint viene generato automaticamente quando Scale Out Master e Scale Out Worker vengono installati contemporaneamente. 

    - Per un ambiente con **più computer**, l'endpoint è costituito dal nome o dall'IP del computer in cui è installato Scale Out Master e dal numero di porta specificato durante l'installazione di Scale Out Master.
   
    ![Configurazione Worker 1](media/worker-config.PNG "Configurazione Worker 1")    

    > [!NOTE]
    > È anche possibile ignorare la configurazione di Worker in questo punto e associare l'istanza di Scale Out Worker a Scale Out Master usando [Scale Out Manager](integration-services-ssis-scale-out-manager.md) dopo l'installazione.

4. Per un ambiente con **più computer**, specificare il certificato TLS/SSL client che viene usato per convalidare Scale Out Master. Per un ambiente con **computer singolo**, non è necessario specificare un certificato TLS/SSL client. 
  
    Fare clic su **Sfoglia** per trovare il file del certificato (con estensione cer). Per usare il certificato TLS/SSL predefinito, selezionare il file `SSISScaleOutMaster.cer` presente in `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn` nel computer in cui è installato Scale Out Master.   

    ![Configurazione Worker 2](media/worker-config-2.PNG "Configurazione Worker 2")

    > [!NOTE]
    > Quando il certificato TLS/SSL usato da Scale Out Master è autofirmato, è necessario installare un certificato TLS/SSL client corrispondente nel computer in cui è installato Scale Out Worker. Se si specifica il percorso di file del certificato TLS/SSL client nella pagina **Configurazione del ruolo di lavoro di scalabilità orizzontale di Integration Services**, il certificato verrà installato automaticamente. In caso contrario, sarà poi necessario installarlo manualmente. 
     
5. Completare l'Installazione guidata di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-worker-from-the-command-prompt"></a>Installare Scale Out Worker al prompt dei comandi

Seguire le istruzioni riportate in [Installazione di SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Impostare i parametri relativi a Scale Out Worker nel modo seguente:

1.  Aggiungere IS_Worker al parametro `/FEATURES`.

2. Configurare Scale Out Worker specificando i parametri seguenti e i relativi valori:
    -   `/ISWORKERSVCACCOUNT`
    -   `/ISWORKERSVCPASSWORD`
    -   `/ISWORKERSVCSTARTUPTYPE`
    -   `/ISWORKERSVCMASTER` (facoltativo)
    -   `/ISWORKERSVCCERT` (facoltativo)
 
## <a name="install-scale-out-worker-client-certificate"></a><a name="InstallCert"></a> Installare il certificato client del ruolo di lavoro di scalabilità orizzontale

Durante l'installazione di Scale Out Worker, nel computer viene creato e installato automaticamente un certificato del ruolo di lavoro. Viene anche installato un certificato client corrispondente, SSISScaleOutWorker.cer, in `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn`. Per consentire a Scale Out Master di autenticare l'istanza di Scale Out Worker, è necessario aggiungere questo certificato client all'archivio radice del computer locale in cui è installato Scale Out Master.
  
Per aggiungere il certificato client all'archivio radice, fare doppio clic sul file con estensione cer e quindi fare clic su **Installa certificato** nella finestra di dialogo Certificato. Viene visualizzata l'**Importazione guidata certificati** .  

## <a name="open-firewall-port"></a><a name="Firewall"></a> Aprire la porta del firewall

Nel computer con Scale Out Master aprire la porta specificata durante l'installazione dell'istanza di Scale Out Master e la porta di SQL Server (1433 per impostazione predefinita) usando Windows Firewall.

> [!Note]
> Dopo aver aperto la porta del firewall, è anche necessario riavviare il servizio Scale Out Worker.
    
## <a name="start-sql-server-scale-out-master-and-worker-services"></a><a name="Start"></a> Avviare i servizi Master di scalabilità orizzontale e Ruolo di lavoro di scalabilità orizzontale di SQL Server

Se il tipo di avvio dei servizi non è stato impostato su **Automatico** durante l'installazione, avviare i servizi seguenti:

-   SQL Server Integration Services Scale Out Master 14.0 (SSISScaleOutMaster140)

-   SQL Server Integration Services Scale Out Worker 14.0 (SSISScaleOutWorker140)

## <a name="enable-scale-out-master"></a><a name="EnableMaster"></a> Abilitare il master di scalabilità orizzontale

Quando si crea il catalogo SSISDB in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio_md](../../includes/ssmanstudio-md.md)], fare clic su **Abilita questo server come SSIS Scale Out Master** nella finestra di dialogo **Crea catalogo**.

Dopo aver creato il catalogo, è possibile abilitare Scale Out Master con [Scale Out Manager](integration-services-ssis-scale-out-manager.md).

## <a name="enable-sql-server-authentication-mode"></a><a name="EnableAuth"></a> Abilitare la modalità di autenticazione di SQL Server
Se l'autenticazione [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] non è stata abilitata durante l'installazione del motore di database, abilitare la modalità di autenticazione di SQL Server nell'istanza di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] che ospita il catalogo SSISDB. 

Quando l'autenticazione di SQL Server è disabilitata, l'esecuzione dei pacchetti non viene bloccata. Il log di esecuzione non può tuttavia scrivere nel database SSISDB.

## <a name="enable-scale-out-worker"></a><a name="EnableWorker"></a> Abilitare il ruolo di lavoro di scalabilità orizzontale

È possibile abilitare Scale Out Worker con [Scale Out Manager](integration-services-ssis-scale-out-manager.md), che offre un'interfaccia utente grafica, o con una stored procedure.

Per abilitare un'istanza di Scale Out Worker con una stored procedure, eseguire la stored procedure `[catalog].[enable_worker_agent]` con **WorkerAgentId** come parametro. 

Dopo la registrazione di Scale Out Worker con Scale Out Master, viene restituito il valore **WorkerAgentId** dalla vista `[catalog].[worker_agents]` in SSISDB. Dopo l'avvio dei servizi Scale Out Master e Worker, la registrazione richiede alcuni minuti.

#### <a name="example"></a>Esempio
L'esempio seguente abilita Scale Out Worker su `computerA`.

```sql
SELECT WorkerAgentId, MachineName FROM [catalog].[worker_agents]
GO
-- Result: --
-- WorkerAgentId                           MachineName  --
-- 6583054A-E915-4C2A-80E4-C765E79EF61D    computerA    --

EXEC [catalog].[enable_worker_agent] '6583054A-E915-4C2A-80E4-C765E79EF61D'
GO 
```

## <a name="next-steps"></a>Passaggi successivi
-   [Eseguire pacchetti nel servizio Integration Services (SSIS) Scale Out](run-packages-in-integration-services-ssis-scale-out.md).