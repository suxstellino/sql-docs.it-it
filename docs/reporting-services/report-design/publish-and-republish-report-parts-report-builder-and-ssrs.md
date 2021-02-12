---
title: Pubblicare e ripubblicare parti del report (Generatore report) | Microsoft Docs
description: Informazioni su come pubblicare una parte del report con metadati modificati con le impostazioni predefinite in un percorso predefinito in Generatore report.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 92dce484-f39b-403c-9caf-d8772bc3aca3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d91904f4d91941549582d8d7c578dcc4682f23f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014737"
---
# <a name="publish-and-republish-report-parts-report-builder-and-ssrs"></a>Pubblicare e ripubblicare parti del report (Generatore report e SSRS)
  Le parti di report sono elementi impaginati pubblicati separatamente in un server di report e che possono essere riusati in altri report impaginati. È possibile pubblicare una parte del report con le impostazioni predefinite in un percorso predefinito o modificare i metadati della parte del report, ad esempio il nome e la descrizione, e salvarla altrove in un server di report. Il salvataggio può avvenire anche in un sito di SharePoint integrato con un server di report, se si dispone delle opportune autorizzazioni.  
  
 È possibile pubblicare solo la parte di report, con il set di dati da cui dipende incorporato, oppure pubblicare separatamente il set di dati. In quest'ultimo caso diventa un set di dati condiviso, al quale la parte di report viene collegata. Per altre informazioni, vedere [Parti del report e set di dati in Generatore report](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
 L'utente può ripubblicare una parte di report esistente. Se si dispone delle autorizzazioni, è possibile salvarla sovrascrivendo l'istanza originale della parte di report nel server, anche se non è stata creata dall'utente. È possibile inoltre pubblicarla come nuova parte di report e salvarla nello stesso percorso o in uno diverso.  
  
## <a name="to-publish-a-report-part"></a>Per pubblicare una parte di report  
  
1.  Nel menu Generatore report selezionare **Pubblica parti del report**.  
  
     Se non si è connessi a un server di report, verrà richiesto di connettersi. Senza la connessione a un server di report non è possibile pubblicare parti di report.  
  
2.  Per salvare le parti di report con le impostazioni predefinite nel percorso predefinito, fare clic su **Pubblica tutte le parti di report con le impostazioni predefinite**.  
  
     In caso contrario, scegliere **Consente di verificare e modificare le parti del report prima di eseguire la pubblicazione**.  
  
3.  Modifica del nome e della descrizione della parte di report: fare doppio clic sul nome per modificarlo e selezionare il campo **Descrizione** per aggiungere una descrizione.  
  
    > [!NOTE]  
    >  È opportuno attribuire alla parte di report un nome e una descrizione in modo da facilitarne l'identificazione da parte degli utenti durante la ricerca. La lunghezza massima per il nome di una parte di report è 260 caratteri per il percorso intero, inclusi i nomi delle cartelle nel server, seguito dal nome effettivo della parte di report.  
  
4.  Per salvare la parte del report in una cartella diversa da quelle specificata nelle impostazioni predefinite, fare clic sul pulsante **Sfoglia** .  
  
5.  Dopo avere impostato le opzioni per tutte le parti del report, fare clic su **Pubblica**.  
  
     Dopo la pubblicazione, nella finestra di dialogo viene mostrato quali parti di report sono state pubblicate correttamente e quali non lo sono state. È possibile visualizzare i dettagli nella finestra di dialogo **Pubblica parti del report** per le parti del report non pubblicate correttamente.  
  
6.  Fare clic su **Close**.  
  
## <a name="to-republish-a-report-part"></a>Per ripubblicare una parte di report  
  
1.  Seguire la procedura precedente per la pubblicazione di una parte di report.  
  
2.  Nella finestra di dialogo **Pubblica parti di report** , se si sceglie **Pubblica come nuova copia della parte del report**, Generatore report non sovrascriverà la parte del report esistente sul server, ma creerà un'altra parte del report.  
  
> [!NOTE]  
>  Se pubblicata come nuova parte di report, presenterà un nuovo ID univoco. Non riceverà più gli aggiornamenti quando la parte di report originale viene modificata.  
  
## <a name="see-also"></a>Vedere anche  
 [Parti del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)   
 [Parti del report e set di dati in Generatore report](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Ricerca di parti del report e impostazione di una cartella predefinita &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs.md)  
  
  
