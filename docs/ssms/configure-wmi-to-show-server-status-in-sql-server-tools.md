---
description: Configurazione di WMI per mostrare lo stato del server in Strumenti SQL Server
title: Configurazione di WMI per mostrare lo stato del server in Strumenti SQL Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI Provider for Server Events, setting permissions
- WMI permissions [SQL Server]
ms.assetid: 7e97197b-ed4d-40d1-9a52-9ab1d92401d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 307211456c8b82aec5bad63d6b1d0023b9de6ae3
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251258"
---
# <a name="configure-wmi-to-show-server-status-in-sql-server-tools"></a>Configurazione di WMI per mostrare lo stato del server in Strumenti SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In questo argomento viene descritto come configurare WMI per mostrare lo stato del server negli strumenti di SQL Server in [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. Durante la connessione ai server i componenti Server registrati e Esplora oggetti di [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], nonché Gestione configurazione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , utilizzano Strumentazione gestione Windows (WMI) per ottenere lo stato dei servizi [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (MSSQLSERVER) e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent (MSSQLSERVER). Per visualizzare lo stato del servizio, l'utente deve disporre dei diritti per accedere in remoto all'oggetto WMI. Per configurare questa autorizzazione, nel server deve essere installato WMI.  
  
## <a name="to-configure-wmi-permission"></a><a name="SSMSProcedure"></a>Per configurare l'autorizzazione WMI  
  
1.  Scegliere **Esegui** dal menu **Start** nel server remoto.  
  
2.  Nella casella **Apri** digitare **wmimgmt.msc** e fare clic su **OK**.  
  
3.  Nel programma **Windows Management Infrastructure** fare clic con il pulsante destro del mouse su **Controllo WMI (Locale)** e scegliere **Proprietà**.  
  
4.  Nella scheda **Sicurezza** della finestra di dialogo **Proprietà - Controllo WMI (Locale)** espandere **Radice** e fare clic su **CIMV2**.  
  
5.  Fare clic sul pulsante **Sicurezza** per aprire la finestra di dialogo **Sicurezza per ROOT\CIMV2** .  
  
6.  Aggiungere un gruppo o un utente alla casella **Nomi utente o gruppo** e selezionarlo.  
  
7.  Nella casella **Autorizzazioni per** _<group or user>_ , selezionare la colonna **Consenti** per l'autorizzazione **Abilita remoto** relativa agli utenti per i quali si vuole rilevare in remoto lo stato del servizio.  
  
## <a name="see-also"></a>Vedere anche  
[Avvio, arresto o sospensione del servizio SQL Server Agent](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
