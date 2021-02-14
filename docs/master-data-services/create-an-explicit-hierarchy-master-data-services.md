---
description: Creare una gerarchia esplicita (Master Data Services)
title: Creare una gerarchia esplicita
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating explicit hierarchies [Master Data Services]
- explicit hierarchies, creating
ms.assetid: ba768393-6990-4eda-8cb0-d58cb3cfc2e2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 04cec56f41b83b011e4c9514d078dada7ddbd402
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351916"
---
# <a name="create-an-explicit-hierarchy-master-data-services"></a>Creare una gerarchia esplicita (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]creare una gerarchia esplicita quando è necessaria una gerarchia incompleta nella quale possono esistere membri a qualsiasi livello. Nelle gerarchie esplicite sono inclusi membri da una singola entità.  
  
 Dopo aver creato una gerarchia esplicita, è possibile aggiungervi membri nell'area funzionale **Esplora** .  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   È necessario abilitare l'entità per le gerarchie esplicite e le raccolte.  
  
### <a name="to-create-an-explicit-hierarchy"></a>Per creare una gerarchia esplicita  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.  
  
3.  Dalla griglia della pagina **Gestisci entità** selezionare la riga per l'entità per cui si vuole creare una gerarchia esplicita.  
  
4.  Fare clic su **gerarchie esplicite**.  
  
5.  Nella pagina **Gestisci gerarchie esplicite** fare clic su **Aggiungi**.  
  
6.  Nella casella **Nome** digitare un nome per la gerarchia.  
  
7.  Facoltativamente, deselezionare la casella di controllo **Gerarchia obbligatoria** per creare una gerarchia come non obbligatoria. Per altre informazioni sui tipi di gerarchia, vedere [Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md).  
  
8.  Fare clic su **Salva**.  
  
## <a name="grid-columns"></a>Colonne della griglia  
 Per ogni gerarchia esplicita creata, viene aggiunta una riga con sette colonne alla griglia. Di seguito sono descritte le colonne.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|Stato|Stato dell'entità. Quando si fa clic su **Salva** , viene visualizzata l'immagine seguente che indica che l'entità è in fase di aggiornamento.<br /><br /> ![Icona per lo stato di aggiornamento](../master-data-services/media/mds-statusicon-updating.png "Icona per lo stato di aggiornamento")<br /><br /> Se si verificano errori durante la creazione o la modifica di un'entità, viene visualizzata l'immagine seguente.<br /><br /> ![Icona per lo stato di errore](../master-data-services/media/mds-statusicon-error.png "Icona per lo stato di errore")<br /><br /> Se lo stato è OK, viene visualizzata l'immagine seguente.<br /><br /> ![Icona per lo stato OK](../master-data-services/media/mds-statusicon-ok.png "Icona per lo stato OK")|  
|Nome|Il nome della gerarchia esplicita.|  
|È obbligatorio|Specifica se la gerarchia esplicita è obbligatoria.|  
|Created By (Creato da)|Nome utente dell'utente che ha creato la gerarchia esplicita.|  
|Data creazione|Data e ora di creazione della gerarchia esplicita.|  
|Aggiornato da|Nome utente dell'ultimo utente che ha aggiornato la gerarchia esplicita.|  
|Aggiornato il|Data e ora dell'ultimo aggiornamento della gerarchia esplicita.|  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   [Creare membri consolidati &#40;Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)  
  
  
  
## <a name="see-also"></a>Vedere anche  
 [Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Gerarchie derivate con estremità esplicite &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)   
 [Modificare il nome di una gerarchia esplicita &#40;Master Data Services&#41;](../master-data-services/change-an-explicit-hierarchy-name-master-data-services.md)  
  
  

