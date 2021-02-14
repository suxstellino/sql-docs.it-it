---
title: Creare il modello
description: Informazioni su come creare un modello in Master Data Services per contenere oggetti modello. È necessario disporre delle autorizzazioni per l'area Amministrazione sistema.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], creating models
- creating models [Master Data Services]
ms.assetid: 9bb3b3b3-bde8-44aa-ad62-eaae21188141
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f153a6b7bf2f4ceef65b06d02a825620ea7b3fb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272622"
---
# <a name="create-a-model-master-data-services"></a>Creare un modello (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]creare un modello per contenere oggetti modello.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model"></a>Per creare un modello  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Vista modelli** scegliere **Gestisci** dalla barra dei menu, quindi fare clic su **Modelli**.  
  
3.  Nella pagina **Gestisci modelli** fare clic su **Aggiungi**. Viene visualizzato un pannello sulla destra.  
  
4.  Nella casella **Nome** digitare il nome del modello.  
  
5.  Nel campo **Descrizione** digitare la descrizione del modello (facoltativo).  
  
6.  Nel campo **Giorni di conservazione log** selezionare una delle opzioni per la conservazione dei dati del log. Il valore predefinito è **Impostazione di sistema**, che indica che il valore viene ereditato dalle impostazioni di sistema nel [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Per altre informazioni, vedere [Impostazioni di sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
     Per ignorare l'impostazione di sistema e non rimuovere i dati del log delle transazioni, selezionare **NO**. Per conservare solo i dati del log di oggi e troncare i dati di log per tutti i giorni precedenti, selezionare **Sì** e impostare il campo **giorni** su 0. Per conservare i dati del log per un numero specificato di giorni, selezionare **SÌ** e impostare il campo **Giorni** sul numero di giorni.  
  
7.  Facoltativamente, scegliere **Crea entità con lo stesso nome del modello** per creare un'entità con lo stesso nome del modello.  
  
8.  Fare clic su **Salva modello**.  
  
 Per ogni modello creato, viene aggiunta una riga con otto colonne alla griglia. Le otto colonne sono:  
  
-   **Stato**: stato del modello. Quando si fa clic sul pulsante **Salva modello** , viene visualizzata l'immagine di ![aggiornamento](../master-data-services/media/mds-model-status-updating.png "Aggiornamento") , che indica che il modello è in fase di aggiornamento. Se si verificano errori durante la creazione o la modifica di un modello, viene visualizzata l'immagine dell' ![errore](../master-data-services/media/mds-model-status-error.png "Errore") . In caso contrario, lo stato è OK e viene visualizzata l'immagine ![OK](../master-data-services/media/mds-model-status-ok.png "OK") .  
  
-   **Nome**: nome del modello.  
  
-   **Descrizione**: descrizione del modello.  
  
-   **Giorni di conservazione log**: numero di giorni per i quali il log per il modello viene conservato.  
  
-   **Creato da**: nome utente dell'utente che ha creato il modello.  
  
-   **Data e ora di creazione**: data e ora di creazione del modello.  
  
-   **Aggiornato da**: nome utente dell'ultimo utente che ha aggiornato l'attributo.  
  
-   **Data e ora di aggiornamento**: data e ora dell'ultimo aggiornamento del modello.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   [Creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Modelli &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)   
 [Entità &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [Eliminare un modello &#40;Master Data Services&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [Modifica Master Data Services &#40;del modello&#41;](../master-data-services/edit-model-master-data-services.md)   
 [Transazioni &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)  
  
  
