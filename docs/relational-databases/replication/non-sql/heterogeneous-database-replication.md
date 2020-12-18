---
description: replica di database eterogenei
title: Replica di database eterogenei | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- heterogeneous database replication, about heterogeneous database replication
- replication [SQL Server], heterogeneous database replication
- heterogeneous database replication
ms.assetid: 3fd983ad-e206-45db-9054-417c9b5bb815
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d5ea494a48932fac171eb4d9e8bc3e032001e55b
ms.sourcegitcommit: 821e7039a342bf76306d66c61db247dc2caabc46
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "96999236"
---
# <a name="heterogeneous-database-replication"></a>replica di database eterogenei  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supporta gli scenari eterogenei seguenti per la replica transazionale e snapshot:  
  
-   Pubblicazione di dati da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a Sottoscrittori non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  

-   Pubblicazione di dati da e verso Oracle con le limitazioni seguenti:  

  |Scenario|2016 o versioni precedenti |2017 o versioni successive |
  |-------|-------|--------|
  |Replica da Oracle |Supporta solo Oracle 10g o versioni precedenti |Supporta solo Oracle 10g o versioni precedenti |
  |Replica verso Oracle |Tutte le versioni precedenti a Oracle 12c |Non supportate |


 La replica eterogenea a Sottoscrittori non SQL Server è deprecata. La pubblicazione Oracle è deprecata. Per spostare dati, creare soluzioni utilizzando Change Data Capture e [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="publishing-data-from-oracle"></a>Pubblicazione di dati da Oracle  
 È possibile utilizzare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per pubblicare dati di Oracle con le stesse funzionalità e la stessa facilità d'uso della replica snapshot e transazionale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Questa funzionalità richiede Oracle versione 10G o precedenti. La pubblicazione di dati da Oracle è ideale per gli scenari seguenti:  
  
|Scenario|Descrizione|  
|--------------|-----------------|  
|Distribuzioni di applicazioni[!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework|Sviluppo con [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , operando al contempo su dati replicati da un database non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|Server di gestione temporanea di data warehousing|Mantenimento dei database di gestione temporanea di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sincronizzati con un database non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|Migrazione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Verifica dell'applicazione in tempo reale a fronte di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] replicando al contempo le modifiche del sistema di origine. Quando il risultato della migrazione è soddisfacente, passare a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
 Per altre informazioni, vedere [Panoramica della pubblicazione Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="publishing-data-to-non-sql-server-subscribers"></a>Pubblicazione di dati in Sottoscrittori non SQL Server  
 I database non [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seguenti sono supportati come Sottoscrittori per le pubblicazioni snapshot e transazionali:  
  
-   Oracle per tutte le piattaforme supportate da Oracle.  
  
-   IBM DB2 per AS400, MVS, Unix, Linux e Windows.  
  
 Per altre informazioni, vedere [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
  
