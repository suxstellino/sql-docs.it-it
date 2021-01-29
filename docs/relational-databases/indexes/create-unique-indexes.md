---
description: Creare indici univoci
title: Creare indici univoci | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- unique indexes
- designing indexes [SQL Server], unique
- clustered indexes, unique
- indexes [SQL Server], unique
- nonclustered indexes [SQL Server], unique
- unique indexes, design guidelines
ms.assetid: 56b5982e-cb94-46c0-8fbb-772fc275354a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2c41e97ad36387fe1f3644e5039eb5cd2b6a61d
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048936"
---
# <a name="create-unique-indexes"></a>Creare indici univoci
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento si illustra come creare un indice univoco per una tabella in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un indice univoco consente di garantire che nella chiave dell'indice non siano contenuti valori duplicati e che pertanto ogni riga della tabella sia univoca. Non vi sono differenze significative tra la creazione di un vincolo UNIQUE e la creazione di un indice univoco indipendente da un vincolo. La convalida dei dati viene eseguita nello stesso modo e da Query Optimizer non viene applicata alcuna distinzione tra un indice univoco creato tramite un vincolo o manualmente. Tuttavia, se si crea un vincolo UNIQUE nella colonna, l'obiettivo dell'indice è chiaro. Per ulteriori informazioni sui vincoli UNIQUE, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 Quando si crea un indice univoco, è possibile impostare un'opzione che consente di ignorare le chiavi duplicate. Se questa opzione è impostata su **Sì** e si cerca di creare chiavi duplicate aggiungendo dati che coinvolgono più righe (tramite l'istruzione INSERT), la riga che contiene la chiave duplicata non verrà aggiunta. Se invece l'opzione è impostata su **No**, l'intera operazione non verrà completata e verrà eseguito il rollback di tutti i dati.  
  
> [!NOTE]  
>  Non è possibile creare un indice univoco in un'unica colonna se in essa è contenuto il valore NULL in più righe. Analogamente, non è possibile creare un indice univoco su più colonne se la combinazione di colonne contiene il valore NULL in più righe. Ai fini dell'indicizzazione, questi valori sono considerati duplicati.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Vantaggi di un indice univoco](#Benefits)  
  
     [Modalità di implementazione tipiche](#Implementations)  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare un indice univoco per una tabella utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="benefits-of-a-unique-index"></a><a name="Benefits"></a> Vantaggi di un indice univoco  
  
-   Gli indici univoci a più colonne consentono di garantire che ogni combinazione di valori nella chiave dell'indice sia univoca. Ad esempio, se si crea un indice univoco basato su una combinazione delle colonne **LastName**, **FirstName** e **MiddleName** , non è possibile che nella tabella siano incluse due righe in cui è presente la stessa combinazione di valori per queste colonne.  
  
-   Se i dati in ogni colonna sono univoci, nella stessa tabella è possibile creare sia un indice cluster univoco sia più indici non cluster univoci.  
  
-   Gli indici univoci consentono di garantire l'integrità dei dati delle colonne definite.  
  
-   Gli indici univoci forniscono informazioni aggiuntive utili a Query Optimizer tramite cui è possibile produrre piani di esecuzione più efficienti.  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Modalità di implementazione tipiche  
 Gli indici univoci vengono implementati nei modi seguenti:  
  
-   **Vincolo PRIMARY KEY o UNIQUE**  
  
     Quando si crea un vincolo PRIMARY KEY, viene automaticamente creato un indice cluster univoco nella colonna o nelle colonne se nella tabella non esiste già un indice cluster e non si specifica un indice non cluster univoco. La colonna chiave primaria non può supportare i valori NULL.  
  
     Quando si crea un vincolo UNIQUE, viene creato un indice non cluster univoco per applicare un vincolo UNIQUE per impostazione predefinita. È possibile specificare un indice cluster univoco se nella tabella non ne esiste già uno.  
  
     Per ulteriori informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md) e [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md).  
  
-   **Indice indipendente da un vincolo**  
  
     In una tabella è possibile definire più indici non cluster.  
  
     Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   **Vista indicizzata**  
  
     Per creare una vista indicizzata, viene definito un indice cluster univoco in una o più colonne della vista. La vista viene eseguita e il set di risultati viene archiviato nel livello foglia allo stesso modo in cui vengono archiviati i dati della tabella in un indice cluster. Per altre informazioni, vedere [Creare viste indicizzate](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se nei dati sono presenti valori di chiave duplicati, non è possibile creare un indice univoco, un vincolo UNIQUE o un vincolo PRIMARY KEY.  
  
-   In un indice non cluster univoco possono essere contenute colonne non chiave. Per altre informazioni, vedere [Creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-unique-index-by-using-the-table-designer"></a>Per creare un indice univoco tramite Progettazione tabelle  
  
1.  In Esplora oggetti espandere il database contenente la tabella in cui si desidera creare un indice univoco.  
  
2.  Espandere la cartella **Tabelle** .  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella nella quale creare un indice univoco e selezionare **Progetta**.  
  
4.  Selezionare **Indici/chiavi** nel menu **Progettazione tabelle**.  
  
5.  Nella finestra di dialogo **Indici/chiavi** fare clic su **Aggiungi**.  
  
6.  Selezionare il nuovo indice dalla casella di testo **Chiave o indice primario/univoco selezionato** .  
  
7.  In **(Generale)** nella griglia principale selezionare **Tipo** , quindi scegliere **Indice** dall'elenco.  
  
8.  Selezionare **Colonne** e quindi fare clic sul pulsante con i puntini di sospensione **(...)** .  
  
9. In **Nome colonna** della finestra di dialogo **Colonne indice** selezionare le colonne da indicizzare. È possibile selezionare fino a 16 colonne. Per ottenere prestazioni ottimali, selezionare una o due colonne per indice. Per ogni colonna selezionata, è possibile specificare se nell'indice i valori della colonna dovranno essere organizzati in ordine crescente o decrescente.  
  
10. Una volta selezionate tutte le colonne per l'indice, fare clic su **OK**.  
  
11. In **(Generale)** nella griglia principale selezionare **Univoco** , quindi scegliere **Sì** dall'elenco.  
  
12. Facoltativo: In **Progettazione tabelle** nella griglia principale selezionare **Ignora chiavi duplicate** , quindi scegliere **Sì** dall'elenco. Eseguire questa operazione se si desidera ignorare i tentativi per aggiungere dati che comporterebbero la creazione di una chiave duplicata nell'indice univoco.  
  
13. Fare clic su **Close**.  
  
14. Dal menu **File** scegliere **Salva**_nome\_tabella_.  
  
#### <a name="create-a-unique-index-by-using-object-explorer"></a>Creare un indice univoco tramite Esplora oggetti  
  
1.  In Esplora oggetti espandere il database contenente la tabella in cui si desidera creare un indice univoco.  
  
2.  Espandere la cartella **Tabelle** .  
  
3.  Espandere la tabella in cui si desidera creare un indice univoco.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Indici**, scegliere **Nuovo indice** e selezionare **Indice non cluster**.  
  
5.  Nella pagina **Generale** della finestra di dialogo **Nuovo indice** immettere il nome del nuovo indice nella casella **Nome indice** .  
  
6.  Selezionare la casella di controllo **Univoco** .  
  
7.  In **Colonne chiave indice** fare clic su **Aggiungi**.  
  
8.  Nella finestra di dialogo **Seleziona colonne da**_nome\_tabella_ selezionare le caselle di controllo delle colonne della tabella da aggiungere all'indice univoco.  
  
9. Fare clic su **OK**.  
  
10. Nella finestra di dialogo **Nuovo indice** fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-unique-index-on-a-table"></a>Per creare un indice univoco per una tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named AK_UnitMeasure_Name and delete it if found  
    IF EXISTS (SELECT name from sys.indexes  
               WHERE name = N'AK_UnitMeasure_Name')   
       DROP INDEX AK_UnitMeasure_Name ON Production.UnitMeasure;   
    GO  
    -- Create a unique index called AK_UnitMeasure_Name  
    -- on the Production.UnitMeasure table using the Name column.  
    CREATE UNIQUE INDEX AK_UnitMeasure_Name   
       ON Production.UnitMeasure (Name);   
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
