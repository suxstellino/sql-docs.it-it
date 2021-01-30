---
title: Gestire la tabella suspect_pages (SQL Server) | Microsoft Docs
description: Informazioni su come gestire la tabella suspect_pages in SQL Server tramite SQL Server Management Studio o Transact-SQL. Le pagine che producono determinati errori sono sospette.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- 824 (Database Engine error)
- restoring pages [SQL Server]
- pages [SQL Server], suspect
- pages [SQL Server], restoring
- suspect_pages system table
- suspect pages [SQL Server]
- restoring [SQL Server], pages
ms.assetid: f394d4bc-1518-4e61-97fc-bf184d972e2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 88859e39a29c227d2cbb56328fbc96a4de7528d1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076909"
---
# <a name="manage-the-suspect_pages-table-sql-server"></a>Gestione della tabella suspect_pages (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come gestire la tabella **suspect_pages** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La tabella **suspect_pages** , usata per la gestione di informazioni sulle pagine sospette, è importante per stabilire se è necessario un ripristino. La tabella [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) è contenuta nel [database msdb](../../relational-databases/databases/msdb-database.md).  
  
 Una pagina è considerata "sospetta" quando nel [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] si verifica uno dei seguenti errori quando viene tentata la lettura di una pagina di dati:  
  
-   Un errore 823 causato da un controllo di ridondanza ciclico (CRC) generato dal sistema operativo, ad esempio un errore del disco (alcuni errori hardware)  
  
-   Un errore 824, ad esempio una pagina incompleta (qualsiasi errore logico)  
  
 L'ID di ogni pagina sospetta viene registrato nella tabella **suspect_pages** . [!INCLUDE[ssDE](../../includes/ssde-md.md)] registra tutte le pagine sospette rilevate durante la normale elaborazione, ad esempio nei casi seguenti:  
  
-   Una pagina deve essere letta da una query.  
  
-   Durante un'operazione DBCC CHECKDB.  
  
-   Durante un'operazione di backup.  
  
 La tabella **suspect_pages** viene aggiornata in base alle necessità durante un'operazione di ripristino, di correzione DBCC o di rimozione del database.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per gestire la tabella suspect_pages utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   **Errori registrati nella tabella suspect_pages**  
  
     La tabella **suspect_pages** include una riga per ogni pagina che ha restituito un errore 824, fino a un limite di 1.000 righe. Nella seguente tabella vengono mostrati errori registrati nella colonna **event_type** della tabella **suspect_pages** .  
  
    |Descrizione dell'errore|Valore **event_type**|  
    |-----------------------|---------------------------|  
    |Errore 823 causato da un errore CRC del sistema operativo o errore 824 diverso da un errore nel checksum o da una pagina incompleta (ad esempio un ID pagina errato)|1|  
    |Errore nel checksum|2|  
    |Pagina incompleta|3|  
    |Pagina ripristinata (la pagina è stata ripristinata dopo essere stata contrassegnata come danneggiata)|4|  
    |Pagina corretta (la pagina è stata corretta da DBCC)|5|  
    |Pagina deallocata da DBCC|7|  
  
     Nella tabella **suspect_pages** vengono anche registrati gli errori temporanei.  Tra le origini degli errori temporanei rientrano gli errori di I/O, ad esempio un cavo disconnesso, o le pagine che non superano temporaneamente un test di checksum ripetuto.  
  
-   **Procedura di aggiornamento della tabella suspect_pages tramite il Motore di database**  
  
     Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue le azioni seguenti nella tabella **suspect_pages** :  
  
    -   Se la tabella non è piena, viene aggiornata per ogni errore 824 in modo da segnalare il verificarsi dell'errore e viene incrementato il contatore degli errori. Se una pagina contiene un errore dopo l'esecuzione di un'operazione di correzione, ripristino o deallocazione, il conteggio **number_of_errors** corrispondente viene incrementato e la relativa colonna **last_update** viene aggiornata  
  
    -   Dopo l'esecuzione di un'operazione di ripristino o di correzione di una pagina elencata, la riga **suspect_pages** viene aggiornata per indicare che la pagina è stata corretta (**event_type** = 5) o ripristinata (**event_type** = 4).  
  
    -   Se viene eseguito un controllo DBCC, tutte le pagine prive di errori vengono contrassegnate come corrette (**event_type** = 5) o deallocate (**event_type** = 7).  
  
-   **Aggiornamenti automatici della tabella suspect_pages**  
  
     Un partner di mirroring di database o una replica di disponibilità AlwaysOn aggiorna la tabella **suspect_pages** dopo che un tentativo di leggere una pagina da un file di dati non riesce per una delle seguenti ragioni.  
  
    -   Un errore del 823 causato da un errore CRC del sistema operativo.  
  
    -   Un errore 824 (danneggiamento logico, ad esempio una pagina incompleta)  
  
     Le azioni seguenti aggiornano anche automaticamente righe nella tabella **suspect_pages** .  
  
    -   L'azione DBCC CHECKDB REPAIR_ALLOW_DATA_LOSS aggiorna la tabella **suspect_pages** per indicare ogni pagina deallocata o corretta.  
  
    -   In seguito a un ripristino (RESTORE) completo del file o della pagina, le voci della pagina vengono contrassegnate come ripristinate.  
  
     Le azioni seguenti eliminano automaticamente righe dalla tabella **suspect_pages** .  
  
    -   ALTER DATABASE REMOVE FILE  
  
    -   DROP DATABASE  
  
-   **Ruolo di gestione dell'amministratore del database**  
  
     Gli amministratori dei database sono responsabili della gestione della tabella, in particolare dell'eliminazione delle righe meno recenti. Poiché le dimensioni della tabella **suspect_pages** sono limitate, se questa si riempie, non verranno registrati nuovi errori. Per evitare che lo spazio della tabella si esaurisca, l'amministratore del database o l'amministratore di sistema devono cancellare manualmente i dati meno recenti dalla tabella tramite l'eliminazione delle righe. È quindi consigliabile archiviare o eliminare periodicamente le righe aventi un valore **event_type** ripristinato o riparato oppure le righe con un valore **last_update** obsoleto.  
  
     Per monitorare l'attività sulla tabella suspect_pages è possibile usare [Classe di evento Database Suspect Data Page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md). Talvolta, a causa di errori temporanei, vengono aggiunte righe alla tabella **suspect_pages** . Tuttavia se vengono aggiunte molte righe alla tabella è probabile che vi sia un problema con il sottosistema I/O. Se si nota un aumento improvviso nel numero di righe che vengono aggiunte alla tabella, si consiglia di esaminare possibili problemi nel sottosistema I/O.  
  
     L'amministratore del database può inoltre inserire o aggiornare i record. Ad esempio, l'aggiornamento di una riga potrebbe essere utile se l'amministratore del database è certo che una determinata pagina sospetta è in realtà rimasta invariata, ma desidera mantenere temporaneamente il record.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Chiunque abbia accesso a **msdb** può leggere i dati nella tabella **suspect_pages** . Chiunque disponga dell'autorizzazione UPDATE nella tabella suspect_pages può aggiornare i relativi record. I membri del ruolo predefinito del database **db_owner** in **msdb** o del ruolo predefinito del server **sysadmin** possono inserire, aggiornare ed eliminare i record.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-manage-the-suspect_pages-table"></a>Per gestire la tabella suspect_pages  
  
1.  In **Esplora oggetti** connettersi a un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], espandere tale istanza, quindi espandere **Database**.  
  
2.  Espandere **Database di sistema**, espandere **msdb**, espandere **Tabelle**, quindi espandere **Tabelle di sistema**.  
  
3.  Espandere **dbo.suspect_pages** e fare clic con il pulsante destro del mouse su **Modifica le prime 200 righe**.  
  
4.  Nella finestra Query, modificare, aggiornare o eliminare le righe desiderate.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-manage-the-suspect_pages-table"></a>Per gestire la tabella suspect_pages  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare gli esempi seguenti nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio seguente vengono eliminate alcune righe dalla tabella `suspect_pages` .  
  
```  
-- Delete restored, repaired, or deallocated pages.  
DELETE FROM msdb..suspect_pages  
   WHERE (event_type = 4 OR event_type = 5 OR event_type = 7);  
GO  
  
```  
  
 In questo esempio vengono restituite le pagine errate nella tabella `suspect_pages` .  
  
```  
-- Select nonspecific 824, bad checksum, and torn page errors.  
SELECT * FROM msdb..suspect_pages  
   WHERE (event_type = 1 OR event_type = 2 OR event_type = 3);  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [Ripristino di pagine &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
    
   
  
  



