---
title: Compattare un database | Microsoft Docs
description: Informazioni su come compattare un database mediante Esplora oggetti in SQL Server utilizzando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkdatabase.f1
helpviewer_keywords:
- shrinking databases
- databases [SQL Server], shrinking
- decreasing database size
- database shrinking [SQL Server]
- reducing database size
ms.assetid: 83afbf74-fd50-4c39-831c-b1f473a50620
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 94fb9ab933a4bd2ed7d42f1b43f86a48df6b3b35
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813001"
---
# <a name="shrink-a-database"></a>Compattare un database
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento si illustra come compattare un database mediante Esplora oggetti in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Compattando i file di dati si recupera spazio spostando le pagine di dati dalla fine del file allo spazio non occupato più vicino all'inizio del file. Quando alla fine del file viene creato sufficiente spazio libero, le pagine di dati possono essere deallocate e restituite al file system.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per compattare un database utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [compattare un database](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile ridurre il database a dimensioni inferiori a quelle minime. Le dimensioni minime corrispondono alle dimensioni specificate quando il database è stato inizialmente creato o alle ultime dimensioni impostate in modo esplicito mediante un'operazione di modifica delle dimensioni dei file, ad esempio DBCC SHRINKFILE. Pertanto, se originariamente è stato creato un database con dimensioni pari a 10 MB e le dimensioni sono aumentate fino a 100 MB, è possibile compattare il database fino a un minimo di 10 MB, anche se tutti i dati nel database sono stati eliminati.  
  
-   Non è possibile compattare un database mentre ne viene eseguito il backup e non è possibile eseguire il backup di un database mentre è in corso un'operazione di compattazione.
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Per visualizzare la quantità corrente di spazio disponibile, cioè non allocato, nel database. Per altre informazioni, vedere [Visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
  
-   Quando si pianifica la compattazione di un database, considerare le informazioni seguenti:  
  
    -   Un'operazione di compattazione è più efficace dopo l'esecuzione di un'operazione che crea una quantità elevata di spazio inutilizzato, ad esempio il troncamento o l'eliminazione di una tabella.  
  
    -   La maggior parte dei database richiede spazio disponibile per lo svolgimento delle normali attività quotidiane. Se si compatta ripetutamente un database ma le sue dimensioni aumentano di nuovo significa che lo spazio compattato è necessario per le normali operazioni. In questi casi è inutile compattare ripetutamente il database.  
  
    -   L'operazione di compattazione generalmente aumenta la frammentazione degli indici del database. Questo è un ulteriore motivo per evitare di compattare ripetutamente un database.  
  
    -   Se non è necessario soddisfare esigenze specifiche, non impostare l'opzione di database AUTO_SHRINK su ON.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-shrink-a-database"></a>Per compattare un database  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], quindi espandere questa istanza.  
  
2.  Espandere **Database**, quindi fare clic con il pulsante destro del mouse sul database che si vuole compattare.  
  
3.  Scegliere **Attività**, **Compatta**, quindi fare clic su **Database**.  
  
     **Database**  
     Consente di visualizzare il nome del database selezionato.  
  
     **Spazio allocato**  
     Consente di visualizzare lo spazio totale utilizzato e inutilizzato per il database selezionato.  
  
     **Spazio disponibile**  
     Consente di visualizzare lo spazio disponibile totale nei file di log e di dati del database selezionato.  
  
     **Riorganizza i file prima di rilasciare lo spazio inutilizzato**  
     La selezione di questa opzione equivale all'esecuzione di DBCC SHRINKDATABASE specificando la percentuale di compattazione. Deselezionare l'opzione equivale all'esecuzione di DBCC SHRINKDATABASE con l'opzione TRUNCATEONLY. Per impostazione predefinita, questa opzione non è selezionata all'apertura della finestra di dialogo. Se viene selezionata, l'utente deve specificare la percentuale di compattazione.  
  
     **Spazio massimo disponibile nei file dopo la compattazione**  
     Immettere la massima percentuale di spazio che si desidera sia disponibile nei file del database dopo la compattazione del database. I valori consentiti sono compresi tra 0 e 99.  
  
4.  Fare clic su **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-shrink-a-database"></a>Per compattare un database  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio si utilizza [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md) per ridurre le dimensioni dei file di dati e di log nel database `UserDB` e per ottenere il `10` percento di spazio disponibile nel database.  
  
 [!code-sql[DBCC#DBCC_SHRINKDB1](../../relational-databases/databases/codesnippet/tsql/shrink-a-database_1.sql)]  
  
##  <a name="follow-up-after-you-shrink-a-database"></a><a name="FollowUp"></a> Completamento: dopo la compattazione di un database  
 I dati spostati per ridurre un file possono essere dispersi in qualsiasi percorso disponibile nel file, provocando la frammentazione dell'indice e rallentando le prestazioni di query che eseguono ricerche in un intervallo dell'indice Per eliminare la frammentazione, valutare la possibilità di ricompilare gli indici sul file dopo la compattazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Compattare un file](../../relational-databases/databases/shrink-a-file.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)   
 [Filegroup e file di database](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
