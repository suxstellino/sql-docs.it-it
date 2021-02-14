---
description: Creazione e distribuzione di una cache per la trasformazione Ricerca
title: Creare e distribuire una cache per la trasformazione Ricerca | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- creating cache files for Lookup transformation
- deploying cache files for Lookup transformation
- Lookup transformation cache files
ms.assetid: cedf5cad-2fac-42d0-ad91-9461e117d330
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 426ec69e927a61acfc083f9f4c0462553ea11d97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348289"
---
# <a name="create-and-deploy-a-cache-for-the-lookup-transformation"></a>Creazione e distribuzione di una cache per la trasformazione Ricerca

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile creare e distribuire un file di cache (.caw) per la trasformazione Ricerca. Il set di dati di riferimento è archiviato nel file di cache.  
  
 La trasformazione Ricerca esegue ricerche unendo in join i dati contenuti nelle colonne di input da un'origine dati connessa con le colonne nel set di dati di riferimento.  
  
 Creare un file di cache utilizzando una Gestione connessione cache e una trasformazione di tipo cache. Per altre informazioni, vedere [Gestione connessione della cache](../../connection-manager/cache-connection-manager.md) e [Trasformazione Cache](../../../integration-services/data-flow/transformations/cache-transform.md).  
  
 Per sapere di più sulla trasformazione Ricerca e i file di cache, vedere [Trasformazione Ricerca](../../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
### <a name="to-create-a-cache-file"></a>Per creare un file di cache  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenente il pacchetto che si desidera utilizzare.  
  
2.  Nella scheda **Flusso di controllo** , aggiungere un'attività Flusso di dati.  
  
3.  Nella scheda **Flusso di dati** , aggiungere una trasformazione di tipo cache al flusso di dati e quindi connettere la trasformazione a un'origine dati.  
  
     Configurare l'origine dati in base alle esigenze.  
  
4.  Fare doppio clic sulla Trasformazione cache e quindi in **Editor trasformazione cache**, nella pagina **Gestione connessione** fare clic su **Nuova** per creare una nuova gestione connessione della cache.  
  
5.  In **Editor gestione connessione della cache**, nella scheda **Generale** configurare la Gestione connessione della cache per salvare la cache selezionando le seguenti opzioni:  
  
    1.  Selezionare **Usa cache dei file**.  
  
    2.  In **Nome file**, digitare il percorso del file.  
  
     Il sistema crea il file quando viene eseguito il pacchetto.  
  
    > [!NOTE]  
    >  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Fare clic sulla scheda **Colonne** e quindi specificare quali colonne sono le colonne di indice usando l'opzione **Posizione dell'indice** .  
  
     Per le colonne non dell'indice, la posizione è 0. Per le colonne di indice, la posizione di indice è un numero sequenziale e positivo.  
  
    > [!NOTE]  
    >  Quando la trasformazione Ricerca viene configurata per utilizzare una Gestione connessione cache, è possibile eseguire il mapping solo delle colonne di indice nel set di dati di riferimento alle colonne di input. Inoltre, è necessario eseguire il mapping di tutte le colonne di indice.  
  
     Per altre informazioni, vedere [Editor gestione connessione cache](../../connection-manager/cache-connection-manager.md).  
  
7.  Configurare la Trasformazione Cache in base alle esigenze.  
  
     Per altre informazioni, vedere [Editor trasformazione cache &#40;pagina Gestione connessioni&#41;](./cache-transform.md) e [Editor trasformazione cache &#40;pagina Mapping&#41;](./cache-transform.md).  
  
8.  Eseguire il pacchetto.  
  
### <a name="to-deploy-a-cache-file"></a>Per distribuire un file di cache  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenente il pacchetto che si desidera utilizzare.  
  
2.  Facoltativamente, creare una configurazione di pacchetto. Per altre informazioni, vedere [Creazione di configurazioni dei pacchetti](../../packages/legacy-package-deployment-ssis.md).  
  
3.  Aggiungere il file di cache al progetto eseguendo le operazioni seguenti:  
  
    1.  In Esplora soluzioni, selezionare il progetto aperto nel passaggio 1.  
  
    2.  Scegliere **Aggiungi elemento esistente** dal menu **Progetto**.  
  
    3.  Selezionare il file di cache e fare clic su **Aggiungi**.  
  
     Il file viene visualizzato nella cartella **Varie** in Esplora soluzioni.  
  
4.  Configurare il progetto per creare un'utilità di distribuzione e quindi compilare il progetto. Per altre informazioni, vedere [Creazione di un'utilità di distribuzione](../../packages/legacy-package-deployment-ssis.md).  
  
     Viene creato un file manifesto, \<*project name*>.SSISDeploymentManifest.xml che elenca i vari file nel progetto, i pacchetti e le configurazioni del pacchetto.  
  
5.  Distribuire il pacchetto nel file system. Per altre informazioni, vedere [Distribuzione di pacchetti con l'utilità di distribuzione](../../packages/legacy-package-deployment-ssis.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un'utilità di distribuzione](../../packages/legacy-package-deployment-ssis.md)  
  
