---
title: Modificare una definizione dei criteri di integrità delle risorse (Utilità SQL Server) | Microsoft Docs
description: Informazioni su come usare SQL Server Management Studio per modificare una definizione dei criteri di integrità delle risorse in modo da poter valutare meglio i dati sulle prestazioni di SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.SWB.UE.UTILITY.ADMINISTRATION.F1
ms.assetid: 27bec0b6-92e9-448e-8c70-fe36802cf128
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: cef58a5cd2587c4845fe2f4a0c090abb8970877a
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049092"
---
# <a name="modify-a-resource-health-policy-definition-sql-server-utility"></a>Modifica di una definizione dei criteri di integrità delle risorse (Utilità SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come modificare una definizione di criteri di integrità di una risorsa in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Prima di modificare un criterio di utilizzo delle risorse in Utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario creare un punto di controllo dell'utilità. Per altre informazioni, vedere [Attività e funzionalità di Utilità SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] È possibile configurare i criteri di utilizzo delle risorse di Utilità per applicazioni livello dati e istanze gestite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I criteri di utilizzo delle risorse possono essere definiti a livello globale per tutte le applicazioni livello dati e le istanze gestite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure possono essere definiti singolarmente per ogni applicazione livello dati e per ogni istanza gestita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È inoltre possibile implementare criteri globali e configurare le singole applicazioni livello dati o istanze gestite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con definizioni dei criteri specifiche.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="modify-global-resource-utilization-policies-in-a-sql-server-utility"></a>Modificare i criteri di utilizzo delle risorse globali in Utilità SQL Server.  
  
1.  Connettersi al punto di controllo dell'utilità in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Nel riquadro di navigazione di Esplora utilità, fare clic su **Amministrazione utilità** per visualizzare o modificare i criteri di monitoraggio globali, quindi fare clic sulla scheda **Criteri** nel riquadro del contenuto di Esplora utilità.  
  
3.  Nel riquadro del contenuto di Esplora utilità selezionare **Set global data-tier monitoring policies** (Imposta criteri globali di monitoraggio livello dati) o **Set global managed instance monitoring policies** (Imposta criteri globali di monitoraggio istanza gestita) facendo clic sulla freccia o sulla descrizione dei criteri.  
  
4.  Utilizzare i controlli a destra delle descrizioni dei criteri per impostare le soglie di sottoutilizzo o di sovrautilizzo.  
  
5.  Usare il pulsante **Applica**, **Ignora** o **Ripristina impostazioni predefinite** in base alle esigenze. Possono essere necessari fino a 15 minuti per la propagazione della modifica dei criteri nel dashboard di Utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e nei dettagli della visualizzazione elenco.  
  
6.  Per aggiornare i dati, fare clic con il pulsante destro del mouse sul nodo **Amministrazione utilità** nel riquadro di spostamento di Esplora utilità e scegliere **Aggiorna**.  
  
#### <a name="modify-resource-health-policy-definitions-for-an-individual-data-tier-application-or-an-individual-managed-instance-of-sql-server-in-a-sql-server-utility"></a>Modificare le definizioni dei criteri di integrità delle risorse per una singola applicazione del livello dati o istanza gestita di SQL Server in Utilità SQL Server  
  
1.  Connettersi al punto di controllo dell'utilità in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Nel riquadro di spostamento di Esplora utilità fare clic su **Deployed Date-tier Applications**(Applicazioni livello dati distribuite) o su **Istanze gestite** per visualizzare o modificare i criteri di monitoraggio per una singola applicazione livello dati o istanza gestita.  
  
3.  Nella visualizzazione elenco del riquadro del contenuto di Esplora utilità fare clic sull'applicazione livello dati o sul nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i cui criteri si vuole modificare, quindi fare clic sulla scheda **Dettagli criteri** .  
  
4.  Selezionare i criteri da visualizzare o modificare facendo clic sulla freccia o sulla descrizione dei criteri. I criteri globali sono selezionati per impostazione predefinita.  
  
5.  Selezionare il pulsante di opzione **Sostituisci criteri globali** per ignorare i criteri globali e implementare una definizione dei criteri per l'applicazione livello dati specificata.  
  
6.  Utilizzare i controlli a destra della descrizione dei criteri per impostare le soglie di sottoutilizzo o di sovrautilizzo.  
  
7.  Usare il pulsante **Applica**, **Ignora** o **Ripristina impostazioni predefinite** in base alle esigenze. Possono essere necessari fino a 15 minuti per la propagazione della modifica dei criteri nel dashboard di Utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e nei dettagli della visualizzazione elenco.  
  
8.  Per aggiornare i dati, fare clic con il pulsante destro del mouse sul nodo **Deployed Data-tier Applications** (Applicazioni livello dati distribuite) nel pannello di spostamento di Esplora utilità e scegliere **Aggiorna**.  
  
## <a name="see-also"></a>Vedere anche  
 [Attività e funzionalità di Utilità SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Visualizzare i risultati dei criteri di integrità delle risorse &#40;SQL Server Utility&#41;](../../relational-databases/manage/view-resource-health-policy-results-sql-server-utility.md)  
  
  
