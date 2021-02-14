---
description: Configurare regole business per l'invio di notifiche (Master Data Services)
title: Configurare regole business per l'invio di notifiche
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], configuring notifications
- e-mail [Master Data Services], configuring business rules
- notifications [Master Data Services], configuring business rules
ms.assetid: b24f7b11-ab53-4642-999c-e17b543b3558
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0b7bbf98bd65399a6eb2235a0acb73c28af277f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272772"
---
# <a name="configure-business-rules-to-send-notifications-master-data-services"></a>Configurare regole business per l'invio di notifiche (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]è possibile configurare le regole di business per inviare notifiche agli utenti relative alle modifiche dei valori di attributo.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre dell'autorizzazione per accedere alle aree funzionali **Amministrazione sistema** e **Autorizzazioni utenti e gruppi** . Se non si dispone dell'autorizzazione per l'area funzionale **Autorizzazioni utenti e gruppi** , non è possibile visualizzare l'elenco di utenti e gruppi cui inviare notifiche.  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   È necessario che sia presente una regola business che utilizza un'azione di convalida. Per altre informazioni, vedere [Creare e pubblicare una regola di business &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
-   È necessario che l'utente o il gruppo che riceve la notifica disponga almeno dell'autorizzazione **Sola lettura** per l'attributo la cui convalida ha esito negativo. Gli utenti o gruppi a cui è stata negata in modo esplicito o implicito l'autorizzazione all'attributo riceveranno la posta elettronica ma non potranno accedere all'attributo in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   Se i messaggi di posta elettronica vengono inviati a un gruppo, verranno ricevuti dai soli membri del gruppo che hanno effettuato l'accesso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
### <a name="to-configure-business-rules-to-send-notifications"></a>Per configurare le regole business per l'invio di notifiche  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Dalla barra dei menu scegliere **Gestisci** e fare clic su **Regole business**.  
  
3.  Nella pagina **Regole business** selezionare un modello nell'elenco **Modello** .  
  
4.  Nell'elenco a discesa **Entità** selezionare un'entità.  
  
5.  Nell'elenco a discesa **Tipi di membri** selezionare un tipo di membro.  
  
6.  Nella griglia selezionare la riga per la regola di business che si vuole modificare e fare clic su **Modifica**.  
  
7.  Selezionare la casella di controllo **Invia notifiche** e dall'elenco a discesa selezionare un utente o un gruppo a cui inviare la notifica di posta elettronica.  
  
8.  Fare clic su **Salva**.  
  
9. Fare clic su **Pubblica tutto**.  
  
10. Nella finestra di dialogo di conferma fare clic su **OK**. Il valore nella colonna **Stato della regola di business** diventa **Attivo** e la colonna **Notifica** mostra l'utente o il gruppo selezionato a cui inviare la notifica.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   Applicare regole business ai dati eseguendo una delle procedure riportate di seguito:  
  
    -   [Convalidare membri specifici rispetto a regole business &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Convalidare una versione usando le regole di business &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
-   Configurare il protocollo della posta elettronica come riportato di seguito:  
  
    -   [Configurare notifiche di posta elettronica &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Notifiche &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [Configurare notifiche di posta elettronica &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
  
