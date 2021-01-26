---
title: Configurare un server per l'attesa su una pipe alternativa | Microsoft Docs
description: Informazioni su come configurare la named pipe su cui è in ascolto il motore di database di SQL Server. Scoprire come connettere un'applicazione client a una named pipe specifica.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Named Pipes [SQL Server], configuring
- listening [SQL Server], pipes
- pipes [SQL Server], alternate
- alternate pipes [SQL Server]
ms.assetid: 914f7491-e2be-4b0d-b3aa-fe5409cdbafa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70abda90ef4b7c047a4833baa792adeff60b8f12
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783226"
---
# <a name="configure-a-server-to-listen-on-an-alternate-pipe"></a>Configurare un server per l'attesa su una pipe alternativa
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come configurare un server per l'ascolto su una pipe alternativa in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando Gestione configurazione SQL Server. Per impostazione predefinita, l'istanza predefinita di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] è in attesa sulla named pipe \\\\.\pipe\sql\query. Le istanze denominate del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e [!INCLUDE[ssEW](../../includes/ssew-md.md)] sono in attesa su altre pipe.  
  
 È possibile connettersi a una named pipe specifica mediante un'applicazione client in tre modi differenti:  
  
-   Avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sul server.  
  
-   Creare un alias nel client, specificando la named pipe.  
  
-   Programmare il client affinché si connetta utilizzando una stringa di connessione personalizzata.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="to-configure-the-named-pipe-used-by-the-sql-server-database-engine"></a>Per configurare l'utilizzo della named pipe mediante il Motore di database di SQL Server  
  
1.  Nel riquadro della console di Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server** e quindi espandere **Protocolli per**  *\<instance name>* .  
  
2.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **Named pipe**, quindi scegliere **Proprietà**.  
  
3.  Nel riquadro **Nome pipe** della scheda **Protocollo** , specificare la pipe su cui si desidera che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] sia in attesa e quindi fare clic su **OK**.  
  
4.  Nel riquadro della console fare clic su **Servizi di SQL Server**.  
  
5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **SQL Server (** \<instance name> **)** e quindi scegliere **Riavvia** per arrestare e riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa su una pipe alternativa, è possibile connettersi a una named pipe specifica mediante un'applicazione client in tre modi differenti:  
  
-   Avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sul server.  
  
-   Creare un alias nel client, specificando la named pipe.  
  
-   Programmare il client affinché si connetta utilizzando una stringa di connessione personalizzata.  
  
## <a name="see-also"></a>Vedere anche  
 [Creare o eliminare un alias server per l'uso da parte di un client &#40;Gestione configurazione SQL Server Manager&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md)   
 [Configurazione di rete del server](../../database-engine/configure-windows/server-network-configuration.md)  
  
  
