---
title: Verificare l'installazione e l'avvio del motore di database | Microsoft Docs
description: Informazioni su come determinare se il motore di database è installato e avviato. Scoprire come usare Gestione configurazione SQL Server per verificare la presenza di componenti installati.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server, determining if installed
- verifying Database Engine installation
- viewing Database Engine installation
- installed Database Engine verification [SQL Server]
ms.assetid: babb02e4-3521-4b75-b5df-e09205594375
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03aa96f16ee200846dae726851c51a1dc1d04e80
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783004"
---
# <a name="determine-whether-the-database-engine-is-installed-and-started"></a>Verificare l'installazione e l'avvio del Motore di database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Un'installazione corretta del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] installa i file nel file system, crea voci nel Registro di sistema e installa numerosi strumenti. In questo argomento viene descritto come determinare se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene installato e avviato in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="how-to-view-and-start-the-database-engine-by-using-sql-server-configuration-manager"></a>Visualizzazione e avvio di Motore di database utilizzando Gestione configurazione SQL Server  
  
1.  Fare clic su **Start**, scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti di configurazione**, quindi **Gestione configurazione SQL Server**.  
  
     Se nel menu **Start** non sono presenti gli elementi indicati, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è installato correttamente. Eseguire il programma di installazione per installare [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
2.  Nel riquadro sinistro di **Gestione configurazione SQL Server** fare clic su **Servizi di SQL Server**. Nel riquadro destro sono elencati numerosi servizi correlati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è installato, il servizio [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene elencato come **SQL Server (MSSQLSERVER)** se si tratta dell'istanza predefinita, oppure come **SQL Server (** \<*instance_name*> **)** , se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è installato come istanza denominata. Salvo il caso in cui il nome dell'istanza sia stato modificato, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene installato come istanza denominata con il nome **SQLEXPRESS**. Un'icona a forma di triangolo di colore verde indica che [!INCLUDE[ssDE](../../includes/ssde-md.md)] è in esecuzione. Un'icona a forma di quadrato di colore rosso indica che [!INCLUDE[ssDE](../../includes/ssde-md.md)] è stato arrestato.  
  
3.  Per avviare [!INCLUDE[ssDE](../../includes/ssde-md.md)], fare clic con il pulsante destro del mouse su [!INCLUDE[ssDE](../../includes/ssde-md.md)]nel riquadro destro e quindi scegliere **Avvia**.  
  
> [!NOTE]  
>  Durante l'installazione, l'utente può selezionare un percorso in cui installare i file di programma e di database. Se l'utente accetta la posizione predefinita, i file vengono installati in [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)] e in C:\Programmi\Microsoft SQL Server\MSSQL.*x*, dove *x* è un numero.  
  
  
