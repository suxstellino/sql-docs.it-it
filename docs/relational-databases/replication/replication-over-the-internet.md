---
description: Replica su Internet
title: Replica su Internet | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Web publishing [SQL Server replication], about Web publishing
- Web publishing [SQL Server replication]
- Internet [SQL Server replication]
- Internet [SQL Server replication], publishing
ms.assetid: 04e7f4ed-e244-4bbe-ba12-09c33abea09e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c4205931b643f0faf76a81cb13e2a08199971b98
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468802"
---
# <a name="replication-over-the-internet"></a>Replica su Internet
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La replica dei dati su Internet consente a utenti remoti e non connessi di accedere ai dati nel momento desiderato tramite una connessione a Internet. È possibile replicare i dati su Internet utilizzando:  
  
-   Una rete privata virtuale (VPN). Per altre informazioni, vedere [Pubblicare i dati su Internet tramite VPN](../../relational-databases/replication/publish-data-over-the-internet-using-vpn.md).  
  
-   L'opzione di sincronizzazione Web per la replica di tipo merge. Per altre informazioni, vedere [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 Tutti i tipi di replica di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono replicare i dati in una VPN, ma è consigliabile considerare la sincronizzazione Web se si usa la replica di tipo merge.  
  
  
