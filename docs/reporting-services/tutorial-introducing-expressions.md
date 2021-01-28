---
title: 'Esercitazione: Introduzione alle espressioni | Microsoft Docs'
description: Informazioni su come usare espressioni con operatori e funzioni comuni per creare report impaginati di Reporting Services efficaci e flessibili.
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 2d05ef4c-5f91-48b2-8795-f0a201a0b3cc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 938ac8fc2e006bc74aab59955374f1b012b66b68
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98669121"
---
# <a name="tutorial-introducing-expressions"></a>Esercitazione: Introduzione alle espressioni
In questa esercitazione di [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] vengono usate espressioni e operatori comuni per creare report impaginati di [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] potenti e flessibili. 

Vengono usate espressioni con le quali concatenare valori di nomi, ricercare valori in un set di dati separato, visualizzare colori diversi in base ai valori dei campi e così via.  
  
Il report è a righe alternate evidenziate in bianco e in un altro colore. che può essere selezionato tramite un parametro incluso nel report.  
  
Nell'immagine seguente viene illustrato un report simile a quello che verrà creato.  
  
![Screenshot di un report di Generatore report in cui vengono usate espressioni.](../reporting-services/media/report-builder-expression-tutorial-in-browser.png) 
  
Tempo stimato per il completamento dell'esercitazione: 30 minuti.  
  
## <a name="requirements"></a>Requisiti  
Per informazioni sui requisiti, vedere [Prerequisiti per le esercitazioni &#40;Generatore report&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Setup"></a>1. Creare un report tabella e un set di dati dalla Creazione guidata tabella o matrice  
In questa sezione vengono creati un report tabella, un'origine dati e un set di dati. Durante la disposizione della tabella verranno inclusi solo pochi campi. Dopo aver completato la procedura guidata, si aggiungeranno manualmente colonne. La procedura guidata semplifica la definizione del layout della tabella.  
  
> [!NOTE]  
> In questa esercitazione la query contiene i valori dei dati e non richiede un'origine dati esterna. Tale condizione rende tuttavia la query piuttosto lunga. In una query di un ambiente aziendale non sarebbe incluso alcun dato. Questo esempio è solo a scopo illustrativo.  
  
### <a name="to-create-a-table-report"></a>Per creare un report tabella  
  
1.  [Avviare Generatore report](../reporting-services/report-builder/start-report-builder.md) dal computer, dal portale Web di [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] o in modalità integrata SharePoint.  
  
    Si apre la finestra di dialogo **Nuovo report o set di dati** .  
  
    Se la finestra di dialogo **Nuovo report o set di dati** non viene visualizzata, scegliere **Nuovo** dal menu **File**.  
  
2.  Nel riquadro sinistro verificare che sia selezionata l'opzione **Nuovo report** .  
  
3.  Nel riquadro destro fare clic su **Creazione guidata tabella o matrice**.  
  
4.  Nella pagina **Scegliere un set di dati** fare clic su **Crea un set di dati** > **Avanti**.  
  
6.  Nella pagina **Scegliere una connessione a un'origine dei dati** selezionare un'origine dati che sia del tipo **SQL Server**. Selezionare un'origine dati dall'elenco o passare al server di report per selezionarne una.  

    > [!NOTE]  
    > L'origine dati scelta non ha importanza purché si disponga delle autorizzazioni appropriate. Non verranno recuperati dati dall'origine dati. Per altre informazioni, vedere [Modalità alternative di acquisizione di una connessione dati &#40;Generatore report&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina **Progetta query** fare clic su **Modifica come testo**.  
  
9. Incollare la query seguente nel relativo riquadro:  
  
    ```  
    SELECT 'Lauren' AS FirstName,'Johnson' AS LastName, 'American Samoa' AS StateProvince, 1 AS CountryRegionID,'Female' AS Gender, CAST(9996.60 AS money) AS YTDPurchase, CAST('2015-6-10' AS date) AS LastPurchase  
    UNION SELECT'Warren' AS FirstName, 'Pal' AS LastName, 'New South Wales' AS StateProvince, 2 AS CountryRegionID, 'Male' AS Gender, CAST(5747.25 AS money) AS YTDPurchase, CAST('2015-7-3' AS date) AS LastPurchase  
    UNION SELECT 'Fernando' AS FirstName, 'Ross' AS LastName, 'Alberta' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(9248.15 AS money) AS YTDPurchase, CAST('2015-10-17' AS date) AS LastPurchase  
    UNION SELECT 'Rob' AS FirstName, 'Caron' AS LastName, 'Northwest Territories' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(742.50 AS money) AS YTDPurchase, CAST('2015-4-29' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Bailey' AS LastName, 'British Columbia' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(1147.50 AS money) AS YTDPurchase, CAST('2015-6-15' AS date) AS LastPurchase  
    UNION SELECT  'Bridget' AS FirstName, 'She' AS LastName, 'Hamburg' AS StateProvince, 4 AS CountryRegionID, 'Female' AS Gender, CAST(7497.30 AS money) AS YTDPurchase, CAST('2015-5-10' AS date) AS LastPurchase  
    UNION SELECT 'Alexander' AS FirstName, 'Martin' AS LastName, 'Saxony' AS StateProvince, 4 AS CountryRegionID, 'Male' AS Gender, CAST(2997.60 AS money) AS YTDPurchase, CAST('2015-11-19' AS date) AS LastPurchase  
    UNION SELECT 'Yolanda' AS FirstName, 'Sharma' AS LastName ,'Micronesia' AS StateProvince, 5 AS CountryRegionID, 'Female' AS Gender, CAST(3247.95 AS money) AS YTDPurchase, CAST('2015-8-23' AS date) AS LastPurchase  
    UNION SELECT 'Marc' AS FirstName, 'Zimmerman' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1200.00 AS money) AS YTDPurchase, CAST('2015-11-16' AS date) AS LastPurchase  
    UNION SELECT 'Katherine' AS FirstName, 'Abel' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Female' AS Gender, CAST(2025.00 AS money) AS YTDPurchase, CAST('2015-12-1' AS date) AS LastPurchase  
    UNION SELECT 'Nicolas' as FirstName, 'Anand' AS LastName, 'Seine (Paris)' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1425.00 AS money) AS YTDPurchase, CAST('2015-12-11' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Peters' AS LastName, 'England' AS StateProvince, 12 AS CountryRegionID, 'Male' AS Gender, CAST(887.50 AS money) AS YTDPurchase, CAST('2015-8-15' AS date) AS LastPurchase  
    UNION SELECT 'Alison' AS FirstName, 'Nath' AS LastName, 'Alaska' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(607.50 AS money) AS YTDPurchase, CAST('2015-10-13' AS date) AS LastPurchase  
    UNION SELECT 'Grace' AS FirstName, 'Patterson' AS LastName, 'Kansas' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(1215.00 AS money) AS YTDPurchase, CAST('2015-10-18' AS date) AS LastPurchase  
    UNION SELECT 'Bobby' AS FirstName, 'Sanchez' AS LastName, 'North Dakota' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(6191.00 AS money) AS YTDPurchase, CAST('2015-9-17' AS date) AS LastPurchase  
    UNION SELECT 'Charles' AS FirstName, 'Reed' AS LastName, 'Nebraska' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8772.00 AS money) AS YTDPurchase, CAST('2015-8-27' AS date) AS LastPurchase  
    UNION SELECT 'Orlando' AS FirstName, 'Romeo' AS LastName, 'Texas' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8578.00 AS money) AS YTDPurchase, CAST('2015-7-29' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    ```  

  
10. Nella barra degli strumenti Progettazione query fare clic su **Esegui** ( **!** ). Nel set di risultati vengono visualizzate 23 righe di dati con le colonne seguenti: FirstName, LastName, StateProvince, CountryRegionID, Gender, YTDPurchase e LastPurchase.  

    ![Screenshot del passaggio Progetta query della Creazione guidata tabella o matrice.](../reporting-services/media/report-builder-expression-tutorial-query-as-text.png)
  
11. Fare clic su **Avanti**.  
  
12. Nella pagina **Disponi campi** trascinare i campi seguenti nell'ordine specificato, dall'elenco **Campi disponibili** all'elenco **Valori** .  
  
    -   StateProvince   
    -   CountryRegionID  
    -   LastPurchase  
    -   YTDPurchase  
  
    Poiché in ID Paese/Area geografica e Acquisti da inizio anno sono inclusi dati numerici, per impostazione predefinita viene applicata l'aggregazione SUM a tali campi, anche se non si vuole sommarli.  
   
13. Nell'elenco **Valori** fare clic con il pulsante destro del mouse su **IDPaeseRegione** e disattivare la casella di controllo **Somma** .  
  
    Al campo CountryRegionID non viene più applicata la somma.  
  
14. Nell'elenco **Valori** fare clic con il pulsante destro del mouse su **YTDPurchase** e sull'opzione **Somma** .  
  
    Al campo YTDPurchase non viene più applicata la somma.  
    
    ![Screenshot dell'elenco di valori che mostra l'opzione Somma pronta per essere cancellata.](../reporting-services/media/report-builder-expression-not-sum.png)
  
15. Fare clic su **Avanti**.  
  
16. Nella pagina **Scegliere il layout** , mantenere tutte le impostazioni predefinite e fare clic su **Avanti**.  

    ![Screenshot del passaggio Scegliere il layout della Creazione guidata tabella o matrice.](../reporting-services/media/report-builder-expression-tutorial-choose-layout.png)
  
17. Fare clic su **Fine**.  
  
## <a name="2-update-default-names-of-the-data-source-and-dataset"></a><a name="UpdateNames"></a>2. Aggiornare i nomi predefiniti dell'origine dati e del set di dati  
  
### <a name="to-update-the-default-name-of-the-data-source"></a>Per aggiornare il nome predefinito dell'origine dati  
  
1.  Nel riquadro Dati report espandere la cartella **Origini dati** .  
  
2.  Fare clic con il pulsante destro del mouse su **DataSource1** e scegliere **Proprietà origine dati.**  
  
3.  Nella casella **Nome** digitare **ExpressionsDataSource**  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-update-the-default-name-of-the-dataset"></a>Per aggiornare il nome predefinito del set di dati  
  
1.  Nel riquadro Dati report espandere la cartella **Set di dati** .  
  
2.  Fare clic con il pulsante destro del mouse su **DataSet1** e scegliere **Proprietà DataSet**.  

    ![Screenshot che mostra come accedere alle proprietà del set di dati in Generatore report.](../reporting-services/media/report-builder-expression-tutorial-rename-dataset.png)
  
3.  Nella casella **Nome** digitare **Expressions**  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="3-display-first-initial-and-last-name"></a><a name="Concatenate"></a>3. Visualizzare iniziale nome e cognome  
In questa sezione, viene usata la funzione **Left** e l'operatore di **concatenazione** (**&**) in un'espressione tramite cui vengono restituiti un'iniziale del nome e un cognome. È possibile compilare l'espressione passaggio dopo passaggio oppure andare avanti nella procedura e copiare e incollare l'espressione dall'esercitazione nella finestra di dialogo **Espressione** .   
  
1.  Fare clic con il pulsante destro del mouse sulla colonna **StateProvince** , scegliere **Inserisci colonna** e fare clic su **A sinistra**.  
  
    Una nuova colonna verrà aggiunta a sinistra della colonna **StateProvince** . 
    
    ![Screenshot che mostra come inserire una colonna a sinistra in un report.](../reporting-services/media/report-builder-expression-tutorial-insert-column.png) 
  
2.  Fare clic sull'intestazione della nuova colonna e digitare **Nome**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cella di dati per la colonna **Name** e scegliere **Espressione**.  

    ![Screenshot che mostra come inserire un'espressione in un report.](../reporting-services/media/report-builder-expression-tutorial-insert-expression.png)
  
4.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Text**.  
  
5.  Nell'elenco **Elemento** fare doppio clic su **Left**.  
  
    La funzione **Left** viene aggiunta all'espressione.  
    
    ![Screenshot che mostra come aggiungere una funzione Left a un'espressione.](../reporting-services/media/report-builder-expression-tutorial-left-function.png)
  
6.  Nell'elenco **Categoria** fare clic su **Fields (Expressions)** .  
  
7.  Nell'elenco **Valori** fare doppio clic su **FirstName**.  
  
8.  Digitare **, 1)**  
  
    Questa espressione consente di estrarre un carattere dal valore **FirstName** , a partire da sinistra.  
  
9. Digitare **&". "&**  

    Viene aggiunto un punto e uno spazio dopo l'espressione.
  
10. Nell'elenco **Valori** fare doppio clic su **LastName**.  
  
    L'espressione completa è: `=Left(Fields!FirstName.Value, 1) &". "& Fields!LastName.Value`  
    
    ![Screenshot che mostra come aggiungere un valore LastName a un'espressione.](../reporting-services/media/report-builder-expression-tutorial-complete-name-expression.png)
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. Fare clic su **Esegui** per visualizzare l'anteprima del report.  

## <a name="optional-format-the-date-and-currency-columns-and-header-row"></a><a name="DateFormat"></a>(facoltativo) Formattare le colonne Data e Valuta e la riga di intestazione  
In questa sezione viene formattata la colonna **Ultimo acquisto** che contiene date, e la colonna Acquisti da inizio anno, che contiene la valuta. Viene anche formattata la riga di intestazione.  
  
### <a name="to-format-the-date-column"></a>Per formattare la colonna della data  
  
1.  Fare clic su **Progettazione** per tornare alla visualizzazione Struttura.  
  
2.  Selezionare la cella di dati nella colonna **Ultimo acquisto** e selezionare **Data** nella scheda **Home** > **Numero**.  

    ![Screenshot che mostra come impostare la colonna Ultimo acquisto su Data.](../reporting-services/media/report-builder-expression-tutorial-date-format.png)
  
3.  Anche nella sezione **Numero** , fare clic sulla freccia accanto a **Stili segnaposto** e selezionare **Valori di esempio**. 

    ![Screenshot che mostra l'opzione Valori di esempio in Generatore report.](../reporting-services/media/report-builder-expression-tutorial-sample-values.png)

    È ora possibile vedere un esempio di formattazione selezionata. 
  
### <a name="to-format-the-currency-column"></a>Per formattare la colonna della valuta

- Selezionare la cella di dati nella colonna **Acquisti da inizio anno** e selezionare **Simbolo valuta** nella sezione **Numero**.
 
### <a name="to-format-the-column-headers"></a>Per formattare le intestazioni di colonna

1. Selezionare la riga delle intestazioni di colonna.

2. Selezionare **A sinistra** nella sezione **Paragrafo** della scheda **Home**. 

    ![Screenshot che mostra come formattare le intestazioni in Generatore report.](../reporting-services/media/report-builder-expression-tutorial-format-headings.png)

3. Fare clic su **Esegui** per visualizzare l'anteprima del report. 

Questo è il report con date formattate, valuta e intestazioni di colonna.

![Screenshot che mostra l'anteprima del report formattato.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted.png)

  
## <a name="4-use-color-to-display-gender"></a><a name="Gender"></a>4. Usare colori per visualizzare il sesso  
In questa sezione vengono aggiunti colori per visualizzare il sesso di una persona. Viene aggiunta una nuova colonna per visualizzare il colore e si specifica il colore che verrà visualizzato nella colonna in base al valore del campo Sesso.  
  
Per conservare il colore che è stato applicato alla cella della tabella durante la creazione di un report a righe alternate evidenziate, aggiungere un rettangolo e applicare un colore di sfondo al rettangolo.  
    
 
### <a name="to-add-an-mf-column"></a>Per aggiungere una colonna di M/F  
  
1.  Fare clic con il pulsante destro del mouse sulla colonna **Nome** , scegliere **Inserisci colonna** e fare clic su **A sinistra**.  
  
    Verrà aggiunta una nuova colonna a sinistra della colonna **Nome** .  
  
2.  Fare clic sull'intestazione della nuova colonna e digitare **M/F**.  
  
### <a name="to-add-a-rectangle"></a>Per aggiungere un rettangolo  
  
1.   Nella scheda **Inserisci** fare clic su **Rettangolo** e fare clic nella cella di dati della colonna **M/F** .  
  
     Verrà aggiunto un rettangolo alla cella.  
     
     ![Screenshot che mostra come inserire un rettangolo.](../reporting-services/media/report-builder-expression-tutorial-insert-rectangle.png)
  
2. Trascinare il divisore di colonna tra **M/F** e **Nome** per rendere la colonna **M/F** più stretta.

    ![Screenshot che mostra come rendere una colonna più stretta.](../reporting-services/media/report-builder-expression-tutorial-narrow-column.png)
  
### <a name="to-use-color-to-show-gender"></a>Per indicare il sesso con il colore  
  
1.  Fare clic con il pulsante destro del mouse sul rettangolo nella cella di dati nella colonna **M/F** e scegliere **Proprietà rettangolo**.  
  
2.  Nella scheda **Riempi** della finestra di dialogo **Proprietà rettangolo** fare clic sul pulsante dell'espressione **fx** accanto a **Colore riempimento**.  
  
3.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Program Flow**.  
  
4.  Nell'elenco **Elemento** fare doppio clic su **Switch**.  
  
5.  Nell'elenco **Categoria** fare clic su **Fields (Expressions)** .  
  
6.  Nell'elenco **Valori** fare doppio clic su **Gender**.  
  
7.  Digitare **="Maschio",** (inclusa la virgola).

8. Nell'elenco **Categoria** fare clic su **Costanti** e nella casella **Valori** fare clic su **Blu fiordaliso**.

    ![Screenshot che mostra come usare un colore per mostrare un genere.](../reporting-services/media/report-builder-expression-tutorial-color-expression-cornflower-blue.png)

9. Aggiungere una virgola. 
  
5.  Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **Sesso** di nuovo.  
  
7.  Digitare **="Femmina",** (inclusa la virgola). 

8. Nell'elenco **Categoria** fare clic su **Costanti** e nella casella **Valori** fare clic su **Cremisi**.

13. Aggiungere una parentesi di chiusura **)** dopo di esso. 
  
    L'espressione completa è: `=Switch(Fields!Gender.Value ="Male", "CornflowerBlue",Fields!Gender.Value ="Female","Tomato")`  
    
    ![Screenshot che mostra l'espressione completa nella finestra di dialogo Espressione.](../reporting-services/media/report-builder-expression-tutorial-color-expression-complete.png)
  
12. Fare clic su **OK** e di nuovo su **OK** per chiudere la finestra di dialogo **Proprietà rettangolo** .  
  
14. Fare clic su **Esegui** per visualizzare l'anteprima del report.  

    ![Screenshot che mostra l'anteprima con la colonna M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-m-f-column.png)

### <a name="to-format-the-color-rectangles"></a>Per formattare i bordi dei rettangoli

1. Fare clic su **Progettazione** per tornare alla visualizzazione Struttura.  

16. Selezionare il rettangolo nella colonna **M/F** . Nella sezione Bordo del riquadro Proprietà impostare queste proprietà:

    - BorderColor = White
    - BorderStyle = Solid
    - BorderWidth = 5pt
    
    ![Screenshot che mostra come formattare i rettangoli a colori nella colonna M/F.](../reporting-services/media/report-builder-expression-tutorial-format-m-f-column.png)

18. Fare clic su **Esegui** per visualizzare di nuovo l'anteprima del report. Questa volta i blocchi di colore hanno uno spazio bianco intorno.

    ![Screenshot che mostra l'anteprima con i rettangoli formattati nella colonna M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted-m-f-column.png)  
  
## <a name="5-look-up-the-countryregion-name"></a><a name="Lookup"></a>5. Cercare il nome CountryRegion  
In questa sezione viene creato un set di dati PaeseRegione e viene usata la funzione **Lookup** per visualizzare il nome di un paese o di una regione anziché il relativo identificatore.  
  
### <a name="to-create-the-countryregion-dataset"></a>Per creare un set di dati CountryRegion  
  
1.  Fare clic su **Progettazione** per tornare alla visualizzazione Struttura.  
  
2.  Nel riquadro Dati report fare clic su **Nuovo** e selezionare **Set di dati**.  
  
3.  In **Proprietà set di dati** fare clic su **Usa un set di dati incorporato nel report**.  
  
4.  Selezionare ExpressionsDataSource nell'elenco **Origine dati** .  
  
5.  Nella casella **Nome** digitare **CountryRegion**  
  
6.  Verificare che sia selezionato il tipo di query **Testo** e fare clic su **Progettazione query**.  
  
7.  Fare clic su **Modifica come testo**.  
  
8.  Copiare e incollare la query seguente nel relativo riquadro:  
  
    ```  
    SELECT 1 AS ID, 'American Samoa' AS CountryRegion  
    UNION SELECT 2 AS CountryRegionID, 'Australia' AS CountryRegion  
    UNION SELECT 3 AS ID, 'Canada' AS CountryRegion  
    UNION SELECT 4 AS ID, 'Germany' AS CountryRegion  
    UNION SELECT 5 AS ID, 'Micronesia' AS CountryRegion  
    UNION SELECT 6 AS ID, 'France' AS CountryRegion  
    UNION SELECT 7 AS ID, 'United States' AS CountryRegion  
    UNION SELECT 8 AS ID, 'Brazil' AS CountryRegion  
    UNION SELECT 9 AS ID, 'Mexico' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Japan' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Australia' AS CountryRegion  
    UNION SELECT 12 AS ID, 'United Kingdom' AS CountryRegion  
    ```  
  
9. Scegliere **Esegui** ( **!** ) per eseguire la query.  
  
    I risultati query sono i nomi e gli identificatori di paesi.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Scegliere di nuovo **OK** per chiudere la finestra di dialogo **Proprietà set di dati** .  

     È ora disponibile un secondo set di dati nella colonna **Dati report** .
  
### <a name="to-look-up-values-in-the-countryregion-dataset"></a>Per cercare valori nel set di dati CountryRegion  
  
1.  Fare clic sul titolo di colonna **Country Region ID** ed eliminare il testo: **ID**, in modo che si legga **Country Region**.  
  
2.  Fare clic con il pulsante destro del mouse sulla cella di dati per la colonna **Country Region** e scegliere **Espressione**.  
  
3.  Eliminare l'espressione eccetto il segno iniziale di uguale (=).  
  
    L'espressione rimanente è: `=`  
  
4.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Vari** e nell'elenco **Elemento** fare doppio clic su **Ricerca**.  
  
6.  Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **IDPaeseRegione**.  
  
8.  Collocare il cursore immediatamente dopo `CountryRegionID.Value`e digitare **,Fields!ID.value, Fields!PaeseRegione.value, "PaeseRegione")**  
  
    L'espressione completa è: `=Lookup(Fields!CountryRegionID.Value,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")`  
  
    La sintassi della funzione **Lookup** consente di specificare una ricerca tra IDPaeseRegione nel set di dati Expressions e ID nel set di dati PaeseRegione tramite la quale viene restituito il valore PaeseRegione dal set di dati PaeseRegione.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Fare clic su **Esegui** per visualizzare l'anteprima del report.  
  
## <a name="6-count-days-since-last-purchase"></a><a name="Count"></a>6. Contare i giorni dall'ultimo acquisto  
In questa sezione viene aggiunta una colonna e viene usata la funzione **Now** o la variabile globale incorporata `ExecutionTime` per calcolare il numero di giorni dagli ultimi acquisti di un cliente.  
  
### <a name="to-add-the-days-ago-column"></a>Per aggiungere la colonna Days Ago  
  
1.  Fare clic su **Progettazione** per tornare alla visualizzazione Struttura.  
  
2.  Fare clic con il pulsante destro del mouse sulla colonna **Last Purchase** , scegliere **Inserisci colonna** e fare clic su **A destra**.  
  
    Una nuova colonna verrà aggiunta a destra della colonna **Last Purchase** .  
  
3.  Nell'intestazione di colonna digitare **Days Ago**  
  
4.  Fare clic con il pulsante destro del mouse sulla cella di dati per la colonna **Days Ago** e scegliere **Espressione**.  
  
5.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Date & Time**.  
  
6.  Nell'elenco **Elemento** fare doppio clic su **DateDiff**.  
  
7.  Immediatamente dopo `DateDiff(`, digitare **"d",** (incluso virgolette "" e virgola). 
  
9. Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **UltimoAcquisto**.  
  
11. Immediatamente dopo `Fields!LastPurchase.Value`, digitare **,** (una virgola). 
  
13. Nell'elenco **Categoria** fare clic di nuovo su **Data/Ora** e nell'elenco **Elemento** fare doppio clic su **Now**.  
  
    > [!WARNING]  
    > Nei report di produzione non è consigliabile usare la funzione **Now** in espressioni valutate più volte quando si esegue il rendering del report, ad esempio nelle righe di dettaglio di un report. Il valore di **Now** cambia da riga a riga e i valori diversi influiscono sui risultati della valutazione delle espressioni, generando risultati leggermente incoerenti. Usare invece la variabile globale `ExecutionTime` di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
15. Eliminare la parentesi aperta dopo `Now(`e digitare una parentesi chiusa **)**  
  
    L'espressione completa è: `=DateDiff("d", Fields!LastPurchase.Value, Now)`  
    
    ![Screenshot che mostra l'espressione completa per la data dall'ultimo acquisto.](../reporting-services/media/report-builder-expression-tutorial-date-since-last-purchase.png)
  
17. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

11. Fare clic su **Esegui** per visualizzare l'anteprima del report.  
  
## <a name="7-use-an-indicator-to-show-sales-comparison"></a><a name="Indicator"></a>7. Utilizzare un indicatore per mostrare il confronto vendite  
In questa sezione viene aggiunta una nuova colonna e viene usato un indicatore per specificare se gli acquisti effettuati da una persona a partire da inizio anno sono superiori o inferiori alla relativa media. La funzione **Round** consente di rimuovere i decimali dai valori.  
  
La configurazione dell'indicatore e dei relativi stati richiede numerosi passaggi. Se si vuole, è possibile passare direttamente alla procedura "Per configurare l'indicatore" e copiare e incollare le espressioni complete da questa esercitazione nella finestra di dialogo **Espressione**.  
  
### <a name="to-add-the--or---avg-sales-column"></a>Per aggiungere la colonna + or - AVG Sales  
  
1.  Fare clic con il pulsante destro del mouse sulla colonna **YTD Purchase** , scegliere **Inserisci colonna** e fare clic su **A destra**.  
  
    Una nuova colonna verrà aggiunta a destra della colonna **YTD Purchase** .  
  
2.  Fare clic sull'intestazione della nuova colonna e digitare **Media vendite + o -**  
  
### <a name="to-add-an-indicator"></a>Per aggiungere un indicatore  
  
1.  Nella scheda **Inserisci** selezionare **Indicatore** e fare clic nella cella di dati della colonna **Media vendite + o -** .  
  
    Verrà visualizzata la finestra di dialogo **Seleziona tipo indicatore** .  
  
2.  Nel gruppo **Direzionale** di set di icone selezionare il set di tre frecce grigie.  

    ![Screenshot che mostra come aggiungere un indicatore.](../reporting-services/media/report-builder-expression-tutorial-select-indicator.png)
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-configure-the-indicator"></a>Per configurare l'indicatore  
  
1.  Fare clic con il pulsante destro del mouse sull'indicatore, selezionare **Proprietà indicatore** e scegliere **Valore e stati**.  
  
2.  Fare clic sul pulsante dell'espressione **fx** accanto alla casella di testo **Valore** .  
  
3.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Math**.  
  
4.  Nell'elenco **Elemento** fare doppio clic su **Round**.  
  
5.  Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **AcquistiDaInizioAnno**.  
  
7.  Immediatamente dopo `Fields!YTDPurchase.Value`, digitare  **-** (segno di sottrazione). 
  
9. Espandere di nuovo **Funzioni comuni** , fare clic su **Aggregazione** e nell'elenco **Elemento** fare doppio clic su **AVG**.  
  
11. Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **AcquistiDaInizioAnno**.  
  
13. Immediatamente dopo `Fields!YTDPurchase.Value`, digitare **, "Espressioni"))**  
  
    L'espressione completa è: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions"))`  
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
16. Nella casella **Unità di misura stati** selezionare **Numerico**.  
  
17. Nella riga con la freccia rivolta verso il basso fare clic sul pulsante **fx** a destra della casella di testo per il valore **Iniziale** .  

    ![Screenshot che mostra come selezionare il pulsante fx accanto alla casella di testo Iniziale.](../reporting-services/media/report-builder-expression-tutorial-indicator-start.png)
  
18. Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Math**.  
  
19. Nell'elenco **Elemento** fare doppio clic su **Round**.  
  
20. Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **AcquistiDaInizioAnno**.  
  
22. Immediatamente dopo `Fields!YTDPurchase.Value`, digitare  **-** (segno di sottrazione). 
  
24. Espandere di nuovo **Funzioni comuni** , fare clic su **Aggregazione** e nell'elenco **Elemento** fare doppio clic su **Avg**.  
  
26. Nell'elenco **Categoria** fare clic su **Campi (Espressioni)** e nell'elenco **Valori** fare doppio clic su **AcquistiDaInizioAnno**.  
  
28. Immediatamente dopo `Fields!YTDPurchase.Value`, digitare **, "Espressioni")) < 0**  
  
    L'espressione completa è: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) < 0`  
  
30. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
31. Nella casella di testo per il valore **Finale** digitare **0**  
  
32. Fare clic sulla riga con la freccia orizzontale e scegliere **Elimina**.  

    ![Screenshot che mostra come eliminare un indicatore.](../reporting-services/media/report-builder-expression-tutorial-delete-indicator-state.png)
    
    A questo punto ci sono solo due frecce, una verso l'alto o una verso il basso.
  
33. Nella riga con la freccia rivolta verso l'alto, nella casella **Inizio** digitare **0**  
  
34. Fare clic sul pulsante **fx** a destra della casella di testo per il valore **Finale** .  
  
35. Nella finestra di dialogo **Espressione** eliminare **100** e creare l'espressione: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) >0`  
  
36. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
37. Scegliere di nuovo **OK** per chiudere la finestra di dialogo **Proprietà indicatore** .  
  
38. Fare clic su **Esegui** per visualizzare l'anteprima del report.  

    ![Screenshot che mostra l'anteprima con la colonna + or-AVG Sales, inclusi tutti i nuovi indicatori.](../reporting-services/media/report-builder-expression-tutorial-preview-indicator.png)
  
## <a name="8-make-a-banded-report"></a><a name="GreenBar"></a>8. Creare un report a righe alternate evidenziate  
Creare un parametro in modo che gli utenti del report possano specificare il colore da applicare alle righe alternate.  
  
### <a name="to-add-a-parameter"></a>Per aggiungere un parametro  
  
1.  Fare clic su **Progettazione** per tornare alla visualizzazione Struttura.  
  
2.  Nel riquadro **Dati report** fare clic con il pulsante destro del mouse su **Parametri** e scegliere **Aggiungi parametro**.  

    ![Screenshot che mostra come aggiungere un parametro.](../reporting-services/media/report-builder-expression-tutorial-add-parameter.png)
  
    Verrà visualizzata la finestra di dialogo **Proprietà parametri report** .  
  
3.  In **Messaggio di richiesta** digitare **Choose color**  
  
4.  Nella casella **Nome** digitare **RowColor**  
  
5.  Nella scheda **Valori disponibili** fare clic su **Imposta valori**.  
  
7.  Scegliere **Aggiungi**.  
  
8.  Nella casella **Etichetta** digitare: **Giallo**  
  
9. Nella casella **Valore** digitare **Yellow**  
  
10. Scegliere **Aggiungi**.  
  
11. Nella casella **Etichetta** digitare **Green**  
  
12. Nella casella **Valore** digitare **PaleGreen**  
  
13. Scegliere **Aggiungi**.  
  
14. Nella casella **Etichetta** digitare **Blue**  
  
15. Nella casella **Valore** digitare **LightBlue**  
  
16. Scegliere **Aggiungi**.  
  
17. Nella casella **Etichetta** digitare **Pink**  
  
18. Nella casella **Valore** digitare **Pink**  

    ![Screenshot della finestra di dialogo Proprietà parametri report che mostra il passaggio Selezionare i valori disponibili per il parametro.](../reporting-services/media/report-builder-expression-tutorial-parameter-available.png)
  
19. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-apply-alternating-colors-to-detail-rows"></a>Per applicare colori alternati alle righe di dettaglio  
  
1.   Selezionare tutte le celle nella riga di dati tranne la cella della colonna **M/F** , che ha già un colore di sfondo.  

     ![Screenshot che mostra le celle selezionate in una riga di dati.](../reporting-services/media/report-builder-expression-tutorial-select-banded.png)
  
4.  Nel riquadro Proprietà fare clic su **BackgroundColor**. 

     Se il riquadro Proprietà non è visualizzato, selezionare la casella di controllo **Proprietà** nella scheda **Vista** .  
  
    Se le proprietà sono elencate per categoria nel riquadro Proprietà, **BackgroundColor** si trova nella categoria **Altre** .  
  
5.  Fare clic sulla freccia a discesa e scegliere **Espressione**.  

    ![Screenshot della casella Proprietà che mostra come associare un'espressione a un BackgroundColor.](../reporting-services/media/report-builder-expression-tutorial-banded-color-property.png)
  
6.  Nella finestra di dialogo **Espressione** espandere **Funzioni comuni** e fare clic su **Program Flow**.  
  
7.  Nell'elenco **Elemento** fare doppio clic su **IIf**.  
  
8.  In **Funzioni comuni** fare clic su **Altre** e nell'elenco **Elemento** fare doppio clic su **RowNumber**.  

9. Immediatamente dopo **RowNumber (** digitare **Nothing) MOD 2,**
  
8. Fare clic su **Parametri** e nell'elenco **Valori** fare doppio clic su **RowColor**.  
  
22. Immediatamente dopo `Parameters!RowColor.Value`, digitare **, "White")**  
  
    L'espressione completa è: `=IIF(RowNumber(Nothing) MOD 2, Parameters!RowColor.Value, "White")`  
    
    ![Screenshot che mostra l'espressione completa per le righe colorate.](../reporting-services/media/report-builder-expression-tutorial-banded-color-expressn.png)
  
24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="run-the-report"></a>Eseguire il report  
  
1.  Nella scheda **Home** fare clic su **Esegui**.  

    Dopo aver eseguito il report, non sarà possibile vedere i dati se non si sceglie un colore per le righe non bianche.
  
3.  Nell'elenco **Scegliere un colore** selezionare un colore per le righe non bianche del report.  
    
    ![Screenshot che mostra come scegliere un colore per le righe non bianche.](../reporting-services/media/report-builder-expression-tutorial-select-color.png)
  
4.  Fare clic su **Visualizza report**.  
  
    Viene eseguito il rendering del report e nelle righe alternate verrà visualizzato lo sfondo scelto. 
    
    ![Screenshot che mostra l'anteprima con le righe con colori alternati.](../reporting-services/media/report-builder-expression-tutorial-preview-banded.png) 
  
## <a name="optional-add-a-report-title"></a><a name="Title"></a>(facoltativo) Aggiungere un titolo al report  
Aggiungere un titolo al report.  
  
### <a name="to-add-a-report-title"></a>Per aggiungere il titolo di un report  
  
1.  Nell'area di progettazione fare clic su **Fare clic per aggiungere il titolo**.  
  
2.  Digitare **Riepilogo vendite comparativo** e selezionare il testo.  
  
3.  Nella casella **Carattere** della scheda **Home** impostare:

    -  Dimensione = 18
    -  Colore = Grigio
    -  Grassetto
  
4.  Nella scheda **Home** fare clic su **Esegui**.  
  
3.  Selezionare un colore per le righe non bianche di un report e fare clic su **Visualizza rapporto**.  
  
## <a name="optional-save-the-report"></a><a name="Save"></a>(facoltativo) Salvare il report  
È possibile salvare i report in un server di report, in una raccolta di SharePoint o nel computer locale. Per altre informazioni, vedere [Salvataggio di report &#40;Generatore report&#41;](../reporting-services/report-builder/saving-reports-report-builder.md).  
  
In questa esercitazione il report verrà salvato in un server di report. Se non si dispone dell'accesso a un server di report, sarà possibile salvare il report nel computer locale.  
  
### <a name="to-save-the-report-to-a-report-server"></a>Per salvare il report in un server di report  
  
1.  Scegliere **Salva con nome** dal menu **File**.  
  
2.  Fare clic su **Siti e server recenti**.  
  
3.  Selezionare o digitare il nome del server di report per il quale si dispone delle autorizzazioni di salvataggio dei report.  
  
    Verrà visualizzato il messaggio "Connessione al server di report". Al termine della connessione, verrà visualizzato il contenuto della cartella di report specificata dall'amministratore del server di report come posizione predefinita per i report.  
  
4.  Assegnare un nome al report e fare clic su **Salva**.  
  
Il report verrà salvato sul server di report. Il nome del server di report al quale si è connessi verrà visualizzato sulla barra di stato nella parte inferiore della finestra.

A questo punto i lettori del report possono visualizzare il report nel [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] portale Web.

![Screenshot del nuovo report completo con ogni espressione visibile.](../reporting-services/media/report-builder-expression-tutorial-final-in-browser.png)

   
## <a name="see-also"></a>Vedere anche  
[Espressioni &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
[Esempi di espressioni &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
[Indicatori &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
[Immagini, caselle di testo, rettangoli e linee &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/images-text-boxes-rectangles-and-lines-report-builder-and-ssrs.md)  
[Tabelle &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/tables-report-builder-and-ssrs.md)  
[Set di dati del report &#40;SSRS&#41;](../reporting-services/report-data/report-datasets-ssrs.md)  
  
  
  

