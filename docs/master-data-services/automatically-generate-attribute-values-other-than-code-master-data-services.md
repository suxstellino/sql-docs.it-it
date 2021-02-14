---
description: Generare automaticamente valori per attributi diversi da Code (Master Data Services)
title: Genera automaticamente valori di attributo
titleSuffix: Master Data Services
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b82f6f81-6e9c-4918-9ea9-4ab5f5d11b15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 90302e701b8975d300e77cc7ca028c76825a79f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339552"
---
# <a name="automatically-generate-attribute-values-other-than-code-master-data-services"></a>Generare automaticamente valori per attributi diversi da Code (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] vengono automaticamente generati valori per l'attributo di un'entità quando si vuole che un valore intero venga assegnato automaticamente come valore ogni volta che viene applicata una regola di business.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Un attributo numerico deve esistere. Per altre informazioni, vedere [Creare un attributo numerico &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md).  
  
### <a name="to-automatically-generate-an-attribute-value"></a>Per generare automaticamente un valore di attributo  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Dalla barra dei menu scegliere **Gestisci** e fare clic su **Regole business**.  
  
3.  Nella pagina **Manutenzione regola business** selezionare un modello dall'elenco **Modello** .  
  
4.  Dall'elenco **Entità** selezionare un'entità.  
  
5.  Dall'elenco **Tipo di membro** selezionare un tipo di membro a cui applicare la regola di business.  
  
6.  Dall'elenco **Attributo** lasciare inalterata l'impostazione predefinita **Tutti**.  
  
7.  Fare clic su **Aggiungi regola di business**.  
  
8.  Fare clic su **Modifica regola business selezionata**.  
  
9. Nel riquadro **Componenti** espandere il nodo **Azioni** .  
  
10. Nel nodo Valore predefinito fare clic su **assume un valore generato** e trascinarlo sull'etichetta **Azioni** del riquadro **THEN**.  
  
11. Nel riquadro **Attributi** fare clic sull'attributo con il valore che si vuole generare e trascinarlo sull'etichetta **Seleziona attributo** del riquadro **Modifica azione** .  
  
12. Digitare un valore nelle caselle **Inizia da** e **Incremento di** . Se i membri esistono già, il valore verrà impostato in base al valore esistente più elevato. Ad esempio, se il valore esistente più elevato è 299 e si imposta **Incremento di** su **1**, il valore del membro successivo sarà impostato su 300.  
  
13. Nel riquadro **Modifica azione** fare clic su **Salva elemento**.  
  
14. Fare clic su **Indietro**.  
  
15. Facoltativamente, nella pagina **Manutenzione regola business** per la riga che contiene la regola business fare doppio clic su una cella nella colonna **Nome**, **Descrizione** o **Notifica** per aggiornare il valore.  
  
16. Fare clic su **Pubblica regole business**.  
  
17. Nella finestra di dialogo di conferma fare clic su **OK**. Lo stato della regola viene modificato in **Attiva**.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   [Convalidare membri specifici rispetto a regole business &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
-   [Convalidare una versione usando le regole di business &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione automatica del codice &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [Regole business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Convalida &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
  
