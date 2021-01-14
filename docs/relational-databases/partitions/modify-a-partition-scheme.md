---
description: Modificare uno schema di partizione
title: Modificare uno schema di partizione | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 515de63f-dfc5-434d-9adb-f3b5992f745a
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb261e69d3491f717be8b34bb2b0f699a4d37840
ms.sourcegitcommit: 629229a7c33a3ed99db63b89127bb016449f7d3d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952036"
---
# <a name="modify-a-partition-scheme"></a>Modificare uno schema di partizione
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile modificare uno schema di partizione in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] designando un filegroup in cui sia inclusa la partizione successiva aggiunta a una tabella partizionata utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. A tale scopo, è necessario assegnare la proprietà NEXT USED a un filegroup. È possibile assegnare la proprietà NEXT USED a un filegroup vuoto o a uno che contiene già una partizione. In altri termini, in un filegroup possono essere incluse più partizioni.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare una tabella o un indice partizionato tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Tutti i filegroup a cui viene applicata l'istruzione ALTER PARTITION SCHEME devono essere online.  

> [!NOTE]
> In Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] sono supportati solo i filegroup primari.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per eseguire l'istruzione ALTER PARTITION SCHEME, è possibile utilizzare le autorizzazioni seguenti:  
  
-   Autorizzazione ALTER ANY DATASPACE. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
-   Autorizzazione CONTROL o ALTER per il database nel quale viene creato lo schema di partizione.  
  
-   Autorizzazione CONTROL SERVER o ALTER ANY DATABASE per il server del database nel quale è stato creato lo schema di partizione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per modificare uno schema di partizione:**  
  
 Non è possibile eseguire questa azione specifica tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per modificare uno schema di partizione, è innanzitutto necessario eliminare lo schema e crearne quindi uno nuovo con le proprietà desiderate tramite la Creazione guidata partizione. Per ulteriori informazioni, vedere [Create Partitioned Tables and Indexes](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)[Using SQL Server Management Studio](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md#SSMSProcedure) in **Crea tabelle e indici partizionati**.  
  
#### <a name="to-delete-a-partition-scheme"></a>Per eliminare uno schema di partizione  
  
1.  Fare clic sul segno più per espandere il database in cui si desidera eliminare lo schema di partizione.  
  
2.  Fare clic sul segno più per espandere la cartella **Archiviazione** .  
  
3.  Fare clic sul segno più per espandere la cartella **Schemi di partizione** .  
  
4.  Fare clic con il pulsante destro del mouse sullo schema di partizione che si vuole eliminare e scegliere **Elimina**.  
  
5.  Nella finestra di dialogo **Elimina oggetto** verificare che sia selezionato lo schema di partizione corretto, quindi fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-modify-a-partition-scheme"></a>Per modificare uno schema di partizione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- add five new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test5fg;  
    GO  
    -- if the "myRangePF1" partition function and the "myRangePS1" partition scheme exist,  
    -- drop them from the AdventureWorks2012 database  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
    DROP PARTITION FUNCTION myRangePF1;  
    GO  
    IF EXISTS (SELECT * FROM sys.partition_schemes  
        WHERE name = 'myRangePS1')  
    DROP PARTITION SCHEME myRangePS1;  
    GO  
    -- create the new partition function "myRangePF1" with four partition groups  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    -- create the new partition scheme "myRangePS1"that will use   
    -- the "myRangePF1" partition function with five file groups.  
    -- The last filegroup, "test5fg," will be kept empty but marked  
    -- as the next used filegroup in the partition scheme.  
    CREATE PARTITION SCHEME myRangePS1  
    AS PARTITION myRangePF1  
    TO (test1fg, test2fg, test3fg, test4fg, test5fg);  
    GO  
    --Split "myRangePS1" between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    GO  
    -- Allow the "myRangePS1" partition scheme to use the filegroup "test5fg"  
    -- for the partition with boundary_values of 100 and 500  
    ALTER PARTITION SCHEME myRangePS1  
    NEXT USED test5fg;  
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md).  
  
  
