---
description: Crea nuovo InfoObject
title: Crea nuovo InfoObject | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3587a633-1c0b-4d63-a22a-6b2b93923c3a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9cb9336b7999373b1f6af5bb5327a35f77fdbe7e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127229"
---
# <a name="create-new-infoobject"></a>Crea nuovo InfoObject

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Usare la finestra di dialogo **Crea nuovo InfoObject** per creare un nuovo InfoObject nel sistema SAP Netweaver BW.  
  
 È possibile aprire la finestra di dialogo **Crea InfoObject** dalla pagina **Gestione connessione** dell' **Editor destinazione SAP BW**. Per ulteriori informazioni sulla destinazione SAP BW, vedere [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentazione per Microsoft Connector 1.1 for SAP BW presuppone la conoscenza dell'ambiente SAP Netweaver BW. Per ulteriori informazioni su SAP Netweaver BW o per informazioni su come configurare oggetti e processi di SAP Netweaver BW, vedere la documentazione SAP.  
  
 **Per aprire la finestra di dialogo Crea nuovo InfoObject**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il pacchetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene la destinazione SAP BW.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sulla destinazione SAP BW.  
  
3.  Nell' **Editor destinazione SAP BW** fare clic su **Gestione connessione** per aprire la pagina **Gestione connessione** dell'editor.  
  
4.  Nella casella di gruppo **Crea oggetti SAP BW** della pagina **Gestione connessione** eseguire una delle operazioni seguenti per creare un InfoObject:  
  
    1.  Per creare un InfoObject direttamente, selezionare **InfoObject** e quindi fare clic su **Crea** per aprire la finestra di dialogo **Crea nuovo InfoObject** .  
  
    2.  Per creare un InfoObject durante la creazione di un InfoCube, selezionare **InfoCube** e quindi fare clic su **Crea**. Nella finestra di dialogo **Crea InfoCube per dati transazione** , nella colonna **IObject** di una delle righe dell'elenco selezionare **Crea** per aprire la finestra di dialogo **Crea nuovo InfoObject** .  
  
        > [!NOTE]  
        >  Ogni riga della tabella rappresenta una colonna nel flusso di dati del pacchetto.  
  
    3.  Per creare un InfoObject durante la creazione di un InfoSource per dati transazionali, selezionare **InfoSource** e quindi fare clic su **Crea**. Nella finestra di dialogo **Crea InfoSource** selezionare **Dati transazione** e quindi fare clic su **OK**. Nella finestra di dialogo **Crea InfoSource per dati transazione** , nella colonna **IObject** di una delle righe dell'elenco selezionare **Crea** per aprire la finestra di dialogo **Crea nuovo InfoObject** .  
  
        > [!NOTE]  
        >  Ogni riga della tabella rappresenta una colonna nel flusso di dati del pacchetto.  
  
    4.  Per creare un InfoObject durante la creazione di un InfoSource per dati master, selezionare **InfoSource** e quindi fare clic su **Crea**. Nella finestra di dialogo **Crea InfoSource** selezionare **Dati master** e quindi fare clic su **OK**. Nella finestra di dialogo **Crea InfoSource per dati master** , fare clic su **Nuovo** per aprire la finestra di dialogo **Crea nuovo InfoObject** .  
  
 È anche possibile aprire la finestra di dialogo **Crea nuovo InfoObject** facendo clic su **Nuovo** nella sezione **Attributi** della finestra di dialogo **Crea nuovo InfoObject** .  
  
## <a name="general-options"></a>Opzioni generali  
 **Caratteristica**  
 Creare un InfoObject che rappresenti i dati di dimensione.  
  
 **Cifra chiave**  
 Creare un InfoObject che rappresenti i dati delle tabelle dei fatti.  
  
 **Nome InfoObject**  
 Immettere un nome per l'InfoObject.  
  
 **Breve descrizione**  
 Immettere una breve descrizione per l'InfoObject.  
  
 **Descrizione lunga**  
 Immettere una descrizione lunga per l'InfoObject.  
  
 **Con dati master**  
 Indicare che l'InfoObject contiene dati master sotto forma di attributi, testo o gerarchie.  
  
> [!NOTE]  
>  Selezionare questa opzione se l'InfoObject rappresenta i dati di dimensione e se è stata selezionata l'opzione **Caratteristica** .  
  
 **Consenti caratteri minuscoli**  
 Sono consentiti i caratteri minuscoli nei dati dell'InfoObject.  
  
 **Salva e attiva**  
 Salva e attiva il nuovo InfoObject.  
  
## <a name="data-type-options"></a>Opzioni per i tipi di dati  
 **CHAR - Stringa di caratteri**  
 Indica che l'InfoObject contiene dati di tipo carattere.  
  
 **NUMC - Stringa numerica**  
 Indica che l'InfoObject contiene dati numerici.  
  
 **DATS - Data (AAAAMMGG)**  
 Indica che l'InfoObject contiene dati di data.  
  
 **TIMS - Ora (HHMMSS)**  
 Indica che l'InfoObject contiene dati orari.  
  
 **Lunghezza**  
 Immettere la lunghezza dei dati.  
  
## <a name="text-options"></a>Opzioni di testo  
 **Con testi**  
 Indica che l'InfoObject contiene testi.  
  
 **Testo breve**  
 Indica che l'InfoObject contiene testi brevi.  
  
 **Testo medio**  
 Indica che l'InfoObject contiene testi medi.  
  
 **Testo lungo**  
 Indica che l'InfoObject contiene testi lunghi.  
  
 **Testo dipendente dalla lingua**  
 Indica che i testi sono dipendenti dalla lingua.  
  
 **Testo dipendente dall'ora**  
 Indica che i testi sono dipendenti dall'ora.  
  
## <a name="attributes-section"></a>Sezione Attributi  
 La sezione **Attributi** è costituita da un elenco di attributi per l'InfoObject e dalle opzioni che consentono di aggiungere e rimuovere gli attributi dall'elenco.  
  
### <a name="attributes-list"></a>Elenco Attributi  
 Nell'elenco **Attributi** sono visualizzati gli attributi dell'InfoObject che viene creato. L'elenco **Attributi** presenta le seguenti intestazioni di colonna:  
  
 **InfoObject**  
 Visualizza il nome dell'InfoObject.  
  
 **Descrizione**  
 Visualizza la descrizione dell'InfoObject.  
  
 **Tipo di InfoObject**  
 Visualizza il tipo dell'InfoObject. Nella tabella seguente sono elencati i valori possibili per il tipo.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|CHA|Caratteristiche|  
|KYF|Cifre chiave|  
|UNI|Unità|  
|TIM|Caratteristiche ora|  
  
### <a name="attributes-options"></a>Opzioni degli attributi  
 Utilizzare le opzioni seguenti per aggiungere e rimuovere attributi per l'InfoObject che viene creato:  
  
 **Aggiungere**  
 Aggiungere un InfoObject esistente come attributo.  
  
 Per aggiungere un InfoObject esistente, fare clic su Aggiungi e quindi usare la finestra di dialogo **Cerca InfoObject** per trovare l'InfoObject. Per altre informazioni su questa finestra di dialogo, vedere [Cerca InfoObject](../../integration-services/data-flow/look-up-infoobject.md).  
  
 **Nuovo**  
 Aggiungere un nuovo InfoObject come attributo.  
  
 Per creare e aggiungere un nuovo InfoObject, fare clic su Nuovo e quindi usare una nuova istanza della finestra di dialogo **Crea nuovo InfoObject** per creare il nuovo InfoObject.  
  
 **Rimuovi**  
 Rimuove l'InfoObject selezionato dall'elenco **Attributi** .  
  
## <a name="see-also"></a>Vedere anche  
 [Crea InfoCube per dati transazione](../../integration-services/data-flow/create-infocube-for-transaction-data.md)   
 [Crea InfoSource](../../integration-services/data-flow/create-infosource.md)   
 [Crea InfoSource per dati transazione](../../integration-services/data-flow/create-infosource-for-transaction-data.md)   
 [Crea InfoSource per dati master](../../integration-services/data-flow/create-infosource-for-master-data.md)   
 [Guida sensibile al contesto di Microsoft Connector per SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
