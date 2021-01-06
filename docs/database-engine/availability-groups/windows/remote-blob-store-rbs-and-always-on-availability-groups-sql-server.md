---
title: Archivio BLOB remoto (RBS) con i gruppi di disponibilità
description: "Descrizione dell'uso di Archivio BLOB remoto (RBS) con i database che fanno parte di un gruppo di disponibilità Always On. "
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 01a70258-d4fd-40bc-bc44-c490b5d6c420
author: cawrites
ms.author: chadam
ms.openlocfilehash: 61316ca08eac7d24b1be4ef50c913aeba3358775
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642516"
---
# <a name="use-remote-blob-store-rbs-with-always-on-availability-groups"></a>Usare Archivio BLOB remoto (RBS) con i gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] offre una soluzione di disponibilità elevata e di ripristino di emergenza per gli oggetti BLOB dell'[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][archivio BLOB remoto](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md) (RBS, Remote Blob Store). [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] protegge tutti i metadati e gli schemi di RBS archiviati in un database di disponibilità replicandoli nelle repliche secondarie. Questo è il database del contenuto di SharePoint. In generale, tramite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] questi metadati di RBS vengono archiviati in modo indipendente dal BLOB.  
  
 La protezione dei dati dei BLOB dell'archivio BLOB remoti dipende dal percorso dell'archivio BLOB, come riportato di seguito:  
  
|Percorso dell'archivio BLOB|Possibilità di protezione dei dati BLOB tramite i gruppi di disponibilità|  
|-------------------------|-----------------------------------------------------|  
|Stesso database in cui sono contenuti i metadati di RBS (archiviato utilizzando un provider FILESTREAM remoto di RBS)|Sì|  
|Database diverso nella stessa istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (archiviato utilizzando un provider FILESTREAM remoto di RBS)|Sì<br /><br /> È consigliabile inserire questo database nello stesso gruppo di disponibilità del database in cui sono contenuti i metadati di RBS.|  
|Database diverso in un'istanza differente di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (archiviato utilizzando un provider FILESTREAM remoto di RBS)|Sì<br /><br /> Questo database deve trovarsi in un gruppo di disponibilità separato.|  
|Archivio BLOB di terze parti|No<br /><br /> Per proteggere questi dati BLOB, utilizzare i meccanismi di disponibilità elevata del provider dell'archivio BLOB.|  
  
##  <a name="limitations"></a><a name="Limitations"></a> Limitazioni  
  
-   I gestori RBS devono essere indirizzati alla replica primaria.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Raccomandazioni  
  
-   Utilizzare un listener del gruppo di disponibilità. Per altre informazioni, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Maintaining Remote BLOB Store](https://msdn.microsoft.com/library/gg316773\(SQL.105\).aspx) (Gestione dell'archivio BLOB remoti) in [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] Documentazione online  
  
-   [Running RBS Maintainer](/archive/blogs/sqlrbs/running-rbs-maintainer) (Esecuzione di Gestore RBS) (blog)  
  
-   [Configure Remote BLOB Storage (RBS) with the FILESTREAM provider (SharePoint 2010)](/archive/blogs/mvpawardprogram/configure-remote-blob-storage-rbs-with-the-filestream-provider-sharepoint-2010) (Configurare l'archivio BLOB remoti (RBS) con il provider FILESTREAM (SharePoint 2010)) (blog)  
  
## <a name="see-also"></a>Vedere anche  
 [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)   
 [Archivio Blob remoto &#40;RBS&#41; &#40;SQL Server&#41;](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
