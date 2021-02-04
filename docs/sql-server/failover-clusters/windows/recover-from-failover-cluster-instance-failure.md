---
title: Eseguire il ripristino da un errore dell'istanza del cluster di failover
description: Informazioni su come eseguire il ripristino in seguito a un failover di un'istanza del cluster di failover usando lo snap-in Gestione cluster di failover in SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], recovery from failure
- failover clustering [SQL Server], recovery from failure
- hardware failures [SQL Server]
- recovering failover cluster failures [SQL Server]
ms.assetid: 3d151d0c-e841-4325-8606-c094de37d7d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 45b917246037b9c333ba07bece6f6630e0ae2464
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251232"
---
# <a name="recover-from-failover-cluster-instance-failure"></a>Eseguire il ripristino da un errore dell'istanza del cluster di failover
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritta la modalità di recupero dagli errori del cluster tramite lo snap-in Gestione cluster di failover quando si verifica il failover in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Lo snap-in Gestione cluster di failover è l'applicazione di gestione cluster per il servizio Windows Server Failover Clustering (WSFC).  
  
-   [Recuperare da un errore irreversibile](#Scenario1)  
  
-   [Recuperare da un errore software](#Scenario2)  
  
##  <a name="recover-from-an-irreparable-failure"></a><a name="Scenario1"></a> Recuperare da un errore irreversibile  
 Eseguire i passi di seguito riportati per recuperare da un errore irreversibile. Questo errore può essere provocato, ad esempio, dall'errore di un controller del disco o del sistema operativo. In questo caso il problema dipende da un errore hardware nel nodo 1 di un cluster a due nodi.  
  
1.  Dopo l'errore del Nodo 1, l'istanza FCI di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] genera un errore sul Nodo 2.  
  
2.  Rimuovere il Nodo 1 dall'istanza FCI. A tale scopo, aprire lo snap-in Gestione cluster di failover dal Nodo 2, fare clic con il pulsante destro del mouse sul Nodo 1, quindi fare clic su **Sposta azioni**, quindi scegliere **Rimuovi nodo**.  
  
3.  Verificare che il Nodo 1 sia stato rimosso dalla definizione del cluster.  
  
4.  Installare il nuovo componente hardware per sostituire quello danneggiato nel Nodo 1.  
  
5.  Tramite lo snap-in Gestione cluster di failover, aggiungere il Nodo 1 al cluster esistente. Per altre informazioni, vedere [Operazioni preliminari all'installazione del clustering di failover](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
6.  Assicurarsi che gli account amministratore siano gli stessi in tutti i nodi del cluster.  
  
7.  Eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per aggiungere il Nodo 1 all'istanza FCI. Per altre informazioni, vedere [Aggiungere o rimuovere nodi in un cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
##  <a name="recover-from-a-reparable-failure"></a><a name="Scenario2"></a> Recuperare da un errore reversibile  
 Eseguire i passi di seguito riportati per recuperare da un errore reversibile. In questo caso, l'errore dipende dalla mancata disponibilità o dall'interruzione del collegamento del Nodo 1, che tuttavia non è danneggiato. L'errore può essere causato da un errore del sistema operativo, da un errore hardware o da un errore dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
1.  Dopo l'errore del Nodo 1, l'istanza FCI genera un errore sul Nodo 2.  
  
2.  Risolvere il problema del Nodo 1.  
  
3.  Accertarsi che tutti i nodi siano online e il servizio WSFC sia funzionante.  
  
4.  Eseguire il failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel nodo recuperato.  
  
  
