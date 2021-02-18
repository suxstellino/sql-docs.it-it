---
title: 'Esercitazione su R: Esplorare e visualizzare i dati'
titleSuffix: SQL machine learning
description: Esplorare i dati di esempio e generare alcuni tracciati in preparazione dell'uso della classificazione binaria in R con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 8e2e8994f92bfb96fa66c2c6a08c7db198902b45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354209"
---
# <a name="r-tutorial-explore-and-visualize-data"></a>Esercitazione su R: Esplorare e visualizzare i dati
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Nella seconda parte di questa serie di esercitazioni in cinque parti verranno esaminati i dati di esempio e verranno generati alcuni tracciati. Più avanti si apprenderà come serializzare gli oggetti grafici in Python e quindi deserializzare gli oggetti e creare tracciati.

Nella seconda parte di questa serie di esercitazioni in cinque parti verranno esaminati i dati di esempio e quindi verranno generati alcuni tracciati usando le funzioni generiche `barplot` e `hist` nel linguaggio R di base.

Uno degli obiettivi principali di questo articolo è illustrare come chiamare funzioni R da [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle stored procedure e salvare i risultati nei formati di file dell'applicazione:

+ Creare una stored procedure usando `barplot` per generare un tracciato R sotto forma di dati varbinary. Usare **bcp** per esportare il flusso binario in un file di immagine.
+ Creare una stored procedure usando `hist` per generare un tracciato, salvando i risultati come output JPG e PDF.

> [!NOTE]
> Poiché la visualizzazione è uno strumento potente per comprendere la forma e la distribuzione dei dati, R offre una gamma di funzioni e pacchetti per la generazione di istogrammi, grafici a dispersione, box plot e altri grafici per l'esplorazione dei dati. R in genere crea immagini usando un dispositivo R per l'output grafico, che è possibile acquisire e archiviare come tipo di dati **varbinary** per il rendering nell'applicazione. È anche possibile salvare le immagini in uno dei formati di file supportati (JPG, PDF e così via).

Contenuto dell'articolo:

> [!div class="checklist"]
> + Esaminare i dati di esempio
> + Creare tracciati usando R in T-SQL
> + Tracciati di output in più formati di file

Nella [prima parte](r-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [terza parte](r-taxi-classification-create-features.md) si apprenderà come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. Tale funzione verrà quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quarta parte](r-taxi-classification-train-model.md) verranno caricati i moduli e verranno chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server.

Nella [quinta parte](r-taxi-classification-deploy-model.md) si apprenderà come rendere operativi i modelli sottoposti a training e salvati nella quarta parte.

## <a name="review-the-data"></a>Esaminare i dati

Lo sviluppo di una soluzione di analisi scientifica dei dati prevede in genere frequenti esplorazioni e visualizzazioni dei dati. Dedicare quindi un po' di tempo a esaminare i dati di esempio, se non è già stato fatto.

Nel set di dati pubblico originale gli identificatori di taxi e i record delle corse si trovano in file separati. Tuttavia, per semplificare l'uso dei dati di esempio, i due set di dati originali sono stati uniti nelle colonne _medallion_, _hack\_license_ e _pickup\_datetime_.  È stato eseguito anche un campionamento dei record in modo da ottenere solo l'1% del numero di record originale. Il set di dati ridotto risultante include 1.703.957 righe e 23 colonne.

**Identificatori di taxi**
  
+ La colonna _medallion_ rappresenta l'ID univoco del taxi.
  
+ La colonna _hack\_license_ contiene il numero di patente del tassista (in forma anonima).
  
**Record delle corse e delle tariffe**
  
+ Il record di ogni corsa include il luogo e l'ora di inizio e fine della corsa e la distanza percorsa.
  
+ Il record di ogni tariffa include i dati del pagamento, ad esempio il tipo di pagamento, l'importo totale e l'importo della mancia.
  
+ Le ultime tre colonne possono essere usate per diverse attività di apprendimento automatico. La colonna _tip\_amount_ contiene valori numerici continui e può essere usata come colonna **label** per l'analisi della regressione. La colonna _tipped_ include solo valori sì/no e viene usata per la classificazione binaria. La colonna _tip\_class_ include più **etichette di classe** e può essere quindi usata come etichetta per le attività di classificazione multiclasse.
  
  In questa procedura dettagliata è descritta soltanto l'attività di classificazione binaria. Si consiglia di provare a creare modelli per le altre due attività di apprendimento automatico, la regressione e la classificazione multiclasse.
  
+ I valori usati per le colonne etichetta sono basati sulla colonna _tip\_amount_ usando le regole business seguenti:
  
  |Nome colonna derivata|Regola|
  |-|-|
  |tipped|Se tip_amount > 0, tipped = 1, altrimenti tipped = 0|
  |tip_class|Classe 0: tip_amount = $0<br /><br />Classe 1: tip_amount > $0 e tip_amount <= $5<br /><br />Classe 2: tip_amount > $5 e tip_amount <= $10<br /><br />Classe 3: tip_amount > $10 e tip_amount <= $20<br /><br />Classe 4: tip_amount > $20|

## <a name="create-plots-using-r-in-t-sql"></a>Creare tracciati usando R in T-SQL

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> A partire da SQL Server 2019, il meccanismo di isolamento richiede di assegnare le autorizzazioni appropriate alla directory in cui è archiviato il file del tracciato. Per altre informazioni su come impostare queste autorizzazioni, vedere la [sezione Autorizzazioni per i file in SQL Server 2019 in Windows: Modifiche al meccanismo di isolamento per Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

Per creare il tracciato, usare la funzione R `barplot`. Questo passaggio traccia un istogramma in base ai dati di una query [!INCLUDE[tsql](../../includes/tsql-md.md)]. È possibile eseguire il wrapping di questa funzione in una stored procedure, **RPlotHistogram**.

1. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in Esplora oggetti fare clic con il pulsante destro del mouse sul database **NYCTaxi_Sample** e scegliere **Nuova query**. In alternativa, in Azure Data Studio selezionare **Nuovo notebook** dal menu **File** e connettersi al database.

2. Incollare lo script seguente per creare una stored procedure che traccia l'istogramma. L'esempio è denominato **RPlotHistogram**.

   ```sql
   CREATE PROCEDURE [dbo].[RPlotHistogram]
   AS
   BEGIN
     SET NOCOUNT ON;
     DECLARE @query nvarchar(max) =  
     N'SELECT tipped FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
                                        @script = N'  
      image_file = tempfile();  
      jpeg(filename = image_file);  
      #Plot histogram  
      barplot(table(InputDataSet$tipped), main = "Tip Histogram", col="lightgreen", xlab="Tipped or not", ylab = "Counts", space=0)
      dev.off();  
      OutputDataSet <- data.frame(data=readBin(file(image_file, "rb"), what=raw(), n=1e6));  
      ',  
      @input_data_1 = @query  
      WITH RESULT SETS ((plot varbinary(max)));  
   END
   GO
   ```

Gli elementi più importanti da comprendere nello script sono i seguenti:
  
+ La variabile `@query` definisce il testo della query (`'SELECT tipped FROM nyctaxi_sample'`) che viene passato allo script R come argomento alla variabile di input dello script `@input_data_1`. Per gli script R che vengono eseguiti come processi esterni, è necessario un mapping uno-a-uno tra gli input dello script e gli input della stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) che avvia la sessione R in SQL Server.
  
+ Nello script R viene definita una variabile (`image_file`) per archiviare l'immagine.

+ La funzione `barplot` viene chiamata per generare il tracciato.
  
+ Il dispositivo R è impostato su **off** perché si sta eseguendo il comando come script esterno in SQL Server. In genere in R quando viene eseguito un comando generale di esecuzione di tracciato viene aperta una finestra grafica detta *dispositivo*. È possibile disattivare il dispositivo se si sta scrivendo in un file o gestendo l'output in altro modo.
  
+ L'oggetto grafico R viene serializzato in un data.frame R per l'output.

### <a name="execute-the-stored-procedure-and-use-bcp-to-export-binary-data-to-an-image-file"></a>Eseguire la stored procedure e usare bcp per esportare i dati binari in un file di immagine

La stored procedure restituisce l'immagine come un flusso di dati varbinary, che ovviamente non è possibile visualizzare direttamente. Tuttavia, è possibile usare l'utilità **bcp** utilità per ottenere i dati varbinary e salvarli come file di immagine in un computer client.
  
1. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]eseguire l'istruzione seguente:
  
    ```sql
    EXEC [dbo].[RPlotHistogram]
    ```
  
    **Risultati**
    
    *plot* *0xFFD8FFE000104A4649...*
  
2. Aprire un prompt dei comandi di PowerShell ed eseguire il comando seguente fornendo come argomenti i valori appropriati per nome di istanza, nome di database, nome utente e credenziali. Se si usano le identità di Windows, è possibile sostituire **-U** e **-P** con **-T**.
  
   ```powershell
   bcp "exec RPlotHistogram" queryout "plot.jpg" -S <SQL Server instance name> -d  NYCTaxi_Sample  -U <user name> -P <password> -T
   ```

   > [!NOTE]
   > Per le opzioni di comando per bcp viene fatta distinzione tra maiuscole e minuscole.
  
3. Se la connessione viene stabilita, verrà richiesto di immettere altre informazioni sul formato di file grafico.

   Premere INVIO a ogni richiesta per accettare le impostazioni predefinite, ad eccezione di quanto segue:

   + Per **prefix-length of field plot**, digitare 0.
  
   + Digitare **Y** per salvare i parametri di output e riutilizzarli in seguito.
  
   ```text
   Enter the file storage type of field plot [varbinary(max)]: 
   Enter prefix-length of field plot [8]: 0
   Enter length of field plot [0]:
   Enter field terminator [none]:
  
   Do you want to save this format information in a file? [Y/n]
   Host filename [bcp.fmt]:
   ```
  
   **Risultati**

   ```text
   Starting copy...
   1 rows copied.
   Network packet size (bytes): 4096
   Clock Time (ms.) Total     : 3922   Average : (0.25 rows per sec.)
   ```

   > [!TIP]
   > Se si salvano le informazioni di formato su file (bcp.fmt), l'utilità **bcp** genera una definizione di formato che è possibile applicare a comandi simili in futuro senza specificare le opzioni di formato di file grafico. Per usare il file di formato, aggiungere `-f bcp.fmt` alla fine di qualsiasi riga di comando, dopo l'argomento password.
  
4. Il file di output verrà creato nella stessa directory in cui è stato eseguito il comando di PowerShell. Per visualizzare il grafico, è sufficiente aprire il file plot.jpg.
  
   ![corse dei taxi con e senza mancia](media/rsql-devtut-tippedornot.jpg "corse dei taxi con e senza mancia")  
  
## <a name="create-a-stored-procedure-using-hist"></a>Creare una stored procedure usando `hist`

Solitamente i data scientist generano più visualizzazioni dei dati per ottenere informazioni dettagliate sui dati da prospettive diverse. In questo esempio verrà creata una stored procedure denominata **RPlotHist** per scrivere istogrammi, grafici a dispersione e altri elementi grafici R in formato JPG e PDF.

Questa stored procedure usa la funzione `hist` per creare l'istogramma, esportando i dati binari in formati diffusi come JPG, PDF e PNG.

1. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in Esplora oggetti fare clic con il pulsante destro del mouse sul database **NYCTaxi_Sample** e scegliere **Nuova query**.

2. Incollare lo script seguente per creare una stored procedure che traccia l'istogramma. L'esempio è denominato **RPlotHist**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPlotHist]  
   AS  
   BEGIN  
     SET NOCOUNT ON;  
     DECLARE @query nvarchar(max) =  
     N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
     @script = N'  
      # Set output directory for files and check for existing files with same names   
       mainDir <- ''C:\\temp\\plots''  
       dir.create(mainDir, recursive = TRUE, showWarnings = FALSE)  
       setwd(mainDir);  
       print("Creating output plot files:", quote=FALSE)
       
       # Open a jpeg file and output histogram of tipped variable in that file.  
       dest_filename = tempfile(pattern = ''rHistogram_Tipped_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.jpg'',sep="")  
       print(dest_filename, quote=FALSE);  
       jpeg(filename=dest_filename);  
       hist(InputDataSet$tipped, col = ''lightgreen'', xlab=''Tipped'',   
           ylab = ''Counts'', main = ''Histogram, Tipped'');  
        dev.off();  

       # Open a pdf file and output histograms of tip amount and fare amount.   
       # Outputs two plots in one row  
       dest_filename = tempfile(pattern = ''rHistograms_Tip_and_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=7);  
       par(mfrow=c(1,2));  
       hist(InputDataSet$tip_amount, col = ''lightgreen'',   
           xlab=''Tip amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram, Tip amount'', xlim = c(0,40), 100);  
       hist(InputDataSet$fare_amount, col = ''lightgreen'',   
           xlab=''Fare amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram,   
           Fare amount'',   
           xlim = c(0,100), 100);  
       dev.off();  
  
       # Open a pdf file and output an xyplot of tip amount vs. fare amount using lattice;  
       # Only 10,000 sampled observations are plotted here, otherwise file is large.  
       dest_filename = tempfile(pattern = ''rXYPlots_Tip_vs_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=4);  
       plot(tip_amount ~ fare_amount,   
           data = InputDataSet[sample(nrow(InputDataSet), 10000), ],   
           ylim = c(0,50),   
           xlim = c(0,150),   
           cex=.5,   
           pch=19,   
           col=''darkgreen'',    
           main = ''Tip amount by Fare amount'',   
           xlab=''Fare Amount ($)'',   
           ylab = ''Tip Amount ($)'');   
       dev.off();',  
     @input_data_1 = @query  
   END
   ```
  
Gli elementi più importanti da comprendere nello script sono i seguenti:

+ L'output della query SELECT all'interno della stored procedure viene archiviato nel frame di dati R predefinito `InputDataSet`. Sarà quindi possibile chiamare diverse funzioni di creazione dei grafici R per generare i file grafici. La maggior parte degli script R incorporati rappresenta le opzioni di queste funzioni di grafici,  ad esempio `plot` o `hist`.
  
+ Il dispositivo R è impostato su **off** perché si sta eseguendo il comando come script esterno in SQL Server. In genere in R quando viene eseguito un comando generale di esecuzione di tracciato viene aperta una finestra grafica detta *dispositivo*. È possibile disattivare il dispositivo se si sta scrivendo in un file o gestendo l'output in altro modo.
  
+ Tutti i file vengono salvati nella cartella locale C:\temp\Plots. La cartella di destinazione è definita dagli argomenti specificati nello script R come parte della stored procedure. Per inviare i file in una cartella diversa, modificare il valore della variabile `mainDir` nello script R incorporato nella stored procedure. È anche possibile modificare lo script per produrre l'output in formati diversi, in più file e così via.

### <a name="execute-the-stored-procedure"></a>Eseguire la stored procedure

Eseguire l'istruzione seguente per esportare i dati binari del tracciato nei formati di file JPEG e PDF.

```sql
EXEC RPlotHist
```

**Risultati**

```text
STDOUT message(s) from external script:
[1] Creating output plot files:[1] C:\temp\plots\rHistogram_Tipped_18887f6265d4.jpg[1] 

C:\temp\plots\rHistograms_Tip_and_Fare_Amount_1888441e542c.pdf[1]

C:\temp\plots\rXYPlots_Tip_vs_Fare_Amount_18887c9d517b.pdf
```

I numeri nei nomi file vengono generati in modo casuale per evitare che si verifichi un errore quando viene eseguito un tentativo di scrivere in un file esistente.

### <a name="view-output"></a>Visualizzare l'output

Per visualizzare il tracciato, aprire la cartella di destinazione ed esaminare i file creati dal codice R nella stored procedure.

1. Passare alla cartella indicata nel messaggio STDOUT (nell'esempio si tratta della cartella C:\temp\plots\)

2. Aprire `rHistogram_Tipped.jpg` per visualizzare il numero di corse per cui è stata corrisposta una mancia rispetto alle corse senza mancia. Questo istogramma è simile a quello generato nel passaggio precedente.

3. Aprire `rHistograms_Tip_and_Fare_Amount.pdf` per visualizzare la distribuzione degli importi delle mance, tracciati in base agli importi delle tariffe delle corse.

   ![istogramma che mostra l'importo delle mance e l'importo delle tariffe delle corse](media/rsql-devtut-tipamtfareamt.PNG "istogramma che mostra l'importo delle mance e l'importo delle tariffe delle corse")

4. Aprire `rXYPlots_Tip_vs_Fare_Amount.pdf` per visualizzare un grafico a dispersione con l'importo della tariffa della corsa sull'asse x e quello della mancia sull'asse y.

   ![importo della mancia tracciato sopra l'importo della tariffa](media/rsql-devtut-tipamtbyfareamt.PNG "importo della mancia tracciato sopra l'importo della tariffa")

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Rivedere i dati di esempio
> + Creare tracciati usando R in T-SQL
> + Tracciati di output in più formati di file

> [!div class="nextstepaction"]
> [Esercitazione su R: Creare funzionalità di dati](r-taxi-classification-create-features.md)