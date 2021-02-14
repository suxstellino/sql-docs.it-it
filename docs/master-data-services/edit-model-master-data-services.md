---
description: Modifica modello (Master Data Services)
title: Modifica modello
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], changing name
ms.assetid: 399eed32-7c61-4239-9c06-996a65219518
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9831a53f56d831dbc52c5d7309a365723ca938ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341722"
---
# <a name="edit-model-master-data-services"></a>Modifica modello (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], è possibile modificare il nome e descrizione di un modello e indicare per quanti giorni si vogliono conservare i log delle transazioni.  
  
 Per altre informazioni, vedere [Transazioni &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-change-a-model"></a>Per modificare il modello  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Vista modelli** scegliere **Gestisci** dalla barra dei menu, quindi fare clic su **Modelli**.  
  
3.  Dalla griglia della pagina **Gestione modelli** selezionare la riga per il modello con il nome o la descrizione che si vogliono modificare.  
  
4.  Fare clic su **Modifica**.  
  
5.  Nella casella **Nome** digitare il nome aggiornato del modello.  
  
6.  Nel campo **Descrizione** digitare la descrizione aggiornata del modello.  
  
7.  Nel campo **Giorni di conservazione log** selezionare una delle opzioni per la conservazione dei dati del log. Il valore predefinito è **Impostazione di sistema**, che indica che il valore viene ereditato dalle impostazioni di sistema nel [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Per altre informazioni, vedere [Impostazioni di sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
     Per ignorare l'impostazione di sistema e non rimuovere i dati del log delle transazioni, selezionare **NO**. Per conservare solo i dati del log di oggi e troncare i dati di log per tutti i giorni precedenti, selezionare **Sì** e impostare il campo **giorni** su 0. Per conservare i dati del log per un numero specificato di giorni, selezionare **SÌ** e impostare il campo **Giorni** sul numero di giorni.  
  
8.  Fare clic su **Salva modello**.  
  
 La colonna **Stato** nella griglia mostra lo stato dell'operazione sul modello. Quando si fa clic sul pulsante **Salva modello** , viene visualizzata l'immagine di ![aggiornamento](../master-data-services/media/mds-model-status-updating.png "Aggiornamento") , che indica che il modello è in fase di aggiornamento. Se si verificano errori durante la creazione o la modifica di un modello, viene visualizzata l'immagine dell' ![errore](../master-data-services/media/mds-model-status-error.png "Errore") . In caso contrario, lo stato è OK e viene visualizzata l'immagine ![OK](../master-data-services/media/mds-model-status-ok.png "OK") .  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un modello &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [Eliminare un modello &#40;Master Data Services&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [Modelli &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
  
