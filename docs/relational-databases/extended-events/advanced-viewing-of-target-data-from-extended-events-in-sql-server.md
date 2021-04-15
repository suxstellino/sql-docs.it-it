---
title: Visualizzazione avanzata dei dati di destinazione da eventi estesi
description: Usare le funzionalità avanzate di SQL Server Management Studio per visualizzare in dettaglio i dati di destinazione da eventi estesi. È possibile visualizzare, esportare e modificare i dati.
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: b2e839d7-1872-46d9-b7b7-6dcb3984829f
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6d1c01e5b9dc88aa2e36da44838142d86f3bc2a2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492469"
---
# <a name="advanced-viewing-of-target-data-from-extended-events-in-sql-server"></a>Visualizzazione avanzata dei dati di destinazione da eventi estesi in SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


Questo articolo illustra come è possibile usare le funzionalità avanzate di SQL Server Management Studio (SSMS.exe) per visualizzare in dettaglio i dati di destinazione da eventi estesi. L'articolo spiega come:


- Aprire e visualizzare i dati di destinazione in diversi modi.
- Esportare i dati di destinazione in diversi formati, usando il menu o la barra degli strumenti specifica per gli eventi estesi.
- Modificare i dati durante la visualizzazione o prima dell'esportazione.



### <a name="prerequisites"></a>Prerequisiti

Il presente articolo presuppone che si sappia già come creare e avviare una sessione evento. Le istruzioni per creare una sessione evento sono già state illustrate nell'articolo seguente:

[Avvio rapido: Eventi estesi in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)


Questo articolo presuppone anche che sia stata installata una versione mensile molto recente di SSMS. La guida all'installazione è disponibile alla pagina:

- [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)



### <a name="differences-with-azure-sql-database"></a>Differenze con il database SQL di Azure


L'implementazione e le funzionalità degli eventi estesi nei due prodotti Microsoft SQL Server e database SQL di Azure sono molto simili, ma esistono alcune differenze relative all'interfaccia utente di SSMS.


- Per il database SQL, la destinazione package0.event_file non può essere un file sull'unità disco locale, ma è necessario usare un contenitore di archiviazione di Azure. Quindi, quando si è connessi al database SQL, l'interfaccia utente di SSMS chiede un contenitore di archiviazione, invece di un percorso locale e di un nome file.


- Nell'interfaccia utente di SSMS la casella di controllo **Controlla dati dinamici** è in grigio e disabilitata perché tale funzionalità non è disponibile per il database SQL.


- Alcuni eventi estesi vengono installati con SQL Server. Sotto il nodo **Sessioni** è possibile osservare **AlwaysOn_health** e un altro paio di elementi che non sono visibili quando si è connessi al database SQL perché non esistono per il database SQL.


Il presente articolo è scritto dal punto di vista di SQL Server. L'articolo usa la destinazione event_file, che è una delle differenze. Le altre differenze citate sono quelle più importanti o meno evidenti.


Per la documentazione sugli eventi estesi specifica del database SQL di Azure, vedere:

- [Eventi estesi nel database SQL di Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)



## <a name="a-general-options"></a>R. Opzioni generali


Le opzioni avanzate generalmente sono accessibili nei modi seguenti:


- Dal normale menu **File** > **Apri** > **File**.
- Facendo clic con il pulsante destro del mouse in **Esplora oggetti** su **Gestione** > **Eventi estesi**.
- Dallo speciale menu **Eventi estesi** e dalla speciale barra degli strumenti per gli eventi estesi.
- Facendo clic con il pulsante destro del mouse nel riquadro a schede che visualizza i dati di destinazione.



## <a name="b-bring-target-data-into-ssms-for-display"></a>B. Trasferire i dati di destinazione in SSMS per visualizzarli


È possibile trasferire i dati della destinazione event_file nell'interfaccia utente di SSMS in diversi modi. Quando si specifica una destinazione event_file, si impostano il percorso e il nome file:

- XEL è l'estensione del nome file.


- Ogni volta che la sessione evento viene avviata, il sistema incorpora un numero intero elevato in un nuovo nome file, per renderlo univoco e distinguerlo da quello dell'occasione precedente in cui la sessione è stata avviata.
  - *Esempio:* Checkpoint_Begins_ES_0_131103935140400000.xel


- I contenuti di un file XEL non sono testo normale visualizzabile con Notepad.exe.
  - Se necessario, per accodare più file XEL insieme, è possibile usare il menu **File** > **Apri** > **Unisci file eventi estesi**.



SSMS può visualizzare dati da qualsiasi destinazione, ma le visualizzazioni sono diverse a seconda della destinazione:

- *event_file:* i dati da una destinazione event_file vengono visualizzati molto chiaramente e con funzionalità avanzate.


- *ring_buffer:* i dati da una destinazione ring_buffer vengono visualizzati come XML non elaborato.


- Per le altre destinazioni, il risultato della destinazione è una via di mezzo tra quello di event_file e di ring_buffer.
  - Tra le altre destinazioni sono incluse event_counter, histogram e pair_matching.


- *etw_classic_sync_target:* SSMS non è in grado di visualizzare dati dal tipo di destinazione etw_classic_sync_target.



### <a name="b1-open-xel-with-menu-file--open--file"></a>B.1 Aprire il file XEL dal menu File > Apri > File


È possibile aprire un singolo file XEL dal menu standard **File** > **Apri** > **File**.

È anche possibile trascinare e rilasciare un file XEL sulla barra della scheda nell'interfaccia utente di SSMS.



### <a name="b2-view-target-data"></a>B.2 Visualizzare i dati di destinazione


L'opzione **Visualizza dati di destinazione** visualizza i dati acquisiti finora.


Nel riquadro **Esplora oggetti** è possibile espandere i nodi e quindi fare clic con il pulsante destro del mouse:

- **Gestione** > **Eventi estesi** > **Sessioni** >  *[sessione-utente]*  >  *[nodo-destinazione-utente]*  > **Visualizza dati di destinazione**.


I dati di destinazione vengono visualizzati in un riquadro a schede in SSMS, come illustrato nello screenshot seguente.


![Destinazione utente > Visualizza dati di destinazione](../../relational-databases/extended-events/media/xevents-ssms-ui20-viewtargetdata.png)


> [!NOTE] 
> **Visualizza dati di destinazione** visualizza i *dati accumulati da più file XEL* dalla sessione evento specificata. Ogni ciclo **Avvia**-**Arresta** crea un file con un numero intero derivato successivamente incorporato nel nome, ma ogni file condivide lo stesso nome radice.



#### <a name="b3-watch-live-data"></a>B.3 Controllare i dati dinamici


Quando la sessione evento è attiva, potrebbe essere necessario controllare i dati dell'evento in tempo reale, non appena vengono ricevuti dalla destinazione.


- **Gestione** > **Eventi estesi** > **Sessioni** >  *[sessione-utente]*  > **Controlla dati dinamici**.


![Sessione utente > Controlla dati dinamici](../../relational-databases/extended-events/media/xevents-ssms-ui55-watchlivedata.png)


La visualizzazione dei dati viene aggiornata in base all'intervallo specificato. Vedere **Latenza di recapito massima** in:

- **Eventi estesi** > **Sessioni** >  *[sessione-utente]*  > **Proprietà** > **Avanzate** > **Latenza di recapito massima**



### <a name="b4-view-xel-with-sysfn_xe_file_target_read_file-function"></a>B.4 Visualizzare il file XEL con la funzione sys.fn_xe_file_target_read_file


Per l'elaborazione batch, la funzione di sistema seguente può generare il codice XML per i record in un file XEL:

- [sys.fn\_xe\_file\_target\_read\_file](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)



## <a name="c-export-the-target-data"></a>C. Esportare i dati di destinazione


Una volta che i dati di destinazione sono presenti in SSMS, è possibile esportarli in formati diversi eseguendo queste operazioni:


1. Spostare lo stato attivo sulla visualizzazione dei dati.

    - Diventano immediatamente visibili una nuova barra degli strumenti e una nuova voce di menu per gli eventi estesi.

    ![Esportare i dati visualizzati, Eventi estesi > Esporta in > (CSV o XEL oppure una tabella)](../../relational-databases/extended-events/media/xevents-ssms-ui75-menuextevent-exportto-xel.png)

2. Fare clic sulla nuova voce di menu **Eventi estesi**.
3. Fare clic su **Esporta in** e quindi scegliere un formato.



## <a name="d-manipulate-data-in-the-display"></a>D. Modificare i dati nella visualizzazione


L'interfaccia utente di SSMS consente di modificare i dati in diversi modi, ma anche di visualizzarli semplicemente così come sono.



### <a name="d1-context-menus-in-the-data-display"></a>D.1 Menu di scelta rapida nella visualizzazione dati


Quando si fa clic con il pulsante destro del mouse nella visualizzazione dati, si aprono menu di scelta rapida diversi a seconda della posizione.



#### <a name="d11-right-click-a-data-cell"></a>D.1.1 Fare clic con il pulsante destro del mouse su una cella di dati


Lo screenshot seguente illustra il menu contestuale visualizzato quando si fa clic con il pulsante destro del mouse nella visualizzazione dati. Lo screenshot illustra anche l'espansione della voce di menu **Copia** .


![Clic con il pulsante destro del mouse su una cella, nella visualizzazione dei dati](../../relational-databases/extended-events/media/xevents-ssms-ui25-rightclickcell.png)



#### <a name="d12-right-click-a-column-header"></a>D.1.2 Fare clic con il pulsante destro del mouse su un'intestazione di colonna


Lo screenshot seguente illustra il menu di scelta rapida visualizzato facendo clic con il pulsante destro del mouse sull'intestazione **timestamp** .


![Clic con il pulsante destro del mouse su un'intestazione di colonna, nella visualizzazione dei dati e griglia dei dettagli](../../relational-databases/extended-events/media/xevents-ssms-ui40-toolbar.png)


Lo screenshot precedente illustra anche la speciale barra degli strumenti per gli eventi estesi. La luminosità del pulsante Dettagli indica che è attivo. L'immagine quindi illustra anche la scheda **Dettagli** . La griglia è una parte ulteriore della visualizzazione dati.



### <a name="d2-choose-columns-merge-columns"></a>D.2 Scegliere e unire le colonne


L'opzione **Scegli colonne** consente di controllare quali colonne di dati vengono visualizzate e quali no. È possibile trovare la voce di menu **Scegli colonne** in alcune posizioni diverse:

- Nel menu **Eventi estesi** .
- Sulla barra degli strumenti Eventi estesi.
- Nel menu di scelta rapida di un'intestazione nella visualizzazione dati.


Quando si fa clic su **Scegli colonne**, viene visualizzata la finestra di dialogo con lo stesso nome.


![Finestra di dialogo Scegli colonne, con le opzioni Colonne unite](../../relational-databases/extended-events/media/xevents-ssms-ui35-choosecolumns.png)



#### <a name="d21-merge-columns"></a>D.2.1 Unire le colonne


La finestra di dialogo **Scegli colonne** ha una sezione dedicata all'unione di più colonne in una sola, a fini di:

- Visualizzazione.
- Esportazione.



### <a name="d3-filters"></a>D.3 Filtri


Nell'area degli eventi estesi è possibile specificare due tipi principali di filtri:

- *Filtri precedenti la destinazione:* filtri che riducono la quantità di dati inviati dal motore degli eventi alla destinazione.

- *Filtri successivi alla destinazione:* filtri che è possibile selezionare nell'interfaccia utente di SSMS per escludere alcuni record di destinazione dalla visualizzazione.


I filtri per la visualizzazione in SSMS sono i seguenti:

- Un filtro basato sull' *intervallo di tempo* , che esamina la colonna **timestamp** .
- Un filtro basato sui *valori di colonna* .


La relazione tra il filtro temporale e il filtro colonne è un operatore booleano "*AND*".


![Intervallo di tempo e filtri colonne nella finestra di dialogo Filtri](../../relational-databases/extended-events/media/xevents-ssms-ui45-filters.png)



### <a name="d4-grouping-and-aggregation"></a>D.4 Raggruppamento e aggregazione


Il raggruppamento di righe in base ai valori corrispondenti in una determinata colonna è il primo passaggio per eseguire un'aggregazione di riepilogo dei dati.



#### <a name="d41-grouping"></a>D.4.1 Raggruppamento


Sulla barra degli strumenti degli eventi estesi il pulsante **Raggruppamento** apre una finestra di dialogo che è possibile usare per raggruppare i dati visualizzati in base a una determinata colonna. Lo screenshot successivo illustra una finestra di dialogo usata per il raggruppamento in base alla colonna *name*.

![Screenshot che mostra la barra degli strumenti con Raggruppamento selezionato e la finestra di dialogo Raggruppamento.](../../relational-databases/extended-events/media/xevents-ssms-ui53-grouping.png)

Una volta eseguito il raggruppamento, la visualizzazione assume un nuovo aspetto, come illustrato sotto.

![Nuovo aspetto della visualizzazione dopo il raggruppamento](../../relational-databases/extended-events/media/xevents-ssms-ui54-grouped.png)



#### <a name="d42-aggregation"></a>D.4.2 Aggregazione


Una volta raggruppati i dati visualizzati, è possibile aggregarli in altre colonne.  Lo screenshot successivo illustra come i dati raggruppati vengono aggregati in base a *count*.

![Screenshot che mostra la barra degli strumenti con Aggregazione selezionata e la finestra di dialogo Aggregazione.](../../relational-databases/extended-events/media/xevents-ssms-ui51-aggregdialogcount.png)

Una volta eseguita l'aggregazione, la visualizzazione assume un nuovo aspetto, come illustrato sotto.

![Screenshot della visualizzazione che indica che è stato aggiunto un valore COUNT.](../../relational-databases/extended-events/media/xevents-ssms-ui52-aggregnewdisplay.png)



### <a name="d5-view-run-time-query-plan"></a>D.5 Visualizzare il piano di query in fase di esecuzione


L'evento **query_post_execution_showplan** consente di visualizzare il piano di query effettivo nell'interfaccia utente di SSMS. Quando il riquadro **Dettagli** è visibile, è possibile visualizzare un grafico del piano di query nella scheda **Query Plan** . Passando il puntatore su un nodo nel piano di query, è possibile visualizzare un elenco di nomi di proprietà e i rispettivi valori per il nodo.


![Piano di query, con l'elenco di proprietà per un nodo](../../relational-databases/extended-events/media/xevents-ssms-ui60-showplangraph.png)

## <a name="see-also"></a>Vedere anche

[XELite: libreria multipiattaforma per la lettura di XEvent da file XEL o flussi SQL live](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/), rilasciata a maggio 2019.

[Cmdlet di PowerShell Read-SQLXEvent](https://www.powershellgallery.com/packages/SqlServer), rilasciato nel luglio 2019.