---
title: Confrontare e sincronizzare i dati di due database
description: Informazioni su come confrontare e sincronizzare i dati da due database. Scoprire come configurare il confronto, visualizzare le differenze e aggiornare la destinazione.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 03/22/2021
ms.openlocfilehash: d869aaf652cf3c5d9baa389ab26303ba46049b71
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890718"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>Procedura: Confrontare e sincronizzare i dati di due database

È possibile confrontare i dati contenuti in due database. I database che si confrontano sono chiamati *origine* e *destinazione*.  
  
> [!NOTE]  
> I *progetti di database* e i pacchetti con estensione dacpac o bacpac non possono essere l'origine o la destinazione in un confronto di dati.  
  
Quando i dati vengono confrontati, viene generato uno script *Data Manipulation Language* (DML) che è possibile usare per sincronizzare i database aggiornando alcuni o tutti i dati nel database di destinazione. Al termine del confronto dei dati, i risultati vengono visualizzati nella finestra Confronto dati di Visual Studio.  
  
Al termine del confronto, è possibile eseguire altri passaggi:  
  
-   È possibile visualizzare le differenze tra i due database. Per altre informazioni, vedere [Visualizzazione delle differenze dei dati](#ViewDifferences).  
  
-   È possibile aggiornare interamente o parzialmente la destinazione in modo che corrisponda all'origine. Per altre informazioni, vedere [Sincronizzazione dei dati di database](#Synchronize).  
  
Per altre informazioni, vedere [Confrontare e sincronizzare i dati in una o più tabelle e i dati di un database di riferimento](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
> [!NOTE]  
> È inoltre possibile confrontare lo *schema* di due database o di due versioni dello stesso database. Per altre informazioni, vedere [Procedura: Usare il confronto schema per confrontare definizioni di database diverse](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>Confronto dei dati di database  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>Per confrontare i dati utilizzando la procedura guidata Nuovo confronto dati  
  
1.  Scegliere **Confronto dati** dal menu **SQL** e fare clic su **Nuovo confronto dati**.  
  
    Viene visualizzata la procedura guidata Nuovo confronto dati. Viene inoltre aperta la finestra Confronto dati e Visual Studio assegna automaticamente un nome quale DataCompare1.  
  
2.  Identificare i database di origine e di destinazione.  
  
    Se l'elenco **Database di origine** o **Database di destinazione** è vuoto, fare clic su **Nuova connessione**. Nella finestra di dialogo **Proprietà connessione** identificare il server in cui risiede il database e il tipo di autenticazione da usare per la connessione al database. Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà connessione** e tornare alla procedura guidata Confronto dati.  
  
    Nella prima pagina della procedura guidata Confronto dati verificare che le informazioni per ogni database siano corrette, specificare quali record includere nei risultati e fare clic su **Avanti**. Viene visualizzata la seconda pagina della procedura guidata Confronto dati e viene riportato un elenco gerarchico delle tabelle e viste nel database.  
  
3.  Selezionare le caselle di controllo per le tabelle e le viste da confrontare. Facoltativamente, espandere i nodi per gli oggetti di database e selezionare le caselle di controllo per le colonne negli oggetti da confrontare.  
  
    > [!NOTE]  
    > Tabelle e viste devono soddisfare due criteri per essere visualizzate nell'elenco. Innanzitutto, gli schemi degli oggetti devono corrispondere tra i database di origine e destinazione. In secondo luogo, solo le tabelle e le viste con una chiave primaria, una chiave univoca, un indice univoco o un vincolo univoco vengono visualizzate nell'elenco. Se nessuna tabella o vista soddisfa entrambi i criteri, l'elenco sarà vuoto.  
  
4.  Se sono presenti più chiavi, è possibile usare la colonna **Chiave di confronto** per specificare la chiave su cui basare il confronto dei dati. Ad esempio, è possibile specificare se basare il confronto sulla colonna chiave primaria o su un'altra colonna chiave (identificabile in modo univoco).  
  
5.  Fare clic su **Fine**.  
  
    Viene avviato il confronto.  
  
    > [!NOTE]  
    > È possibile interrompere un'operazione di confronto dei dati in corso scegliendo **Confronto dati** dal menu **SQL** e facendo clic su **Interrompi confronto dati**.  
  
    Al termine del confronto, è possibile visualizzare le differenze dei dati tra i due database. È anche possibile aggiornare parzialmente o completamente i dati nel database di destinazione in modo corrispondente ai dati nel database di origine.  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>Per confrontare i dati utilizzando il modello di automazione di Visual Studio  
  
1.  Aprire il menu **Visualizza**, scegliere **Altre finestre**, quindi **Finestra di comando**.  
  
2.  Nella finestra di comando, digitare il comando seguente:  
  
    ```  
    Tools.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    Sostituire i segnaposto (*sServerName*, *sDatabaseName*, *sUserName*, *sPassword*, *sDisplayName*, *tServerName*, *tDatabaseName*, *tUserName*, *tPassword* e *tDisplayName*) con i valori relativi ai database di origine e di destinazione.  
  
    Se non si specifica un'origine e una destinazione, viene visualizzata la finestra di dialogo **Nuovo confronto dati**. Per ulteriori informazioni sui parametri per il comando Tools. NewDataComparison, vedere informazioni [di riferimento sui comandi di automazione per le funzionalità di database di Visual Studio Team System](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100)).  
  
    I dati nei database di origine e di destinazione specificati vengono confrontati. I risultati vengono visualizzati in una sessione di Confronto dati. Per altre informazioni sulla visualizzazione dei risultati o sulla sincronizzazione dei dati, vedere [Visualizzazione delle differenze dei dati](#ViewDifferences) e [Sincronizzazione dei dati di database](#Synchronize).  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>Visualizzazione delle differenze dei dati  
Una volta completato il confronto dei dati nei due database, in Confronto dati viene elencato ogni *oggetto di database* confrontato e ne viene riportato lo stato. È anche possibile visualizzare i risultati per i record in ogni oggetto, raggruppati in base allo stato. Per altre informazioni sulle designazioni dello stato, vedere [Confrontare e sincronizzare i dati in una o più tabelle e i dati di un database di riferimento](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
Dopo avere visualizzato le differenze, è possibile aggiornare la destinazione in modo corrispondente all'origine per alcuni o tutti gli oggetti o record diversi, mancanti o nuovi. Per altre informazioni, vedere [Sincronizzazione dei dati di database](#Synchronize).  
  
#### <a name="to-view-data-differences"></a>Per visualizzare le differenze dei dati  
  
1.  Confrontare i dati in un database di origine e di destinazione. Per altre informazioni, vedere [Confronto dei dati di database](#CompareDatabaseData).  
  
2.  (Facoltativo) Eseguire una o entrambe le operazioni seguenti:  
  
    -   Per impostazione predefinita, sono visualizzati i risultati per tutti gli oggetti, indipendentemente dal loro stato. Per visualizzare solo gli oggetti che presentano uno stato specifico, scegliere un'opzione nell'elenco **Filtro**.  
  
    -   Per visualizzare i risultati per i record in un oggetto specifico, fare clic sull'oggetto nel riquadro principale dei risultati e quindi fare clic su una scheda nel riquadro di visualizzazione dei record. In ogni scheda vengono visualizzati tutti i record nell'oggetto che presenta uno stato particolare: diverso, solo nell'origine, solo nella destinazione e identico. I dati vengono visualizzati per record e per colonna.  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>Sincronizzazione dei dati di database  
Dopo aver confrontato i dati in due database, è possibile sincronizzarli aggiornando completamente o parzialmente la destinazione in modo che corrisponda all'origine. È possibile confrontare i dati in due tipi di oggetti di database: tabelle e viste.  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>Per aggiornare i dati di destinazione tramite il comando Scrivi aggiornamenti  
  
1.  Confrontare i dati in un database di origine e di destinazione. Per altre informazioni, vedere [Confronto dei dati di database](#CompareDatabaseData).  
  
    Al termine del confronto, nella finestra Confronto dati vengono elencati i risultati per gli oggetti confrontati. Quattro colonne (denominate Record diversi, Solo nell'origine, Solo nella destinazione e Record identici) visualizzano informazioni sugli oggetti che non sono identici. Per ognuno di questi oggetti, queste colonne indicano il numero di record rilevati diversi e il numero di record che verrebbero modificati da un'operazione di aggiornamento. Questi due numeri inizialmente sono uguali, ma nel passaggio 4 è possibile modificare quali oggetti aggiornare.  
  
    Per altre informazioni, vedere [Confrontare e sincronizzare i dati in una o più tabelle e i dati di un database di riferimento](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  Nella tabella della finestra Confronto dati fare clic su una riga.  
  
    Il riquadro dei dettagli mostra i risultati per i record nell'oggetto di database selezionato. I record vengono raggruppati in base allo stato in schede che è possibile utilizzare per specificare i dati che verranno propagati dall'origine alla destinazione.  
  
3.  Nel riquadro dei dettagli fare clic su una scheda il cui nome contenga un numero diverso da zero (0).  
  
    La colonna **Aggiorna** della tabella **Solo nella destinazione** contiene caselle di controllo che è possibile usare per selezionare le righe da aggiornare. Per impostazione predefinita, tutte le caselle di controllo sono selezionate.  
  
4.  Deselezionare le caselle di controllo per i record nella destinazione che non si desidera aggiornare con i dati dall'origine.  
  
    Quando si deseleziona una casella di controllo, si riduce il numero dei record da aggiornare e la visualizzazione viene modificata per riflettere le azioni eseguite. Questo numero viene visualizzato nella riga dello stato del riquadro dei dettagli e nella colonna corrispondente nel riquadro principale dei risultati, come descritto nel passaggio 1.  
  
5.  (Facoltativo) Fare clic su **Genera script**.  
  
    Viene aperta una finestra dell'editor Transact\-SQL e viene visualizzato lo script *Data Manipulation Language* (DML) che verrà usato per aggiornare la destinazione.  
  
6.  Per sincronizzare record diversi, mancanti o nuovi, fare clic su **Aggiorna destinazione**.  
  
    > [!NOTE]  
    > Mentre il database di destinazione viene aggiornato, è possibile annullare l'operazione facendo clic su **Interrompi scrittura sulla destinazione**.  
  
    I dati dei record selezionati nella destinazione vengono aggiornati con i dati dei record corrispondenti nell'origine.  
  
    > [!NOTE]  
    > Se si decide di aggiornare le viste indicizzate, l'operazione **Aggiorna destinazione** potrebbe non riuscire se tale azione causa l'inserimento di chiavi duplicate nella stessa tabella.  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>Per aggiornare i dati di destinazione usando uno script Transact\-SQL  
  
1.  Confrontare i dati in un database di origine e di destinazione. Per altre informazioni, vedere [Confronto dei dati di database](#CompareDatabaseData).  
  
    Al termine del confronto, nella finestra Confronto dati vengono elencati gli oggetti confrontati. Per altre informazioni, vedere [Confrontare e sincronizzare i dati in una o più tabelle e i dati di un database di riferimento](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  (Facoltativo) Nel riquadro dei dettagli deselezionare le caselle di controllo per i record nella destinazione che non si desidera aggiornare, come descritto nella procedura precedente.  
  
3.  Fare clic su **Genera script**.  
  
    In una nuova finestra viene visualizzato lo script Transact\-SQL che propagherebbe le modifiche necessarie per rendere i dati nella destinazione corrispondenti ai dati nell'origine. Alla nuova finestra viene assegnato un nome quale **DataUpdate_Database_1.sql**.  
  
    Questo script riflette le modifiche apportate nel riquadro dei dettagli. Ad esempio, è possibile che si sia deselezionata una casella di controllo per una determinata riga nella pagina Solo nella destinazione per la tabella [dbo].[Shippers]. In questo caso, lo script non aggiorna tale riga.  
  
4.  (Facoltativo) Modificare questo script nella finestra **DataUpdate_Database_1.sql**.  
  
5.  (Facoltativo ma consigliato) Eseguire il backup del database di destinazione.  
  
6.  Fare clic su **Esegui** per aggiornare il database di destinazione.  
  
    Specificare una connessione al database di destinazione che si desidera aggiornare.  
  
    > [!IMPORTANT]  
    > Per impostazione predefinita, gli aggiornamenti vengono eseguiti nell'ambito di una transazione. Se si verificano errori, è possibile eseguire il rollback dell'intero aggiornamento. Questo comportamento può essere modificato.  
  
    I dati dei record selezionati nella destinazione vengono aggiornati con i dati dei record corrispondenti nell'origine.  
  
## <a name="see-also"></a>Vedere anche  
[Confrontare e sincronizzare i dati in una o più tabelle e i dati di un database di riferimento](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  
