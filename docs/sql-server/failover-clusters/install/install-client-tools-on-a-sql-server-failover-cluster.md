---
title: 'Installare gli strumenti client: cluster di failover'
description: Informazioni su come installare strumenti client, come SQL Server Management Studio, in un'istanza di failover di SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3c82d510-9798-46be-bebb-cac9bef56936
author: cawrites
ms.author: chadam
ms.openlocfilehash: a596212e17e6fb273b9a6c4f5a2548aff1bc1e7f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642842"
---
# <a name="install-client-tools-on-a-sql-server-failover-cluster"></a>Installare strumenti client in un cluster di failover di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Gli strumenti client, ad esempio [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , sono funzionalità condivise comuni a tutte le istanze dello stesso computer. Sono compatibili con le versioni precedenti e consentono l'installazione affiancata delle versioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supportate. In un nodo può esistere solo una versione dello strumento client alla volta.  
  
 Se gli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono installati durante l'installazione del cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel primo nodo, questi verranno aggiunti automaticamente a tutti i nodi che verranno aggiunti in un secondo momento all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tramite la funzionalità per l'aggiunta del nodo.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La documentazione online non viene aggiunta automaticamente agli ulteriori nodi aggiunti al cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tramite la funzionalità per l'aggiunta del nodo. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] È possibile installare la documentazione online manualmente nei nodi che si vuole dispongano di una copia locale della documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Se gli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non vengono installati durante l'installazione iniziale del cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , sarà possibile installarli in un secondo momento, come descritto nelle procedure che seguono.  
  
## <a name="installation-procedures"></a>Procedure di installazione  
  
#### <a name="installing-ssnoversion-client-tools-using-the-setup-user-interface"></a>Installazione degli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tramite l'interfaccia utente del programma di installazione  
  
1.  Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Nella cartella di installazione radice fare doppio clic sul file Setup.exe. Per eseguire l'installazione dalla condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe.  
  
2.  Nella pagina **Installazione** fare clic su **Nuova installazione autonoma di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o aggiunta di funzionalità a un'installazione esistente**. Non fare clic su **Installazione nuovo cluster di failover di[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** .  
  
3.  Prima che l'installazione continui, Controllo configurazione sistema verificherà lo stato del sistema del computer.  
  
4.  Nella pagina **Tipo di installazione** fare clic su **Esegui una nuova installazione di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** .  
  
5.  Nella pagina **Selezione funzionalità** selezionare gli strumenti che si vuole installare e attenersi alle istruzioni della restante parte del processo di installazione.  
  
#### <a name="installing-ssnoversion-client-tools-at-the-command-prompt"></a>Installazione degli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dal prompt dei comandi  
  
1.  Per installare gli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e la documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], eseguire il comando seguente: Setup.exe/q/Action=Install /Features=Tools  
  
2.  Per installare solo gli strumenti di gestione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di base, eseguire il comando seguente: Setup.exe/q/Action=Install Features=SSMS. Verranno installati il supporto di [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] per [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)], [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)], l'utilità sqlcmd e il provider PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
3.  Per installare gli strumenti di gestione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] completi, eseguire il comando seguente: Setup.exe/q/Action=Install /Features=ADV_SSMS. Per altre informazioni sui valori dei parametri per le funzionalità, vedere [Installazione di SQL Server 2016 dal prompt dei comandi](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="uninstalling-ssnoversion-client-tools"></a>Disinstallazione degli strumenti client di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 Questi strumenti vengono visualizzati in Installazione applicazioni nel Pannello di controllo come **[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** e possono essere rimossi dalla stessa posizione. Se si utilizza l'opzione per la rimozione dei nodi per disinstallare un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dal cluster di failover, non verranno contemporaneamente disinstallati i componenti client.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
