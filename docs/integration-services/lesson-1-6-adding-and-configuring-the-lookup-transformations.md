---
description: 'Lezione 1-6: Aggiungere e configurare le trasformazioni Ricerca'
title: 'Passaggio 6: Aggiungere e configurare le trasformazioni Ricerca | Microsoft Docs'
ms.custom: ''
ms.date: 03/19/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.openlocfilehash: dbfe5a2fd1472ce67edddeb2eff8a223352dccce
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125618"
---
# <a name="lesson-1-6-add-and-configure-the-lookup-transformations"></a>Lezione 1-6: Aggiungere e configurare le trasformazioni Ricerca

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Dopo aver configurato l'origine file flat per l'estrazione di dati dal file di origine si definiranno le trasformazioni Ricerca necessarie per ottenere i valori di **CurrencyKey** e **DateKey**. Una trasformazione Ricerca esegue una ricerca tramite l'unione in join dei dati della colonna di input specificata con una colonna di un set di dati di riferimento. Il set di dati di riferimento può essere una vista o tabella esistente, una nuova tabella o il risultato di un'istruzione SQL. In questa esercitazione la trasformazione Ricerca usa una gestione connessione OLE DB per connettersi al database che contiene i dati di origine del set di dati di riferimento.  
  
> [!NOTE]  
> È anche possibile configurare la trasformazione Ricerca per collegarsi a una cache che contiene il set di dati di riferimento. Per altre informazioni, vedere [Trasformazione Ricerca](../integration-services/data-flow/transformations/lookup-transformation.md).  
  
In questa attività verranno aggiunti al pacchetto e configurati i due componenti di trasformazione Ricerca seguenti:  
  
-   Una trasformazione che esegue la ricerca di valori della colonna **CurrencyKey** della tabella delle dimensioni **DimCurrency** in base alla corrispondenza con i valori della colonna **CurrencyID** del file flat.  
  
-   Una trasformazione che esegue una ricerca di valori della colonna **DateKey** della tabella delle dimensioni **DimDate** in base alla corrispondenza con i valori della colonna **CurrencyDate** del file flat.  
  
In entrambi i casi la trasformazione Ricerca usa la gestione connessione OLE DB creata in precedenza.  
  
## <a name="add-and-configure-the-lookup-currency-key-transformation"></a>Aggiungere e configurare la trasformazione Lookup Currency Key  
  
1.  Nella **Casella degli strumenti SSIS** espandere **Comune**, quindi trascinare **Ricerca** sull'area di progettazione della scheda **Flusso di dati** . Posizionare **Ricerca** esattamente sotto l'origine **Extract Sample Currency Data**.  
  
2.  Selezionare l'origine file flat **Extract Sample Currency Data** e trascinare la freccia blu corrispondente sulla trasformazione **Ricerca** appena aggiunta per collegare i due componenti.  
  
3.  Nell'area di progettazione **Flusso di dati** selezionare **Ricerca** nella trasformazione **Ricerca** e cambiare il nome in **Lookup Currency Key**.  
  
4.  Fare doppio clic sulla trasformazione **Lookup Currency Key** per visualizzare l'**Editor trasformazione Ricerca**.  
  
5.  Nella pagina **Generale** effettuare le selezioni seguenti:  
  
    1.  Selezionare **Full cache**.  
  
    2.  Nell'area **Tipo di connessione** selezionare **Gestione connessione OLE DB**.  
  
6.  Nella pagina **Connessione** effettuare le selezioni seguenti:  
  
    1.  Nella finestra di dialogo **Gestione connessione OLE DB** assicurarsi che sia visualizzato **localhost.AdventureWorksDW2012** .  
  
    2.  Selezionare **Usa i risultati di una query SQL**, quindi immettere o incollare l'istruzione SQL seguente:  
  
        ```sql
        SELECT * FROM [Sales].[Currency]
        WHERE [CurrencyCode]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
    3.  Selezionare **Anteprima** per verificare i risultati della query.
  
7.  Nella pagina **Colonne** effettuare le selezioni seguenti:  
  
    1.  Nel pannello **Colonne di input disponibili** trascinare **CurrencyID** sul pannello **Colonne di ricerca disponibili** e rilasciarlo su **CurrencyAlternateKey**.  
  
    2.  Nell'elenco **Colonne di ricerca disponibili** selezionare la casella di controllo a sinistra di **CurrencyKey**.  
  
8.  Selezionare **OK** per tornare all'area di progettazione **Flusso di dati**.  
  
9. Fare clic con il pulsante destro del mouse sulla trasformazione Lookup Currency Key e selezionare **Proprietà**.  
  
10. Nella finestra **Proprietà** verificare che la proprietà **LocaleID** corrisponda a **Inglese (Stati Uniti)** e che la proprietà **DefaultCodePage** corrisponda a **1252**.  
  
## <a name="add-and-configure-the-lookup-date-key-transformation"></a>Aggiungere e configurare la trasformazione Lookup Date Key  
  
1.  Nella **Casella degli strumenti SSIS** trascinare **Ricerca** sull'area di progettazione **Flusso di dati** . Posizionare **Ricerca** esattamente sotto la trasformazione **Lookup Currency Key**.  
  
2.  Selezionare la trasformazione **Lookup Currency Key** e trascinare la freccia azzurra corrispondente sulla nuova trasformazione **Ricerca** per collegare i due componenti.  
  
3.  Nella finestra di dialogo **Selezione input e output** selezionare **Output corrispondenza ricerca** nella casella di riepilogo **Output** e quindi selezionare **OK**.  
  
4.  Nell'area di progettazione **Flusso di dati** selezionare il nome **Ricerca** nella trasformazione **Ricerca** appena aggiunta e cambiare tale nome in **Lookup Date Key**.  
  
5.  Fare doppio clic sulla trasformazione **Lookup Date Key** .  
  
6.  Nella pagina **Generale** selezionare **Partial cache**.  
  
7.  Nella pagina **Connessione** effettuare le selezioni seguenti:  
  
    1.  Nella finestra di dialogo **Gestione connessione OLE DB** assicurarsi che sia visualizzato **localhost.AdventureWorksDW2012**.  
  
    2.  Nella casella **Usa una tabella o una vista** immettere o selezionare **[dbo].[DimDate]**.  
  
8.  Nella pagina **Colonne** effettuare le selezioni seguenti:  
  
    1.  Nel pannello **Colonne di input disponibili** trascinare **CurrencyDate** sul pannello **Colonne di ricerca disponibili** e rilasciarlo su **FullDateAlternateKey**.  Se viene visualizzato un messaggio che indica una mancata corrispondenza del tipo di dati, modificare il tipo di dati di CurrencyDate in [DT_DBDATE].
  
    2.  Nell'elenco **Colonne di ricerca disponibili** selezionare la casella di controllo a sinistra di **DateKey**.  
  
9. Nella pagina **Avanzate** rivedere le opzioni di memorizzazione nella cache.  
  
10. Selezionare **OK** per tornare all'area di progettazione **Flusso di dati**.  
  
11. Fare clic con il pulsante destro del mouse sulla trasformazione **Lookup Date Key** e selezionare **Proprietà**.
  
12. Nella finestra **Proprietà** verificare che la proprietà **LocaleID** corrisponda a **Inglese (Stati Uniti)** e che la proprietà **DefaultCodePage** corrisponda a **1252**.  
  
## <a name="go-to-next-task"></a>Esecuzione del passaggio successivo
[Passaggio 7: Aggiungere e configurare la destinazione OLE DB](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a>Vedere anche  
[Trasformazione Ricerca](../integration-services/data-flow/transformations/lookup-transformation.md)  
