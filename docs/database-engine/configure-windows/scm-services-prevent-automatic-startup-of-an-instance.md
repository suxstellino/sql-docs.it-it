---
title: Impedire l'avvio automatico di un'istanza di SQL Server (Gestione configurazione SQL Server) | Microsoft Docs
description: Informazioni su come impedire che un'istanza di SQL Server venga avviata automaticamente. Per eseguire questa attività, vedere come impostare la modalità di avvio su manuale.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- automatic SQL Server startup
- SQL Server, stopping
- SQL Server, automatic startup
- canceling automatic startup
- stopping SQL Server
- preventing automatic startups [SQL Server]
ms.assetid: 782663cf-f3d7-4cc6-b621-21e4550f0322
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b103099bf785b3fb8ed44c553db6b2fad5f60b2f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236573"
---
# <a name="scm-services---prevent-automatic-startup-of-an-instance"></a>Impedire l'avvio automatico di un'istanza di SQL Server (Gestione configurazione SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come impedire che un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] venga avviata automaticamente in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando Gestione configurazione SQL Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è normalmente configurato per l'avvio automatico. È possibile modificare questa configurazione impostando su manuale la modalità di avvio per l'istanza.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="to-prevent-automatic-startup-of-an-instance-of-sql-server"></a>Per impedire l'avvio automatico di un'istanza di SQL Server  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione** e quindi **Gestione configurazione SQL Server**.  
  
    > [!NOTE]  
    >  Poiché Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è uno snap-in per il programma [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console e non un programma autonomo, Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene visualizzato come applicazione nelle versioni più recenti di Windows.  
    >   
    >  -   **Windows 10**:  
    >          per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nella **pagina iniziale** digitare SQLServerManager13.msc (per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]). Per le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sostituire 13 con un numero inferiore. Se si fa clic su SQLServerManager13.msc, viene aperto Gestione configurazione. Per aggiungere Gestione configurazione alla pagina iniziale o alla barra delle applicazioni, fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Apri percorso file**. In Esplora file di Windows fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Aggiungi a Start** o **Aggiungi alla barra delle applicazioni**.  
    > -   **Windows 8**:  
    >          Per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nell'icona promemoria **Cerca** in **App** digitare **SQLServerManager\<version>.msc**, ad esempio **SQLServerManager13.msc**, quindi premere **INVIO**.  
  
2.  In Gestione configurazione SQL Server espandere **Servizi** e quindi fare clic su **SQL Server**.  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **MSSQLServer** quindi scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **SQL Server \<**_instancename_**> Proprietà**, nella scheda **Servizio**, nella casella **Generale** impostare il valore di **Modalità di avvio** su **Manuale**.  
  
5.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà - SQL Server \<**_instancename_**>** e quindi chiudere Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
