---
title: Avviare e arrestare il servizio del server di report | Microsoft Docs
description: Informazioni su come avviare e arrestare il servizio Windows che contiene il servizio Web ReportServer, il portale Web e un'applicazione di elaborazione in background.
ms.date: 03/22/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- stopping Report Server service
- Report Server Windows service, starting
- Report Server service, starting
- starting Report Server service
ms.assetid: 6ec69ac3-27b0-472d-91e1-733af9078ed2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d56cf652fab81403c50a0de122c1b5ad004de1a8
ms.sourcegitcommit: ab0c654d924eeb5647e47444abb59d934345b205
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450142"
---
# <a name="start-and-stop-the-report-server-service"></a>Avviare e arrestare il servizio del server di report

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Un server di report viene implementato come un servizio Windows che contiene il servizio Web ReportServer, il portale Web e un'applicazione di elaborazione in background. Se si desidera utilizzare qualsiasi funzionalità del server di report, è necessario che il servizio sia in esecuzione. L'arresto del servizio determina l'interruzione di tutte le operazioni eseguite sul server di report.  
  
 Mentre il servizio è arrestato, le richieste di elaborazione di report e sottoscrizioni pianificate che sarebbero state eseguite in caso di attività del servizio vengono aggiunte alla coda. Questa situazione si verifica poiché i processi eseguiti da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent creano gli eventi. Se si desidera evitare un backlog di operazioni mentre il servizio è disattivato, arrestare anche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 Per avviare e arrestare il servizio del server di report, è possibile utilizzare diversi strumenti, ad esempio lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e lo strumento Servizi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager è un'opzione solo per SQL Server Reporting Services 2016 e versioni precedenti. Non include Reporting Services 2017 e versioni successive o Server di report di Power BI.
  
 Se, oltre ad avviare o arrestare il servizio, si modifica ad esempio l'account del servizio, è necessario usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. L'utilizzo di altri strumenti per modificare l'account del servizio può interrompere l' [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installazione e la chiave di crittografia. Per altre informazioni, vedere [Configurare l'account del servizio del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
 Non è possibile sospendere e riprendere l'esecuzione del servizio. Non sono disponibili parametri di avvio. Sebbene non esistano dipendenze esplicite, se il server di report deve supportare sottoscrizioni o operazioni su report pianificate, è necessario che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia in esecuzione.  
  
## <a name="use-the-reporting-services-configuration-tool"></a>Usare lo strumento di configurazione di Reporting Services  
  
1. Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi al server di report.  
  
2. Nella pagina Stato Server report selezionare **Arresta** o **Avvia**.  
  
## <a name="use-administrative-tools"></a>Usare Strumenti di amministrazione  

1. In strumenti di amministrazione aprire **Servizi**.
2. Fare clic con il pulsante destro del mouse su **SQL Server Reporting Services (MSSQLSERVER)**, **SQL Server Reporting Services** o **server di report di Power bi**.
3. Selezionare **Arresta** o **Riavvia**.

  
Per Reporting Services 2016 e versioni precedenti, se si eseguono più istanze o se il server di report è in esecuzione come istanza denominata, verificare che il nome dell'istanza tra parentesi corrisponda all'istanza del server di report che si desidera arrestare o riavviare.  

  
## <a name="see-also"></a>Vedere anche  
 [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Avvio, arresto o sospensione del servizio SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
