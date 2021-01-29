---
description: Disabilitazione di indici e vincoli
title: Disabilitare indici e vincoli | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.disableindexes.f1
helpviewer_keywords:
- disabled indexes [SQL Server], index operations
- nonclustered indexes [SQL Server], disabling
- disabled indexes [SQL Server], guidelines
- clustered indexes, disabling
- constraints [SQL Server], disabling
- disabled indexes [SQL Server], viewing
- FOREIGN KEY constraints, disabling
- statistical information [SQL Server], indexes
- index disabling [SQL Server]
- indexed views [SQL Server], disabled indexes
ms.assetid: 2198f1af-fa44-47e9-92df-f4fde322ba18
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97718ff757e09cd33116c47f31155f6ae0edb08b
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048894"
---
# <a name="disable-indexes-and-constraints"></a>Disabilitazione di indici e vincoli
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento si descrive come disabilitare un indice o i vincoli in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La disabilitazione di un indice impedisce all'utente di accedere all'indice e, per gli indici cluster, ai dati della tabella sottostante. La definizione dell'indice viene mantenuta nei metadati e le statistiche relative all'indice vengono preservate negli indici non cluster. La disabilitazione di un indice non cluster o cluster di una vista consente di eliminare fisicamente i dati dell'indice. La disabilitazione di un indice cluster in una tabella impedisce l'accesso ai dati. Questi ultimi vengono comunque mantenuti nella tabella, ma non sono disponibili per le operazioni DML (Data Manipulation Language) finché l'indice non viene eliminato o ricompilato.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per disabilitare un indice tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   In caso di disabilitazione, un indice non viene aggiornato.  
  
-   In Query Optimizer l'indice disabilitato non viene considerato durante la creazione dei piani di esecuzione della query. Inoltre, le query che fanno riferimento all'indice disabilitato con un hint di tabella non vengono eseguite correttamente.  
  
-   Non è possibile creare un indice assegnandogli lo stesso nome di un indice disabilitato esistente.  
  
-   Un indice disabilitato può essere eliminato.  
  
-   Quando si disabilita un indice univoco, vengono disabilitati anche il vincolo PRIMARY KEY o UNIQUE e tutti i vincoli FOREIGN KEY che fanno riferimento alle colonne indicizzate di altre tabelle. Quando si disabilita un indice cluster, vengono disabilitati anche tutti i vincoli FOREIGN KEY in ingresso e in uscita nella tabella sottostante. Quando viene disabilitato l'indice, i nomi dei vincoli vengono elencati in un messaggio di avviso. Dopo aver ricompilato l'indice, è necessario abilitare manualmente tutti i vincoli utilizzando l'istruzione ALTER TABLE CHECK CONSTRAINT.  
  
-   Gli indici non cluster vengono disabilitati automaticamente quando viene disabilitato l'indice cluster associato e non possono essere abilitati fino all'abilitazione dell'indice cluster nella tabella o nella vista o all'eliminazione dell'indice cluster nella tabella. Gli indici non cluster devono essere abilitati in modo esplicito, a meno che l'indice cluster non sia stato abilitato utilizzando l'istruzione ALTER INDEX ALL REBUILD.  
  
-   L'istruzione ALTER INDEX ALL REBUILD consente di ricompilare e abilitare tutti gli indici disabilitati nella tabella, ad eccezione degli indici disabilitati nelle viste. Gli indici nelle viste devono essere abilitati in un'istruzione ALTER INDEX ALL REBUILD distinta.  
  
-   La disabilitazione di un indice cluster in una tabella consente di disabilitare anche tutti gli indici cluster e non cluster nelle viste che fanno riferimento a quella tabella. Questi indici devono essere ricompilati immediatamente dopo quelli inclusi nella tabella cui viene fatto riferimento.  
  
-   È possibile accedere alle righe di dati degli indici cluster disabilitati solo per eliminare o ricompilare l'indice cluster.  
  
-   È possibile ricompilare online un indice non cluster disabilitato quando nella tabella non è incluso un indice cluster disabilitato. Tuttavia, è sempre necessario ricompilare offline un indice cluster disabilitato se si utilizza l'istruzione ALTER INDEX REBUILD o CREATE INDEX WITH DROP_EXISTING. Per altre informazioni sulle operazioni online sugli indici, vedere [Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md).  
  
-   L'istruzione CREATE STATISTICS non può essere eseguita correttamente in una tabella in cui è incluso un indice cluster disabilitato.  
  
-   L'opzione di database AUTO_CREATE_STATISTICS crea nuove statistiche per una colonna quando l'indice viene disabilitato e si verificano le condizioni seguenti:  
  
    -   L'opzione AUTO_CREATE_STATISTICS è impostata su ON  
  
    -   Non è disponibile alcuna statistica esistente per la colonna.  
  
    -   Le statistiche sono necessarie durante l'ottimizzazione delle query.  
  
-   Se un indice cluster è disabilitato, tramite l'istruzione [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) non possono essere restituite informazioni sulla tabella sottostante, ma può essere indicato che l'indice cluster è disabilitato. L'istruzione[DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) non può essere utilizzata per deframmentare un indice disabilitato, altrimenti l'operazione non viene completata e viene visualizzato un messaggio di errore. Per ricompilare un indice disabilitato, è possibile utilizzare l'istruzione [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md) .  
  
-   La creazione di un nuovo indice cluster comporta l'abilitazione degli indici non cluster disabilitati precedentemente. Per altre informazioni, vedere [Enable Indexes and Constraints](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per eseguire l'istruzione ALTER INDEX, è necessario disporre almeno dell'autorizzazione ALTER per la tabella o la vista.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-disable-an-index"></a>Per disabilitare un indice  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera disabilitare un indice.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera disabilitare un indice.  
  
4.  Fare clic sul segno più per espandere la cartella **Indici** .  
  
5.  Fare clic con il pulsante destro del mouse sull'indice che si vuole disabilitare e selezionare **Disabilita**.  
  
6.  Nella finestra di dialogo **Disabilita indici** verificare che nella griglia **Indici da disabilitare** sia presente l'indice corretto e fare clic su **OK**.  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>Per disabilitare tutti gli indici di una tabella  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera disabilitare gli indici.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera disabilitare gli indici.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Indici** e selezionare **Disabilita tutti**.  
  
5.  Nella finestra di dialogo **Disabilita indici** verificare che nella griglia **Indici da disabilitare** siano presenti gli indici corretti e fare clic su **OK**. Per rimuovere un indice dalla griglia **Indici da disabilitare** , selezionare l'indice desiderato, quindi premere il tasto CANC.  
  
 Le informazioni seguenti sono disponibili nella finestra di dialogo **Disabilita indici** :  
  
 **Index Name**  
 Consente di visualizzare il nome dell'indice. Durante l'esecuzione, in questa colonna viene anche visualizzata un'icona che ne indica lo stato.  
  
 **Nome tabella**  
 Visualizza il nome della tabella o della vista in cui l'indice è stato creato.  
  
 **Tipo di indice**  
 Visualizza il tipo di indice: **Cluster**, **Non cluster**, **Spaziale** o **XML**.  
  
 **Status**  
 Visualizza lo stato dell'operazione di disabilitazione. I valori possibili al termine dell'esecuzione sono:  
  
-   Vuoto  
  
     Prima dell'esecuzione l'indicazione **Stato** è vuota.  
  
-   **In corso**  
  
     L'operazione di disabilitazione degli indici è stata avviata ma non ancora completata.  
  
-   **Success**  
  
     L'operazione di disabilitazione è stata completata.  
  
-   **Error (Errore) (Error (Errore)e)**  
  
     Si è verificato un errore durante la disabilitazione degli indici e non è stato possibile completare correttamente l'operazione.  
  
-   **Arrestato**  
  
     La disabilitazione dell'indice non è stata completata poiché l'operazione è stata arrestata dall'utente.  
  
 **Message**  
 Visualizza il testo dei messaggi di errore generati durante l'operazione. Durante l'esecuzione dell'operazione, gli errori vengono visualizzati come collegamenti ipertestuali. Nel testo di tali collegamenti è descritto l'errore verificatosi. La colonna **Messaggio** in genere non è sufficientemente ampia per contenere il testo completo del messaggio. Per leggere il messaggio completo, eseguire una delle seguenti operazioni:  
  
-   Spostare il puntatore del mouse sulla cella del messaggio per visualizzare una descrizione comando contenente il testo dell'errore.  
  
-   Fare clic sul collegamento ipertestuale per visualizzare una finestra di dialogo contenente l'errore completo.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-disable-an-index"></a>Per disabilitare un indice  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- disables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    DISABLE;  
    ```  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>Per disabilitare tutti gli indici di una tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Disables all indexes on the HumanResources.Employee table.  
    ALTER INDEX ALL ON HumanResources.Employee  
    DISABLE;  
    ```  
  
 Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
