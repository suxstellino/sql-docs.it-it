---
title: Aggiungere parametri di propagazione a un report (Generatore report) | Microsoft Docs
description: Informazioni su come includere parametri di propagazione nei report in Generatore report per gestire grandi quantità di dati.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 3a22eec3-57a7-478e-b6fc-102a9dbe0591
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 77a96c10d94bb27d52f8527e6822ff036e4a214e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596759"
---
# <a name="add-cascading-parameters-to-a-report-report-builder-and-ssrs"></a>Aggiunta di parametri di propagazione a un report (Generatore report e SSRS)
  I parametri di propagazione consentono di gestire quantità elevate di dati del report. È possibile definire un set di parametri correlati in modo che l'elenco dei valori di un parametro dipenda dal valore scelto per un altro parametro. Il primo parametro può essere ad esempio indipendente e presentare un elenco di categorie di prodotti. Quando l'utente seleziona una categoria, il secondo parametro dipende dal valore del primo parametro. I relativi valori vengono aggiornati con un elenco di sottocategorie all'interno della categoria scelta. Quando l'utente visualizza il report, per filtrarne i dati vengono utilizzati sia i valori dei parametri di categoria che di sottocategoria.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Per creare parametri di propagazione, è innanzitutto necessario definire la query del set di dati e includere un parametro di query per ogni parametro di propagazione necessario. È inoltre necessario creare un set di dati distinto per ogni parametro di propagazione allo scopo di fornire i valori disponibili. Per altre informazioni, vedere [Aggiungere, modificare o eliminare valori disponibili per un parametro di report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-available-values-for-a-report-parameter.md).  
  
 Per i parametri di propagazione l'ordine è importante in quanto la query del set di dati relativa a un parametro riportato più avanti nell'elenco include un riferimento a ciascun parametro riportato in precedenza. In fase di esecuzione l'ordine dei parametri nel riquadro dei dati del report determina l'ordine in cui le query del parametro vengono visualizzate nel report e quindi l'ordine in cui un utente sceglie ogni valore del parametro successivo.  
  
 Per informazioni sulla creazione di parametri a cascata con più valori includendo la caratteristica Seleziona tutto, vedere [How to have a Select All Multi-Value Cascading Parameter](/archive/blogs/psssql/)(Come creare un parametro a cascata multivalore Seleziona tutto).  
  
## <a name="to-create-the-main-dataset-with-a-query-that-includes-multiple-related-parameters"></a>Per creare il set di dati principale con una query che include più parametri correlati  
  
1.  Nel riquadro dei dati del report fare clic con il pulsante destro del mouse su un'origine dati e quindi scegliere **Aggiungi set di dati**.  
  
2.  Nella casella **Nome** digitare il nome del set di dati.  
  
3.  In **Origine dati** scegliere il nome dell'origine dati o fare clic su **Nuova** per crearne una.  
  
4.  In **Tipo di query** scegliere il tipo di query per l'origine dati selezionata. In questo argomento si presuppone che venga usato il tipo di query **Testo** .  
  
5.  In **Query** digitare la query da usare per recuperare i dati per questo report. La query deve includere le parti seguenti:  
  
    1.  Un elenco di campi dell'origine dati. In un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] , ad esempio, l'istruzione SELECT specifica un elenco di nomi di colonne del database di una determinata tabella o vista.  
  
    2.  Un parametro della query per ogni parametro di propagazione. Un parametro della query limita i dati recuperati dall'origine dati specificando determinati valori da includere o escludere dalla query. In genere, i parametri della query si trovano in una clausola di restrizione nella query. In un'istruzione SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] , ad esempio, i parametri della query si trovano nella clausola WHERE.  
  
6.  Fare clic su **Esegui** ( **!** ). Dopo aver incluso i parametri della query e aver eseguito la query, verranno creati automaticamente i parametri del report corrispondenti ai parametri della query.  
  
    > [!NOTE]  
    >  L'ordine in cui si presentano i parametri della query quando si esegue per la prima volta una query determina anche l'ordine in cui vengono creati nel report. Per modificare l'ordine, vedere [Modificare l'ordine di un parametro del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/change-the-order-of-a-report-parameter-report-builder-and-ssrs.md)  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Procedere quindi alla creazione di un set di dati che specifichi i valori del parametro indipendente.  
  
## <a name="to-create-a-dataset-to-provide-values-for-an-independent-parameter"></a>Per creare un set di dati in modo da specificare i valori di un parametro indipendente  
  
1.  Nel riquadro dei dati del report fare clic con il pulsante destro del mouse su un'origine dati e quindi scegliere **Aggiungi set di dati**.  
  
2.  Nella casella **Nome** digitare il nome del set di dati.  
  
3.  In **Origine dati** verificare che il nome sia quello dell'origine dati selezionata nel passaggio 1.  
  
4.  In **Tipo di query** scegliere il tipo di query per l'origine dati selezionata. In questo argomento si presuppone che venga usato il tipo di query **Testo** .  
  
5.  In **Query** digitare la query da usare per recuperare i valori per questo parametro. In genere le query per i parametri indipendenti non contengono parametri di query. Per creare, ad esempio, una query per un parametro che specifichi tutti i valori di categoria, è possibile usare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] simile alla seguente:  
  
    ```  
    SELECT DISTINCT <column name> FROM <table>  
    ```  
  
     Il comando SELECT DISTINCT rimuove i valori duplicati dal set di risultati in modo che sia possibile ottenere tutti i valori univoci dalla colonna specificata nella tabella indicata.  
  
     Fare clic su **Esegui** ( **!** ). Nel set di risultati sono riportati i valori disponibili per il primo parametro.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Impostare quindi le proprietà del primo parametro in modo da utilizzare questo set di dati per popolarne i valori disponibili in fase di esecuzione.  
  
## <a name="to-set-available-values-for-a-report-parameter"></a>Per impostare i valori disponibili per un parametro di report  
  
1.  Nella cartella Parametri del riquadro dei dati del report fare clic con il pulsante destro del mouse sul primo parametro e quindi scegliere **Proprietà parametri**.  
  
2.  Nella casella **Nome** verificare che il nome del parametro sia corretto.  
  
3.  Fare clic su **Valori disponibili**.  
  
4.  Fare clic su **Ottieni valori da una query**. Verranno visualizzati tre campi.  
  
5.  Nell'elenco a discesa di **Set di dati** fare clic sul nome del set di dati creato nella procedura precedente.  
  
6.  Nel campo **Valore** fare clic sul nome del campo contenente il valore del parametro.  
  
7.  Nel campo **Etichetta** fare clic sul nome del campo che fornisce l'etichetta del parametro.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Procedere quindi alla creazione di un set di dati che specifichi i valori di un parametro dipendente.  
  
## <a name="to-create-a-dataset-to-provide-values-for-a-dependent-parameter"></a>Per creare un set di dati in modo da specificare i valori di un parametro dipendente  
  
1.  Nel riquadro dei dati del report fare clic con il pulsante destro del mouse su un'origine dati e quindi scegliere **Aggiungi set di dati**.  
  
2.  Nella casella **Nome** digitare il nome del set di dati.  
  
3.  In **Origine dati** verificare che il nome sia quello dell'origine dati selezionata nel passaggio 1.  
  
4.  In **Tipo di query** scegliere il tipo di query per l'origine dati selezionata. In questo argomento si presuppone che venga usato il tipo di query **Testo** .  
  
5.  In **Query** digitare la query da usare per recuperare i valori per questo parametro. In genere le query per i parametri dipendenti includono parametri di query per ogni parametro dal quale dipende questo parametro. Per creare, ad esempio, una query per un parametro che specifichi tutti i valori di sottocategoria (parametro dipendente) per una categoria (parametro indipendente), è possibile usare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] simile alla seguente:  
  
    ```  
    SELECT DISTINCT Subcategory FROM <table>   
    WHERE (Category = @Category)  
    ```  
  
     Nella clausola WHERE, Category è il nome di un campo di \<table> e @Category è un parametro di query. Questa istruzione produce un elenco di sottocategorie per la categoria specificata in @Category. In fase di esecuzione tale valore verrà compilato con il valore selezionato dall'utente per il parametro del report con nome identico.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Impostare quindi le proprietà del secondo parametro in modo da utilizzare questo set di dati per popolarne i valori disponibili in fase di esecuzione.  
  
## <a name="to-set-available-values-for-the-second-parameter"></a>Per impostare i valori disponibili per il secondo parametro  
  
1.  Nella cartella Parametri del riquadro dei dati del report fare clic con il pulsante destro del mouse sul primo parametro e quindi scegliere **Proprietà parametri**.  
  
2.  Nella casella **Nome** verificare che il nome del parametro sia corretto.  
  
3.  Fare clic su **Valori disponibili**.  
  
4.  Fare clic su **Ottieni valori da una query**.  
  
5.  Nell'elenco a discesa di **Set di dati** fare clic sul nome del set di dati creato nella procedura precedente.  
  
6.  Nel campo **Valore** fare clic sul nome del campo contenente il valore del parametro.  
  
7.  Nel campo **Etichetta** fare clic sul nome del campo che fornisce l'etichetta del parametro.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-test-the-cascading-parameters"></a>Per verificare i parametri di propagazione  
  
1.  Fare clic su **Esegui**.  
  
2.  Scegliere un valore nell'elenco a discesa del primo parametro indipendente.  
  
     Il componente Elaborazione report esegue la query del set di dati per il parametro successivo e passa il valore scelto per il primo parametro. L'elenco a discesa per il secondo parametro viene popolato con i valori disponibili basati sul valore del primo parametro.  
  
3.  Nell'elenco a discesa del secondo parametro dipendente scegliere un valore.  
  
     In seguito alla selezione dell'ultimo parametro il report non viene eseguito automaticamente in modo da consentire all'utente di modificare la scelta effettuata.  
  
4.  Fare clic su **Visualizza report**. La visualizzazione del report verrà aggiornata in base ai parametri scelti.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere, modificare o eliminare un parametro di report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)   
 [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Esercitazione: Aggiungere un parametro al report &#40;Generatore report&#41;](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)   
 [Esercitazioni di Generatore report](../../reporting-services/report-builder-tutorials.md)   
 [Aggiungere filtri per set di dati, aree dati e gruppi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
