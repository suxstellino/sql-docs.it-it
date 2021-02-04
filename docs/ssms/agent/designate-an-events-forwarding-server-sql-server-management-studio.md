---
description: Impostare un server di inoltro eventi
title: Impostare un server di inoltro eventi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- event forwarding servers [SQL Server]
- events [SQL Server], forwarding
- alerts [SQL Server], forwarded events
ms.assetid: 81dfcbe4-3000-4e77-99de-bf85fef63a12
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: ecb37153660b08e03170b2d0e8655eb6db20b42c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236371"
---
# <a name="designate-an-events-forwarding-server"></a>Impostare un server di inoltro eventi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Istanza gestita di SQL di Azure da SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come designare un server al quale verranno inoltrati eventi da [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si noti che l'inoltro di eventi si applica agli eventi inoltrati tra server, non agli eventi inoltrati tra istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ospitate in un singolo computer. Notare inoltre che per ricevere eventi inoltrati, il server di gestione degli avvisi deve essere un'istanza predefinita di SQL Server.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-designate-an-events-forwarding-server"></a>Per impostare un server di inoltro eventi  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server da cui si desidera inoltrare gli eventi a un altro server.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent** e scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà SQL Server Agent -**_nome_server_ fare clic su **Avanzate** in **Selezione pagina**.  
  
4.  In **Inoltro eventi SQL Server** selezionare la casella di controllo **Inoltra eventi a un altro server** .  
  
5.  Nell'elenco **Server** fare clic su un server e quindi in **Eventi** selezionare una delle opzioni seguenti:  
  
    -   Selezionare **Eventi non gestiti** per inoltrare solo gli eventi che non sono stati gestiti da avvisi locali.  
  
    -   Selezionare **Tutti gli eventi** per inoltrare tutti gli eventi, indipendentemente dal fatto che siano stati gestiti o meno da avvisi locali.  
  
6.  Nell'elenco **Eventi con gravità maggiore o uguale a** selezionare il livello di gravità che contraddistingue gli eventi da inoltrare al server selezionato.  
  
7.  Al termine, fare clic su **OK**.  
