---
title: Installare Riesecuzione distribuita
titleSuffix: SQL Server Distributed Replay
description: Questo articolo descrive i modi in cui è possibile installare Riesecuzione distribuita, ovvero usando l'Installazione guidata, la finestra del prompt dei comandi o un file di configurazione.
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea1171da-f50e-4f16-bedc-5e468a46477f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: fc4c971d62ac4a54a786b426ea8f7855882fa0c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345922"
---
# <a name="install-distributed-replay"></a>Installare Riesecuzione distribuita

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

È possibile disinstallare Riesecuzione distribuita in tre modi diversi:  
  
-   [Installare Riesecuzione distribuita dall'Installazione guidata](#bkmk_wizard)  
  
-   [Installare Riesecuzione distribuita dal prompt dei comandi](#bkmk_command_prompt)  
  
-   [Installare i componenti Riesecuzione distribuita tramite un file di configurazione](#bkmk_configuration_file)  
  
##  <a name="install-distributed-replay-from-the-installation-wizard"></a><a name="bkmk_wizard"></a> Installare Riesecuzione distribuita dall'Installazione guidata  
 Installare le funzionalità di Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'Installazione guidata di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Quando si pianifica il percorso di installazione delle funzionalità, considerare gli aspetti seguenti:  
  
-   Lo strumento di amministrazione può essere installato nello stesso computer del controller di Riesecuzione distribuita o in computer diversi.  
  
-   In ogni ambiente di Riesecuzione distribuita può essere presente un solo controller.  
  
-   Il servizio client può essere installato al massimo in 16 computer (fisici o virtuali).  
  
-   Nel computer che esegue il controller di Riesecuzione distribuita è possibile installare una sola istanza del servizio client. Se nell'ambiente di Riesecuzione distribuita sarà presente più di un client, si sconsiglia di installare il servizio client nello stesso computer del controller, poiché tale operazione potrebbe causare la diminuzione della velocità complessiva di Riesecuzione distribuita.  
  
-   Per gli scenari di test delle prestazioni, non si consiglia di installare lo strumento di amministrazione, ovvero il servizio controller di Riesecuzione distribuita, o il servizio client nell'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'installazione di tutte queste caratteristiche nel server di destinazione deve essere limitata al test funzionale per la compatibilità dell'applicazione.  
  
-   Dopo l'installazione, prima di avviare il servizio client Riesecuzione distribuita nei client è necessario che il servizio controller, ovvero il controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sia in esecuzione.  
  
> [!NOTE]  
>  Per rimuovere o modificare le funzionalità di Riesecuzione distribuita, utilizzare la finestra **Programmi e funzionalità** di Windows nel **Pannello di controllo**. Selezionare [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nella finestra **Disinstalla o modifica programma** , quindi fare clic su **Rimuovi** per aprire l'Installazione guidata di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Nella pagina **Seleziona funzionalità** selezionare le funzionalità di Riesecuzione distribuita che si desidera rimuovere.  
  
 **Prerequisiti:**  
  
-   Verificare che i computer che si desidera utilizzare dispongano dei requisiti descritti nell'argomento [Requisiti relativi a Riesecuzione distribuita](../../tools/distributed-replay/distributed-replay-requirements.md).  
  
-   Prima di iniziare questa procedura, è necessario creare gli account utente di dominio in cui verranno eseguiti i servizi controller e client. È consigliabile che tali account non siano membri del gruppo Administrators di Windows. Per altre informazioni, vedere la sezione Account utente e di servizio nell'argomento [Sicurezza di Distributed Replay](../../tools/distributed-replay/distributed-replay-security.md) .  
  
    > [!NOTE]  
    >  Se lo strumento di amministrazione, il servizio controller e il servizio client sono in esecuzione nello stesso computer è possibile utilizzare gli account utente locali.  
  
 **Percorsi di installazione:**  
  
 Presupponendo che vengano utilizzati i percorsi dei file predefiniti e un'installazione standard, la directory di base si trova in C:\Programmi\Microsoft SQL Server. In tale directory, i file binari e gli assembly vengono installati nei percorsi seguenti:  
  
-   In un sistema a 32 bit:  
  
     [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Strumenti  
  
     \- OPPURE -  
  
     \<Share Feature Directory>\Tools\\(directory della funzionalità condivisa alternativa specificata dall'utente)  
  
-   In un sistema a 64 bit:  
  
     C:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (x86)\130\Tools  
  
     \- OPPURE -  
  
     \<Share Feature Directory (x86)>\Tools\\(directory (x86) della funzionalità condivisa alternativa specificata dall'utente)  
  
#### <a name="to-install-distributed-replay-features"></a>Per installare le funzionalità di Riesecuzione distribuita  
  
1.  Per avviare l'installazione di una qualsiasi funzionalità di Riesecuzione distribuita, avviare l'Installazione guidata di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
2.  Nella pagina **Regole di supporto per l'installazione** sono riportati i problemi che si potrebbero verificare durante l'installazione dei file di supporto dell'installazione di SQL Server. Prima di continuare con l'installazione, è necessario correggere qualsiasi errore del supporto dell'installazione.  
  
3.  Nella pagina **Product Key** selezionare un pulsante di opzione per indicare se si intende installare una versione gratuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o una versione di produzione del prodotto con una chiave PID. Per altre informazioni, vedere [Edizioni e componenti di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
4.  Nella pagina **Condizioni di licenza** leggere il contratto di licenza, quindi selezionare la casella di controllo per accettarne le condizioni. Per migliorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è inoltre possibile abilitare l'opzione relativa all'utilizzo delle funzionalità e inviare report a [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  Nella pagina **File di supporto per l'installazione** fare clic su **Installa** per installare o aggiornare i file di supporto dell'installazione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
6.  Nella pagina **Impostazione ruolo** selezionare **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Installazione funzionalità**, quindi fare clic su **Avanti** per passare alla pagina **Selezione funzionalità** .  
  
7.  Nella pagina **Selezione funzionalità** specificare le funzionalità che si desidera installare.  
  
    -   Per installare lo strumento di amministrazione, selezionare **Strumenti di gestione - Di base**.  
  
    -   Per installare il servizio controller, selezionare **Controller di Riesecuzione distribuita**.  
  
    -   Per installare il servizio client, selezionare **Client Riesecuzione distribuita**.  
  
     **Importante**: quando si configura il controller di Riesecuzione distribuita, è possibile specificare uno o più account utente da usare per eseguire i servizi client di Riesecuzione distribuita. Di seguito viene fornito l'elenco degli account supportati:  
  
    -   Account utente di dominio  
  
    -   Account utente locale creato dall'utente  
  
    -   Amministratore  
  
    -   Account virtuale e account del servizio gestito  
  
    -   Servizi di rete, Servizi locali e Sistema  
  
     Gli account di gruppo (locale o dominio) e gli altri account predefiniti (come Everyone) non sono accettati.  
  
8.  Facoltativamente, fare clic sul pulsante con i puntini di sospensione (...) per modificare il percorso della directory delle funzionalità condivise.  
  
    1.  Nei computer a 32 bit il percorso di installazione predefinito è **C:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
    2.  Nei computer a 64 bit il percorso di installazione predefinito è **C:\Programmi (x86)\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
9. Al termine dell'operazione, scegliere **Avanti**.  
  
10. Nella pagina **Regole di installazione** viene convalidata la configurazione del computer tramite il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Una volta completato il processo di convalida, fare clic su **Avanti**.  
  
11. Nella pagina **Requisiti di spazio su disco** viene calcolato lo spazio su disco necessario per le funzionalità specificate. Tale spazio viene quindi confrontato con lo spazio su disco disponibile.  
  
12. Nella pagina **Segnalazione errori** specificare le informazioni che si desidera inviare a [!INCLUDE[msCoName](../../includes/msconame-md.md)] per contribuire a migliorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, l'opzione Segnalazione errori è abilitata.  
  
13. Nella pagina **Regole di configurazione per l'installazione** Controllo configurazione sistema consentirà di eseguire uno o più set di regole per convalidare la configurazione del computer con le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificate.  
  
14. Nella pagina **Installazione del programma** fare clic su **Installa**.  
  
    > [!IMPORTANT]  
    >  Dopo aver installato i componenti Riesecuzione distribuita, è necessario creare regole del firewall nei computer controller e client e concedere a ogni computer client autorizzazioni nel server di destinazione. Per altre informazioni, vedere [Completare i passaggi successivi all'installazione](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
### <a name="net-framework-security"></a>Sicurezza di .NET Framework  
 Per installare qualsiasi funzionalità di Riesecuzione distribuita, è necessario disporre di autorizzazioni di amministratore. Solo un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che dispone di autorizzazioni sysadmin può aggiungere gli account del servizio client al ruolo del server sysadmin del server di prova. Per alcune considerazioni relative alla sicurezza di Riesecuzione distribuita, vedere [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
##  <a name="install-distributed-replay-from-the-command-prompt"></a><a name="bkmk_command_prompt"></a> Installare Riesecuzione distribuita dal prompt dei comandi  
 L'installazione di una nuova istanza di Riesecuzione distribuita dal prompt dei comandi consente di specificare le funzionalità da installare e le relative modalità di configurazione. L'installazione dal prompt dei comandi supporta l'installazione, il ripristino, l'aggiornamento e la disinstallazione dei componenti Riesecuzione distribuita. Quando l'installazione viene eseguita tramite il prompt dei comandi, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è supportata la modalità non interattiva completa tramite il parametro /Q.  
  
> [!NOTE]  
>  Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.  
  
### <a name="installation-parameters"></a>Parametri di installazione  
 L'elenco delle funzionalità di livello principale include [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]e Strumenti. La funzionalità Strumenti installa gli strumenti di gestione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]e altri componenti condivisi. Per installare i componenti Riesecuzione distribuita, specificare i parametri seguenti:  
  
|Componente|Parametro|  
|---------------|---------------|  
|Controller di Riesecuzione distribuita|**DREPLAY_CTLR**|  
|Client Riesecuzione distribuita|**DREPLAY_CLT**|  
|Strumento di amministrazione|**Strumenti**|  
  
> [!IMPORTANT]  
>  Dopo aver installato i componenti Riesecuzione distribuita, è necessario creare regole del firewall nei computer controller e client e concedere a ogni computer client autorizzazioni nel server di destinazione. Per altre informazioni, vedere [Completare i passaggi successivi all'installazione](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
 Utilizzare i parametri elencati nella tabella seguente per sviluppare script della riga di comando per l'installazione.  
  
|Parametro|Descrizione|Valori supportati|  
|---------------|-----------------|----------------------|  
|/CTLRSVCACCOUNT<br /><br /> **Facoltativo**|Account del servizio per controller di Riesecuzione distribuita.|Vengono controllati l'account e la password|  
|/CTLRSVCPASSWORD<br /><br /> **Facoltativo**|Password per l'account del servizio controller di Riesecuzione distribuita.|Vengono controllati l'account e la password|  
|/CTLRSTARTUPTYPE<br /><br /> **Facoltativo**|Tipo di avvio per il servizio controller di Riesecuzione distribuita.|Automatico<br /><br /> Disabled<br /><br /> Manuale|  
|/CTLRUSERS<br /><br /> **Facoltativo**|Specifica gli utenti che dispongono di autorizzazioni per il servizio controller di Riesecuzione distribuita.|Set di stringhe di account utente in cui vengono usati i caratteri " " (spazio) come delimitatore<br /><br /> **Importante**: quando si configura il servizio controller di Riesecuzione distribuita, è possibile specificare uno o più account utente da usare per eseguire i servizi client di Riesecuzione distribuita. Di seguito viene fornito l'elenco degli account supportati:<br /><br /> Account utente di dominio<br /><br /> Account utente locale creato dall'utente<br /><br /> Amministratore<br /><br /> Amministratore<br /><br /> Account virtuale e account del servizio gestito<br /><br /> Servizi di rete, Servizi locali e Sistema<br /><br /> <br /><br /> Nota: Gli account di gruppo (locale o dominio) e gli altri account predefiniti (come Everyone) non sono accettati.|  
|/CLTSVCACCOUNT<br /><br /> **Facoltativo**|Account del servizio per client Riesecuzione distribuita.|Vengono controllati l'account e la password|  
|/CLTSVCPASSWORD<br /><br /> **Facoltativo**|Password per l'account del servizio client Riesecuzione distribuita.|Vengono controllati l'account e la password|  
|/CLTSTARTUPTYPE<br /><br /> **Facoltativo**|Tipo di avvio per il servizio client Riesecuzione distribuita.|Automatico<br /><br /> Disabled<br /><br /> Manuale|  
|/CLTCTLRNAME<br /><br /> **Facoltativo**|Nome del computer con cui il client comunica per il servizio controller di Riesecuzione distribuita.||  
|/CLTWORKINGDIR<br /><br /> **Facoltativo**|Directory di lavoro per il servizio client Riesecuzione distribuita.|Percorso valido|  
|/CLTRESULTDIR<br /><br /> **Facoltativo**|Directory dei risultati per il servizio client Riesecuzione distribuita.|Percorso valido|  
  
### <a name="sample-syntax"></a>Sintassi di esempio:  
 **Per installare il componente Controller di Riesecuzione distribuita**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CTLR /IAcceptSQLServerLicenseTerms /CTLRUSERS="domain\user1" "domain\user2" /CTLRSVCACCOUNT="domain\svcuser" /CTLRSVCPASSWORD="password" /CTLRSTARTUPTYPE=Automatic  
```  
  
 **Per installare il componente Client Riesecuzione distribuita**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CLT /IAcceptSQLServerLicenseTerms /CLTSVCACCOUNT="domain\svcuser" /CLTSVCPASSWORD="password" /CLTSTARTUPTYPE=Automatic /CLTCTLRNAME=ControllerMachineName /CLTWORKINGDIR="C:\WorkingDir" /CLTRESULTDIR="C:\ResultDir  
```  
  
##  <a name="install-distributed-replay-using-a-configuration-file"></a><a name="bkmk_configuration_file"></a> Installare i componenti Riesecuzione distribuita tramite un file di configurazione  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione consente di generare un file di configurazione in base alle impostazioni predefinite del sistema e ai dati di input dell'utente. Se si specifica che si desidera installare gli strumenti di gestione, è possibile utilizzare il file di configurazione per distribuire i tre componenti Riesecuzione distribuita, ovvero lo strumento di amministrazione, il controller di Riesecuzione distribuita e il client Riesecuzione distribuita. Sono supportate le operazioni di installazione, ripristino e disinstallazione dei componenti Riesecuzione distribuita.  
  
 Il programma di installazione supporta l'utilizzo del file di configurazione solo tramite la riga di comando. L'ordine di elaborazione dei parametri durante l'utilizzo del file di configurazione viene indicato di seguito:  
  
-   Il file di configurazione sovrascrive le impostazioni predefinite in un pacchetto.  
  
-   I valori della riga di comando sovrascrivono quelli presenti nel file di configurazione.  
  
 Per ulteriori informazioni su come utilizzare un file di configurazione, vedere [Installare SQL Server 2016 tramite un file di configurazione](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
> [!IMPORTANT]  
>  Dopo aver installato i componenti Riesecuzione distribuita, è necessario creare regole del firewall nei computer controller e client e concedere a ogni computer client autorizzazioni nel server di destinazione. Per altre informazioni, vedere [Completare i passaggi successivi all'installazione](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
#### <a name="to-generate-a-configuration-file"></a>Per generare un file di configurazione  
  
1.  Seguire l'Installazione guidata nella pagina **Inizio installazione** . Il percorso del file di configurazione viene specificato nella pagina **Inizio installazione** nella sezione relativa al percorso del file di configurazione.  
  
2.  Annullare l'installazione senza completarla per generare il file INI.  
  
#### <a name="to-install-distributed-replay-using-the-configuration-file"></a>Per installare i componenti Riesecuzione distribuita tramite il file di configurazione  
  
-   Eseguire l'installazione tramite il prompt dei comandi e specificare il file ConfigurationFile.ini utilizzando il parametro ConfigurationFile.  
  
 **Sintassi di esempio**  
  
 Di seguito viene fornito un esempio di come specificare il file di configurazione al prompt dei comandi:  
  
```
Setup.exe /CTLRSVCPASSWORD="ctlrsvcpswd" /CLTSVCPASSWORD="cltsvcpswd" / ConfigurationFile=ConfigurationFile.INI\  
```

> [!NOTE]
> È necessario specificare entrambe le password nella riga di comando perché non è possibile configurarle nel file di configurazione.  

## <a name="see-also"></a>Vedere anche

- [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)
- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Requisiti relativi a Riesecuzione distribuita](../../tools/distributed-replay/distributed-replay-requirements.md)
- [Opzioni della riga di comando dello strumento di amministrazione &#40;Utilità Riesecuzione distribuita&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)
- [Configurare Riesecuzione distribuita](../../tools/distributed-replay/configure-distributed-replay.md)