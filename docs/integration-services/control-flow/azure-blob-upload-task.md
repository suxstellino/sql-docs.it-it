---
description: Attività di caricamento BLOB di Azure
title: Attività di caricamento BLOB di Azure | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobuptask.f1
- sql14.dts.designer.afpblobuptask.f1
ms.assetid: 6ea068b0-4cd8-45b5-b89d-09b8f25040c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 588bb693d5bcf8561d7f697dc910c483d63160a7
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783079"
---
# <a name="azure-blob-upload-task"></a>Attività di caricamento BLOB di Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


L' **attività di caricamento BLOB di Azure** consente a un pacchetto SSIS di caricare file nell'archivio BLOB di Azure.
    
Per aggiungere un' **attività di caricamento BLOB di Azure**, trascinare l'attività in Progettazione SSIS e fare doppio clic oppure clic con il pulsante destro del mouse e quindi scegliere **Modifica** per visualizzare la finestra di dialogo seguente relativa all' **editor dell'attività di caricamento BLOB di Azure** .  
  
 **Attività di caricamento BLOB di Azure** è un componente del [Feature Pack di SQL Server Integration Services (SSIS) per Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
  
 La tabella seguente fornisce le descrizioni dei campi della finestra di dialogo.  

|**Campo**|**Descrizione**|  
|---|---|  
|AzureStorageConnection|Consente di specificare un'istanza esistente di Gestione connessioni dell'archiviazione di Azure o di creare una nuova istanza che fa riferimento a un account di archiviazione di Azure che indica la posizione in cui sono ospitati i file BLOB.|  
|BlobContainer|Specifica il nome del contenitore BLOB che contiene i file caricati come BLOB.|  
|BlobDirectory|Specifica la directory BLOB in cui viene archiviato il file caricato come BLOB in blocchi. La directory BLOB è una struttura gerarchica virtuale. Se il BLOB esiste già, viene sostituito.|  
|LocalDirectory|Specifica il nome della directory locale che contiene i file da caricare.|  
|SearchRecursively|Consente di specificare se eseguire la ricerca in modo ricorsivo all'interno di sottodirectory.|  
|FileName|Specifica un filtro per i nomi per la selezione di file con il modello di nomi specificato. Ad esempio, `MySheet*.xls\*` include file come `MySheet001.xls` e `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Specifica un filtro basato su un intervallo di tempo. Sono inclusi i file modificati dopo **TimeRangeFrom** e prima di **TimeRangeTo**.|  
