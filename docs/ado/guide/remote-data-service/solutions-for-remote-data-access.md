---
description: Soluzioni per RDA (Remote Data Access)
title: Soluzioni per l'accesso ai dati remoti | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS [ADO]
ms.assetid: d311cc67-7db7-4c43-9590-d465564695e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 01516d04ce20fc1590ca41985078bcd47f47e47e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031773"
---
# <a name="solutions-for-remote-data-access"></a>Soluzioni per RDA (Remote Data Access)
## <a name="the-issue"></a>Il problema  
 ADO consente all'applicazione di accedere direttamente alle origini dati e di modificarle (talvolta denominate sistema a due livelli). Se, ad esempio, la connessione riguarda l'origine dati che contiene i dati, si tratta di una connessione diretta in un sistema a due livelli.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Tuttavia, potrebbe essere necessario accedere indirettamente alle origini dati tramite un intermediario, ad esempio Microsoft® Internet Information Services (IIS). Questa disposizione viene a volte definita sistema a tre livelli. IIS è un sistema client/server che offre un modo efficiente per un'applicazione locale, o client, per richiamare un programma remoto, o server, in Internet o in una rete Intranet. Il programma server consente di ottenere l'accesso all'origine dati e, facoltativamente, di elaborare i dati acquisiti.  
  
 La pagina Web Intranet, ad esempio, contiene un'applicazione scritta in Microsoft® Visual Basic Scripting Edition (VBScript), che si connette a IIS. IIS a sua volta si connette all'origine dati effettiva, recupera i dati, li elabora in qualche modo, quindi restituisce le informazioni elaborate all'applicazione.  
  
 In questo esempio, l'applicazione non si connette mai direttamente all'origine dati. IIS ha fatto. E IIS hanno eseguito l'accesso ai dati per mezzo di ADO.  
  
> [!NOTE]
>  Non è necessario che l'applicazione client/server sia basata su Internet o su una rete Intranet, ovvero basata sul Web, che può essere costituita esclusivamente da programmi compilati in una rete locale. Il caso tipico è tuttavia un'applicazione basata sul Web.  
  
 Poiché un controllo visivo, ad esempio una griglia, una casella di controllo o un elenco, può utilizzare le informazioni restituite, le informazioni restituite devono essere facilmente utilizzate da un controllo visivo.  
  
 Si desidera un'interfaccia di programmazione delle applicazioni semplice ed efficiente che supporta sistemi a tre livelli e restituisce informazioni con la stessa facilità con cui è stato recuperato in un sistema a due livelli. Remote Data Service (RDS) è questa interfaccia.  
  
## <a name="the-solution"></a>Soluzione  
 RDS definisce un modello di programmazione, ovvero la sequenza di attività necessarie per accedere e aggiornare un'origine dati, per ottenere l'accesso ai dati tramite un intermediario, ad esempio Internet Information Services (IIS). Il modello di programmazione riepiloga l'intera funzionalità di Servizi Desktop remoto.  
  
## <a name="see-also"></a>Vedere anche  
 [Modello di programmazione RDS di base](./basic-rds-programming-model.md)   
 [Scenario RDS](./rds-scenario.md)   
 [Esercitazione su RDS](./rds-tutorial.md)   
 [Utilizzo e sicurezza per RDS](./rds-usage-and-security.md)