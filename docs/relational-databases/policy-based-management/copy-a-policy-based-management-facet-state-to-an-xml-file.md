---
title: Copiare lo stato di un facet della gestione basata su criteri in un file XML
description: Descrive come copiare lo stato di un facet della gestione basata su criteri in un file XML usando SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, copy facet state to XML file
ms.assetid: 7d604ab1-6dd6-4f8e-a79c-eba99ab106fd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1fc174835560ec7d96ffac265406449e4503163
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048802"
---
# <a name="copy-a-policy-based-management-facet-state-to-an-xml-file"></a>Copiare lo stato di un facet della gestione basata su criteri in un file XML
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento verrà descritto come copiare lo stato di un facet della gestione basata su criteri in un file XML in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per copiare lo stato di un facet in un file XML tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le procedure descritte in questo argomento richiedono l'appartenenza al ruolo PolicyAdministratorRole nel database msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-copy-a-facet-state-to-an-xml-file"></a>Per copiare lo stato di un facet in un file XML  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], su un oggetto istanza, su un database o su un oggetto di database, quindi scegliere **Facet**.  
  
2.  Nella finestra di dialogo **Visualizza facet -** _nome_oggetto_ fare clic su **Esporta stato corrente come criteri**.  
  
3.  Nella finestra di dialogo **Esporta come criterio** digitare il percorso e il nome del file oppure usare il pulsante Sfoglia ( **...** ) per trovare il file, quindi digitare il nome del file XML. Per ulteriori informazioni sulle opzioni disponibili in questa finestra di dialogo, vedere [Export As Policy Dialog Box](../../relational-databases/policy-based-management/export-as-policy-dialog-box.md).  
  
4.  Al termine, fare clic su **OK**.  
  
  
