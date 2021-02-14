---
description: Creare un attributo basato su dominio (componente aggiuntivo MDS per Excel)
title: Creare un attributo basato su dominio
ms.custom: microsoft-excel-add-in
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 7b3e30dc-8f41-4a5d-8009-ae5a4426a64b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c54f2a787fa14afe89d593faca7fc0072525ef7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272422"
---
# <a name="create-a-domain-based-attribute-mds-add-in-for-excel"></a>Creare un attributo basato su dominio (componente aggiuntivo MDS per Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]gli amministratori possono creare un attributo basato su dominio per vincolare i valori in una colonna a un set di valori specifico.  
  
 I valori possono essere già presenti nel foglio di lavoro oppure possono derivare da un'entità esistente.  
  
> [!NOTE]  
>  Se gli utenti digitano un valore nella colonna vincolata, anziché selezionarlo nell'elenco, gli errori vengono visualizzati nella colonna **$InputStatus$** al momento della pubblicazione.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre dell'autorizzazione di accesso alle aree funzionali **Amministrazione sistema** e **Visualizzatore** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md).  
  
-   Il modello e l'entità devono esistere già.  
  
### <a name="to-perform-this-procedure"></a>Per eseguire questa procedura:  
  
1.  In Excel caricare l'entità contenente la colonna (attributo) che si desiderare vincolare. Per altre informazioni, vedere [Esportare dati in Excel da Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md).  
  
2.  Fare clic su una cella nella colonna che si desidera vincolare.  
  
3.  Nel gruppo **Compila modello** fare clic su **Proprietà attributi**.  
  
4.  Nella finestra di dialogo **Proprietà attributi** scegliere **Elenco vincolato (basato su dominio)** nell'elenco **Tipo di attributo**.  
  
5.  Nell'elenco **Popola l'attributo con valori di** :  
  
    -   Per usare valori del foglio di lavoro, scegliere **colonna selezionata**. Verranno create una nuova entità e una nuova tabella di staging con i valori dalla colonna selezionata.  
  
    -   Per utilizzare valori di un'entità esistente, scegliere il nome dell'entità.
    
    Se sono presenti più di 50 entità, è possibile applicare un filtro e cercare l'entità. In alternativa selezionare un'entità nell'elenco a discesa.  
  
6.  Se si sceglie **colonna selezionata** nel passaggio precedente, nella casella **Nome nuova entità** digitare un nome per la nuova entità. Il nome può corrispondere a quello della colonna (attributo).  
  
7.  Fare clic su **OK**. In ogni cella nella colonna è ora presente un elenco di valori tra cui gli utenti possono scegliere.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   Per aggiungere ed eliminare valori nell'elenco vincolato, caricare l'entità su cui l'attributo è basato. Per altre informazioni sul caricamento delle entità, vedere [Esportare dati in Excel da Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Attributi basati su dominio &#40;Master Data Services&#41;](../../master-data-services/domain-based-attributes-master-data-services.md)   
 [Creare un'entità &#40;Componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-an-entity-mds-add-in-for-excel.md)   
 [Compilazione di un modello &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/building-a-model-mds-add-in-for-excel.md)  
  
  
