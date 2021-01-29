---
title: Modificare il tempo di recupero di riferimento di un database
description: Informazioni su come impostare o modificare il tempo di recupero di riferimento di un database SQL Server in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 6a64388940661139a52a10cd781fa13033e6fb70
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049177"
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>Modificare il tempo di recupero di riferimento di un database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come impostare o modificare il tempo di recupero di riferimento di un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per impostazione predefinita, il tempo di recupero di riferimento è 60 secondi e il database usa *checkpoint indiretti*. Il tempo di recupero di riferimento stabilisce un limite superiore per il tempo di recupero per questo database.  
  
> [!NOTE]  
>  Il limite superiore specificato per un determinato database dall'impostazione del tempo di recupero di riferimento può essere superato se una transazione con esecuzione prolungata provoca tempi eccessivi di UNDO.  
  
-   **Prima di iniziare:**  [Limitazioni e restrizioni](#Restrictions), [Sicurezza](#Security)  
  
-   **Per modificare il tempo di recupero di riferimento tramite:**  [SQL Server Management Studio](#SSMSProcedure) o [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni 
  
> [!CAUTION]  
>  Un carico di lavoro transazionale online su un database configurato per i checkpoint indiretti potrebbe subire un calo delle prestazioni. I checkpoint indiretti assicurano che il numero di pagine dirty è inferiore a una determinata soglia, in modo che il recupero del database viene completato entro il tempo di recupero di riferimento. A differenza dei checkpoint indiretti, che usano il numero di pagine dirty, l'opzione di configurazione Intervallo di recupero usa il numero di transazioni per determinare il tempo di recupero. Quando i checkpoint indiretti sono abilitati per un database che riceve un numero elevato di operazioni DML, il writer in background può iniziare a scaricare i buffer dirty su disco in modo intensivo per garantire che il tempo necessario per eseguire il recupero non superi il tempo di recupero di riferimento impostato per il database. Questo può causare in determinati sistemi ulteriore attività di I/O che può comportare un collo di bottiglia delle prestazioni se il sottosistema del disco opera al di sopra o in prossimità della soglia di I/O.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per modificare il tempo di recupero di riferimento**  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], quindi espanderla.  
  
2.  Espandere il contenitore **Database**, quindi fare clic con il pulsante destro del mouse sul database da modificare e scegliere il comando **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà database** fare clic sulla pagina **Opzioni** .  
  
4.  Nel campo **Tempo di recupero di riferimento (secondi)** del pannello **Recupero** specificare un numero di secondi come limite superiore del tempo di recupero per il database.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per modificare il tempo di recupero di riferimento**  
  
1.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui risiede il database.  
  
2.  Usare l'istruzione [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md), come indicato di seguito:  
  
     TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     A partire da [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], il valore predefinito è 1 minuto. Se maggiore di 0 (impostazione predefinita per le versioni meno recenti), specifica il limite superiore per il tempo di recupero per il database specificato in caso di arresto anomalo.  
  
     SECONDS  
     Indica che *target_recovery_time* viene espresso come numero di secondi.  
  
     MINUTES  
     Indica che *target_recovery_time* viene espresso come numero di minuti.  
  
     Nell'esempio seguente viene impostato il tempo di recupero di riferimento del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] su `60` .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
