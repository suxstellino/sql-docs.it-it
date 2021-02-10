---
title: Esegui Database Experimentation Assistant al prompt dei comandi
description: Informazioni su come acquisire una traccia in Database Experimentation Assistant (DEA) e quindi analizzare i risultati, tutto da un prompt dei comandi.
ms.custom: seo-lt-2019
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 0e52cf31a94f2fd63f92305b80f7f7ae049c2809
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066446"
---
# <a name="run-database-experimentation-assistant-at-a-command-prompt"></a>Esegui Database Experimentation Assistant al prompt dei comandi

Questo articolo descrive come acquisire una traccia in Database Experimentation Assistant (DEA) e quindi analizzare i risultati, tutto da un prompt dei comandi.

   > [!NOTE]
   > Per ulteriori informazioni su ogni operazione di DEA, provare a eseguire il comando seguente:
   >
   > `Deacmd.exe -o <operation> --help`
   >
   > Il nome dell'operazione è obbligatorio. le operazioni valide sono **Analysis**, **StartCapture** e **StopCapture**.

## <a name="start-a-new-workload-capture-by-using-the-dea-command"></a>Avviare una nuova acquisizione del carico di lavoro tramite il comando DEA

Per avviare una nuova acquisizione del carico di lavoro, al prompt dei comandi eseguire il comando seguente:

`Deacmd.exe -o StartCapture -n <Trace FileName> -x <Trace Format> -h <SQLServerInstance> -f <database name> -e <Encrypt Connection> -m <Authetication Mode> -u <user name> -p <password> -l <Location of Output Folder> -d <duration>`

**Esempio**

`Deacmd.exe -o StartCapture -n sql2008capture -x 0 -h localhost -f adventureworks -e --trust -m 0 -l c:\test  -d 60`

**Opzioni aggiuntive**

Quando si avvia una nuova acquisizione del carico di lavoro con il `Deacmd.exe` comando, è possibile usare le opzioni aggiuntive seguenti:

| Opzione| Descrizione |  
| --- | --- |
| -n, --name | Necessaria nome file di traccia |
| -x,--Format | Necessaria formato della traccia (Trace = 0, XEvent = 1) |
| -d,--Duration | Necessaria durata massima per l'acquisizione, in minuti |
| -l, --location | Necessaria percorso della cartella di output per l'archiviazione dei file di traccia/XEvent nel computer host |
| -t,--Type | (Valore predefinito: 0) tipo/edizione di SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Istanza gestita = 2) |
| -h,--host | Necessaria SQL Server nome host e/o nome istanza per avviare l'acquisizione |
| -e,--crittografa | (Valore predefinito: true) Crittografare la connessione all'istanza di SQL Server. Il valore predefinito è true. |
| --attendibilità | (Impostazione predefinita: false) Certificato server attendibile durante la connessione all'istanza di SQL Server. Il valore predefinito è false. |
| -f,--DatabaseName | (Impostazione predefinita:) Nome del database in cui filtrare le tracce. se non è specificato, l'acquisizione viene avviata su tutti i database |
| -m,--AuthMode | (Valore predefinito: 0) modalità di autenticazione (Windows = 0, autenticazione SQL = 1) |
| -u,--username | Nome utente per la connessione al SQL Server |
| -p,--password | Password per la connessione al SQL Server |

## <a name="replay-a-workload-capture"></a>Riprodurre un'acquisizione del carico di lavoro

**Utilizzo di Riesecuzione distribuita**

Se si usa Riesecuzione distribuita, seguire questa procedura.

1. Accedere al computer del controller di Riesecuzione distribuita.
2. Per convertire la traccia del carico di lavoro acquisita usando il comando DEA in un file IRF, al prompt dei comandi eseguire il comando seguente:

    `DReplay preprocess -m "dreplaycontroller" -i "Path to first trace file" -d "<Folder path on controller>\IrfFolder"`

3. Avviare un'acquisizione di traccia nel computer di destinazione che esegue SQL Server utilizzando StartReplayCaptureTrace. SQL.

    a.  In SQL Server Management Studio (SSMS) aprire <Dea_InstallPath \> \Scripts\StartReplayCaptureTrace.SQL.

    b.  Eseguire in `Set @durationInMins=0` modo che l'acquisizione della traccia non venga arrestata automaticamente dopo un periodo di tempo specificato.

    c.  Per impostare le dimensioni massime del file per ogni file di traccia, eseguire `Set @maxfilesize` . La dimensione consigliata è 200 (in MB).

    d.  Modificare `@Tracefile` per impostare un nome univoco per il file di traccia.

    e.  Modificare `@dbname` per specificare un nome di database se il carico di lavoro deve essere acquisito solo in un database specifico. Per impostazione predefinita, il carico di lavoro sull'intero server viene acquisito.

4. Per riprodurre il file IRF nell'istanza di SQL Server di destinazione, al prompt dei comandi eseguire il comando seguente:

    `DReplay replay -m "dreplaycontroller" -d "<Folder Path on Dreplay Controller>\IrfFolder" -o -s "SQL2016Target" -w "dreplaychild1,dreplaychild2,dreplaycild3,dreplaychild4"`

    a.  Per monitorare lo stato, al prompt dei comandi eseguire `DReplay status -f 1` .

    b.  Per arrestare la riproduzione, ad esempio se si nota che il passaggio% è inferiore al previsto, al prompt dei comandi eseguire `DReplay cancel` .

5. Arrestare l'acquisizione di traccia nell'istanza di SQL Server di destinazione.
6. In SSMS aprire `<Dea_InstallPath>\Scripts\StopCaptureTrace.sql` .
7. Modificare `@Tracefile` in modo che corrisponda al percorso del file di traccia nel computer di destinazione che esegue SQL Server.
8. Eseguire lo script nel computer di destinazione che esegue SQL Server.

**Uso della riproduzione incorporata**

Se si utilizza la riproduzione incorporata, non sarà necessario configurare Riesecuzione distribuita. È possibile utilizzare la funzionalità di riproduzione incorporata tramite la riga di comando, ma nel frattempo è possibile utilizzare l'interfaccia utente grafica per eseguire la riproduzione utilizzando la riproduzione incorporata.

## <a name="analyze-traces-using-the-dea-command"></a>Analizzare le tracce tramite il comando DEA

Per avviare una nuova analisi della traccia, al prompt dei comandi eseguire il comando seguente:

`Deacmd.exe -o analysis -a <Target1 trace filepath> -b <Target2 trace filepath> -r reportname -h <SQLserverInstance> -e <encryptconnection> -u <username>`

**Esempio**

`Deacmd.exe -o analysis -a C:\Trace\SQL2008Source\Trace.trc -b C:\ Trace\SQL2014Trace\Trace.trc -r upgrade20082014 -h localhost -e`

Per visualizzare i report di analisi di questi file di traccia, è necessario usare l'interfaccia utente grafica per visualizzare i grafici e le metriche organizzate.  Tuttavia, il database di analisi viene scritto nell'istanza SQL Server specificata, pertanto è possibile eseguire una query direttamente sulle tabelle di analisi generate.

**Opzioni aggiuntive**

Quando si analizzano le tracce usando il comando DEA, è possibile usare le opzioni aggiuntive seguenti:

| Opzione| Descrizione |  
| --- | --- |
| -a,--tracea | Necessaria percorso del file di eventi per un'istanza di. C:\traces\Sql2008trace.trc. di esempio  Se è presente un batch di file, selezionare il primo file e DEA controllerà automaticamente i file di rollover. Se i file si trovano in BLOB, specificare il percorso della cartella in cui si desidera archiviare i file di eventi in locale.  C:\TRACES\ di esempio |
| -b,--traceB | Necessaria percorso del file di evento per l'istanza B. C:\traces\Sql2014trace.trc. di esempio Se è presente un batch di file, selezionare il primo file e DEA controllerà automaticamente i file di rollover. Se i file si trovano in BLOB, specificare il percorso della cartella in cui si desidera archiviare i file di eventi in locale.  C:\TRACES\ di esempio |
| -r,--reportName | Necessaria nome dell'analisi corrente. Il report di analisi che viene generato verrà identificato con questo nome. |
| -t,--Type | (Valore predefinito: 0) tipo/edizione di SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Istanza gestita = 2) |
| -h,--host | Necessaria Nome host SQL Server e/o nome istanza |
| -e,--crittografa | (Valore predefinito: true) Crittografare la connessione all'istanza di SQL Server. Il valore predefinito è true. |
| --attendibilità | (Impostazione predefinita: false) Certificato server attendibile durante la connessione all'istanza di SQL Server. Il valore predefinito è false. |
| -m,--AuthMode | (Valore predefinito: 0) modalità di autenticazione (Windows = 0, autenticazione SQL = 1) |
| -u,--username | Nome utente per la connessione al SQL Server |
| --p | Password per la connessione al SQL Server |
| --AB | (Impostazione predefinita: false) Il percorso di archiviazione della traccia A è in BLOB. Se usato, è necessario specificare anche--Abu (Trace an BLOB URL) |
| --BB | (Impostazione predefinita: false) Il percorso di archiviazione della traccia B è in BLOB. Se utilizzato, è necessario specificare anche--BBU (URL BLOB di traccia B) |
| --Abu | URL BLOB per un'istanza con chiave SAS |
| --BBU | URL BLOB per l'istanza B con chiave SAS |

## <a name="see-also"></a>Vedi anche

- Per ulteriori informazioni sull'utilizzo di DEA, vedere [Panoramica di database Experimentation Assistant](database-experimentation-assistant-overview.md).
