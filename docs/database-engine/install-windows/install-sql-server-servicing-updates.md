---
title: Installare gli aggiornamenti di manutenzione di SQL Server | Microsoft Docs
description: Questo articolo offre informazioni sull'installazione di aggiornamenti di SQL Server durante una nuova installazione o dopo l'installazione di SQL Server.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 7d6c962b-c8d0-49f7-a2ac-00ad8dca930a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6c9ff0ec46dd63f50bdacf0e3feec5df1fbd90f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348377"
---
# <a name="install-sql-server-servicing-updates"></a>Installare gli aggiornamenti di manutenzione per SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Questo articolo offre informazioni sull'installazione degli aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]. In questa sezione vengono fornite informazioni sui seguenti argomenti:
  
- Installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] durante una nuova installazione  
  
- Installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] dopo che è stato installato.  
  
Installare tempestivamente gli ultimi aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per assicurarsi che i sistemi abbiano gli aggiornamenti di sicurezza più recenti.  
  
## <a name="installing-updates-for-noversion-during-a-new-installation"></a>Installazione di aggiornamenti per [!INCLUDE[noVersion](../../includes/ssNoVersion-md.md)] durante una nuova installazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione integra gli aggiornamenti più recenti del pacchetto con l'installazione del prodotto principale in modo che quest'ultimo e i relativi aggiornamenti applicabili vengano installati contemporaneamente. Aggiornamento prodotto consente la ricerca degli aggiornamenti applicabili da:  
  
- [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update  
  
- Windows Server Update Services (WSUS)  
  
- Cartella locale  
  
- Condivisione di rete  
  
Dopo avere individuato le versioni più recenti degli aggiornamenti applicabili, questi vengono scaricati e integrati dal programma di installazione con il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrente. Tramite l'aggiornamento del prodotto è possibile includere un aggiornamento cumulativo, un Service Pack o un Service Pack con aggiornamento cumulativo.  
  
## <a name="installing-updates-for-ssnoversion-after-it-has-already-been-installed"></a>Installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] dopo che è stato installato  
In un'istanza installata di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]si consiglia di applicare gli aggiornamenti della sicurezza e gli aggiornamenti critici più recenti, inclusi General Distribution Release (GDR), Service Pack e aggiornamenti cumulativi. Per altre informazioni, vedere l'[annuncio di marzo 2016 relativo al modello di manutenzione incrementale di SQL Server](/archive/blogs/sqlreleaseservices/announcing-updates-to-the-sql-server-incremental-servicing-model-ism).

> [!NOTE]
> A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], viene adottato un ciclo di vita della manutenzione Mainstream stimabile e semplificato, pertanto i Service Pack non saranno più disponibili. Saranno disponibili solo gli aggiornamenti cumulativi e le GDR (General Distribution Release) quando necessario.
> Per altre informazioni, vedere l'[annuncio di settembre 2017 relativo al modello di manutenzione moderna per SQL Server](/archive/blogs/sqlreleaseservices/announcing-the-modern-servicing-model-for-sql-server).
  
Gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update (MU), Windows Server Update Services (WSUS) e Area download Microsoft. Sicurezza e aggiornamenti Critici per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update. Per essere in grado di vedere questi aggiornamenti è necessario a scegliere MU tramite Windows Update nel Pannello Controllo.  
  
Quando si riceve un aggiornamento tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update, verrà effettuato l'aggiornamento in modalità automatica di tutte le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alla versione più recente. Per maggiore flessibilità o se non si ha accesso a Internet o a WSUS, è necessario ottenere gli aggiornamenti dall'Area download [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
[Installare SQL Server dall'Installazione guidata &#40;Installazione&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)        
[Add Features to an Instance of SQL Server &#40;Setup&#41;](./add-features-to-an-instance-of-sql-server-setup.md)        (Aggiungere funzionalità a un'istanza di SQL Server (programma di installazione))  
[Ripristinare un'installazione non riuscita di SQL Server](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)