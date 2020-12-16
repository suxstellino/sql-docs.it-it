---
title: Database di distribuzione | Microsoft Docs
description: In SQL Server il database di distribuzione archivia i metadati e i dati cronologici relativi a tutti i tipi di replica, oltre alle transazioni per la replica transazionale.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configuredistributionwizard.distributiondatabase.f1
ms.assetid: 5b42a083-7a11-41d8-9e3f-320c7c907237
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6fa550fd5cbe31535161e25ef86a394d44badcef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484883"
---
# <a name="distribution-database"></a>Database di distribuzione
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Nel database di distribuzione vengono archiviati i metadati e i dati di cronologia relativi a tutti i tipi di replica, nonché le transazioni per la replica transazionale.  
  
 In molti casi, è sufficiente un singolo database di distribuzione. Tuttavia, nel caso in cui più server di pubblicazione utilizzino un unico server di distribuzione, valutare l'opportunità di creare un database di distribuzione per ogni server di pubblicazione. in modo da garantire che il flusso di dati di ogni database di distribuzione risulti distinto. È possibile specificare un database di distribuzione per il server di distribuzione utilizzando la Configurazione guidata distribuzione. Se necessario, specificare database di distribuzione aggiuntivi nella finestra di dialogo **Proprietà server di distribuzione** .  
  
## <a name="options"></a>Opzioni  
 **Nome database di distribuzione**  
 Consente di immettere un nome per il database di distribuzione. Il nome predefinito del database di distribuzione è 'distribution'. Se si specifica un nome, questo può avere una lunghezza massima di 128 caratteri, deve essere univoco nell'ambito di un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e conforme alle regole per gli identificatori. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
 **Cartella per il file del database di distribuzione** e **Cartella per il file di log del database di distribuzione**  
 Immettere il percorso del database di distribuzione e dei file di log. I percorsi devono fare riferimento a dischi locali del server di distribuzione e iniziare con una lettera di unità locale seguita da due punti, ad esempio C:. Le lettere di unità mappate e i percorsi di rete non sono consentiti.  
  
> [!NOTE]  
>  è possibile ridurre il tempo necessario per la scrittura di transazioni e ottenere un miglioramento delle prestazioni della replica inserendo il log di distribuzione in un'unità disco distinta da quella del database di distribuzione.  
  
## <a name="see-also"></a>Vedere anche  
 [Configura distribuzione](../../relational-databases/replication/configure-distribution.md)   
 [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
  
  
