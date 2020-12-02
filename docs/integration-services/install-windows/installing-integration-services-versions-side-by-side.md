---
description: Installazione side-by-side di versioni di Integration Services
title: Installazione side-by-side di versioni di Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- interoperability and coexistence [Integration Services]
- Integration Services, interoperability and coexistence
ms.assetid: edfbcd56-012f-462e-a542-95491394fda9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d57148134c727b03a30d75af415d1e2476dca32d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88345997"
---
# <a name="installing-integration-services-versions-side-by-side"></a>Installazione side-by-side di versioni di Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile installare   
      [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] installazione side-by-side di Integration Services (SSIS) con versioni precedenti di SSIS. Questo argomento descrive alcune limitazioni delle installazioni side-by-side.  
  
## <a name="designing-and-maintaining-packages"></a>Progettazione e gestione dei pacchetti  
 Per progettare e gestire pacchetti destinati a SQL Server 2016, SQL Server 2014 o SQL Server 2012 usare SQL Server Data Tools (SSDT) per Visual Studio 2015. Per ottenere SSDT, vedere [Scaricare la versione più recente di SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).  
  
 Nelle pagine delle proprietà per un progetto di Integration Services, nella scheda **Generale** di **Proprietà di configurazione** selezionare la proprietà **TargetServerVersion** , quindi scegliere SQL Server 2016, SQL Server 2014 o SQL Server 2012.  
  
|Versione di destinazione di SQL Server|Ambiente di sviluppo per pacchetti SSIS|  
|----------------------------------|-----------------------------------------------|  
|2016|SQL Server Data Tools per Visual Studio 2015|  
|2014|SQL Server Data Tools per Visual Studio 2015<br /><br /> oppure<br /><br /> SQL Server Data Tools - Business Intelligence per Visual Studio 2013|  
|2012|SQL Server Data Tools per Visual Studio 2015<br /><br /> oppure<br /><br /> SQL Server Data Tools - Business Intelligence per Visual Studio 2012|  
|2008|Business Intelligence Development Studio in SQL Server 2008|  
  
 Quando si aggiunge un pacchetto esistente a un progetto esistente, il pacchetto viene convertito nel formato di destinazione dal progetto.  
  
## <a name="running-packages"></a>Esecuzione di pacchetti  
 È possibile usare la versione [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] dell'utilità **dtexec** o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per eseguire i pacchetti di Integration Services creati nelle versioni precedenti degli strumenti di sviluppo. Quando gli strumenti di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] caricano un pacchetto sviluppato in una versione precedente degli strumenti di sviluppo, lo strumento converte temporaneamente il pacchetto in memoria nel formato pacchetto usato da [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] . Se il pacchetto presenta problemi che impediscono la conversione, lo strumento [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] non riesce a eseguire il pacchetto fino a quando tali problemi non vengono risolti. Per altre informazioni, vedere [Aggiornare pacchetti di Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
  
