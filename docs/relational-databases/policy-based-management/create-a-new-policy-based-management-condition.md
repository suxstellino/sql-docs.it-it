---
description: Creare una nuova condizione della gestione basata su criteri
title: Creare una nuova condizione della gestione basata su criteri | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policy conditions
ms.assetid: 8a612f7e-6c70-49db-a4de-48431e097cc5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f8eef68a189d9e3c3c8cce16de803ce3b87f0207
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048818"
---
# <a name="create-a-new-policy-based-management-condition"></a>Creare una nuova condizione della gestione basata su criteri
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questo argomento descrive come creare una condizione della gestione basata su criteri in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per creare una condizione tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'appartenenza al ruolo PolicyAdministratorRole nel database msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-create-a-condition"></a>Per creare una condizione  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui creare una condizione della gestione basata su criteri.  
  
2.  Fare clic sul segno più per espandere la cartella **Gestione** .  
  
3.  Fare clic sul segno più per espandere la cartella **Gestione criteri**.  
  
4.  Fare clic sul segno più per espandere la cartella **Facet** .  
  
5.  Fare clic con il pulsante destro del mouse sul facet nel quale creare una nuova condizione e selezionare **Nuova condizione**.  
  
6.  Nella casella **Nome** della finestra di dialogo **Crea nuova condizione** digitare il nome della nuova condizione.  
  
7.  Confermare il facet corretto nell'elenco **Facet** oppure selezionare un facet diverso.  
  
8.  In **Espressione** creare le espressioni della condizione selezionando una proprietà facet nella casella **Campo** , insieme all'operatore e al valore associati. Se si aggiungono più espressioni, è possibile crearne un join utilizzando **And** oppure **Or**. Per altre informazioni sulle opzioni disponibili in questa finestra di dialogo, vedere [Finestra di dialogo Crea nuova condizione o Apri condizione, pagina Generale](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Finestra di dialogo Crea nuova condizione o Apri condizione, pagina Descrizione](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md)[Finestra di dialogo Modifica avanzata &#40;Condizione&#41;](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
9. Al termine, fare clic su **OK**.  
  
  
