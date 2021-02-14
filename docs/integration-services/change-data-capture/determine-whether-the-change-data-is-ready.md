---
description: Come determinare se i dati delle modifiche sono pronti
title: Determinare se i dati delle modifiche sono pronti | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],determining readiness
ms.assetid: 04935f35-96cc-4d70-a250-0fd326f8daff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9fd82f56cb1edb459e662e31778fcb8341a6887c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353036"
---
# <a name="determine-whether-the-change-data-is-ready"></a>Come determinare se i dati delle modifiche sono pronti

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nel flusso di controllo di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che esegue un caricamento incrementale dei dati delle modifiche, la seconda attività consiste nel verificare che i dati delle modifiche per l'intervallo selezionato siano pronti. Questo passaggio è necessario in quanto il processo di acquisizione asincrono potrebbe non avere ancora elaborato tutte le modifiche fino all'endpoint selezionato.  
  
> [!NOTE]  
>  La prima attività per il flusso di controllo consiste nel calcolare gli endpoint dell'intervallo di modifiche. Per altre informazioni su questa attività, vedere [Definizione di un intervallo dei dati delle modifiche](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md). Per una descrizione del processo complessivo di progettazione del flusso di controllo, vedere [Change Data Capture &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="understanding-the-components-of-the-solution"></a>Informazioni sui componenti della soluzione  
 La soluzione descritta in questo argomento usa 4 componenti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   Un contenitore Ciclo For che valuta ripetutamente l'output di un'attività Esegui SQL.  
  
-   Un'attività Esegui SQL che esegue una query su tabelle speciali gestite dal processo Change Data Capture e che utilizza quindi tali informazioni per determinare se i dati sono pronti.  
  
-   Un componente che implementa un ritardo nell'elaborazione quando i dati non sono pronti. Può trattarsi di un'attività Script o di un'attività Esegui SQL.  
  
-   Facoltativamente, un componente che segnala un errore o un timeout quando l'attività Esegui SQL restituisce un valore indicante un errore o una condizione di timeout.  
  
 Tali componenti impostano o leggono i valori di numerose variabili del pacchetto per controllare il flusso di esecuzione all'interno del ciclo e successivamente nel pacchetto.  
  
#### <a name="to-set-up-package-variables"></a>Per configurare le variabili del pacchetto  
  
1.  Nella finestra [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]Variabili **di** creare le variabili seguenti:  
  
    1.  Creare una variabile con un tipo di dati integer per memorizzare il valore di stato restituito dall'attività Esegui SQL.  
  
         In questo esempio viene utilizzato il nome di variabile DataReady con un valore iniziale pari a 0.  
  
    2.  Creare una variabile per memorizzare il periodo di tempo per il ritardo quando i dati non sono pronti. Se si prevede di utilizzare un'attività Script per implementare il ritardo, la variabile deve essere associata a un tipo di dati integer. Se si prevede di utilizzare un'attività Esegui SQL con un'istruzione WAITFOR, la variabile deve avere un tipo di dati stringa per accettare valori quali "00.00.10".  
  
         In questo esempio viene utilizzato il nome di variabile DelaySeconds con un valore iniziale pari a 10.  
  
    3.  Creare una variabile con un tipo di dati integer per memorizzare l'iterazione corrente del ciclo.  
  
         In questo esempio viene utilizzato il nome di variabile TimeoutCount con un valore iniziale pari a 0.  
  
    4.  Creare una variabile con un tipo di dati integer per specificare il numero di volte in cui il ciclo deve eseguire il test dei dati prima di segnalare una condizione di timeout.  
  
         In questo esempio viene utilizzato il nome di variabile TimeoutCeiling con un valore iniziale pari a 20.  
  
    5.  Creare una variabile con un tipo di dati integer che è possibile utilizzare per indicare il primo caricamento dei dati delle modifiche (facoltativo).  
  
         In questo esempio viene utilizzato il nome di variabile IntervalID e viene verificata solo l'eventuale presenza di un valore pari a 0 per indicare il caricamento iniziale.  
  
## <a name="configuring-a-for-loop-container"></a>Configurazione di un contenitore Ciclo For  
 Dopo avere impostato le variabili, il contenitore Ciclo For rappresenta il primo componente da aggiungere.  
  
#### <a name="to-configure-a-for-loop-container-to-wait-until-change-data-is-ready"></a>Per configurare un contenitore Ciclo For per attendere che i dati delle modifiche siano pronti  
  
1.  Nella scheda **Flusso di controllo** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] aggiungere un contenitore Ciclo For al flusso di controllo.  
  
2.  Connettere l'attività Esegui SQL per il calcolo degli endpoint dell'intervallo al contenitore Ciclo For.  
  
3.  In **Editor ciclo For** selezionare le opzioni seguenti:  
  
    1.  Per **InitExpression**, immettere `@DataReady = 0`.  
  
         Questa espressione imposta il valore iniziale della variabile del ciclo.  
  
    2.  Per **EvalExpression**, immettere `@DataReady == 0`.  
  
         Quando questa espressione restituisce **False**, l'esecuzione passa all'esterno del ciclo e viene avviato il caricamento incrementale.  
  
## <a name="configuring-the-execute-sql-task-that-queries-for-change-data"></a>Configurazione dell'attività Esegui SQL per l'esecuzione di una query per i dati delle modifiche  
 È necessario aggiungere un'attività Esegui SQL all'interno del contenitore Ciclo For. Questa attività esegue una query sulle tabelle gestite dal processo Change Data Capture nel database. Il risultato della query è un valore di stato che indica se i dati delle modifiche sono pronti.  
  
 Nella tabella seguente la prima colonna indica i valori restituiti dall'attività Esegui SQL tramite la query Transact-SQL di esempio. La seconda colonna indica il modo in cui gli altri componenti rispondono a tali valori.  
  
|Valore restituito|Significato|Risposta|  
|------------------|-------------|--------------|  
|0|Indica che i dati delle modifiche non sono pronti.<br /><br /> Non è presente alcun record di Change Data Capture successivo al punto finale dell'intervallo selezionato.|L'esecuzione continua con il componente che implementa un ritardo. Il controllo viene restituito al contenitore Ciclo For, che continua a verificare l'attività Esegui SQL a condizione che il valore restituito sia 0.|  
|1|Potrebbe indicare che i dati delle modifiche non sono stati acquisiti per l'intervallo completo o che sono stati eliminati. Questo caso viene considerato una condizione di errore.<br /><br /> Non è presente alcun record di Change Data Capture precedente al punto iniziale dell'intervallo selezionato.|L'esecuzione continua con il componente facoltativo che registra l'errore.|  
|2|Indica che i dati sono pronti.<br /><br /> Non è presente alcun record di Change Data Capture che sia precedente al punto iniziale e successivo al punto finale dell'intervallo selezionato.|L'esecuzione passa all'esterno del contenitore Ciclo For e viene avviato il caricamento incrementale.|  
|3|Indica il caricamento iniziale di tutti i dati delle modifiche disponibili.<br /><br /> La logica condizionale ottiene questo valore da una variabile del pacchetto speciale utilizzata solo a questo scopo.|L'esecuzione passa all'esterno del contenitore Ciclo For e viene avviato il caricamento incrementale.|  
|5|Indica che è stato raggiunto il valore di TimeoutCeiling.<br /><br /> Il ciclo ha testato i dati per il numero specificato di volte e i dati non sono ancora disponibili. Senza questo test o uno simile, l'esecuzione del pacchetto potrebbe continuare per un tempo illimitato.|L'esecuzione prosegue con il componente facoltativo che registra il timeout.|  
  
#### <a name="to-configure-an-execute-sql-task-to-query-whether-change-data-is-ready"></a>Per configurare un'attività Esegui SQL per l'esecuzione di una query per verificare se i dati sono pronti  
  
1.  Aggiungere un'attività Esegui SQL all'interno del contenitore Ciclo For.  
  
2.  Nella pagina **Generale** in **Editor attività Esegui SQL** selezionare le opzioni seguenti:  
  
    1.  Per **ResultSet**, selezionare **Riga singola**.  
  
    2.  Configurare una connessione valida al database di origine.  
  
    3.  Per **SQLSourceType**, selezionare **Input diretto**.  
  
    4.  Per **SQLStatement** immettere l'istruzione SQL seguente:  
  
        ```  
        declare @DataReady int, @TimeoutCount int  
  
        if not exists (select tran_end_time from cdc.lsn_time_mapping  
                where tran_end_time > ?  )  
            select @DataReady = 0  
        else  
            if ? = 0  
                select @DataReady = 3   
        else  
            if not exists (select tran_end_time from cdc.lsn_time_mapping  
                    where tran_end_time <= ? )  
                select @DataReady = 1   
        else  
            select @DataReady = 2  
  
        select @TimeoutCount = ?  
        if (@DataReady = 0)  
            select @TimeoutCount = @TimeoutCount + 1  
        else  
            select @TimeoutCount = 0  
  
        if (@TimeoutCount > ?)  
            select @DataReady = 5  
  
        select @DataReady as DataReady, @TimeoutCount as TimeoutCount  
  
        ```  
  
3.  Nella pagina **Mapping parametri** di **Editor attività Esegui SQL** creare i mapping seguenti:  
  
    1.  Eseguire il mapping tra la variabile ExtractEndTime e il parametro 0.  
  
    2.  Mapping tra la variabile IntervalID e il parametro 1.  
  
    3.  Eseguire il mapping tra la variabile ExtractStartTime e il parametro 2.  
  
    4.  Mapping tra la variabile TimeoutCount e il parametro 3.  
  
    5.  Mapping tra la variabile TimeoutCeiling e il parametro 4.  
  
4.  Nella pagina **Set dei risultati** di **Editor attività Esegui SQL** eseguire il mapping tra il risultato di DataReady e la variabile DataReady e tra il risultato di TimeoutCount e la variabile TimeoutCount.  
  
## <a name="waiting-until-the-change-data-is-ready"></a>Attesa che i dati delle modifiche siano pronti  
 È possibile utilizzare uno dei numerosi metodi per implementare un ritardo quando i dati delle modifiche non sono pronti. Nelle due procedure seguenti viene illustrato come utilizzare un'attività Script o un'attività Esegui SQL per implementare il ritardo.  
  
> [!NOTE]  
>  Uno script precompilato è soggetto a un overhead minore rispetto a un'attività Esegui SQL.  
  
#### <a name="to-implement-a-delay-by-using-a-script-task"></a>Per implementare un ritardo tramite un'attività Script  
  
1.  Aggiungere un'attività Script all'interno del contenitore Ciclo For.  
  
2.  Connettere l'attività Esegui SQL che esegue una query per determinare se i dati delle modifiche sono pronti alla nuova attività Script.  
  
3.  Per il vincolo di precedenza che connette l'attività Esegui SQL all'attività Script, aprire **Editor vincoli di precedenza** e selezionare le opzioni seguenti:  
  
    1.  Per **Operazione valutazione**, selezionare **Espressione e vincolo**.  
  
    2.  Per **Valore**, selezionare **Esito positivo**.  
  
         Il valore **Esito positivo** del vincolo si riferisce al risultato dell'attività precedente, in questo caso l'esito positivo dell'attività Esegui SQL.  
  
    3.  Per **Espressione**, immettere `@DataReady == 0 && @TimeoutCount <= @TimeoutCeiling`.  
  
    4.  Selezionare **AND logico. Se questa opzione non è già selezionata, tutti i vincoli devono restituire il valore True**.  
  
4.  Nella pagina **Script** di **Editor attività Script** per **ReadOnlyVariables** selezionare dall'elenco la variabile di tipo integer **User::DelaySeconds** .  
  
5.  Nella pagina **Script** in **Editor attività Script** fare clic su **Modifica script** per aprire l'ambiente di sviluppo dello script.  
  
6.  Nella routine Main immettere una delle righe di codice seguenti:  
  
    -   Se si utilizza il linguaggio di programmazione C#, immettere la riga di codice seguente:  
  
        ```  
        System.Threading.Thread.Sleep((int)Dts.Variables["DelaySeconds"].Value * 1000);  
        ```  
  
         \- - oppure -  
  
    -   Se si utilizza il linguaggio di programmazione [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], immettere la riga di codice seguente:  
  
        ```  
        System.Threading.Thread.Sleep(Ctype(Dts.Variables("DelaySeconds").Value, Integer) * 1000)  
  
        ```  
  
        > [!NOTE]  
        >  Come argomento del metodo **Thread.Sleep** è previsto un argomento specificato in millisecondi.  
  
7.  Lasciare la riga di codice predefinita, che restituisce **DtsExecResult.Success** dall'esecuzione dello script.  
  
8.  Chiudere l'ambiente di sviluppo dello script ed **Editor attività Script**.  
  
#### <a name="to-implement-a-delay-by-using-an-execute-sql-task"></a>Per implementare un ritardo tramite un'attività Esegui SQL  
  
1.  Aggiungere un'attività Esegui SQL all'interno del contenitore Ciclo For.  
  
2.  Connettere l'attività Esegui SQL che esegue una query per determinare se i dati delle modifiche sono pronti alla nuova attività Esegui SQL.  
  
3.  Per il vincolo di precedenza che connette le due attività Esegui SQL, aprire **Editor vincoli di precedenza** e selezionare le opzioni seguenti:  
  
    1.  Per **Operazione valutazione**, selezionare **Espressione e vincolo**.  
  
    2.  Per **Valore**, selezionare **Esito positivo**.  
  
         Il valore **Esito positivo** del vincolo si riferisce al risultato dell'attività Esegui SQL precedente.  
  
    3.  Per **Espressione**, immettere `@DataReady == 0`.  
  
    4.  Selezionare **AND logico. Se questa opzione non è già selezionata, tutti i vincoli devono restituire il valore True**.  
  
         Questa opzione richiede che entrambe le condizioni, il vincolo e l'espressione, siano True.  
  
4.  Nella pagina **Generale** in **Editor attività Esegui SQL** selezionare le opzioni seguenti:  
  
    1.  Per **ResultSet**, selezionare **Riga singola**.  
  
    2.  Configurare una connessione valida al database di origine.  
  
    3.  Per **SQLSourceType**, selezionare **Input diretto**.  
  
    4.  Per **SQLStatement** immettere l'istruzione SQL seguente:  
  
        ```  
        WAITFOR DELAY ?  
  
        ```  
  
5.  Nella pagina **Mapping parametri** dell'editor eseguire il mapping tra la variabile stringa DelaySeconds e il parametro 0.  
  
## <a name="handling-an-error-condition"></a>Gestione di una condizione di errore  
 Se si desidera, è possibile configurare un componente aggiuntivo nel ciclo per registrare una condizione di errore o di timeout:  
  
-   Questo componente può registrare una condizione di errore quando il valore della variabile DataReady = 1. Questo valore indica che non sono disponibili dati delle modifiche prima dell'inizio dell'intervallo selezionato.  
  
-   Questo componente può registrare una condizione di timeout anche quando viene raggiunto il valore della variabile TimeoutCeiling. Questo valore indica che il ciclo ha testato i dati per il numero specificato di volte e i dati non sono ancora disponibili. Senza questo test o uno simile, l'esecuzione del pacchetto potrebbe continuare per un tempo illimitato.  
  
#### <a name="to-configure-an-optional-script-task-to-log-an-error-condition"></a>Per configurare un'attività Script facoltativa per la registrazione di una condizione di errore  
  
1.  Se si desidera segnalare l'errore o il timeout scrivendo un messaggio nel log, configurare la registrazione per il pacchetto. Per altre informazioni, vedere [Abilitare la registrazione di pacchetti in SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#ssdt).  
  
2.  Aggiungere un'attività Script all'interno del contenitore Ciclo For.  
  
3.  Connettere l'attività Esegui SQL che esegue una query per determinare se i dati delle modifiche sono pronti alla nuova attività Script.  
  
4.  Per il vincolo di precedenza che connette l'attività Esegui SQL all'attività Script, aprire **Editor vincoli di precedenza** e selezionare le opzioni seguenti:  
  
    1.  Per **Operazione valutazione**, selezionare **Espressione e vincolo**.  
  
    2.  Per **Valore**, selezionare **Esito positivo**.  
  
         Il valore **Esito positivo** del vincolo si riferisce al risultato dell'attività precedente, in questo caso l'esito positivo dell'attività Esegui SQL.  
  
    3.  Per **Espressione**, immettere `@DataReady == 1 || @DataReady == 5`.  
  
    4.  Selezionare **AND logico. Se questa opzione non è già selezionata, tutti i vincoli devono restituire il valore True**.  
  
         Questa opzione richiede che entrambe le condizioni, il vincolo e l'espressione, siano True.  
  
5.  Nella pagina **Script** di **Editor attività Script** per **ReadOnlyVariables** selezionare **User::DataReady** e **User::ExtractStartTime** dall'elenco per rendere i valori disponibili per lo script.  
  
     Se si desidera includere informazioni da determinate variabili di sistema, ad esempio System::PackageName, nelle informazioni scritte nel log, selezionare anche tali variabili.  
  
6.  Nella pagina **Script** in **Editor attività Script** fare clic su **Modifica script** per aprire l'ambiente di sviluppo dello script.  
  
7.  Nella routine Main immettere il codice per registrare un errore chiamando il metodo **Dts.Log** o per generare un evento chiamando uno dei metodi dell'interfaccia **Dts.Events** . Informare dell'errore il pacchetto restituendo `Dts.TaskResult = Dts.Results.Failure`.  
  
     Nell'esempio seguente viene illustrato come scrivere un messaggio nel log. Per altre informazioni, vedere [Registrazione nell'attività Script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md), [Generazione di eventi nell'attività Script](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)e [Risultati restituiti dall'attività Script](../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
    ```  
    ' User variables.  
    Dim dataReady As Integer = _  
      CType(Dts.Variables("DataReady").Value, Integer)  
    Dim extractStartTime As Date = _  
      CType(Dts.Variables("ExtractStartTime").Value, DateTime)  
  
    ' System variables.  
    Dim packageName As String = _  
      Dts.Variables("PackageName").Value.ToString()  
    Dim executionStartTime As Date = _  
      CType(Dts.Variables("StartTime").Value, DateTime)  
  
    Dim eventMessage As New System.Text.StringBuilder()  
  
    If dataReady = 1 OrElse dataReady = 5 Then  
  
      If dataReady = 1 Then  
        eventMessage.AppendLine("Start Time Error")  
      Else  
        eventMessage.AppendLine("Timeout Error")  
      End If  
  
      With eventMessage  
        .Append("The package ")  
        .Append(packageName)  
        .Append(" started at ")  
        .Append(executionStartTime.ToString())  
        .Append(" and ended at ")  
        .AppendLine(DateTime.Now().ToString())  
        If dataReady = 1 Then  
          .Append("The specified ExtractStartTime was ")  
          .AppendLine(extractStartTime.ToString())  
        End If  
      End With  
  
      System.Windows.Forms.MessageBox.Show(eventMessage.ToString())  
  
      Dts.Log(eventMessage.ToString(), 0, Nothing)  
  
      Dts.TaskResult = Dts.Results.Failure  
  
    Else  
  
      Dts.TaskResult = Dts.Results.Success  
  
    End If  
  
    ```  
  
8.  Chiudere l'ambiente di sviluppo dello script ed **Editor attività Script**.  
  
## <a name="next-step"></a>passaggio successivo  
 Dopo avere determinato che i dati delle modifiche sono pronti, il passaggio successivo consiste nel prepararsi a eseguire una query per tali dati delle modifiche.  
  
 **Argomento successivo:** [Preparazione dell'esecuzione di una query per i dati delle modifiche](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)  
  
  
