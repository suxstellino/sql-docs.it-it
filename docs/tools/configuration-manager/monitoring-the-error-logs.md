---
title: Monitoraggio dei log degli errori
description: Usare il log degli errori di SQL Server, il registro applicazioni di Windows e il visualizzatore file di log SQL Server Management Studio per risolvere i problemi relativi a SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server]
- database performance [SQL Server], errors
- Windows application logs [SQL Server]
- monitoring performance [SQL Server], errors
- server performance [SQL Server], errors
- comparing error and application log output
- errors [SQL Server], logs
- tuning databases [SQL Server], errors
- database monitoring [SQL Server], errors
- SQL Server error log
- logs [SQL Server], SQL Server error logs
- error logs [SQL Server]
- logs [SQL Server], Windows application logs
ms.assetid: e250336b-0695-44f6-a42f-23222f94e377
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 730d2761f2331c30277b79b3ddb3be0d45e4c3fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349790"
---
# <a name="monitoring-the-error-logs"></a>Monitoraggio dei log degli errori
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alcuni eventi di sistema e definiti dall'utente vengono registrati nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e nel registro applicazioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Sia nel registro errori che nel registro applicazioni viene applicato automaticamente un timestamp a tutti gli eventi registrati. Utilizzare le informazioni contenute nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la risoluzione di problemi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Il registro applicazioni di Windows offre un quadro generale degli eventi generati nel sistema operativo Windows, nonché degli eventi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Tramite il Visualizzatore eventi di Windows è possibile visualizzare il contenuto del registro applicazioni di Windows e filtrare le informazioni. È possibile, ad esempio, filtrare eventi informativi, di avviso, di errore, di controllo con esito positivo e di controllo con esito negativo.  
  
## <a name="comparing-error-and-application-log-output"></a>confronto tra output del log degli errori e output del registro applicazioni  
 Per identificare la causa di eventuali problemi, è possibile utilizzare sia il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che il registro applicazioni di Windows. Ad esempio, durante il monitoraggio del log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero venire visualizzati messaggi di errore privi di informazioni sulla causa dell'errore. Confrontando la data e l'ora degli eventi riportate nel registro errori e la data e l'ora riportate nel registro applicazioni è possibile circoscrivere le possibili cause. Nel Visualizzatore file di log di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è possibile integrare in un unico elenco il log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e il registro applicazioni di Windows. Ciò consente di capire più facilmente gli eventi del server e gli eventi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correlati. Per ulteriori informazioni, vedere l'argomento "Visualizzatore file di log" nella documentazione online di SQL Server.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Visualizzazione del log degli errori di SQL Server](../../tools/configuration-manager/viewing-the-sql-server-error-log.md)|Contiene informazioni sul log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e descrive come visualizzarlo.|  
|[Visualizzazione del registro applicazioni di Windows](../../tools/configuration-manager/viewing-the-windows-application-log.md)|Contiene informazioni sul registro applicazioni di Windows e descrive come visualizzarlo.|  
  
  
