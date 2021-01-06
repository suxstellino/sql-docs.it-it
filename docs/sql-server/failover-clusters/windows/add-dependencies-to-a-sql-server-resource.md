---
description: Aggiunta di dipendenze a una risorsa di SQL Server
title: Aggiungere dipendenze a una risorsa dell'istanza del cluster di failover di SQL Server
descriptoin: Describes how to add dependencies to an Always On failover cluster instance (FCI) resource using the Failover Cluster Manager.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- resource dependencies [SQL Server]
- failover clustering [SQL Server], dependencies
- clusters [SQL Server], dependencies
- dependencies [SQL Server], clustering
ms.assetid: 25dbb751-139b-4c8e-ac62-3ec23110611f
author: cawrites
ms.author: chadam
ms.openlocfilehash: f9582167d337ed9a06f95030f4c68857a4abd389
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642850"
---
# <a name="add-dependencies-to-a-sql-server-resource"></a>Aggiunta di dipendenze a una risorsa di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento descrive come aggiungere dipendenze a una risorsa dell'istanza del cluster di failover Always On tramite lo snap-in Gestione cluster di failover. Lo snap-in Gestione cluster di failover è l'applicazione di gestione cluster per il servizio Windows Server Failover Clustering (WSFC).  
  
-   **Prima di iniziare:**  [Limitazioni e restrizioni](#Restrictions), [Prerequisiti](#Prerequisites)  
  
-   **Per aggiungere una dipendenza a una risorsa SQL Server utilizzando:** [Gestore cluster di failover di Windows](#WinClusManager)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 È importante notare che se si aggiungono altre risorse al gruppo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , tali risorse devono sempre comprendere le rispettive risorse del nome di rete SQL univoche e quelle dell'indirizzo IP SQL.  
  
 Utilizzare le risorse del nome di rete SQL e le risorse dell'indirizzo IP SQL esistenti esclusivamente per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se le risorse di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono condivise con altre risorse, potrebbero verificarsi i problemi seguenti:  
  
-   Interruzioni non previste.  
  
-   Installazioni di service pack non riuscite.  
  
-   Errori del programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Se si verifica questo problema, non è possibile installare istanze aggiuntive di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o eseguire la manutenzione consueta.  
  
 Si tenga presente che possono verificarsi anche i problemi seguenti:  
  
-   FTP con replica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Nel caso di istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che utilizzano FTP con la replica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , è necessario che il servizio FTP utilizzi uno dei dischi fisici utilizzati dall'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurata per l'utilizzo del servizio FTP.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Se si aggiunge una risorsa a un gruppo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ed è attiva una dipendenza dalla risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per garantire la disponibilità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , [!INCLUDE[msCoName](../../../includes/msconame-md.md)] consiglia di aggiungere una dipendenza dalla risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent. Non aggiungere alcuna dipendenza dalla risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per garantire che la disponibilità del computer in cui viene eseguito [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] rimanga elevata, configurare la risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent in modo che non influisca sul gruppo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in caso di errore della risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent.  
  
-   Condivisioni di file e risorse di stampa. Quando si installano risorse di tipo condivisione file o cluster di stampa, è consigliabile non salvarle nelle stesse risorse di disco fisico del computer in cui viene eseguito [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], perché potrebbero verificarsi cali delle prestazioni e perdita di servizio nel computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Considerazioni relative a MS DTC: dopo l'installazione del sistema operativo e la configurazione dell'istanza del cluster di failover, è necessario configurare [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) per il funzionamento in un cluster utilizzando lo snap-in Gestione cluster di failover. Se non si configura MS DTC per il clustering, l'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non verrà bloccata, tuttavia una configurazione non corretta di MS DTC può influire sulla funzionalità dell'applicazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
     Se si installa MS DTC nel gruppo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e sono disponibili altre risorse dipendenti da MS DTC, quest'ultimo non sarà disponibile se tale gruppo è offline o si verifica un failover. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] consiglia di inserire MS DTC nel proprio gruppo con la propria risorsa disco fisica, se possibile.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 Se si installa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un gruppo di risorse WSFC con più unità disco e si sceglie di salvare i dati in una delle unità, la risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verrà impostata in modo da essere dipendente solo da tale unità. Per salvare dati o i log in un altro disco, è necessario prima aggiungere una dipendenza alla risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per il disco aggiuntivo.  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WinClusManager"></a> Utilizzo dello snap-in Gestione cluster di failover  
 **Per aggiungere dipendenze a una risorsa di SQL Server**  
  
-   Aprire lo snap-in Gestione cluster di failover.  
  
-   Individuare il gruppo contenente la risorsa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] appropriata da rendere dipendente.  
  
-   Se la risorsa per il disco è già inclusa nel gruppo, andare al passaggio 4. In caso contrario, individuare il gruppo che contiene il disco. Se tale gruppo e il gruppo contenente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non appartengono allo stesso nodo, spostare il gruppo contenente la risorsa per il disco nel nodo proprietario del gruppo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Selezionare la risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , aprire la finestra di dialogo **Proprietà** e utilizzare la scheda **Dipendenze** per aggiungere il disco al set di dipendenze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
  
