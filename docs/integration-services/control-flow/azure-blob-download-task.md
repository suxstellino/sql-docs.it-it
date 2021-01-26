---
description: Attività di download di BLOB di Azure
title: Attività di download di BLOB di Azure | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21ae3810f5f52ab8d77760cc08a887b74ab65977
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783088"
---
# <a name="azure-blob-download-task"></a>Attività di download di BLOB di Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


L'attività di download di BLOB di Azure consente a un pacchetto SSIS di scaricare file dall'archiviazione BLOB di Azure.

Per aggiungere un' **attività di download di BLOB di Azure**, trascinare l'attività in Progettazione SSIS e farvi doppio clic oppure clic con il pulsante destro del mouse, quindi scegliere **Modifica** per visualizzare la finestra di dialogo seguente relativa all' **editor dell'attività di download di BLOB di Azure** .  
  
 **Attività di download di BLOB di Azure** è un componente del [Feature Pack di SQL Server Integration Services (SSIS) per Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 La tabella seguente fornisce una descrizione dei campi della finestra di dialogo.  

|**Campo**|**Descrizione**|  
|---|---|
|AzureStorageConnection|Consente di specificare un'istanza esistente di Gestione connessioni dell'archiviazione di Azure o di creare una nuova istanza che fa riferimento a un account di archiviazione di Azure che indica la posizione in cui sono ospitati i file BLOB.|  
|BlobContainer|Consente di specificare il nome del contenitore BLOB che include i file BLOB da scaricare.|  
|BlobDirectory|Consente di specificare il nome della directory BLOB che include i file BLOB da scaricare. La directory BLOB è una struttura gerarchica virtuale.|  
|SearchRecursively|Consente di specificare se eseguire la ricerca in modo ricorsivo all'interno di sottodirectory.|  
|LocalDirectory|Consente di specificare la directory locale in cui vengono archiviati i file BLOB scaricati.|  
|FileName|Specifica un filtro per i nomi per la selezione di file con il modello di nomi specificato. Ad esempio, `MySheet*.xls\*` include file come `MySheet001.xls` e `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Specifica un filtro basato su un intervallo di tempo. Sono inclusi i file modificati dopo **TimeRangeFrom** e prima di **TimeRangeTo**.|  
