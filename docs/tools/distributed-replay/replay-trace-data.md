---
title: Riprodurre i dati di traccia
titleSuffix: SQL Server Distributed Replay
description: Con la funzionalità Riesecuzione distribuita di SQL Server, usare l'opzione di riproduzione dello strumento di amministrazione per avviare la fase di riproduzione dell'evento della riesecuzione distribuita.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 3f8a09d9580f6ff4fd06fc23ab0b5fa20cdc4835
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838902"
---
# <a name="replay-trace-data"></a>Riproduzione di dati di traccia

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  È possibile avviare una riproduzione distribuita con la funzionalità Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dopo aver preparato i dati di traccia di input. Per altre informazioni, vedere [Preparazione dei dati di traccia di input](../../tools/distributed-replay/prepare-the-input-trace-data.md).  
  
 Usare l'opzione **replay** dello strumento di amministrazione per avviare la fase di riproduzione dell'evento della riesecuzione distribuita. Questa fase è costituita da due parti: il recapito dei dati di traccia e l'avvio e la sincronizzazione della riproduzione distribuita.  
  
 ![Riproduzione di eventi distribuita](../../tools/distributed-replay/media/eventreplay.gif "Riproduzione di eventi distribuita")  
  
 È possibile riprodurre i dati di traccia in una delle due modalità di sequenza disponibili, ovvero la modalità di stress o la modalità di sincronizzazione. Il comportamento predefinito consiste nel riprodurre i dati di traccia in modalità di stress. Per altre informazioni sulla fase di riproduzione dell'evento e sulle modalità di sequenza, vedere [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md).  
  
> [!NOTE]  
>  È necessario acquisire i dati di traccia di input in una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatibile con Distributed Replay. I dati di traccia di input devono essere compatibili anche con il server di destinazione su cui si desidera riprodurre i dati di traccia. Per altre informazioni sui requisiti relativi alla versione, vedere [Requisiti relativi a Riesecuzione distribuita](../../tools/distributed-replay/distributed-replay-requirements.md).  
  
### <a name="to-replay-the-trace"></a>Per riprodurre la traccia  
  
1.  **(Facoltativo) Modificare le impostazioni di configurazione della riproduzione**: per modificare le impostazioni di configurazione della riproduzione, ad esempio la modalità di sequenza e diversi valori di scala, è necessario modificare l'elemento `<ReplayOptions>` del file XML di configurazione della riproduzione `DReplay.exe.replay.config`. È possibile modificare anche l'elemento `<OutputOptions>` per specificare impostazioni di output, ad esempio se registrare o meno il conteggio delle righe. Se si modifica il file di configurazione della riproduzione, è consigliabile modificarne una copia anziché l'originale. Per modificare le impostazioni, effettuare le operazioni seguenti:  
  
    1.  Creare una copia del file di configurazione della riproduzione predefinito `DReplay.exe.replay.config`e rinominare il nuovo file. Il file di configurazione della riproduzione predefinito si trova nella cartella di installazione dello strumento di amministrazione.  
  
    2.  Modificare le impostazioni di configurazione della riproduzione nel nuovo file di configurazione.  
  
    3.  Quando si avvia la fase di riproduzione dell'evento (passaggio successivo), usare il parametro *config_file* dell'opzione **replay** per specificare il percorso del file di configurazione modificato.  
  
     Per altre informazioni sul file di configurazione della riproduzione, vedere [Configurare Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md).  
  
2.  **Avviare la fase di riproduzione dell'evento**: per avviare la riproduzione distribuita, è necessario eseguire lo strumento di amministrazione con l'opzione **replay**. Per altre informazioni, vedere [Opzione replay &#40;strumento di amministrazione Distributed Replay&#41;](../../tools/distributed-replay/replay-option-distributed-replay-administration-tool.md).  
  
    1.  Aprire l'utilità del prompt dei comandi di Windows (**CMD.exe**) e passare al percorso di installazione dello strumento di amministrazione Riesecuzione distribuita (**DReplay.exe**).  
  
    2.  (Facoltativo) Usare il parametro *controller* , **-m**, per specificare il controller, se il servizio controller viene eseguito in un computer diverso dallo strumento di amministrazione.  
  
    3.  Usare il parametro *controller_working_directory* , **-d**, per specificare il percorso in cui è stato salvato il file intermedio nel controller durante la fase di pre-elaborazione.  
  
    4.  (Facoltativo) Usare **-o** per acquisire l'attività di riproduzione in un file di traccia dei risultati in ciascun client.  
  
    5.  (Facoltativo) Usare il parametro *target_server* , **-s**, per specificare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui i client Riesecuzione distribuita dovranno riprodurre il carico di lavoro dei file di traccia. Questo parametro non è necessario se è stato utilizzato l'elemento `<Server>` per specificare il server di destinazione nell'elemento `<ReplayOptions>` del file di configurazione della riproduzione.  
  
    6.  Usare il parametro *clients* , **-w**, per specificare i client Riesecuzione distribuita che dovranno partecipare alla riproduzione. Elencare i nomi dei computer client, separati da virgole. Nota: Gli indirizzi IP non sono consentiti.  
  
    7.  (Facoltativo) Usare il parametro *config_file* , **-c**, per specificare il percorso del file di configurazione della riproduzione. Utilizzare questo parametro per puntare al nuovo file di configurazione se è stata modificata una copia del file di configurazione della riproduzione predefinito.  
  
    8.  (Facoltativo) Usare il parametro *status_interval* , **-f**, per specificare se si vuole che lo strumento di amministrazione visualizzi messaggi di stato a una frequenza diversa da 30 secondi.  
  
     La sintassi seguente, ad esempio, avvia la fase di riproduzione nello stesso computer del servizio controller, utilizza una directory di lavoro del controller situata in `c:\WorkingDir`, acquisisce l'attività di riproduzione in ogni client partecipante, utilizza i client `client1` e `client2` per eseguire la riproduzione e ottiene le impostazioni di configurazione della riproduzione rimanenti da un file di configurazione della riproduzione modificato che si trova in `c:\modifiedreplay.config`:  
  
     `dreplay replay -d c:\WorkingDir -o -w client1,client2 -c c:\modifiedreplay.config`  
  
3.  Al termine della riproduzione distribuita, lo strumento di amministrazione restituisce informazioni di riepilogo. Se è stata specificata l'opzione **-o** , l'attività di riproduzione è stata salvata in file di traccia dei risultati in ciascun client. Per altre informazioni sui file di traccia dei risultati, vedere [Controllo dei risultati della riproduzione](../../tools/distributed-replay/review-the-replay-results.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti relativi a Riesecuzione distribuita](../../tools/distributed-replay/distributed-replay-requirements.md)   
 [Opzioni della riga di comando dello strumento di amministrazione &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)   
 [Configurare Riesecuzione distribuita](../../tools/distributed-replay/configure-distributed-replay.md)  
  
  
