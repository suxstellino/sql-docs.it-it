---
title: Connettere una web part Filtro o Documenti con una web part Visualizzatore di report di Reporting Services | Microsoft Docs
description: Per un prodotto SharePoint, informazioni su come creare un dashboard o una pagina web part in cui siano incluse una web part Filtro o Documenti e una web part Visualizzatore di report.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0e58600c502aa6ebb0a031480ab5bb66f48833ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074852"
---
# <a name="connect-filter-or-documents-web-part-with-a-reporting-services-report-viewer-web-part"></a>Connettere una web part Filtro o Documenti con una web part Visualizzatore di report di Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Se si usa un prodotto SharePoint è possibile creare un dashboard o una pagina web part in cui siano incluse una web part Filtro o Documenti e una web part Visualizzatore di report. Le versioni supportate sono [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] o [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]. Sono anche supportate le versioni [!INCLUDE[winSPServ3](../../includes/winspserv3-md.md)] e [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007. La connessione di una web part Filtro consente all'utente che seleziona i valori di un filtro in una web part Filtro di inviarli a un report con parametri nella stessa pagina. La connessione di una web part Documenti consente all'utente che fa clic su un report nella raccolta documenti di visualizzarlo in una web part Visualizzatore di report adiacente.

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.

 La web part Filtro si usa per inviare valori a uno o più parametri in un report. Per usare una web part Filtro è necessario che per il report siano stati definiti parametri compatibili con i valori, i tipi di dati e il formato inviati dalla web part.  
  
 La web part Documenti è associata alla raccolta documenti del sito principale. Per visualizzare, aggiungere o rimuovere elementi dalla raccolta Documenti, fare clic su **Visualizza tutto il contenuto del sito**. In Raccolte fare clic su **Documenti**. Per gestire gli elementi della raccolta Documenti, è possibile usare i menu **Nuovo**, **Carica** e **Azioni** .  
  
## <a name="connect-a-filter-web-part"></a>Connettere una web part Filtro
  
1.  Aprire o creare la pagina web part o il dashboard.  
  
2.  Scegliere **Modifica pagina** dal menu **Azioni sito**.  
  
3.  Fare clic su **Aggiungi web part**.  
  
4.  Nella categoria **Varie** di **Tutte le web part** selezionare **Visualizzatore report di SQL Server Reporting Services**.  
  
5.  Scegliere **Aggiungi**. La web part verrà aggiunta nella parte superiore dell'area.  
  
6.  In un'altra area della stessa pagina web part o dello stesso dashboard fare clic su **Aggiungi web part**.  
  
7.  Nella sezione **Filtri** di **Tutte le web part** selezionare una web part.  
  
8.  Scegliere **Aggiungi**. La web part verrà aggiunta nella parte superiore dell'area.  
  
9. Nell'area che contiene la web part fare clic sul menu **Modifica** della web part, scegliere **Connessioni**, **Send Filter Values To** (Invia valori filtro a), quindi **Visualizzatore report** - *nome report*.  
  
10. Archiviare le modifiche e salvare la pagina.  
  
## <a name="connect-a-documents-web-part"></a>Connettere una web part Documenti  
  
1.  Aprire o creare la pagina web part o il dashboard.  
  
2.  Scegliere **Modifica pagina** dal menu **Azioni sito**.  
  
3.  Fare clic su **Aggiungi web part**.  
  
4.  Nella sezione **Elenchi e raccolte** di **Tutte le web part** selezionare **Documenti**.  
  
5.  Scegliere **Aggiungi**. La web part verrà aggiunta nella parte superiore dell'area.  
  
6.  Fare clic sul pulsante **Applica** , disponibile nella parte inferiore del riquadro Strumenti, e quindi scegliere **OK** per chiudere il riquadro.  
  
7.  In un'altra area della stessa pagina web part o dello stesso dashboard fare clic su **Aggiungi web part**.  
  
8.  Nella categoria **Varie** di **Tutte le web part** selezionare **Visualizzatore report di SQL Server Reporting Services**.  
  
9. Scegliere **Aggiungi**. La web part verrà aggiunta nella parte superiore dell'area.  
  
10. Nell'area che contiene la web part fare clic sul menu **Modifica** della web part, scegliere **Connessioni**, **Get report definitions from** (Recupera definizioni report da), quindi **Documenti**.  
  
11. Archiviare le modifiche e salvare la pagina.  
  
## <a name="see-also"></a>Vedere anche

 [Aggiungere la web part Visualizzatore di report a una pagina Web](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)   
 [Web part Visualizzatore di report in un sito di SharePoint](./report-viewer-web-part-sharepoint-site.md)   
 [Personalizzare la web part di Visualizzatore di report](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)