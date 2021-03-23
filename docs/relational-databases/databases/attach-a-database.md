---
title: Collegare un database | Microsoft Docs
description: Informazioni su come associare un database in SQL Server tramite SQL Server Management Studio o Transact-SQL. Usare questa funzionalità per copiare, spostare o aggiornare un database.
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.attachdatabase.f1
helpviewer_keywords:
- database attaching [SQL Server]
- attaching databases [SQL Server]
ms.assetid: b4efb0ae-cfe6-4d81-a4b4-6e4916885caa
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9912ecd1d8f258a343adf9db4e61d4e264e5273d
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890753"
---
# <a name="attach-a-database"></a>Collegare un database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento si illustra come collegare un database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. È possibile usare questa funzionalità per copiare, spostare o aggiornare un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Il database deve essere innanzitutto scollegato. Se si tenta di collegare un database che non è stato scollegato, verrà restituito un errore. Per altre informazioni, vedere [Scollegare un database](../../relational-databases/databases/detach-a-database.md).  
  
-   Durante il collegamento di un database è necessario che siano disponibili tutti i file di dati (file MDF e LDF). Se un file di dati si trova in un percorso diverso rispetto al momento della creazione o dell'ultimo collegamento del database, è necessario specificare il percorso corrente.  
  
-   Quando si collega un database, se i file MDF e LDF si trovano in directory diverse e uno dei percorsi include \\\\?\GlobalRoot, l'operazione avrà esito negativo.  
  
###  <a name="is-attach-the-best-choice"></a><a name="Recommendations"></a> Il collegamento è la scelta migliore?  
Quando si spostano file di database all'interno della stessa istanza, è consigliabile spostare i database usando la procedura di rilocazione pianificata `ALTER DATABASE` invece delle operazioni di scollegamento e collegamento. Per altre informazioni, vedere [Spostare database utente](../../relational-databases/databases/move-user-databases.md). 
 
Non è consigliabile usare le operazioni di collegamento e scollegamento da Backup e ripristino. Non esistono backup del log delle transazioni ed è possibile che i file vengano eliminati accidentalmente.
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
Le autorizzazioni di accesso ai file vengono impostate durante l'esecuzione di alcune operazioni del database, inclusi il collegamento e lo scollegamento. Per informazioni sulle autorizzazioni per i file impostate quando un database viene collegato o scollegato, vedere [Protezione dei dati e dei file di log](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) dalla [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] documentazione online (lettura ancora valida). 
  
È consigliabile evitare di collegare o ripristinare database provenienti da origini sconosciute o non attendibili. Tali database possono contenere codice dannoso che potrebbe eseguire codice [!INCLUDE[tsql](../../includes/tsql-md.md)] indesiderato o causare errori modificando lo schema o la struttura fisica di database. Prima di utilizzare un database da un'origine sconosciuta o non attendibile, eseguire [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database in un server non di produzione ed esaminare il codice contenuto nel database, ad esempio le stored procedure o altro codice definito dall'utente. Per altre informazioni sul collegamento di database e sulle modifiche apportate ai metadati in caso di collegamento di un database, vedere [Collegamento e scollegamento di un database (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
È necessaria l'autorizzazione `CREATE DATABASE`, `CREATE ANY DATABASE` o `ALTER ANY DATABASE`.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  

### <a name="to-attach-a-database"></a>Per collegare un database  
  
1.  In Esplora oggetti [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]ed espandere la vista di questa istanza in SSMS.  
  
2.  Fare clic con il pulsante destro del mouse su **Database** , quindi scegliere **Collega**.  
  
3.  Nella finestra di dialogo **Collega database** fare clic su **Aggiungi** per specificare il database da collegare, quindi nella finestra di dialogo **Individua file di database** selezionare l'unità disco in cui si trova il database ed espandere l'albero di directory per individuare e selezionare il file con estensione mdf del database, ad esempio:

     `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\AdventureWorks2012_Data.mdf`  
  
    > [!IMPORTANT]  
    > Se si tenta di selezionare un database già collegato, verrà generato un errore.  
  
     **Database da collegare**  
     Consente di visualizzare informazioni sui database selezionati.  
  
     \<no column header>  
     Consente di visualizzare un'icona che indica lo stato dell'operazione di collegamento. Le icone possibili sono illustrate di seguito nella descrizione di **Stato** .  
  
     **Percorso file MDF**  
     Consente di visualizzare il percorso e il nome del file MDF selezionato.  
  
     **Nome database**  
     Consente di visualizzare il nome del database.  
  
     **Collega come**  
     Facoltativamente, è possibile specificare un nome diverso per il database da collegare.  
  
     **Proprietario**  
     Consente di visualizzare un elenco a discesa di possibili proprietari del database in cui è possibile selezionare un proprietario diverso.  
  
     **Status**  
     Consente di visualizzare lo stato del base in base alla tabella seguente.  
  
    |Icona|Testo Stato|Descrizione|  
    |----------|-----------------|-----------------|  
    |(Nessuna icona)|(Nessun testo)|L'operazione di collegamento non è stata avviata o può essere sospesa per questo oggetto. È il valore predefinito all'apertura della finestra di dialogo.|  
    |Triangolo verde che punta a destra|In corso|L'operazione di collegamento è stata avviata ma non ancora completata.|  
    |Segno di spunta verde|Operazione completata|L'oggetto è stato collegato.|  
    |Cerchio rosso con croce bianca|Errore|Si è verificato un errore durante l'operazione. Il collegamento non è stato completato.|  
    |Cerchio con due quadranti neri a destra e a sinistra e due quadranti bianchi in alto e in basso|Arrestato|L'operazione di collegamento non è stata completata perché l'utente ne ha arrestato l'esecuzione.|  
    |Cerchio con freccia curva che punta in senso antiorario.|È stato eseguito il rollback|L'operazione di collegamento è stata completata ma ne è stato eseguito il rollback a causa di un errore durante il collegamento di un altro oggetto.|  
  
     **Messaggio**  
     Non viene visualizzato alcun messaggio oppure viene visualizzato il collegamento ipertestuale "Impossibile trovare il file".  
  
     **Aggiungere**  
     Consente di individuare i file principali del database necessari. Se l'utente seleziona un file con estensione mdf, le informazioni appropriate vengono inserite automaticamente nei rispettivi campi della griglia **Database da collegare** .  
  
     **Rimuovi**  
     Consente di rimuovere il file selezionato dalla griglia **Database da collegare** .  
  
     **"** *<nome_database>* **" dettagli database**  
     Consente di visualizzare i nomi dei file da collegare. Per verificare o modificare il percorso di un file, fare clic sul pulsante **Sfoglia** ( **...** ).  
  
    > [!NOTE]  
    > Se il file non esiste, nella colonna **Messaggio** verrà visualizzato il testo "File non trovato". Se non rilevato, un file di log può trovarsi in un'altra directory o essere stato eliminato. È necessario aggiornare il percorso del file nella griglia **Dettagli database** in modo che indichi la posizione corretta oppure rimuovere il file di log dalla griglia. Se non viene rilevato un file di dati con estensione ndf, è necessario aggiornare il percorso nella griglia in modo che indichi la posizione corretta.  
  
     **Nome file originale**  
     Consente di visualizzare il nome del file collegato appartenente al database.  
  
     **Tipo di file**  
     Indica il tipo di file, ovvero **Dati** o **Log**.  
  
     **Percorso file corrente**  
     Consente di visualizzare il percorso del file di database selezionato. Il percorso può essere modificato manualmente.  
  
     **Messaggio**  
     Non viene visualizzato alcun messaggio oppure viene visualizzato il collegamento ipertestuale **Impossibile trovare il file**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
### <a name="to-attach-a-database"></a>Per collegare un database  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Usare l'istruzione [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) con la clausola `FOR ATTACH`.  
  
     Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio si collegano i file del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] e si rinomina il database in `MyAdventureWorks`.  
  
    ```sql  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks_Data.mdf'),   
        (FILENAME = 'C:\MySQLServer\AdventureWorks_Log.ldf')   
        FOR ATTACH;  
    ```  
  
    > [!NOTE]  
    > In alternativa, è possibile usare la stored procedure [sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) o [sp_attach_single_file_db](../../relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql.md) . Tuttavia, queste stored procedure verranno eliminate nelle versioni future di Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. È consigliabile utilizzare `CREATE DATABASE ... FOR ATTACH` in alternativa.  
  
##  <a name="follow-up-after-upgrading-a-sql-server-database"></a><a name="FollowUp"></a> Completamento: Dopo l'aggiornamento di un database di SQL Server  

Una volta aggiornato utilizzando il metodo di collegamento, il database viene reso immediatamente disponibile e viene aggiornato automaticamente. Se il database include indici full-text, questi vengono importati, reimpostati o ricompilati dal processo di aggiornamento, a seconda dell'impostazione della proprietà del server **Opzione di aggiornamento full-text** . Se l'opzione di aggiornamento è impostata su **Importa** o **Ricompila**, gli indici full-text non saranno disponibili durante l'aggiornamento. A seconda della quantità di dati indicizzati, l'importazione può richiedere diverse ore, mentre la ricompilazione può risultare dieci volte più lunga. Si noti inoltre che, quando l'opzione di aggiornamento è impostata su **Importa** e un catalogo full-text non è disponibile, gli indici full-text associati vengono ricompilati.  
  
Dopo l'aggiornamento, il livello di compatibilità del database rimane a livello di compatibilità prima dell'aggiornamento, a meno che il livello di compatibilità precedente non sia supportato nella nuova versione. In questo caso, il livello di compatibilità del database aggiornato è impostato sul livello di compatibilità supportato più basso.

Se, ad esempio, si collega un database con livello di compatibilità 90 prima di collegarlo a un'istanza di [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] , dopo l'aggiornamento il livello di compatibilità viene impostato su 100, che corrisponde al livello di compatibilità supportato più basso in [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] . Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
> [!NOTE]
> Se si collega un database da un'istanza che esegue [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versioni precedenti con Change Data Capture (CDC) abilitato, è necessario eseguire anche il comando seguente per aggiornare i metadati di Change Data Capture (CDC).
  
```sql
USE <database name>
EXEC sys.sp_cdc_vupgrade  
``` 
 
## <a name="see-also"></a>Vedere anche  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) 
 <br>[Gestire i metadati quando si rende disponibile un database in un altro server](manage-metadata-when-making-a-database-available-on-another-server.md)  
 [Scollegamento di un database](../../relational-databases/databases/detach-a-database.md)  
  
