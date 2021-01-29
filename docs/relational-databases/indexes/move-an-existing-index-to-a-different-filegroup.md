---
description: Spostare un indice esistente in un filegroup diverso
title: Spostare un indice esistente in un filegroup diverso | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- moving tables
- switching filegroups for index
- moving indexes
- indexes [SQL Server], moving
- filegroups [SQL Server], switching
ms.assetid: 167ebe77-487d-4ca8-9452-4b2c7d5cb96e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4ccacb2b46b609b61485821f250644596f0e3955
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048884"
---
# <a name="move-an-existing-index-to-a-different-filegroup"></a>Spostare un indice esistente in un filegroup diverso
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritta la procedura per spostare un indice esistente dal relativo filegroup corrente in un filegroup diverso in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per spostare un indice esistente in un filegroup diverso tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se una tabella presenta un indice cluster, lo spostamento di tale indice in un nuovo filegroup causa lo spostamento della tabella in tale filegroup.  
  
-   Non è possibile spostare gli indici creati utilizzando un vincolo UNIQUE o PRIMARY KEY tramite [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Per spostare questi indici, usare l'istruzione [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) con l'opzione DROP_EXISTING=ON in [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup-using-table-designer"></a>Per spostare un indice esistente in un filegroup diverso tramite Progettazione tabelle  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella che contiene l'indice da spostare.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella contenente l'indice da spostare e selezionare **Progetta**.  
  
4.  Scegliere **Indici/chiavi** nel menu **Progettazione tabelle**.  
  
5.  Selezionare l'indice che si desidera spostare.  
  
6.  Nella griglia principale espandere **Specifica spazio dei dati**.  
  
7.  Selezionare **Nome filegroup o schema di partizione** e selezionare dall'elenco il filegroup o lo schema di partizione in cui si desidera spostare l'indice.  
  
8.  Fare clic su **Close**.  
  
9. Selezionare **Salva** table_name **dal menu**_File_.  

#### <a name="to-move-an-existing-index-to-a-different-filegroup-in-object-explorer"></a>Per spostare un indice esistente in un filegroup diverso in Esplora oggetti  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella che contiene l'indice da spostare.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella contenente l'indice da spostare.  
  
4.  Fare clic sul segno più per espandere la cartella **Indici** .  
  
5.  Fare clic con il pulsante destro del mouse sull'indice da spostare e selezionare **Proprietà**.  
  
6.  In **Selezione pagina** selezionare **Archiviazione**.  
  
7.  Selezionare il filegroup in cui si desidera spostare l'indice.  
  
     Se la tabella o l'indice è partizionato, selezionare lo schema di partizione in cui si desidera spostare l'indice. Per ulteriori informazioni sugli indici partizionati, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
     Per spostare un indice cluster, è possibile utilizzare l'elaborazione online. L'elaborazione online consente l'accesso simultaneo degli utenti ai dati sottostanti e agli indici non cluster durante l'operazione sull'indice. Per altre informazioni, vedere [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
     Nei computer multiprocessore che utilizzano [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]è possibile configurare il numero di processori utilizzati per l'esecuzione dell'istruzione dell'indice specificando il valore massimo per il grado di parallelismo. La funzionalità Operazioni indicizzate parallele non è disponibile in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere Funzionalità supportate dalle edizioni di SQL Server 2016. Per altre informazioni sulle operazioni indicizzate parallele, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
8.  Fare clic su **OK**.  
  
 Le informazioni seguenti sono disponibili nella pagina **Archiviazione** della finestra di dialogo **Proprietà indice -** _nome_indice_:  
  
 **Filegroup**  
 Archivia l'indice nel filegroup specificato. Nell'elenco sono visualizzati solo filegroup standard (ROW). La selezione predefinita nell'elenco è il filegroup PRIMARY del database.  
  
 **Filegroup FILESTREAM**  
 Specifica il filegroup per i dati FILESTREAM. Questo elenco visualizza solo i filegroup FILESTREAM. La selezione predefinita nell'elenco è il filegroup PRIMARY FILESTREAM del database.  
  
 **Schema di partizione**  
 Archivia l'indice in uno schema di partizione. Facendo clic su **Schema partizione** si abilita la griglia sottostante. La selezione predefinita nell'elenco è lo schema di partizione usato per archiviare i dati di tabella. Selezionando uno schema di partizione diverso nell'elenco si aggiornano le informazioni visualizzate nella griglia.  
  
 L'opzione relativa allo schema di partizione non è disponibile se il database non contiene schemi di partizione.  
  
 **Schema di partizione FILESTREAM**  
 Specifica lo schema di partizione per i dati FILESTREAM. Lo schema di partizione deve essere simmetrico con la combinazione specificata nell'opzione **Schema partizione** .  
  
 Se la tabella non è partizionata, il campo è vuoto.  
  
 **Parametro schema partizione**  
 Consente di visualizzare il nome della colonna che partecipa allo schema di partizione.  
  
 **Colonne della tabella**  
 Consente di selezionare la tabella o la vista su cui eseguire il mapping allo schema di partizione.  
  
 **Tipo di dati colonna**  
 Consente di visualizzare le informazioni sul tipo di dati relative alla colonna.  
  
> [!NOTE]  
>  Se la colonna della tabella è una colonna calcolata, l'opzione **Tipo di dati colonna** visualizza "colonna calcolata".  
  
 **Consenti elaborazione online di istruzioni DML durante lo spostamento dell'indice**  
 Consente agli utenti di accedere ai dati della tabella o dell'indice cluster sottostanti e a eventuali indici non cluster associati durante l'operazione sull'indice.  
  
> [!NOTE]  
>  Questa opzione non è disponibile per gli indici XML o se l'indice è un indice cluster disabilitato.  
  
 **Imposta massimo grado di parallelismo**  
 Consente di limitare il numero di processori da usare durante l'esecuzione di piani paralleli. Il valore predefinito è 0 e corrisponde al numero effettivo di CPU disponibili. L'impostazione del valore su 1 impedisce la generazione di piani paralleli. L'impostazione del valore su un numero maggiore di 1 limita il numero massimo di processori usati da una singola esecuzione della query. Questa opzione diventa disponibile solo se la finestra di dialogo è nello stato **Ricompila** o **Ricrea** .  
  
> [!NOTE]  
>  Se viene specificato un valore maggiore del numero di CPU disponibili, verrà usato l'effettivo numero di CPU disponibili.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup"></a>Per spostare un indice esistente in un filegroup diverso  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Creates the TransactionsFG1 filegroup on the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP TransactionsFG1;  
    GO  
    /* Adds the TransactionsFG1dat3 file to the TransactionsFG1 filegroup. Please note that you will have to change the filename parameter in this statement to execute it without errors.  
    */  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = TransactionsFG1dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\DATA\TransactionsFG1dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP TransactionsFG1;  
    GO  
    /*Creates the IX_Employee_OrganizationLevel_OrganizationNode index  
      on the TransactionsPS1 filegroup and drops the original IX_Employee_OrganizationLevel_OrganizationNode index.  
    */  
    CREATE NONCLUSTERED INDEX IX_Employee_OrganizationLevel_OrganizationNode  
        ON HumanResources.Employee (OrganizationLevel, OrganizationNode)  
        WITH (DROP_EXISTING = ON)  
        ON TransactionsFG1;  
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
