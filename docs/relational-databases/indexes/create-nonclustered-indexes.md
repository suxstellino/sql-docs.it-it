---
description: Creare indici non cluster
title: Creare indici non cluster | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index creation [SQL Server], nonclustered indexes
- nonclustered indexes [SQL Server], creating
- nonclustered indexes [SQL Server], UNIQUE constraint
- indexes [SQL Server], nonclustered
- nonclustered indexes [SQL Server], PRIMARY KEY constraint
ms.assetid: 9402029a-1227-46c4-93aa-c2122eb1b943
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9200edd16f0a33a52b12493660b18cfe45ae6086
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048967"
---
# <a name="create-nonclustered-indexes"></a>Creare indici non cluster
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  È possibile creare indici non cluster in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un indice non cluster è una struttura di indice separata dai dati archiviati in una tabella che riordina una o più colonne selezionate. Spesso gli indici non cluster consentono di trovare i dati più rapidamente rispetto alla ricerca nella tabella sottostante. Le query talvolta possono ottenere risposta unicamente mediante i dati presenti nell'indice non cluster oppure l'indice non cluster può puntare il [!INCLUDE[ssDE](../../includes/ssde-md.md)] alle righe nella tabella sottostante. Gli indici non cluster vengono creati generalmente per migliorare le prestazioni delle query di utilizzo frequente non coperte dall'indice cluster oppure per individuare le righe in una tabella senza un indice cluster (denominato heap). In una vista tabella o indicizzata è possibile creare più indici non cluster.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Modalità di implementazione tipiche  
 Gli indici non cluster vengono implementati nei modi seguenti:  
  
-   **Vincoli UNIQUE**  
  
     Quando si crea un vincolo UNIQUE, viene creato un indice non cluster univoco per applicare un vincolo UNIQUE per impostazione predefinita. È possibile specificare un indice cluster univoco se nella tabella non ne esiste già uno. Per altre informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
-   **Indice indipendente da un vincolo**  
  
     Per impostazione predefinita, viene creato un indice non cluster se non è stato specificato l'indice cluster. Il numero massimo di indici non cluster che è possibile creare per tabella è 999. Sono inclusi gli indici creati tramite i vincoli PRIMARY KEY o UNIQUE, ma non gli indici XML.  
  
-   **Indice non cluster su una vista indicizzata**  
  
     Dopo la creazione di un indice cluster univoco su una vista, è possibile creare indici non cluster. Per altre informazioni, vedere [Creare viste indicizzate](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-nonclustered-index-by-using-the-table-designer"></a>Per creare un indice non cluster tramite Progettazione tabelle  
  
1.  In Esplora oggetti espandere il database contenente la tabella in cui si desidera creare un indice non cluster.  
  
2.  Espandere la cartella **Tabelle** .  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella in cui creare un indice non cluster e selezionare **Progetta**.  
  
4.  Scegliere **Indici/chiavi** nel menu **Progettazione tabelle**.  
  
5.  Nella finestra di dialogo **Indici/chiavi** fare clic su **Aggiungi**.  
  
6.  Selezionare il nuovo indice dalla casella di testo **Chiave o indice primario/univoco selezionato** .  
  
7.  Nella griglia selezionare **Crea come CLUSTERED**, quindi scegliere **No** dall'elenco a discesa a destra della proprietà.  
  
8.  Fare clic su **Close**.  
  
9. Nel menu **File** scegliere **Salva**_table_name_.  

#### <a name="to-create-a-nonclustered-index-by-using-object-explorer"></a>Per creare un indice non cluster tramite Esplora oggetti  
  
1.  In Esplora oggetti espandere il database contenente la tabella in cui si desidera creare un indice non cluster.  
  
2.  Espandere la cartella **Tabelle** .  
  
3.  Espandere la tabella in cui si desidera creare un indice non cluster.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Indici**, scegliere **Nuovo indice** e selezionare **Indice non cluster**.  
  
5.  Nella pagina **Generale** della finestra di dialogo **Nuovo indice** immettere il nome del nuovo indice nella casella **Nome indice** .  
  
6.  In **Colonne chiave indice** fare clic su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Seleziona colonne da**_table_name_ selezionare le caselle di controllo delle colonne di tabella da aggiungere all'indice non cluster.  
  
8.  Fare clic su **OK**.  
  
9. Nella finestra di dialogo **Nuovo indice** fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-nonclustered-index-on-a-table"></a>Per creare un indice non cluster in una tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named IX_ProductVendor_VendorID and delete it if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
                WHERE name = N'IX_ProductVendor_VendorID')   
        DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;   
    GO  
    -- Create a nonclustered index called IX_ProductVendor_VendorID   
    -- on the Purchasing.ProductVendor table using the BusinessEntityID column.   
    CREATE NONCLUSTERED INDEX IX_ProductVendor_VendorID   
        ON Purchasing.ProductVendor (BusinessEntityID);   
    GO  
    ```  
  
## <a name="related-content"></a>Contenuti correlati  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[Guida per la progettazione di indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md) 
