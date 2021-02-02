---
title: Creare statistiche | Microsoft Docs
description: Informazioni su come creare statistiche di ottimizzazione query per le colonne di una tabella o di una vista indicizzata in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.stat.properties.f1
- sql13.swb.statistics.filter.f1
- sql13.swb.stat.columns.f1
- sql13.swb.statistics.propertis.f1
helpviewer_keywords:
- creating statistics
- statistics [SQL Server], creating
ms.assetid: 95a455fb-664d-4c95-851e-c6b62d7ebe04
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba784c0c2fc9733f482a5af09870b48951c6079d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250305"
---
# <a name="create-statistics"></a>Creare statistiche
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  È possibile creare statistiche di ottimizzazione delle query per una o più colonne di una tabella o una vista indicizzata in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per la maggior parte delle query, tramite Query Optimizer vengono già generate le statistiche necessarie per un piano di query di alta qualità, ma in alcuni casi è necessario creare statistiche aggiuntive.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Creare statistiche tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Prima di creare statistiche con l'istruzione CREATE STATISTICS, verificare che l'opzione AUTO_CREATE_STATISTICS sia impostata a livello di database. In questo modo, tramite Query Optimizer continuano a essere normalmente create statistiche di colonna singola per colonne dei predicati di query.  
  
-   È possibile elencare fino a 32 colonne per ogni oggetto statistiche.  
  
-   Non è possibile eliminare, rinominare o modificare la definizione di una colonna di tabella specificata in un predicato delle statistiche filtrate.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'utente deve essere il proprietario della tabella o della vista indicizzata o un membro di uno dei seguenti ruoli: ruolo predefinito del server **sysadmin** , ruolo predefinito del database **db_owner** o ruolo predefinito del database **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-statistics"></a>Per creare statistiche  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il database in cui si desidera creare una nuova statistica.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera creare una nuova statistica.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Statistiche** e selezionare **Nuove statistiche...** .  
  
     Nella pagina **Generale** della finestra di dialogo **Nuove statistiche per Tabella** _nome\_tabella_ vengono visualizzate le proprietà seguenti.  
  
     **Nome tabella**  
     Consente di visualizzare il nome della tabella descritta dalle statistiche.  
  
     **Nome statistiche**  
     Consente di visualizzare il nome dell'oggetto di database in cui sono archiviate le statistiche.  
  
     **Colonne statistiche**  
     In questa griglia vengono visualizzate le colonne descritte dal set di statistiche specifico. Tutti i valori presenti nella griglia sono di sola lettura.  
  
     **Nome**  
     Consente di visualizzare il nome della colonna descritta dalle statistiche. Può trattarsi di una singola colonna o di una combinazione di colonne di una singola tabella.  
  
     **Tipo di dati**  
     Consente di indicare il tipo di dati delle colonne descritte dalle statistiche.  
  
     **Dimensione**  
     Consente di visualizzare le dimensioni del tipo di dati per ogni colonna.  
  
     **Identità**  
     Se è stata selezionata, questa opzione indica una colonna di identità.  
  
     **Consenti valori NULL**  
     Consente di indicare se la colonna accetta valori NULL.  
  
     **Aggiungere**  
     Consente di aggiungere ulteriori colonne dalla tabella alla griglia delle statistiche.  
  
     **Rimuovi**  
     Consente di rimuovere la colonna selezionata dalla griglia delle statistiche.  
  
     **Sposta su**  
     Consente di spostare la colonna selezionata in una posizione precedente nella griglia delle statistiche. La posizione all'interno della griglia può avere un impatto significativo sull'utilità delle statistiche.  
  
     **Sposta giù**  
     Consente di spostare la colonna selezionata in una posizione successiva nella griglia delle statistiche.  
  
     **Ultimo aggiornamento delle statistiche per le colonne selezionate:**  
     Indica la data delle statistiche. Le statistiche sono più utili se sono aggiornate. Aggiornare le statistiche dopo aver apportato un gran numero di modifiche ai dati o aver aggiunto dati non comuni. Le statistiche per le tabelle con una distribuzione dei dati uniforme devono essere aggiornate con minore frequenza.  
  
     **Aggiorna statistiche per le colonne selezionate**  
     Selezionare questa opzione per aggiornare le statistiche alla chiusura della finestra di dialogo.  
  
     Nella pagina **Filtro** della finestra di dialogo **Nuove statistiche per Tabella** _nome\_tabella_ viene visualizzata la proprietà seguente.  
  
     **Espressione filtro**  
     Specifica le righe di dati da includere nelle statistiche filtrate. Ad esempio, usare `Production.ProductSubcategoryID IN ( 1,2,3 )`  
  
5.  Nella pagina **Generale** della finestra di dialogo **Nuove statistiche per Tabella** _nome\_tabella_ fare clic su **Aggiungi**.  
  
     Nella finestra di dialogo **Seleziona colonne** vengono visualizzate le proprietà seguenti. Queste informazioni sono di sola lettura.  
  
     **Nome**  
     Consente di visualizzare il nome della colonna descritta dalle statistiche. Può trattarsi di una singola colonna o di una combinazione di colonne di una singola tabella.  
  
     **Tipo di dati**  
     Consente di indicare il tipo di dati delle colonne descritte dalle statistiche.  
  
     **Dimensione**  
     Consente di visualizzare le dimensioni del tipo di dati per ogni colonna.  
  
     **Identità**  
     Se viene selezionata, questa opzione consente di indicare una colonna di identità.  
  
     **Consenti valori Null**  
     Consente di indicare se la colonna accetta valori NULL.  
  
6.  Nella finestra di dialogo **Seleziona colonne** selezionare le caselle di controllo per tutte le colonne per cui si desidera creare una statistica e quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Nuove statistiche per Tabella** _nome\_tabella_ fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-statistics"></a>Per creare statistiche  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- Create new statistic object called ContactMail1  
    -- on the BusinessEntityID and EmailPromotion columns in the Person.Person table.   
  
    CREATE STATISTICS ContactMail1  
        ON Person.Person (BusinessEntityID, EmailPromotion);   
    GO  
    ```  
  
4.  La statistica creata nella procedura precedente può garantire risultati migliori per la query seguente.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT LastName, FirstName  
    FROM Person.Person  
    WHERE EmailPromotion = 2  
    ORDER BY LastName, FirstName;   
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
  
