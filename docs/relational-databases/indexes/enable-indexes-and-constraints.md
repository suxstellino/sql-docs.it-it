---
description: Abilitazione di indici e vincoli
title: Abilitare indici e vincoli | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], enabling
- nonclustered indexes [SQL Server], enabling a disabled index
- index enabling [SQL Server]
- disabled indexes [SQL Server], how to enable
- constraints [SQL Server], enabling
- clustered indexes, enabling disabled indexes
ms.assetid: c55c8865-322e-4ab0-ba04-ea1f56735353
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0d309a308e71057f71fd20b0d0652d14a6011ce5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048904"
---
# <a name="enable-indexes-and-constraints"></a>Abilitazione di indici e vincoli
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento si descrive come abilitare un indice disabilitato in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dopo la disabilitazione, un indice rimane nello stato disabilitato finché non viene ricompilato o rimosso.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per abilitare un indice disabilitato utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Dopo la ricompilazione dell'indice, sarà necessario abilitare manualmente tutti i vincoli disabilitati in seguito alla disabilitazione dell'indice. Per abilitare i vincoli PRIMARY KEY e UNIQUE, è necessario ricompilare l'indice associato. Ricompilare e abilitare questo indice prima di abilitare i vincoli FOREIGN KEY che fanno riferimento al vincolo PRIMARY KEY o UNIQUE. Per abilitare i vincoli FOREIGN KEY, utilizzare l'istruzione ALTER TABLE CHECK CONSTRAINT.  
  
-   Non è possibile ricompilare un indice cluster disabilitato se l'opzione ONLINE è impostata su ON.  
  
-   Se l'indice cluster è abilitato o disabilitato e l'indice non cluster è disabilitato, l'operazione sull'indice cluster ha l'effetto seguente sull'indice non cluster disabilitato.  
  
    |Operazione sull'indice cluster|Indice non cluster disabilitato...|  
    |----------------------------|-----------------------------------|  
    |ALTER INDEX REBUILD.|Rimane disabilitato.|  
    |ALTER INDEX ALL REBUILD.|Viene ricompilato e abilitato.|  
    |DROP INDEX.|Rimane disabilitato.|  
    |CREATE INDEX WITH DROP_EXISTING.|Rimane disabilitato.|  
  
     La creazione di un nuovo indice cluster produce lo stesso risultato dell'istruzione ALTER INDEX ALL REBUILD.  
  
-   Le operazioni consentite su indici non cluster associati a un indice cluster dipendono dallo stato, disabilitato o abilitato, di entrambi i tipi di indice. Nella tabella seguente sono riepilogate le operazioni consentite sugli indici non cluster.  
  
    |Operazione sull'indice non cluster|Indici cluster e non cluster disabilitati|Indice cluster abilitato e indice non cluster in entrambi gli stati|  
    |-------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
    |ALTER INDEX REBUILD.|Operazione non eseguita.|Operazione eseguita.|  
    |DROP INDEX.|Operazione eseguita.|Operazione eseguita.|  
    |CREATE INDEX WITH DROP_EXISTING.|Operazione non eseguita.|Operazione eseguita.|  

-   Durante la ricompilazione di indici non cluster compressi disabilitati, data_compression avrà come valore predefinito 'none' a indicare che gli indici saranno non compressi. Sulla base delle impostazioni di compressione, i metadati vengono persi quando gli indici non cluster sono disabilitati. Per risolvere questo problema, è necessario specificare la compressione dei dati esplicita nell'istruzione di ricompilazione.

###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. Se si usa DBCC DBREINDEX, l'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-enable-a-disabled-index"></a>Per abilitare un indice disabilitato  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera abilitare un indice.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera abilitare un indice.  
  
4.  Fare clic sul segno più per espandere la cartella **Indici** .  
  
5.  Fare clic con il pulsante destro del mouse sull'indice che si vuole abilitare e selezionare **Ricompila**.  
  
6.  Nella finestra di dialogo **Ricompila indici** verificare che nella griglia **Indici da ricompilare** sia presente l'indice corretto e fare clic su **OK**.  
  
#### <a name="to-enable-all-indexes-on-a-table"></a>Per abilitare tutti gli indici in una tabella  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera abilitare gli indici.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera abilitare gli indici.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Indici** e selezionare **Ricompila tutto**.  
  
5.  Nella finestra di dialogo **Ricompila indici** verificare che nella griglia **Indici da ricompilare** siano presenti gli indici corretti e fare clic su **OK**. Per rimuovere un indice dalla griglia **Indici da ricompilare** , selezionare l'indice desiderato, quindi premere il tasto CANC.  
  
 Le informazioni seguenti sono disponibili nella finestra di dialogo **Ricompila indici** :  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-enable-a-disabled-index-using-alter-index"></a>Per abilitare un indice disabilitato utilizzando ALTER INDEX  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD;   
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-create-index"></a>Per abilitare un indice disabilitato utilizzando CREATE INDEX  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- re-creates the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    -- using the OrganizationLevel and OrganizationNode columns  
    -- and then deletes the existing IX_Employee_OrganizationLevel_OrganizationNode index  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)  
    WITH (DROP_EXISTING = ON);  
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-dbcc-dbreindex"></a>Per abilitare un indice disabilitato utilizzando DBCC DBREINDEX  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", IX_Employee_OrganizationLevel_OrganizationNode);  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-alter-index"></a>Per abilitare tutti gli indici in una tabella utilizzando ALTER INDEX  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    ALTER INDEX ALL ON HumanResources.Employee  
    REBUILD;  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-dbcc-dbreindex"></a>Per abilitare tutti gli indici in una tabella utilizzando DBCC DBREINDEX  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", " ");  
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md), [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md) e [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md).  
  
  
