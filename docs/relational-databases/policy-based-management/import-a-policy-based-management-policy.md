---
description: Importare i criteri della gestione basata su criteri
title: Importare i criteri della gestione basata su criteri | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, import policy
ms.assetid: 850b7ef9-d2b7-4754-bf04-7cb419ffb776
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1ea5d3c83667dbd194e9fb82ae7bce2e815d479b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "91412767"
---
# <a name="import-a-policy-based-management-policy"></a>Importare i criteri della gestione basata su criteri
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento verrà descritto come importare un'istanza dei criteri della gestione basata su criteri in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni
 È necessaria l'appartenenza al ruolo PolicyAdministratorRole nel database msdb.

  
##  <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio  
  
### <a name="to-import-a-policy-instance"></a>Per importare un'istanza dei criteri  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si troverà l'istanza dei criteri appena importata.  
  
2.  Fare clic sul segno più per espandere la cartella **Gestione** .  
  
3.  Fare clic sul segno più per espandere la cartella **Gestione criteri**.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Criteri** e selezionare **Importa criteri**.  
  
5.  Nella finestra di dialogo **Importa** digitare il percorso e il nome del file oppure usare il pulsante Sfoglia (**...**) per trovare il file XML che contiene i criteri, quindi selezionare il file. Per ulteriori informazioni sulle opzioni disponibili nella finestra di dialogo **Importa** , vedere [Import Policies Dialog Box](../../relational-databases/policy-based-management/import-policies-dialog-box.md).  
  
6.  Al termine, fare clic su **OK**.  


## <a name="example-policies"></a>Criteri di esempio
 I criteri di esempio sono disponibili nel [repository di codice degli esempi di SQL Server](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/epm-framework/sample-policies). Questi criteri possono essere importati e usati come base per criteri di gestione basati su criteri personalizzati.
