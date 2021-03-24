---
title: Collegamento e scollegamento di un database (SQL Server)
description: È possibile rimuovere e riallegare i file di dati e di log delle transazioni di un database di SQL Server per modificare l'istanza del database o per spostare il database.
ms.custom: ''
ms.date: 06/30/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- databases [SQL Server], detaching
- detach database [SQL Server]
- databases [SQL Server], attaching
- removing databases
- transaction logs [SQL Server], detaching
- databases [SQL Server], removing
- restoring [SQL Server], attached databases
- transaction logs [SQL Server], attaching
- differential backups, after detaching
- moving databases
- attach database [SQL Server]
- detaching databases [SQL Server]
- differential base [SQL Server]
- attaching databases [SQL Server]
- databases [SQL Server], moving
ms.assetid: d0de0639-bc54-464e-98b1-6af22a27eb86
author: stevestein
ms.author: sstein
ms.openlocfilehash: ffa25f98d738b2cac2ec4d796bb946e9bac9e03b
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890740"
---
# <a name="database-detach-and-attach-sql-server"></a>Collegamento e scollegamento di un database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
È possibile scollegare i file di dati e di log delle transazioni di un database e, successivamente, ricollegarli alla stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o a un'istanza diversa. Scollegare e collegare un database risulta utile se si desidera assegnare il database a un'istanza diversa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nello stesso computer oppure spostarlo.  
  
  
##  <a name="security"></a><a name="Security"></a> Sicurezza  
Le autorizzazioni di accesso ai file vengono impostate durante l'esecuzione di alcune operazioni del database, inclusi il collegamento e lo scollegamento.  
  
> [!IMPORTANT]  
> È consigliabile evitare di collegare o ripristinare database provenienti da origini sconosciute o non attendibili. Tali database possono contenere codice dannoso che potrebbe eseguire codice [!INCLUDE[tsql](../../includes/tsql-md.md)] indesiderato o causare errori modificando lo schema o la struttura fisica di database.   
> Prima di utilizzare un database da un'origine sconosciuta o non attendibile, eseguire [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database in un server non di produzione ed esaminare il codice contenuto nel database, ad esempio le stored procedure o altro codice definito dall'utente.  
  
##  <a name="detaching-a-database"></a><a name="DetachDb"></a> Scollegamento di un database  
Lo scollegamento di un database determina la rimozione del database dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mentre i file di dati e i file del log delle transazioni inclusi nel database non vengono modificati. È quindi possibile utilizzare questi file per collegare il database a qualsiasi istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluso il server dal quale è stato scollegato il database.  
  
Non è possibile scollegare un database quando si verifica una delle condizioni seguenti:  
  
-   Il database è replicato e pubblicato. Se replicato, il database deve essere non pubblicato. Prima dello scollegamento, è necessario disabilitare la pubblicazione eseguendo [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    > Se non è possibile usare **sp_replicationdboption**, rimuovere la replica eseguendo [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Uno snapshot del database esiste nel database.  
  
    Prima di scollegare il database, è necessari eliminare tutti i relativi snapshot. Per altre informazioni, vedere [Eliminare uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)o a un'istanza diversa.  
  
    > [!NOTE]  
    > Non è possibile scollegare o collegare uno snapshot del database.  

-   Il database fa parte di un gruppo di disponibilità Always On.  
  
    Non è possibile scollegare il database finché non viene rimosso dal gruppo di disponibilità. Per altre informazioni, vedere [Rimuovere un database primario da un gruppo di disponibilità Always On](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).
  


-   È in corso il mirroring del database in una sessione di mirroring.  
  
    Non è possibile scollegare il database fino al termine della sessione. Per altre informazioni, vedere [Rimozione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   Il database è sospetto. Non è possibile scollegare un database sospetto. Per poter eseguire lo scollegamento, è prima necessario attivare la modalità di emergenza. Per ulteriori informazioni sull'attivazione della modalità di emergenza per un database, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-  Il database è un database di sistema.  
  
### <a name="backup-and-restore-and-detach"></a>Backup e ripristino e scollegamento  
Lo scollegamento di un database di sola lettura comporta la perdita di informazioni sulle basi differenziali dei backup differenziali. Per altre informazioni, vedere [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
### <a name="responding-to-detach-errors"></a>Risposta agli errori di scollegamento  
Gli errori generati durante lo scollegamento di un database possono impedire la chiusura corretta del database e la ricompilazione del log delle transazioni. Se viene visualizzato un messaggio di errore, eseguire gli interventi di correzione seguenti:  
  
1.  Ricollegare tutti file associati al database, non solo il file primario.  
  
2.  Risolvere il problema che ha causato l'errore.  
  
3.  Scollegare di nuovo il database.  
  
##  <a name="attaching-a-database"></a><a name="AttachDb"></a> Collegamento di un database  
È possibile collegare un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] copiato o scollegato. Quando si collega un database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] contenente file di cataloghi full-text in un'istanza del server di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , i file di catalogo vengono collegati dal percorso precedente insieme agli altri file del database, come in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni, vedere [Aggiornamento della ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).  
  
Durante il collegamento di un database è necessario che siano disponibili tutti i file di dati (file MDF e NDF). Se un file di dati si trova in un percorso diverso rispetto al momento della creazione o dell'ultimo collegamento del database, è necessario specificare il percorso corrente.  
  
> [!NOTE]  
> Se il file di dati primario in corso di collegamento è di sola lettura, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] si considera il database di sola lettura.  
  
Quando un database crittografato viene collegato per la prima volta a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il proprietario del database deve aprire la chiave master del database eseguendo l'istruzione `OPEN MASTER KEY DECRYPTION BY PASSWORD = 'password'`. È consigliabile abilitare la decrittografia automatica della chiave master eseguendo l'istruzione `ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY`. Per altre informazioni, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) e [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
 I requisiti necessari per il collegamento dei file di log dipendono in parte dallo stato di lettura/scrittura o di sola lettura del database, come illustrato di seguito:  
  
-   Per i database di lettura/scrittura è in genere possibile collegare un file di log in una nuova posizione. In alcuni casi, tuttavia, per ricollegare un database sono necessari i relativi file di log esistenti. È pertanto importante mantenere sempre tutti i file di log scollegati finché il database non è stato collegato correttamente senza di essi.  
  
    Se un database di lettura/scrittura include un singolo file di log e non se ne specifica una nuova posizione, durante l'operazione di collegamento il file viene cercato nella vecchia posizione. Se viene trovato, viene utilizzato il vecchio file di log, indipendentemente dal fatto che il database sia stato o meno chiuso correttamente. Se il vecchio file di log non viene trovato, il database è stato chiuso correttamente e non è presente una catena di log attiva, con l'operazione di collegamento si tenta di compilare un nuovo file di log per il database.  
  
-   Se il file di dati primario in corso di collegamento è di sola lettura, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] si considera il database di sola lettura. Nel caso di database di sola lettura, è necessario che i file di log si trovino nella posizione specificata nel file primario del database. La compilazione di un nuovo file di log non è possibile, perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può aggiornare la posizione del log memorizzata nel file primario.  
 
###  <a name="metadata-changes-on-attaching-a-database"></a><a name="Metadata"></a> Modifiche ai metadati durante il collegamento di un database  
Quando si esegue lo scollegamento e il successivo ricollegamento di un database di sola lettura, le informazioni sul backup relative alla base differenziale corrente vanno perdute. La *base differenziale* è il backup completo più recente di tutti i dati nel database o in un subset dei file o dei filegroup del database. Senza le informazioni sul backup di base, il database **master** non risulta più sincronizzato con il database di sola lettura, per cui i backup differenziali eseguiti successivamente possono restituire risultati imprevisti. Di conseguenza, se si utilizzano backup differenziali con un database di sola lettura, è consigliabile definire una nuova base differenziale eseguendo un backup completo dopo aver ricollegato il database. Per informazioni sui backup differenziali, vedere [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
Al momento del collegamento, viene eseguito l'avvio del database. In genere, il collegamento di un database non altera lo stato in cui questo si trovava al momento dello scollegamento o della copia. Tuttavia, le operazioni di collegamento e scollegamento comportano la disabilitazione del concatenamento della proprietà tra database relativo al database in oggetto. Per informazioni su come abilitare il concatenamento, vedere [Opzione di configurazione del server cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md). 

 > [!IMPORTANT]
 > Per impostazione predefinita e per questioni di sicurezza, le opzioni *is_broker_enabled*, *is_honor_broker_priority_on* e *is_trustworthy_on* sono disattivate ogni volta che il database viene collegato. Per informazioni su come attivare queste opzioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  Per altre informazioni sui metadati, vedere [Gestione dei metadati quando si rende disponibile un database in un altro server](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).
  
### <a name="backup-and-restore-and-attach"></a>Backup e ripristino e collegamento  
Analogamente a qualsiasi database completamente o parzialmente offline, non è possibile collegare un database con file in fase di ripristino. Se si arresta la sequenza di ripristino, è possibile collegare il database. Sarà quindi possibile riavviare la sequenza di ripristino.  
  
###  <a name="attaching-a-database-to-another-server-instance"></a><a name="OtherServerInstance"></a> Collegamento di un database a un'altra istanza del server  
  
> [!IMPORTANT]  
> Un database creato con una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può essere collegato con versioni precedenti. Questo impedisce che il database venga fisicamente usato con una versione precedente di [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Tuttavia, ciò interessa solo lo stato dei metadati e non influisce sul [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md). Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).   
  
Quando si collega un database a un'altra istanza del server, per garantire un utilizzo coerente a utenti e applicazioni, potrebbe essere necessario ricreare alcuni o tutti i metadati per il database, ad esempio account di accesso e processi, nell'altra istanza del server. Per altre informazioni, vedere [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
**Per scollegare un database**  
  
-   [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [Scollegamento di un database](../../relational-databases/databases/detach-a-database.md)  
  
**Per collegare un database**  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)  
  
-   [Collegare un database](../../relational-databases/databases/attach-a-database.md)  
  
-   [sp_attach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md)  
  
-   [sp_attach_single_file_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql.md)  
  
**Per aggiornare un database utilizzando le operazioni di scollegamento e collegamento**  
  
-   [Aggiornamento di un database utilizzando le operazioni di scollegamento e collegamento &#40;Transact-SQL&#41;](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)  
  
**Per spostare un database utilizzando le operazioni di scollegamento e collegamento**  
  
-   [Spostamento di un database tramite la funzionalità di scollegamento e collegamento &#40;Transact-SQL&#41;](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md)  
  
**Per eliminare uno snapshot del database**  
  
-   [Eliminare uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Filegroup e file di database](../../relational-databases/databases/database-files-and-filegroups.md)  
  
