---
description: Requisiti relativi a Riesecuzione distribuita
title: Requisiti relativi a Riesecuzione distribuita
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 6fffee7d-891f-4d9d-b2c3-dd19855a1c2c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: bb04afa0b997797680d40b376d7427f3af3ee807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349556"
---
# <a name="distributed-replay-requirements"></a>Requisiti relativi a Riesecuzione distribuita
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Prima di usare la funzionalità Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esaminare i requisiti del prodotto indicati in questo argomento.  
  
## <a name="input-trace-requirements"></a>Requisiti della traccia di input  
 Affinché possano essere riprodotti correttamente, i dati di traccia devono soddisfare i requisiti per la versione e il formato e contenere le colonne e gli eventi necessari.  
  
### <a name="input-trace-versions"></a>Versioni della traccia di input  
 Riesecuzione distribuita supporta dati di traccia di input raccolti nelle versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]seguenti:  
  
-   Aggiornamento cumulativo 1 e versioni successive di [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]. Vedere [Aggiornamenti cumulativi per SQL Server 2017](https://aka.ms/sql2017cu).
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]   
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
-   [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>Formati della traccia di input  
 I dati di traccia di input possono utilizzare uno dei formati seguenti:  
  
-   Singolo file di traccia con estensione `.trc` .  
  
-   Set di file di traccia di rollover che rispettano la convenzione di denominazione per il rollover dei file, ad esempio `<TraceFile>.trc`, `<TraceFile>_1.trc`, `<TraceFile>_2.trc`, `<TraceFile>_3.trc`, ... `<TraceFile>_n.trc`.  
  
### <a name="input-trace-events-and-columns"></a>Eventi e colonne della traccia di input  
 I dati di traccia di input devono contenere colonne ed eventi specifici, che devono essere rieseguiti da Riesecuzione distribuita. Il modello **TSQL_Replay** in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] contiene tutte le colonne e tutti gli eventi necessari, oltre a informazioni aggiuntive. Per altre informazioni sul modello, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md).  
  
> [!WARNING]  
>  Se non si usa il modello **TSQL_Replay** per acquisire i dati di traccia di input o se i requisiti della traccia di input non sono soddisfatti, è possibile che si verifichino risultati di riproduzione imprevisti.  
  
 È inoltre possibile creare un modello di traccia personalizzato e utilizzarlo per riprodurre eventi con Riesecuzione distribuita, purché contenga gli eventi seguenti:  
  
-   Connessione di controllo  
  
-   Disconnessione di controllo  
  
-   ExistingConnection  
  
-   RPC Output Parameter  
  
-   RPC:Completed  
  
-   RPC:Starting  
  
-   SQL:BatchCompleted  
  
-   SQL:BatchStarting  
  
 Se si riproducono cursori sul lato server, sono necessari anche gli eventi seguenti:  
  
-   CursorClose  
  
-   CursorExecute  
  
-   CursorOpen  
  
-   CursorPrepare  
  
-   CursorUnprepare  
  
 Se si riproducono istruzioni SQL preparate sul lato server, sono necessari anche gli eventi seguenti:  
  
-   Exec Prepared SQL  
  
-   Prepare SQL  
  
 Tutti i dati di traccia di input devono contenere le colonne seguenti:  
  
-   Event Class  
  
-   EventSequence  
  
-   TextData  
  
-   Nome dell'applicazione  
  
-   LoginName  
  
-   DatabaseName  
  
-   ID database  
  
-   HostName  
  
-   Binary Data  
  
-   SPID  
  
-   Ora di Inizio  
  
-   EndTime  
  
-   IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>Combinazioni di traccia di input e server di destinazione supportate  
 Nella tabella seguente sono elencate le versioni supportate dei dati di traccia e, per ciascuna di esse, sono indicate le versioni supportate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su cui è possibile riprodurre i dati.  
  
|Versione dei dati di traccia di input|Versioni supportate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'istanza del server di destinazione|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>Requisiti del sistema operativo  
 I sistemi operativi supportati per l'esecuzione dello strumento di amministrazione e dei servizi client e del controller sono gli stessi dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni sui sistemi operativi supportati per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Requisiti hardware e software per l'installazione di SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
 Le funzionalità di Riesecuzione distribuita sono supportate sia in sistemi operativi x86 che x64. Per i sistemi operativi basati su x64, è supportata solo la modalità Windows on Windows (WOW).  
  
## <a name="installation-limitations"></a>Limitazioni relative all'installazione  
 In ogni computer può essere installata una sola istanza di ogni funzionalità di Riesecuzione distribuita. Nella tabella seguente viene indicato il numero di installazioni consentito per ogni funzionalità in un singolo ambiente di Riesecuzione distribuita.  
  
|Funzionalità di Riesecuzione distribuita|Numero massimo di installazioni per ambiente di riproduzione|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Servizio controller di Riesecuzione distribuita|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Servizio client Riesecuzione distribuita|16 (computer fisici o virtuali)|  
|Strumento di amministrazione|Nessuna limitazione|  
  
> [!NOTE]  
>  Benché sia possibile installare solo un'istanza dello strumento di amministrazione in ogni computer, è possibile avviare più istanze dello strumento di amministrazione. I comandi eseguiti da più strumenti di amministrazione vengono risolti in base all'ordine di ricezione.  
  
## <a name="data-access-provider"></a>Provider di accesso ai dati  
 Riesecuzione distribuita supporta solo il provider di accesso ai dati ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="target-server-preparation-requirements"></a>Requisiti di preparazione del server di destinazione  
 È consigliabile posizionare il server di destinazione in un ambiente di testing. Per riprodurre dati di traccia su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diversa rispetto a quella in cui sono stati registrati in origine, verificare che nel server di destinazione siano state effettuate le operazioni seguenti:  
  
-   Tutti gli account di accesso e gli utenti contenuti nei dati di traccia devono essere presenti nello stesso database nel server di destinazione.  
  
-   Tutti gli account di accesso e gli utenti presenti nel server di destinazione devono disporre delle stesse autorizzazioni di cui disponevano nel server originale.  
  
-   È consigliabile che gli ID di database nella destinazione e nell'origine siano uguali. In caso contrario, tuttavia, è possibile trovare una corrispondenza in base a **DatabaseName** , se presente nella traccia.  
  
-   Il database predefinito per ogni account di accesso contenuto nei dati di traccia deve essere impostato (nel server di destinazione) sul rispettivo database di destinazione dell'account di accesso. Si supponga, ad esempio, che i dati di traccia da riprodurre contengano attività per l'account di accesso **Fred** nel database **Fred_Db** nell'istanza originale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza, nel server di destinazione il database predefinito per l'account di accesso **Fred** deve essere impostato sul database corrispondente a **Fred_Db** , anche se il nome del database è diverso. Per impostare il database predefinito dell'account di accesso, utilizzare la stored procedure di sistema `sp_defaultdb` .  
  
 La riproduzione degli eventi associati ad account di accesso mancanti o non corretti genera errori di riproduzione, ma l'operazione non viene interrotta.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Sicurezza di Riesecuzione distribuita](../../tools/distributed-replay/distributed-replay-security.md)   
 [Install Distributed Replay - Overview](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
  
