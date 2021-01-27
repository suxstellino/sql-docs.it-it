---
description: Spostare un database tramite la funzionalità di scollegamento e collegamento (Transact-SQL)
title: Spostare un database tramite la funzionalità di scollegamento e collegamento (Transact-SQL)
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- database attaching [SQL Server]
- moving databases [SQL Server]
- database detaching [SQL Server]
- relocating databases [SQL Server]
- detaching databases [SQL Server]
- attaching databases [SQL Server]
ms.assetid: 6732a431-cdef-4f1e-9262-4ac3b77c275e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: d57963f6203ab107c19ff1713f21a41378995725
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813488"
---
# <a name="move-a-database-using-detach-and-attach-transact-sql"></a>Spostare un database tramite la funzionalità di scollegamento e collegamento (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si illustra come spostare un database scollegato in un'altra posizione e come ricollegarlo alla stessa istanza oppure a un'altra istanza del server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Tuttavia, è consigliabile spostare i database utilizzando la procedura di rilocazione pianificata ALTER DATABASE anziché la funzionalità di scollegamento e collegamento. Per altre informazioni, vedere [Spostare database utente](../../relational-databases/databases/move-user-databases.md).  
  
> [!IMPORTANT]  
>  È consigliabile evitare di collegare o ripristinare database provenienti da origini sconosciute o non attendibili. Tali database possono contenere codice dannoso che potrebbe eseguire codice [!INCLUDE[tsql](../../includes/tsql-md.md)] indesiderato o causare errori modificando lo schema o la struttura fisica di database. Prima di utilizzare un database da un'origine sconosciuta o non attendibile, eseguire [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database in un server non di produzione ed esaminare il codice contenuto nel database, ad esempio le stored procedure o altro codice definito dall'utente.  
  
## <a name="procedure"></a>Procedura  
  
#### <a name="to-move-a-database-by-using-detach-and-attach"></a>Per spostare un database utilizzando la funzionalità di scollegamento e collegamento  
  
1.  Scollegare il database. Per altre informazioni, vedere [Scollegare un database](../../relational-databases/databases/detach-a-database.md).  
  
2.  In Esplora risorse o in una finestra del prompt dei comandi di Windows spostare nella nuova posizione il file o i file del database scollegato e i relativi file di log.  
  
     È consigliabile spostare i file di log anche se si prevede di crearne di nuovi. In alcuni casi, per il ricollegamento di un database sono necessari i file di log esistenti. Mantenere pertanto sempre tutti i file di log scollegati fino a quando il database non è stato collegato senza di essi.  
  
    > [!NOTE]  
    >  Se si tenta di collegare il database senza specificare il file di log, verrà eseguita una ricerca di tale file nella relativa posizione originale. Se nella posizione originale esiste ancora una copia del log, verrà collegata tale copia. Per evitare di utilizzare il file di log originale, specificare il percorso del nuovo file di log oppure rimuovere la copia originale del file di log dopo averlo copiato nella nuova posizione.  
  
3.  Collegare i file copiati. Per altre informazioni, vedere [Attach a Database](../../relational-databases/databases/attach-a-database.md).  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente si crea una copia del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] denominata `MyAdventureWorks`. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono eseguite in una finestra dell'editor di query connessa all'istanza del server a cui è collegata.  
  
1.  Scollegare il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] eseguendo le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti:  
  
    ```sql
    USE master;  
    GO  
    EXEC sp_detach_db @dbname = N'AdventureWorks2012';  
    GO  
    ```  
  
2.  Copiare i file di database (AdventureWorks208R2_Data.mdf e AdventureWorks208R2_log) rispettivamente in: C:\MySQLServer\AdventureWorks208R2_Data.mdf e C:\MySQLServer\AdventureWorks208R2_Log.ldf, utilizzando il metodo desiderato.  
  
    > [!IMPORTANT]  
    >  Nel caso di un database di produzione, posizionare su dischi separati il database e il log delle transazioni.  
  
     Per copiare i file in rete su un disco di un computer remoto, utilizzare il nome UNC (Universal Naming Convention) della posizione remota. Il formato di un nome UNC è **\\\\** _Servername_ **\\** _Sharename_ **\\** _Path_ **\\** _Filename_. Come per la scrittura di file nel disco rigido locale, è necessario che l'account utente utilizzato dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]disponga delle autorizzazioni appropriate per la lettura o la scrittura di un file nel disco remoto.  
  
3.  Collegare il database spostato e, facoltativamente, collegare il relativo log tramite l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti:  
  
    ```sql
    USE master;  
    GO  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Data.mdf'),  
        (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Log.ldf')  
        FOR ATTACH;  
    GO  
    ```  
  
     In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]un database appena collegato non è immediatamente visibile in Esplora oggetti. Per visualizzarlo, in Esplora oggetti scegliere **Aggiorna** dal menu **Visualizza**. Quando si espande il nodo **Database** in Esplora oggetti, il database appena collegato viene visualizzato nell'elenco dei database.  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
