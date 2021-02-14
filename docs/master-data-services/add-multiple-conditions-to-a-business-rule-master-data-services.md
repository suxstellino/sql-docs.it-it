---
description: Aggiungere più condizioni a una regola business (Master Data Services)
title: Aggiungere condizioni a una regola business
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], multiple conditions
ms.assetid: 5f0f6958-6cf2-444b-bdcd-05b887637a0b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e5529b10ef0a293a19c45eb8a0e904fcf1fc0f42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339688"
---
# <a name="add-multiple-conditions-to-a-business-rule-master-data-services"></a>Aggiungere più condizioni a una regola business (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]aggiungere più condizioni **AND** o **OR** a una regola di business quando si vuole creare una regola più complessa.  
  
> [!NOTE]  
>  Se si crea una regola di business che usa l'operatore **OR** , considerare la possibilità di creare una regola separata per ogni istruzione condizionale che può essere valutata indipendentemente. È quindi possibile escludere regole in base alle esigenze, offrendo maggiore flessibilità e una più facile risoluzione dei problemi.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   È necessario che sia presente una regola business. Per altre informazioni, vedere [Creare e pubblicare una regola di business &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
### <a name="to-add-multiple-conditions-to-a-business-rule"></a>Per aggiungere più condizioni a una regola business  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Dalla barra dei menu scegliere **Gestisci** e fare clic su **Regole business**.  
  
3.  Nella pagina **Regole business** selezionare un modello dall'elenco a discesa **Modello** .  
  
4.  Nell'elenco a discesa **Entità** selezionare un'entità.  
  
5.  Nell'elenco a discesa **Tipi di membri** selezionare un tipo di membro.  
  
6.  Fare clic sulla riga della regola business che si desidera modificare.  
  
7.  Fare clic su **Modifica**.  
  
8.  Nel blocco **If** a sinistra dell'elenco a discesa dell'operatore logico selezionare **AND/OR/ NOT**.  
  
9. Fare clic su **Aggiungi**. Viene visualizzato un pannello.  
  
10. Nell'elenco a discesa **Attributo** selezionare un attributo.  
  
11. Nell'elenco a discesa **Operatore** selezionare una condizione.  
  
12. Completare tutti i campi obbligatori.  
  
13. Fare clic su **Salva**. Viene aggiunta una nuova riga alla griglia **If** .  
  
14. Facoltativamente, per aggiungere altre condizioni completare i passaggi 8-13.  
  
    > [!TIP]  
    >  Per eliminare una condizione, selezionarla, fare clic con il pulsante destro del mouse su di essa e scegliere **Elimina**.  
  
    > [!TIP]  
    >  È possibile selezionare più condizioni e fare clic con il pulsante destro del mouse per raggrupparle o separarle all'interno di un operatore logico specifico.  
  
## <a name="see-also"></a>Vedere anche  
 [Regole business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Modificare il nome di una regola business &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [Configurare le regole di business per l'invio di notifiche &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)  
  
  
