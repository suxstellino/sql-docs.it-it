---
title: Pianificazione di un'installazione di SQL Server | Microsoft Docs
description: Questo articolo consente di pianificare l'installazione di SQL Server. Include i collegamenti alle risorse necessarie per l'installazione di SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: quickstart
helpviewer_keywords:
- installing SQL Server, planning
ms.assetid: b1d56f2f-603f-48f2-b902-c715f14a6db9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 948855113c0ae31484c2323e8ab482307ecf8f3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336134"
---
# <a name="planning-a-sql-server-installation"></a>Pianificazione di un'installazione di SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], effettuare le operazioni seguenti:  
  
-   Esaminare i requisiti di installazione, i controlli di configurazione del sistema e le considerazioni sulla sicurezza per un'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per installare o eseguire l'aggiornamento a una versione successiva. Prima dell'aggiornamento, vedere [Aggiornare SQL Server](../../database-engine/install-windows/upgrade-sql-server.md).  
  
-   Utilizzare le utilità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Indipendentemente dal metodo di installazione, è necessario confermare l'accettazione delle condizioni di licenza del software come utente singolo o per conto di un'entità, a meno che l'utilizzo del software non sia disciplinato da un contratto separato, ad esempio un contratto multilicenza [!INCLUDE[msCoName](../../includes/msconame-md.md)] o un contratto di terze parti con un fornitore di software indipendente o un OEM.  
  
 Le condizioni di licenza vengono visualizzate per la revisione e l'accettazione nell'interfaccia utente del programma di installazione. Le installazioni automatiche, con i parametri `/Q` o `/QS`, devono includere il parametro `/IAcceptSQLServerLicenseTerms`. Scaricare e leggere separatamente le condizioni di licenza disponibili in [Condizioni e informazioni sulle licenze per Microsoft SQL Server](https://www.microsoft.com/Licensing/product-licensing/sql-server.aspx). Per le condizioni relative ai contratti multilicenza, vedere [Condizioni di licenza e documentazione](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=53). Per le versioni precedenti di SQL Server, vedere [Condizioni di Licenza per il software Microsoft](https://go.microsoft.com/fwlink/?LinkID=148209).  
  
> [!NOTE]  
>  A seconda della modalità di ricezione del software, ad esempio attraverso un contratto multilicenza [!INCLUDE[msCoName](../../includes/msconame-md.md)] , l'utilizzo del software potrebbe essere soggetto a condizioni aggiuntive.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Novità relative all'installazione di SQL Server](../../sql-server/install/what-s-new-in-sql-server-installation.md)  
 In questo articolo vengono descritti i dettagli sulle funzionalità di installazione nuove o migliorate disponibili in questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Requisiti hardware e software per l'installazione di [SQL Server 2016 e 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) o [SQL Server in Linux](../../linux/sql-server-linux-setup.md). Questo articolo elenca i requisiti hardware e software minimi per l'installazione e l'esecuzione di un'istanza di SQL Server. .  
  
 [Considerazioni sulla sicurezza per un'installazione di SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 Questo articolo illustra alcune procedure consigliate per la sicurezza da prendere in considerazione prima dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
 Questo articolo illustra la configurazione predefinita dei servizi disponibili in questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e le opzioni di configurazione per i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che è possibile impostare durante e dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Protocolli e librerie di rete](../../sql-server/install/network-protocols-and-network-libraries.md)  
 Questo articolo illustra la configurazione predefinita dei protocolli di rete in questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e le opzioni di configurazione disponibili.  
  
 [Usare più versioni e istanze di SQL Server](../../sql-server/install/work-with-multiple-versions-and-instances-of-sql-server.md)  
 Questo articolo illustra le considerazioni relative all'installazione di più versioni e istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Versioni in lingua locale di SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md)  
 Questo articolo illustra le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-sections"></a>Sezioni correlate  
 [Installare SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 In questa sezione è contenuta una panoramica delle diverse opzioni disponibili per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Installare le funzionalità di business intelligence di SQL Server](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 In questa sezione della documentazione relativa al programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene illustrata la modalità di installazione delle funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che fanno parte della piattaforma di Business Intelligence di Microsoft.  
  
 [Eseguire l'aggiornamento di SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 La sezione fornisce una panoramica dell'aggiornamento da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [Disinstallare SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 Fare riferimento a questa sezione per disinstallare completamente un'istanza esistente di [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] e preparare il sistema in modo che sia possibile reinstallare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Installazione del cluster di failover di SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 In questa sezione della documentazione relativa al programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono descritte le modalità di installazione e configurazione del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [Installare SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [Soluzioni a disponibilità elevata &#40;SQL Server&#41;](../../database-engine/sql-server-business-continuity-dr.md)   
 [Operazioni preliminari all'installazione del clustering di failover](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Eseguire l'aggiornamento a SQL Server usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)