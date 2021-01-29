---
description: Specificare un fattore di riempimento per un indice
title: Specificare un fattore di riempimento per un indice | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- fill factor [SQL Server]
- page splits [SQL Server]
ms.assetid: 237a577e-b42b-4adb-90cf-aa7fb174f3ab
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: e1e0a2b406d6817c43a5a4a51bea90b556c5cea6
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048774"
---
# <a name="specify-fill-factor-for-an-index"></a>Specificare un fattore di riempimento per un indice
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  In questo argomento si descrive il fattore di riempimento e come specificare un valore del fattore di riempimento in un indice in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 L'opzione del fattore di riempimento viene fornita per ottimizzare l'archiviazione dati e le prestazioni degli indici. Quando si crea o si ricompila un indice, il valore del fattore di riempimento determina la percentuale di spazio in ogni pagina al livello foglia da riempire di dati, riservando lo spazio rimanente in ogni pagina come spazio libero per la crescita futura. Se ad esempio si specifica un fattore di riempimento pari a 80, significa che il 20% di ogni pagina al livello foglia verrà lasciato vuoto, rendendo disponibile lo spazio per l'espansione dell'indice con l'aggiunta di dati alla tabella sottostante. Lo spazio vuoto viene riservato tra le righe anziché alla fine dell'indice.  
  
 Il valore del fattore di riempimento è una percentuale compresa tra 1 e 100 e l'impostazione predefinita a livello di server è 0. Questo significa che le pagine al livello foglia vengono riempite completamente.  
  
> [!NOTE]  
>  I valori 0 e 100 del fattore di riempimento sono equivalenti.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Considerazioni sulle prestazioni](#Performance)  
  
     [Sicurezza](#Security)  
  
-   **Per specificare un fattore di riempimento in un indice tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="performance-considerations"></a><a name="Performance"></a> Considerazioni sulle prestazioni  
  
#### <a name="page-splits"></a>Divisioni di pagina  
 Un valore del fattore di riempimento scelto correttamente consente di ridurre le potenziali divisioni di pagina rendendo disponibile uno spazio sufficiente per l'espansione dell'indice durante l'aggiunta di dati alla tabella sottostante. Quando viene aggiunta una nuova riga a una pagina di indice completa, tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene spostata circa la metà delle righe in una nuova pagina per lasciare spazio per la nuova riga. Questa riorganizzazione è nota come divisione di pagina. Una divisione di pagina consente di creare spazio per nuovi record, ma può richiedere una certa quantità di tempo e un elevato utilizzo di risorse. Può inoltre provocare la frammentazione, che a sua volta causa l'aumento delle operazioni di I/O. Quando si verificano frequenti divisioni di pagina, l'indice può essere ricompilato utilizzando un valore del fattore di riempimento nuovo o esistente per ridistribuire i dati. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Benché un valore del fattore di riempimento basso e diverso da 0 può ridurre la necessità di dividere le pagine con l'aumento delle dimensioni dell'indice, l'indice richiederà uno spazio di archiviazione maggiore, con possibili effetti negativi sulle prestazioni di lettura. Anche per un'applicazione progettata per il supporto di numerose operazioni di inserimento e aggiornamento, il numero di letture nel database è in genere maggiore del numero di scritture in base a un fattore di 5 a 11. Se pertanto si specifica un fattore di riempimento diverso dal valore predefinito, è possibile che le prestazioni di lettura del database risultino ridotte in misura inversamente proporzionale all'impostazione del fattore di riempimento. Un valore del fattore di riempimento pari a 50 può ad esempio dimezzare le prestazioni di lettura del database. Ciò avviene perché l'indice contiene più pagine, aumentando pertanto le operazioni di I/O su disco necessarie per recuperare i dati.  
  
#### <a name="adding-data-to-the-end-of-the-table"></a>Aggiunta di dati alla fine della tabella  
 Un valore del fattore di riempimento diverso da 0 o 100 può risultare efficace per le prestazioni se i nuovi dati vengono distribuiti uniformemente in tutta la tabella. Se tuttavia tutti i dati vengono aggiunti alla fine della tabella, lo spazio vuoto nelle pagine di indice non verrà riempito. Se ad esempio la colonna chiave dell'indice è una colonna IDENTITY, la chiave per le nuove righe aumenta di continuo e le righe di indice vengono aggiunte logicamente alla fine dell'indice. Se le righe esistenti verranno aggiornate con dati che ne aumentano le dimensioni, utilizzare un fattore di riempimento minore di 100. I byte aggiuntivi in ogni pagina consentiranno di ridurre le divisioni di pagina causate dalla maggiore lunghezza delle righe.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-specify-a-fill-factor-by-using-table-designer"></a>Per specificare un fattore di riempimento tramite Progettazione tabelle  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si vuole specificare il fattore di riempimento di un indice.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella in cui si vuole specificare il fattore di riempimento di un indice e selezionare **Progettazione**.  
  
4.  Scegliere **Indici/chiavi** nel menu **Progettazione tabelle**.  
  
5.  Selezionare l'indice con il fattore di riempimento che si desidera specificare.  
  
6.  Espandere **Specifica riempimento**, selezionare la riga **Fattore di riempimento** e immettere il fattore di riempimento desiderato nella riga.  
  
7.  Fare clic su **Close**.  
  
8.  Selezionare **Salva** table_name **dal menu**_File_.  
  
#### <a name="to-specify-a-fill-factor-in-an-index-by-using-object-explorer"></a>Per specificare un fattore di riempimento in un indice tramite Esplora oggetti  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si vuole specificare il fattore di riempimento di un indice.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si vuole specificare il fattore di riempimento di un indice.  
  
4.  Fare clic sul segno più per espandere la cartella **Indici** .  
  
5.  Fare clic con il pulsante destro del mouse sull'indice con il fattore di riempimento che si vuole specificare e scegliere **Proprietà**.  
  
6.  In **Selezione pagina** selezionare **Opzioni**.  
  
7.  Nella riga **Fattore di riempimento** immettere il fattore di riempimento desiderato.  
  
8.  Fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-specify-a-fill-factor-in-an-existing-index"></a>Per specificare un fattore di riempimento in un indice esistente  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio viene ricompilato un indice esistente e viene applicato il fattore di riempimento specificato durante l'operazione di ricompilazione.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Rebuilds the IX_Employee_OrganizationLevel_OrganizationNode index   
    -- with a fill factor of 80 on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD WITH (FILLFACTOR = 80);   
    GO  
    ```  
  
#### <a name="another-way-to-specify-a-fill-factor-in-an-index"></a>Modalità alternativa per specificare un fattore di riempimento in un indice  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    /* Drops and re-creates the IX_Employee_OrganizationLevel_OrganizationNode index on the HumanResources.Employee table with a fill factor of 80.  
    */  
  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)   
    WITH (DROP_EXISTING = ON, FILLFACTOR = 80);   
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
