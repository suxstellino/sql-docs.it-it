---
title: 'Report per dispositivi mobili di SQL Server: procedura dettagliata completa'
description: Informazioni su come creare report per dispositivi mobili in SQL Server Mobile Report Publisher, salvarli nel portale Web di Reporting Services e visualizzarli nelle app per dispositivi mobili di Power BI.
ms.date: 12/07/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: e198575e-b154-4342-b944-2bf19ec49bfd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df09e4168790a4607de19348f50e76a4fbd0f383
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596545"
---
# <a name="sql-server-mobile-reports-end-to-end-walk-through"></a>Report per dispositivi mobili di SQL Server: procedura dettagliata completa
Procedura dettagliata per creare report per dispositivi mobili per schermi di qualsiasi dimensione con [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] nel portale Web di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] e visualizzarli nell'app Power BI per dispositivi mobili.

Consente di creare report per dispositivi mobili in un'area di progettazione con righe e colonne della griglia regolabili ed elementi flessibili del report per dispositivi mobili. È possibile connettersi a un'ampia gamma di origini dati locali, caricare cartelle di lavoro di Excel oppure creare report per dispositivi mobili, quindi salvarli in un portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] e visualizzarli in un browser o nelle app Power BI per dispositivi mobili.  
  
In questo articolo viene descritto:   
  
- La creazione di un'origine dati condivisa e di un set di dati nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , usando il database AdventureWorks come origine dati di esempio.  
- La creazione di un report per dispositivi mobili di Reporting Services in [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]  
- La pubblicazione di un report per dispositivi mobili nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
- La visualizzazione di report per dispositivi mobili nell'app Power BI per dispositivi mobili.  
  
## <a name="before-we-start"></a>Prima di iniziare  
Per proseguire, sono necessari questi prodotti:  
  
* Per creare origini dati e indicatori KPI e per pubblicare set di dati e report per dispositivi mobili, è necessario accedere a un [server di report di Reporting Services in modalità nativa](../install-windows/install-reporting-services-native-mode-report-server.md).  
* Per creare set di dati condivisi, [installare Generatore report](../install-windows/install-report-builder.md).  
* Per creare report per dispositivi mobili, [installare SQL Server Mobile Report Publisher](../reporting-services-features-and-tasks-ssrs.md).  
* [Database di esempio AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
*  OPPURE: database di esempio World Wide Importers, disponibile nella pagina [Esempi di Microsoft SQL Server](../../samples/sql-samples-where-are.md).
* Per visualizzare il risultato: 
  *   [Iscriversi al servizio Power BI](https://go.microsoft.com/fwlink/?LinkID=513879) e
  *  [Scaricare l'app Power BI per dispositivi mobili](/power-bi/consumer/mobile/mobile-apps-for-mobile-devices) per il proprio dispositivo mobile: telefono iOS o Android o dispositivo Windows 10.  

  
## <a name="create-a-shared-data-source"></a>Creazione di un'origine dati condivisa  
  
È possibile creare un'origine dati condivisa per i report per dispositivi mobili da una delle origini dati supportate da Reporting Services. Vedere l'[elenco delle origini dati supportate](../report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
1. Nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] fare clic su **Nuovo** > **Origine dati**.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
3. Immettere le informazioni sull'origine dati > **OK**.  
  
    Per impostazione predefinita, le origini dati non vengono visualizzate nel portale.    
   
5. Per visualizzare le origini dati, fare clic su **Visualizza** > **Origine dati**.  
  
   ![PBI_SSMRP_DisplayDataSources](../../reporting-services/mobile-reports/media/pbi-ssmrp-displaydatasources.png)  
   
6. L'origine dati verrà ora visualizzata nel portale di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
  
   ![PBI_SSMRP_PortlDataSource](../../reporting-services/mobile-reports/media/pbi-ssmrp-portldatasource.png)  
  
Altre informazioni sulle [origini dati condivise in Reporting Services](../report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
   
## <a name=""></a><a name="shared-dataset">Creare un set di dati condiviso</a>  
  
Per creare il set di dati condiviso, usare uno strumento client di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] esistente, ad esempio Progettazione Report in [!INCLUDE[ssBIDevStudioFull_md](../../includes/ssbidevstudiofull-md.md)].  Questa procedura dettagliata usa [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]. [Installare Generatore report](../install-windows/install-report-builder.md) oppure avviarlo dal portale Web. Verranno creati tre set di dati, uno per il valore dell'indicatore KPI, uno per la tendenza KPI e uno con più campi per il report per dispositivi mobili di Reporting Services.     
  
1. Nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] fare clic su **Nuovo** > **Report impaginato** per avviare [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)   
2. Fare clic su **Nuovo set di dati**.  
  
   ![PBI_SSMRP_RBNewDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-rbnewdataset.png)  
   
3. Fare clic su **Esplora altre origini dati**.  
   
4. Nel campo Nome digitare il nome del server in cui è stata salvata l'origine dati, in questo formato:   
   
   Nome: https://*localhost*/ReportServer  
   Elementi di tipo: origini dati (file con estensione rsds)  
   
5. Fare clic su **Apri** e passare all'origine dati creata sul server.  
   
6. Selezionare l'origine dati e fare clic nuovamente su **Apri** .    
  
7. Progettare il set di dati in [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_RB_QueryDesignr600](../../reporting-services/mobile-reports/media/pbi-ssmrp-rb-querydesignr600.png)  
   
8. Al termine, salvare il set di dati nel server di report di [!INCLUDE[PRODUCT_NAME](../../includes/ssrs.md)] .    
   
A questo punto è possibile usare il set di dati come base per i report per dispositivi mobili e per gli indicatori KPI.  È possibile creare più set di dati nella stessa origine dati. È anche possibile creare più indicatori KPI e report per dispositivi mobili in questi set di dati condivisi.   
  
## <a name=""></a><a name="create-KPI">Creare un indicatore KPI</a>  
Creare gli indicatori KPI direttamente nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .    
  
1. Nell'angolo superiore destro del portale Web fare clic su **Nuovo** > **Nuovo indicatore KPI**.   
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
      
   Nella schermata di creazione degli indicatori KPI è possibile immettere manualmente i valori oppure usare un set di dati condiviso.    
2. Modificare **Valore** da **Imposta manualmente** a **Campo del set di dati**.  
   
   ![PBI_SSMRP_KPI_DatasetField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpi-datasetfield.png)  
   
3. Fare clic sui puntini di sospensione ( **...** ) nella casella **Scegliere un campo del set di dati** e selezionare un set di dati dal passaggio precedente.  
   
   ![PBI_SSMRP_KPIPickDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickdataset.png)  
   
4. Scegliere il campo nel set di dati.    
   
   ![PBI_SSMRP_KPIPickField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickfield.png)  
     
5. Scegliere l'aggregazione desiderata. Negli indicatori KPI può essere visualizzato solo un numero, pertanto il campo verrà aggregato per visualizzare tale numero.

   ![Screenshot della sezione per la scelta di un campo da AWSalesYTD che mostra la sezione relativa all'aggregazione media.](../../reporting-services/mobile-reports/media/reporting-services-kpi-pick-aggregation.png)

6. Fare clic su **OK**.

7. Nella casella **Set di tendenze** fare clic su **Tendenza del set di dati**.  
  
6. Nella casella **Scegliere una tendenza del set di dati** fare clic sui puntini di sospensione ( **...** )  
   
7. Selezionare un campo e fare clic su **OK**.  

   ![PBI_SSMRP_KPIPickTrend](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipicktrend.png)  
  
8. Assegnare un nome all'indicatore KPI e selezionare un tipo di visualizzazione, quindi fare clic su **Crea**.   
  
   L'indicatore KPI viene visualizzato nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
   
    ![PBI_SSMRP_NewKPI](../../reporting-services/mobile-reports/media/pbi-ssmrp-newkpi.png)  
    
## <a name=""></a><a name="create-mobile-report">Creare un report per dispositivi mobili di Reporting Services</a>  
   
Per creare un report per dispositivi mobili di Reporting Services, [installare SQL Server Mobile Report Publisher](../reporting-services-features-and-tasks-ssrs.md)o avviarlo dal portale Web di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . 

Quando si apre per la prima volta [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)], verrà visualizzata un'area di disegno vuota in cui è possibile creare il report per dispositivi mobili. È possibile creare prima oggetti visivi oppure iniziare con i propri dati. Se si creano prima oggetti visivi, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] genera automaticamente dati simulati correlati al report e cambia in modo dinamico man mano che si modificano le selezioni degli oggetti visivi. È possibile provare questa funzionalità.   
  
## <a name="start-with-the-visuals"></a>Iniziare con gli oggetti visivi  
  
1. Nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] fare clic su **Nuovo** > **Report per dispositivi mobili** per avviare [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)

   [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] apre la griglia del layout master.  
  
2. Nella scheda **Layout** scorrere fino alla sezione Grafici.  
  
   ![PBI_SSMRP_LayoutTabCharts2](../../reporting-services/mobile-reports/media/pbi-ssmrp-layouttabcharts2.png)  
  
2. Trascinare **Mappa ad albero** fino alla griglia e trascinare l'angolo inferiore destro in modo che abbia una larghezza pari a tre colonne e un'altezza pari a tre righe.  
  
   ![PBI_SSMRP_TreeMap](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemap.png)  
  
3. È possibile visualizzarne le proprietà visive nella parte inferiore. Può essere necessario scorrere orizzontalmente per visualizzarle tutte.   
  
   ![PBI_SSMRP_TreeMapVisProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapvisprops.png)  
  
4. Con l'oggetto visivo mappa ad albero selezionato, selezionare la scheda **Dati** nell'angolo superiore sinistro.   
  
   A questo punto verranno visualizzati i campi e i valori simulati generati da [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] , nonché ciò che rappresentano le dimensioni e il colore nella mappa ad albero.  
  
   ![PBI_SSMRP_TreeMapDataProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapdataprops.png)  
  
6. Fare clic sulla scheda **Layout**.  
  
7. Fare clic sulla ruota dentata Opzioni ![PBI_SSMRP_Cog](../../reporting-services/mobile-reports/media/pbi-ssmrp-cog.png) nell'angolo superiore destro della mappa ad albero per visualizzare il menu in essa contenuto.   
  
   ![PBI_SSMRP_OptionsWheel](../../reporting-services/mobile-reports/media/pbi-ssmrp-optionswheel.png)  
  
8. Fare clic sulla freccia al centro della rotellina per chiuderla.  
  
## <a name="add-your-own-data"></a>Aggiungere i propri dati  
  
1. Passare alla scheda **Dati** .    
   
2. Per aggiungere i propri dati, fare clic su **Aggiungi dati** nell'angolo superiore destro e passare ai propri dati.    
  
3. È possibile usare dati da una cartella di lavoro di Excel locale, ma in questo caso i dati provengono dal set di dati condiviso nel portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . Viene visualizzato il messaggio "Server aggiunto".  
  
4. Selezionare il server e quindi il set di dati creato.  
   
3. Nella scheda **Dati** nel riquadro **Proprietà dati** modificare **Le dimensioni rappresentano**, **Il colore rappresenta** e altre proprietà con i campi presenti nei propri dati. 
   
   *  I campi **Le dimensioni rappresentano**, **Il colore rappresenta** e **Valore centrale personalizzato** devono contenere valori numerici. 
   *  **Raggruppa per** è una categoria, pertanto è un campo di testo.
   
   ![Screenshot della sezione Proprietà dati.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-data-properties.png)
   
6. Selezionare **Anteprima** per visualizzare la mappa ad albero aggiornata con i propri dati.  

## <a name="add-a-gauge-to-your-mobile-report"></a>Aggiungere un misuratore al report per dispositivi mobili

Verrà aggiunto un misuratore per visualizzare il confronto tra le vendite da inizio anno e le vendite dell'anno precedente usando lo stesso set di dati.

1. Nella scheda Layout trascinare un semicerchio nell'area di disegno. Posizionarlo sotto la mappa ad albero e trascinare l'angolo inferiore destro in modo che abbia una larghezza pari a tre quadrati e un'altezza pari a due quadrati. 

2. Anche in questo caso inizialmente verranno usati dati simulati. 

   Si noti che in **Proprietà visive** per impostazione predefinita **I valori più alti sono preferibili** ed **Etichetta delta** è impostata su **Percentuale della destinazione**. I valori predefiniti di **Interruzioni intervallo** possono essere modificati, ma per il momento sono corretti.

   ![Screenshot della sezione per l'impostazione degli intervalli delle proprietà visive dell'anello del report per dispositivi mobili.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-visual-properties.png)
   
3. Nella scheda **Dati** selezionare la tabella con i propri dati e selezionare il campo **Valore principale** e il campo con il quale lo si vuole confrontare in **Valore di confronto**.

4. È possibile scegliere aggregazioni diverse per definire un numero per il campo **Valore principale** e uno per **Valore di confronto**. Per impostazione predefinita, è una somma.

   ![Screenshot delle opzioni o del valore di confronto.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-sum.png)

5. Selezionare **Anteprima** per visualizzarne l'aspetto. 

   ![Screenshot dell'anteprima dell'anello del report per dispositivi mobili.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-preview.png)

## <a name="add-a-selection-list-as-a-filter"></a>Aggiungere un elenco di selezione come filtro

Gli elenchi di selezione funzionano come filtri dei dati in Power BI ed Excel. È possibile aggiungerne uno per filtrare gli altri oggetti visivi nel report per dispositivi mobili.

1. Nella scheda **Layout** trascinare un elenco di selezione alla destra della mappa ad albero e trascinare l'angolo inferiore destro in modo che abbia una larghezza pari a due quadrati e un'altezza pari a quella dell'area di disegno, ovvero cinque quadrati. 

   ![Screenshot dell'elenco di selezione del report per dispositivi mobili.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list.png)

2. Nella scheda **Dati** , **Proprietà dati**, impostare **Chiavi** ed **Etichette** su un campo nei propri dati in base al quale si vuole filtrare i risultati.

   ![Screenshot della sezione Proprietà dati dell'elenco di selezione del report per dispositivi mobili.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list-data-properties.png)
   
## <a name="create-a-mobile-report-for-phones"></a>Creare un report per dispositivi mobili per i telefoni  
  
Ora che sono stati creati gli oggetti visivi nel layout master, è possibile creare un report per dispositivi mobili con un layout ottimizzato in modo specifico per gli utenti dei telefoni.    
  
1. Nell'angolo superiore destro fare clic sull'icona dell'area di disegno > **Telefono**.  
  
2. Nella scheda Layout in **Control Instances**(Istanze di controllo) verranno visualizzati i due grafici creati.   
  
3. Trascinare la mappa ad albero fino all'area di disegno del telefono e modificarla in modo che abbia una larghezza pari a quattro colonne e un'altezza pari a tre righe.  
  

## <a name="save-your-mobile-report"></a>Salvare il report per dispositivi mobili  
È possibile salvare il report localmente o in un portale Web di [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . In locale [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] salva il report con i dati memorizzati nella cache, pertanto è possibile aprirlo e continuare a lavorarci. Non è tuttavia possibile visualizzarlo in un dispositivo mobile.   
  
1. Fare clic sull'icona Salva nell'angolo superiore sinistro.   
   
2. Per condividerlo con altri utenti e visualizzarlo in un dispositivo mobile, fare clic su **Salva nel server**.  
  
3. Nel server passare alla cartella in cui si vuole salvare il report per dispositivi mobili.  
  
4. Fare clic su **Scegli cartella** > **Salva**.  
  
   Viene visualizzato un messaggio che conferma il salvataggio del report.  
    
   Dopo averlo salvato, è possibile tornare al portale e visualizzare l'anteprima del report per dispositivi mobili.   
    
5. Toccare l'anteprima per visualizzare il report nel portale Web.  
  
## <a name="view-your-report-on-the-web-portal"></a>Visualizzare il report nel portale Web

  
## <a name="view-your-report-on-a-mobile-device"></a>Visualizzare il report in un dispositivo mobile   
  
Per visualizzare il report di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , è necessario innanzitutto:

*  Se non si ha ancora un account,[iscriversi al servizio Power BI](https://go.microsoft.com/fwlink/?LinkID=513879).
*  [Scaricare l'app Power BI per dispositivi mobili](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) nel proprio dispositivo mobile.  

### <a name="view-your-mobile-report"></a>Visualizzare il report per dispositivi mobili
  
1.  Aprire e accedere all'app Power BI nel dispositivo mobile.  
    
2.  Per visualizzare i report per dispositivi mobili e gli indicatori KPI di Reporting Services, toccare **Reporting Services**.  
![PBI_iPad_GetStartedSm](../../reporting-services/mobile-reports/media/pbi-ipad-getstartedsm.png)  
  
3. Toccare l'icona delle opzioni ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) nell'angolo superiore destro e quindi **Connetti al server**.  
  
   ![PBI_iPad_SSMRP_ConnectCrop](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcrop.png)  
  
4. Assegnare un nome al server e specificare l'indirizzo del server, il proprio indirizzo di posta elettronica e la propria password in questo formato:  
  
   ![PBI_iPad_SSMRP_ConnectContoso](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcontoso.png)   
  
5.  Il server verrà ora visualizzato nella barra di spostamento sinistra.  
  
    ![PBI_iPad_SSMRP_LeftNavBiggr](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-leftnavbiggr.png)  
      
> [!TIP]
> Toccare l'icona delle opzioni ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) in qualsiasi momento per passare dai report per dispositivi mobili nel portale Web di Reporting Services ai dashboard nel servizio Power BI.  

  
## <a name="view-kpis-and-mobile-reports-in-the-power-bi-app"></a>Visualizzare gli indicatori KPI e i report per dispositivi mobili nell'app Power BI  
  
Toccare la scheda **Indicatori KPI** o **Report per dispositivi mobili** .   
  
![PBI_iPad_SSMRP_Portal](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-portal.png)  
  
- Toccare un indicatore KPI per visualizzarlo in modalità messa a fuoco.  
  
    ![PBI_iPad_SSMRP_Tile](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-tile.png)  
  
- Toccare un report per dispositivi mobili per aprirlo e interagire con esso nell'app Power BI.  
  
Gli indicatori KPI e i report per dispositivi mobili vengono visualizzati nelle stesse cartelle in cui si trovano nel portale Web di Reporting Services.   
  
## <a name="see-also"></a>Vedere anche  
 
-  l'articolo relativo ai [report per dispositivi mobili e indicatori KPI dei server di report locali nell'app Power BI per dispositivi mobili](/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports) per i dispositivi iOS e Android
-  l'articolo relativo ai [report per dispositivi mobili e indicatori KPI dei server di report locali nell'app Power BI per dispositivi mobili per Windows 10](https://powerbi.microsoft.com/documentation/powerbi-mobile-win10-kpis-mobile-reports/)    
  
