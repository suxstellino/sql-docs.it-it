---
title: Sospendere e riprendere una sessione di mirroring del database
description: Informazioni su come sospendere e riprendere una sessione di mirroring del database di SQL Server con SQL Server Management Studio o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- resuming database mirroring
- database mirroring [SQL Server], sessions
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: 05ede3b4-6abe-4442-abb7-9f5aee1d6bc0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9e3348259a8fbfbdb59e0ce5939f6c3e95ee2419
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783340"
---
# <a name="pause-or-resume-a-database-mirroring-session-sql-server"></a>Sospendere o riprendere una sessione di mirroring del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come sospendere o riprendere il mirroring del database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per eseguire ReplaceThisText utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la sospensione o ripresa del mirroring del database](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
 È possibile sospendere una sessione di mirroring del database in qualsiasi momento. Questa operazione potrebbe migliorare le prestazioni in caso di colli di bottiglia. Inoltre, la sessione può essere ripresa in qualsiasi momento.  
  
> [!CAUTION]  
>  Dopo un servizio forzato, quando il server principale originale esegue nuovamente la connessione, il mirroring viene sospeso. Se si riprende il mirroring in questa situazione, è possibile che si verifichi una perdita di dati nel server principale originale. Per informazioni sulla gestione della potenziale perdita di dati, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Per sospendere o riprendere una sessione di mirroring del database, utilizzare la pagina **Proprietà database - Mirroring** .  
  
#### <a name="to-pause-or-resume-database-mirroring"></a>Per sospendere o riprendere il mirroring del database  
  
1.  Durante una sessione di mirroring del database, connettersi all'istanza del server principale e in Esplora oggetti fare clic sul nome del server per espanderne l'albero.  
  
2.  Espandere **Database** e selezionare il database.  
  
3.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Server mirror**. Viene visualizzata la pagina **Mirroring** della finestra di dialogo **Proprietà database** .  
  
4.  Per sospendere la sessione, scegliere **Sospendi**.  
  
     Verrà richiesta una conferma. Se si fa clic su **Sì** la sessione viene sospesa e il pulsante diventa **Riprendi**.  
  
     Per altre informazioni sull'impatto della sospensione di una sessione, vedere [Sospensione e ripresa del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md).  
  
5.  Per riprendere la sessione fare clic su **Riprendi**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-pause-database-mirroring"></a>Per sospendere il mirroring del database  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] per qualsiasi partner.  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] riportata di seguito.  
  
     ALTER DATABASE *nome_database* SET PARTNER SUSPEND  
  
     dove *nome_database* è il database con mirroring di cui si vuole sospendere la sessione.  
  
     Nell'esempio seguente viene sospeso il database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER SUSPEND;  
    ```  
  
##### <a name="to-resume-database-mirroring"></a>Per riprendere il mirroring del database  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] per qualsiasi partner.  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Eseguire l'istruzione Transact-SQL seguente:  
  
     ALTER DATABASE *nome_database* SET PARTNER RESUME  
  
     dove *nome_database* è il database con mirroring di cui si vuole riprendere la sessione.  
  
     Nell'esempio seguente viene sospeso il database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER RESUME;  
    ```  
  
##  <a name="follow-up-after-pausing-or-resuming-database-mirroring"></a><a name="FollowUp"></a> Completamento: Dopo la sospensione o ripresa del mirroring del database  
  
-   **Dopo la sospensione del mirroring del database**  
  
     Nel database primario adottare le precauzioni per evitare un log delle transazioni pieno. Per altre informazioni, vedere [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
-   **Dopo la ripresa del mirroring del database**  
  
     Quando si riprende una sessione di mirroring del database, il database con mirroring viene posto in stato SYNCHRONIZING. Se il livello di sicurezza corrisponde a FULL, il database mirror viene aggiornato in base al database principale e lo stato del database mirror diventa SYNCHRONIZED. A questo punto è possibile che si verifichi un failover. Se il server di controllo del mirroring è presente e in stato ON, è possibile che si verifichi un failover automatico. Se invece tale server non è presente, è possibile che si verifichi un failover manuale.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Rimuovere il mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
