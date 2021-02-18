---
title: Opzione replay nello strumento di amministrazione
titleSuffix: SQL Server Distributed Replay
description: Questo articolo descrive l'opzione della riga di comando replay e la sintassi dello strumento di amministrazione Riesecuzione distribuita di SQL Server, che avvia la fase di riproduzione dell'evento.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d7bce6a5-d414-488d-a3cd-50c1c62019c4
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: fa147de01aaa2e70615aff9e0e4ec248aa4769c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354487"
---
# <a name="replay-option-distributed-replay-administration-tool"></a>Opzione replay (strumento di amministrazione Distributed Replay)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Lo strumento di amministrazione Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **DReplay.exe**, è uno strumento da riga di comando che consente di comunicare con il controller di Riesecuzione distribuita. Questo argomento descrive l'opzione della riga di comando **replay** e la sintassi corrispondente.  
  
 L'opzione **replay** avvia la fase di riproduzione dell'evento, in cui il controller recapita i dati di riproduzione ai client specificati, avvia la riesecuzione distribuita e sincronizza i client. Ogni client che partecipa alla riproduzione può eventualmente registrare l'attività di riproduzione e salvare in locale un file di traccia dei risultati.  
  
 ![Icona di collegamento all'argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") Per altre informazioni sulle convenzioni relative alla sintassi dello strumento di amministrazione, vedere [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
dreplay replay [-m controller] -d controller_working_dir [-o]  
    [-s target_server] -w clients [-c config_file]  
    [-f status_interval]  
```  
  
#### <a name="parameters"></a>Parametri  
 **-m** _controller_  
 Specifica il nome computer del controller. È possibile utilizzare "`localhost`" o "`.`" per fare riferimento al computer locale.  
  
 Se il parametro **-m** non è specificato, viene usato il computer locale.  
  
 **-d** _controller_working_dir_  
 Specifica la directory nel controller in cui verrà archiviato il file intermedio. Il parametro **-d** è obbligatorio.  
  
 Di seguito vengono indicati i requisiti per questo parametro:  
  
-   La directory deve trovarsi nel controller.  
  
-   È necessario specificare il percorso completo, che inizia con una lettera di unità, ad esempio `c:\WorkingDir`.  
  
-   Il percorso non deve terminare con una barra rovesciata "`\`".  
  
-   I percorsi UNC non sono supportati.  
  
 **-o**  
 Acquisisce l'attività di riproduzione dei client e la salva in un file di traccia dei risultati nel percorso specificato dall'elemento `<ResultDirectory>` nel file di configurazione del client `DReplayClient.xml`.  
  
 Quando il parametro **-o** non è specificato, il file di traccia dei risultati non viene generato. L'output della console restituisce informazioni di riepilogo al termine della riproduzione, ma non sono disponibili altre statistiche di riproduzione.  
  
 **-s** _target_server_  
 Specifica l'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su cui deve essere riprodotto il carico di lavoro distribuito. È necessario specificare questo parametro nel formato **nome_server[\nome istanza]** .  
  
 Non è possibile utilizzare "`localhost`" o "`.`" come server di destinazione.  
  
 Il parametro **-s** non è obbligatorio se si specifica l'elemento `<Server>` nella sezione `<ReplayOptions>` del file di configurazione della riproduzione `DReplay.exe.replay.config`.  
  
 Se si usa il parametro **-s** , l'elemento `<Server>` nella sezione `<ReplayOptions>` del file di configurazione della riproduzione viene ignorato.  
  
 **-w** _clients_  
 Questo parametro obbligatorio è un elenco delimitato da virgole (senza spazi) che specifica i nomi computer dei client che devono partecipare alla riproduzione distribuita. Gli indirizzi IP non sono consentiti. Tenere presente che i client devono essere già registrati nel controller.  
  
> [!NOTE]  
>  Ogni client viene registrato nel controller specificato nel file di configurazione del client all'avvio del servizio client.  
  
 **-c** _config_file_  
 Percorso completo del file di configurazione della riproduzione. Viene utilizzato per specificare il percorso quando il file viene archiviato in una posizione diversa.  
  
 Il parametro **-c** non è obbligatorio se si vuole usare i valori predefiniti del file di configurazione della riproduzione `DReplay.exe.replay.config`.  
  
 **-f** _status_interval_  
 Specifica la frequenza in secondi in base alla quale visualizzare lo stato.  
  
 Se **-f** non è specificato, l'intervallo predefinito è 30 secondi.  
  
## <a name="examples"></a>Esempi  
 In questo esempio il comportamento della riproduzione distribuita deriva per lo più da un file di configurazione della riproduzione modificato, denominato `DReplay.exe.replay.config`.  
  
-   Il parametro **-m** specifica che un computer denominato `controller1` funge da controller. È necessario specificare il nome computer quando il servizio controller viene eseguito in un computer diverso.  
  
-   Il parametro **-d** specifica il percorso del file intermedio nel controller, `c:\WorkingDir`.  
  
-   Il parametro **-o** indica che ogni client specificato acquisisce l'attività di riproduzione e la salva in un file di traccia dei risultati. Nota: è possibile usare l'elemento `<ResultTrace>` nel file di configurazione per specificare se registrare il conteggio delle righe e il set di risultati.  
  
-   Il parametro **-w** specifica che i computer compresi tra `client1` e `client4` partecipano come client alla riesecuzione distribuita.  
  
-   Il parametro **-c** viene usato per puntare al file di configurazione modificato `DReplay.exe.replay.config`.  
  
-   Il parametro **-s** non è obbligatorio perché è stato specificato l'elemento `<Server>` nell'elemento `<ReplayOptions>` del file di configurazione della riproduzione `DReplay.exe.replay.config`.  
  
 La fase di riproduzione dell'evento viene avviata con la sintassi seguente quando lo strumento di amministrazione viene eseguito da un computer diverso dal controller:  
  
```  
dreplay replay -m controller1 -d c:\WorkingDir -o -w client1,client2,client3,client4 -c c:\DReplay.exe.replay.config  
```  
  
 Per specificare una modalità di sequenza sincrona, l'elemento `<SequencingMode>` del file `DReplay.exe.replay.config` viene impostato su un valore uguale al valore `synchronization`. La sezione `<ResultTrace>` del file di configurazione della riproduzione viene modificata per specificare la registrazione del conteggio delle righe. Queste modifiche vengono illustrate nell'esempio XML seguente.  
  
```  
<?xml version='1.0'?>  
<Options>  
    <ReplayOptions>  
        <Server>server_name\replay_target_instance</Server>  
        <SequencingMode>synchronization</SequencingMode>  
        <ConnectTimeScale></ConnectTimeScale>  
        <ThinkTimeScale></ThinkTimeScale>  
        <HealthmonInterval>60</HealthmonInterval>  
        <QueryTimeout>3600</QueryTimeout>  
        <ThreadsPerClient></ThreadsPerClient>  
    </ReplayOptions>  
    <OutputOptions>  
        <ResultTrace>  
            <RecordRowCount>Yes</RecordRowCount>  
            <RecordResultSet>No</RecordResultSet>  
        </ResultTrace>  
    </OutputOptions>  
</Options>  
```  
  
 Per specificare una modalità di sequenza di stress, l'elemento `<SequencingMode>` del file `DReplay.exe.replay.config` viene impostato su un valore uguale al valore `stress`. Gli elementi `<ConnectTimeScale>` e `<ThinkTimeScale>` vengono impostati sul valore `50` , per specificare 50%. Per altre informazioni sul tempo di connessione e il tempo interazione utente, vedere [Configurare Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md). Queste modifiche vengono illustrate nell'esempio XML seguente.  
  
```  
<?xml version='1.0'?>  
<Options>  
    <ReplayOptions>  
        <Server>server_name\replay_target_instance_name</Server>  
        <SequencingMode>stress</SequencingMode>  
        <ConnectTimeScale>50</ConnectTimeScale>  
        <ThinkTimeScale>50</ThinkTimeScale>  
        <HealthmonInterval>60</HealthmonInterval>  
        <QueryTimeout>3600</QueryTimeout>  
        <ThreadsPerClient></ThreadsPerClient>  
    </ReplayOptions>  
    <OutputOptions>  
        <ResultTrace>  
            <RecordRowCount>Yes</RecordRowCount>  
            <RecordResultSet>No</RecordResultSet>  
        </ResultTrace>  
    </OutputOptions>  
</Options>  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario eseguire lo strumento di amministrazione come utente interattivo, scegliendo tra utente locale e account utente di dominio. Per utilizzare un account utente locale, lo strumento di amministrazione e il controller devono essere eseguiti nello stesso computer.  
  
 Per altre informazioni, vedere [Sicurezza di Distributed Replay](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Riproduzione di dati di traccia](../../tools/distributed-replay/replay-trace-data.md)   
 [Esaminare i risultati di riproduzione](../../tools/distributed-replay/review-the-replay-results.md)   
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Configurare Riesecuzione distribuita](../../tools/distributed-replay/configure-distributed-replay.md)   
 [Forum di SQL Server Distributed Replay](https://social.technet.microsoft.com/Forums/sl/sqldru/)   
 [Using Distributed Replay to Load Test Your SQL Server - Part 2](/archive/blogs/msdn/mspfe/using-distributed-replay-to-load-test-your-sql-serverpart-2)  (Uso di Riesecuzione distribuita per testare il caricamento di SQL Server, seconda parte)  
 [Using Distributed Replay to Load Test Your SQL Server - Part 1](/archive/blogs/batuhanyildiz/using-distributed-replay-to-load-test-your-sql-serverpart-1) (Uso di Riesecuzione distribuita per testare il caricamento di SQL Server, prima parte)  
  
