---
title: Sicurezza di Riesecuzione distribuita
description: Questo articolo descrive la procedura di configurazione della sicurezza per Riesecuzione distribuita di SQL Server e include importanti considerazioni relative alle procedure di protezione e rimozione dei dati.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 907fbe327e712c4e9a6aa989a33822baa6122a07
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836878"
---
# <a name="distributed-replay-security"></a>Sicurezza di Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Prima di installare e usare la funzionalità Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario leggere le importanti informazioni sulla sicurezza riportate in questo argomento. In questo argomento vengono descritti i passaggi per la configurazione della sicurezza post-installazione che è necessario eseguire prima di poter utilizzare Distributed Replay. Questo argomento illustra anche considerazioni importanti sulla protezione dei dati e importanti passaggi relativi alla rimozione.  
  
## <a name="user-and-service-accounts"></a>Account utente e di servizio  
 Nella tabella seguente vengono descritti gli account utilizzati per Distributed Replay. Dopo l'installazione di Distributed Replay, è necessario assegnare le entità di sicurezza con cui verranno eseguiti gli account dei servizi controller e client. È pertanto consigliabile configurare gli account utente di dominio corrispondenti prima di installare le funzionalità di Distributed Replay.  
  
|Account utente|Requisiti|  
|------------------|------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Account del servizio controller di Riesecuzione distribuita|Può essere un account utente di dominio o locale. Se si utilizza un account utente locale, lo strumento di amministrazione, il controller e il client devono essere tutti eseguiti nello stesso computer.<br /><br /> **\*\* Nota sulla sicurezza \*\*** È consigliabile evitare che l'account sia un membro del gruppo Administrators locale in Windows.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Account del servizio client di Riesecuzione distribuita|Può essere un account utente di dominio o locale. Se si utilizza un account utente locale, il controller, il client e l'istanza di SQL Server di destinazione devono essere tutti eseguiti nello stesso computer.<br /><br /> **\*\* Nota sulla sicurezza \*\*** È consigliabile evitare che l'account sia un membro del gruppo Administrators locale in Windows.|  
|Account utente interattivo utilizzato per eseguire lo strumento Distributed Replay Administration Tool|Può essere un account utente locale o di dominio. Per utilizzare un account utente locale, lo strumento di amministrazione e il controller devono essere eseguiti nello stesso computer.|  
  
 **Importante**: quando si configura il controller di Riesecuzione distribuita, è possibile specificare uno o più account utente da usare per eseguire i servizi client di Riesecuzione distribuita. Di seguito viene fornito l'elenco degli account supportati:  
  
-   Account utente di dominio  
  
-   Account utente locale creato dall'utente  
  
-   Amministratore  
  
-   Account virtuale e account del servizio gestito  
  
-   Servizi di rete, Servizi locali e Sistema  
  
 Gli account di gruppo (locale o dominio) e gli altri account predefiniti (come Everyone) non sono accettati.  
  
 Per impostare gli account di servizio o le relative password dopo aver installato Distributed Replay, è possibile utilizzare lo strumento Servizi Windows. Per modificare gli account di servizio associati ai servizi di Distributed Replay Controller o Client, effettuare le operazioni seguenti:  
  
1.  Effettuare una delle operazioni seguenti a seconda del sistema operativo in uso:  
  
    -   Fare clic sul menu **Start**, digitare **services.msc** nella casella **Cerca** , quindi premere INVIO.  
  
    -   Fare clic sul menu **Start**, scegliere **Esegui**, digitare **services.msc**, quindi premere INVIO.  
  
2.  Nella finestra di dialogo **Servizi** fare clic con il pulsante destro del mouse sul servizio che si vuole configurare, quindi scegliere **Proprietà**.  
  
3.  Nella scheda **Accedi** fare clic su **Account seguente**.  
  
4.  Configurare l'account utente che si desidera utilizzare.  
  
## <a name="file-and-folder-permissions"></a>Autorizzazioni per file e cartelle  
 Dopo aver specificato gli account del servizio, è necessario concedere le necessarie autorizzazioni per file e cartelle a tali account del servizio. Configurare le autorizzazioni per file e cartelle in base alla tabella seguente:  
  
|Account|Autorizzazioni per le cartelle|  
|-------------|------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Account del servizio controller di Riesecuzione distribuita|`<Controller_Installation_Path>\DReplayController` (Lettura, Scrittura, Eliminazione)<br /><br /> `DReplayServer.xml` file (Lettura, Scrittura)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Account del servizio client di Riesecuzione distribuita|`<Client_Installation_Path>\DReplayClient` (Lettura, Scrittura, Eliminazione)<br /><br /> `DReplayClient.xml` file (Lettura, Scrittura)<br /><br /> Le directory di lavoro e dei risultati, secondo quanto specificato nel file di configurazione del client rispettivamente tramite gli elementi `WorkingDirectory` e `ResultDirectory` . (Lettura, Scrittura)|  
  
## <a name="dcom-permissions"></a>Autorizzazioni DCOM  
 DCOM viene utilizzato per la comunicazione RPC (Remote Procedure Call) tra il controller e lo strumento di amministrazione e tra il controller e tutti i client. È necessario configurare le autorizzazioni DCOM a livello di computer e specifiche dell'applicazione sul controller dopo l'installazione delle funzionalità di Distributed Replay.  
  
 Per configurare le autorizzazioni DCOM del controller, effettuare le seguenti operazioni:  
  
1.  **Aprire dcomcnfg.exe, lo snap-in Servizi componenti**: si tratta dello strumento usato per configurare le autorizzazioni DCOM.  
  
    1.  Nel computer controller fare clic sul menu **Start**.  
  
    2.  Digitare **dcomcnfg.exe** nella casella **Cerca** .  
  
    3.  Premere INVIO.  
  
2.  **Configurare le autorizzazioni DCOM a livello di computer**: concedere le autorizzazioni DCOM a livello di computer corrispondenti per ogni account elencato nella tabella seguente. Per altre informazioni sull'impostazione di autorizzazioni a livello di computer, vedere [Elenco di controllo: Gestire le applicazioni DCOM](/windows/win32/com/setting-machine-wide-security-using-dcomcnfg).  
  
3.  **Configurare le autorizzazioni DCOM specifiche dell'applicazione**: concedere le autorizzazioni DCOM corrispondenti specifiche dell'applicazione per ogni account elencato nella tabella seguente. Il nome dell'applicazione DCOM per il servizio controller è **DReplayController**. Per altre informazioni sull'impostazione di autorizzazioni specifiche dell'applicazione, vedere [Elenco di controllo: Gestire le applicazioni DCOM](/windows/win32/com/setting-processwide-security-using-dcomcnfg).  
  
 Nella tabella seguente sono descritte le autorizzazioni DCOM richieste per l'account utente interattivo dello strumento di amministrazione e per gli account del servizio client:  
  
|Funzionalità|Account|Autorizzazioni DCOM necessarie sul controller|  
|-------------|-------------|---------------------------------------------|  
|Distributed Replay Administration Tool|Account utente interattivo|Accesso locale<br /><br /> Accesso remoto<br /><br /> Avvio locale<br /><br /> Avvio remoto<br /><br /> Attivazione locale<br /><br /> Attivazione remota|  
|Client Riesecuzione distribuita|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Account del servizio client di Riesecuzione distribuita|Accesso locale<br /><br /> Accesso remoto<br /><br /> Avvio locale<br /><br /> Avvio remoto<br /><br /> Attivazione locale<br /><br /> Attivazione remota|  
  
> [!IMPORTANT]  
>  Per facilitare la protezione da query dannose o attacchi Denial of Service, accertarsi di utilizzare solo un account utente attendibile per l'account del servizio client. Questo account sarà in grado di connettersi e riprodurre i carichi di lavoro sull'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="sql-server-permissions"></a>Autorizzazioni di SQL Server  
 Gli account del servizio client di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono usati per la connessione all'istanza di destinazione del carico di lavoro di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per queste connessioni è supportata solo la modalità Autenticazione di Windows.  
  
 Dopo avere installato il servizio client di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un set di computer, all'entità di sicurezza usata per tali account di servizio è necessario concedere il ruolo del server sysadmin per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si prevede di riprodurre il carico di lavoro dei file di traccia. Questo passaggio non viene eseguito automaticamente durante l'esecuzione del programma di installazione di Distributed Replay.  
  
## <a name="data-protection"></a>Protezione dei dati  
 Nell'ambiente Distributed Replay, agli account utente seguenti viene concesso l'accesso completo all'istanza del server di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ai dati di traccia di input e ai file di traccia dei risultati:  
  
-   Account utente interattivo utilizzato per eseguire lo strumento di amministrazione.  
  
-   Account del servizio Controller.  
  
-   Account del servizio Client.  
  
-   Membri del gruppo Administrators locale sul controller.  
  
-   Membri del gruppo Administrators locale sui client.  
  
    > [!IMPORTANT]  
    >  Questi account dispongono dell'accesso completo alle informazioni personali o alle informazioni riservate contenute nei file di dati di traccia, intermedi, di recapito o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzati da Distributed Replay.  
  
 È consigliabile adottare le seguenti precauzioni relative alla sicurezza:  
  
-   Archiviare i dati di traccia di input, i risultati di traccia di output e i file di database in un percorso che utilizza il file system NTFS (NTFS) e applicare gli elenchi di controllo di accesso (ACL) appropriati. Se è necessario, crittografare i dati archiviati nel computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tenere presente che gli ACL non vengono applicati ai file di traccia e non viene eseguito alcun tipo di mascheramento dei dati. Questi file dovranno essere eliminati rapidamente una volta utilizzati.  
  
-   Applicare gli ACL e i criteri di conservazione appropriati a tutti i file intermedi e di recapito generati da Distributed Replay.  
  
-   Per la protezione del trasporto di rete usare Transport Layer Security (TLS), denominato in precedenza Secure Sockets Layer (SSL).  
  
## <a name="important-removal-steps"></a>Importanti passaggi relativi alla rimozione  
 È consigliabile utilizzare Distributed Replay solo in un ambiente di testing. Dopo avere completato i test e prima di eseguire il provisioning di tali computer per un'attività diversa, accertarsi di effettuare le operazioni seguenti:  
  
-   Disinstallare le funzionalità di Distributed Replay e rimuovere i file di configurazione correlati dal controller e da tutti i client.  
  
-   Eliminare qualsiasi file di traccia, intermedio, di recapito e di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzati per i test. I file intermedi e di recapito vengono archiviati nella directory di lavoro rispettivamente nel controller e nel client.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Install Distributed Replay - Overview](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
