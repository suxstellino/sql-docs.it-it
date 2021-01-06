---
title: Mirroring e snapshot del database
description: Informazioni sull'interoperabilità dell'uso di snapshot del database e mirroring del database per ripartire il carico di lavoro dei report in SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- snapshots [SQL Server database snapshots], database mirroring
- database snapshots [SQL Server], database mirroring
ms.assetid: 0bf1be90-7ce4-484c-aaa7-f8a782f57c5f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: d4dc48ce78438ef6b2a50576cde51cbe2bb12085
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642144"
---
# <a name="database-mirroring-and-database-snapshots-sql-server"></a>Mirroring e snapshot del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per ripartire il carico di lavoro dei report, è possibile avvalersi di un database mirror gestito per scopi di disponibilità. Per utilizzare un database mirror per la gestione dei report, creare nel database mirror uno snapshot del database e indirizzare le richieste di connessione client allo snapshot più recente. Uno snapshot del database è uno snapshot statico, in sola lettura e consistente a livello di transazioni del relativo database di origine nello stato in cui quest'ultimo si trovava al momento della creazione dello snapshot. Per creare uno snapshot del database in un database mirror, è necessario che il database si trovi nello stato di mirroring sincronizzato.  
  
 A differenza del database mirror stesso, uno snapshot del database è accessibile ai client. Fino a quando il server mirror comunica con il server principale, è possibile indirizzare i client di report a uno snapshot. Si noti che poiché uno snapshot del database è statico, i nuovi dati non sono disponibili. Per rendere disponibili agli utenti dati relativamente recenti, è necessario creare periodicamente un nuovo snapshot del database e fare in modo che le applicazioni indirizzino le connessioni client in arrivo allo snapshot più recente.  
  
 Un nuovo snapshot del database è quasi vuoto, ma aumenta nel tempo man mano che un numero crescente di pagine del database vengono aggiornate per la prima volta. Poiché ogni snapshot in un database aumenta in modo incrementale in questo modo, ogni snapshot del database utilizza tante risorse quanto un database normale. A seconda delle configurazioni del server mirror e del server principale, la presenza di un numero eccessivo di snapshot del database in un database mirror può compromettere le prestazioni nel database principale. È pertanto consigliabile conservare solo gli snapshot relativamente più recenti nei database mirror. In genere, dopo aver creato uno snapshot di sostituzione, è consigliabile reindirizzare le query in arrivo al nuovo snapshot ed eliminare quello meno recente dopo il completamento delle query correnti.  
  
> [!NOTE]  
>  Per altre informazioni sugli snapshot del database, vedere [Snapshot del database &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
 In presenza di un cambio di ruolo, il database e i relativi snapshot vengono riavviati, con conseguente disconnessione temporanea degli utenti. Gli snapshot del database rimangono quindi nell'istanza del server dove sono stati creati, ovvero il nuovo database principale. Gli utenti possono continuare a utilizzare gli snapshot dopo il failover, anche se ciò comporta un carico aggiuntivo per il nuovo server principale. Se è necessario evitare un peggioramento delle prestazioni, è consigliabile creare uno snapshot nel nuovo database mirror quando diventa disponibile, reindirizzare i client al nuovo snapshot ed eliminare tutti gli snapshot del database dal database mirror precedente.  
  
> [!NOTE]  
>  Per una soluzione dedicata per la creazione di report che consente una scalabilità orizzontale appropriata, prendere in considerazione la replica. Per altre informazioni, vedere [Replica di SQL Server](../../relational-databases/replication/sql-server-replication.md).  
  
## <a name="example"></a>Esempio  
 In questo esempio vengono creati snapshot in un database con mirroring.  
  
 Si supponga che il database di una sessione di mirroring del database sia [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. In questo esempio vengono creati tre snapshot del database nella copia mirror del database `AdventureWorks` che risiede nell'unità `F` . Gli snapshot sono denominati `AdventureWorks_0600`, `AdventureWorks_1200`e `AdventureWorks_1800` per identificare l'ora approssimativa in cui sono stati creati.  
  
1.  Creare il primo snapshot del database sul mirror di [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]:  
  
    ```  
    CREATE DATABASE AdventureWorks_0600  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_0600.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
2.  Creare il secondo snapshot del database sul mirror di [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Gli utenti che utilizzano ancora `AdventureWorks_0600` possono continuare a utilizzarlo.  
  
    ```  
    CREATE DATABASE AdventureWorks_1200  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1200.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     A questo punto è possibile indirizzare a livello di programmazione le nuove connessioni client allo snapshot più recente.  
  
3.  Creare il terzo snapshot sul mirror [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Gli utenti che utilizzano ancora `AdventureWorks_0600` o `AdventureWorks_1200` possono continuare a utilizzarli.  
  
    ```  
    CREATE DATABASE AdventureWorks_1800  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1800.SNP')  
        AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     A questo punto è possibile indirizzare a livello di programmazione le nuove connessioni client allo snapshot più recente.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creare uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
-   [Visualizzazione di uno snapshot del database &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Eliminare uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
  
## <a name="see-also"></a>Vedere anche  
 [Snapshot del database &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)   
 [Connettere client a una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)  
  
  
