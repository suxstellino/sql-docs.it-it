---
description: Esportare dati in Excel da Master Data Services
title: Esportare i dati in Excel
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: dd29389b-928c-4e50-995c-c6af27f97805
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e8b7ab33b2d10ab09a8ab3fcf76532dd571936df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337110"
---
# <a name="export-data-to-excel-from-master-data-services"></a>Esportare dati in Excel da Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]è necessario esportare i dati dal repository MDS per poterli usare.  
  
 Per filtrare il set di dati prima del caricamento, vedere [Filtrare i dati prima dell'esportazione &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre dell'autorizzazione per accedere all'area funzionale **Esplora** .  
  
### <a name="to-export-data-from-mds-into-excel"></a>Per esportare i dati da MDS in Excel  
  
1.  Aprire Excel e nella scheda **Dati master** connettersi a un repository MDS. Per altre informazioni, vedere [Connettersi a un repository MDS &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md).  
  
2.  Nel riquadro **Esplora dati master** selezionare un modello e una versione. L'elenco di entità viene popolato.  
  
    -   Se il riquadro **Esplora dati master** non è visibile, nel gruppo **Connetti e carica** fare clic su **Esplora dati master**.  
  
    -   Se il riquadro **Esplora dati master** è disabilitato, significa che il foglio esistente contiene già i dati gestiti da MDS. Per abilitare il riquadro, aprire un nuovo foglio di lavoro.  
  
3.  Nel riquadro **Esplora dati master** nell'elenco di entità fare doppio clic sull'entità che si vuole caricare.  
  
    > [!NOTE]  
    >  -   Solo il primo milione di membri viene caricato in Excel. Per filtrare l'elenco prima di caricarlo, sulla barra multifunzione del gruppo **Connetti e carica** fare clic su **Filtro**.  
    > -   Nelle colonne che sono elenchi vincolati (attributi basati su dominio) vengono caricati per impostazione predefinita solo i primi 25.000 valori. È possibile modificare questo numero nella proprietà MaximumDbaEntitySize nel file excelusersettings.config che si trova nel computer in cui è installato Excel. Questo file si trova in C:\Users\\<utente\>\AppData\Local\Microsoft\Microsoft SQL Server\130\MasterDataServices\\.  
    >   
    >      Se un attributo basato su dominio ha un numero di valori che supera l'impostazione della proprietà MaximumDbEntitySize, l'elenco di valori non viene caricato.  
  
    > [!NOTE]  
    >  Quando si caricano dati delimitati da testo usando il componente aggiuntivo per Microsoft Excel con Excel a 32 bit e le impostazioni per le proprietà relative al **numero di celle da caricare** e al **numero di celle da pubblicare** sono entrambe impostate sul valore massimo 1000, si verificherà un errore di memoria insufficiente. È necessario usare Excel a 64 bit per usare le impostazioni massime per le due proprietà relative al **numero di celle da caricare** e al **numero di celle da pubblicare**.  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Importare dati da Excel in Master Data Services &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/import-data-from-excel-to-master-data-services-mds-add-in-for-excel.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica: esportazione dei dati in Excel &#40;Componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)   
 [Finestra di dialogo filtro &#40;Componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-dialog-box-mds-add-in-for-excel.md)   
 [Panoramica: Importazione di dati da Excel &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
