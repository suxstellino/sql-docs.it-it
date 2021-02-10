---
description: Caricamento di oggetti di database convertiti in SQL Server (MySQLToSQL)
title: Caricamento di oggetti di database convertiti in SQL Server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac993a6d-0283-4823-8793-6b217677dfa3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: c3945a348a40c9770334f00d119aa7e9564f8525
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042035"
---
# <a name="loading-converted-database-objects-into-sql-server-mysqltosql"></a>Caricamento di oggetti di database convertiti in SQL Server (MySQLToSQL)
Dopo aver convertito i database MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è possibile caricare gli oggetti di database risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. È possibile fare in modo che SSMA crei gli oggetti oppure è possibile creare script per gli oggetti ed eseguire gli script manualmente. SSMA consente inoltre di aggiornare i metadati di destinazione con il contenuto effettivo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il database SQL di Azure.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Scelta tra sincronizzazione e script  
Se si desidera caricare gli oggetti di database convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure senza modifiche, è possibile fare in modo che SSMA crei o ricrei direttamente gli oggetti di database. Questo metodo è rapido e semplice, ma non consente la personalizzazione del codice Transact-SQL che definisce gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti o SQL Azure.  
  
Se si desidera modificare Transact-SQL utilizzato per creare oggetti o se si desidera un maggiore controllo sulla creazione degli oggetti, utilizzare SSMA per creare gli script. È quindi possibile modificare questi script, creare singolarmente ogni oggetto e utilizzare SQL Server Agent per pianificare la creazione di tali oggetti.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Utilizzo di SSMA per sincronizzare oggetti con SQL Server  
Per usare SSMA per creare SQL Server o oggetti di database SQL di Azure, selezionare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati, quindi sincronizzare gli oggetti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, come illustrato nella procedura seguente. Per impostazione predefinita, se gli oggetti sono già presenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure e se i metadati SSMA hanno alcune modifiche locali o aggiornamenti alla definizione di tali oggetti, SSMA modificherà le definizioni degli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. È possibile modificare il comportamento predefinito modificando **le impostazioni del progetto**.  
  
> [!NOTE]  
> È possibile selezionare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli oggetti del database SQL di Azure o esistenti che non sono stati convertiti da database MySQL. Questi oggetti, tuttavia, non verranno ricreati o modificati da SSMA.  
  
##### <a name="to-synchronize-objects-with-sql-server-or-sql-azure"></a>Per sincronizzare oggetti con SQL Server o SQL Azure  
  
1.  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora Metadati espandere il nodo superiore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, quindi espandere **database**.  
  
2.  Selezionare gli oggetti da elaborare:  
  
    -   Per sincronizzare un database completo, selezionare la casella di controllo accanto al nome del database.  
  
    -   Per sincronizzare o omettere singoli oggetti o categorie di oggetti, selezionare o deselezionare la casella di controllo accanto all'oggetto o alla cartella.  
  
3.  Dopo aver selezionato gli oggetti da elaborare in SQL Server o SQL Azure Esplora metadati, fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Sincronizza con database**.  
  
    È inoltre possibile sincronizzare singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre, quindi scegliendo **Sincronizza con database**.  
  
    Successivamente, SSMA visualizzerà la finestra di dialogo **Sincronizza con database, in** cui è possibile visualizzare due gruppi di elementi. Sul lato sinistro, SSMA Mostra gli oggetti di database selezionati rappresentati in un albero. Sul lato destro è possibile visualizzare un albero che rappresenta gli stessi oggetti nei metadati SSMA. È possibile espandere l'albero facendo clic sul pulsante a destra o a sinistra. La direzione della sincronizzazione viene visualizzata nella colonna azione posizionata tra i due alberi.  
  
    Un segno di azione può trovarsi nei tre stati seguenti:  
  
    -   Una freccia sinistra indica che il contenuto dei metadati verrà salvato nel database (impostazione predefinita).  
  
    -   Una freccia destra indica che il contenuto del database sovrascriverà i metadati SSMA.  
  
    -   Un segno incrociato indica che non verrà eseguita alcuna azione.  
  
    -   Fare clic sul segno di azione per modificare lo stato. La sincronizzazione effettiva verrà eseguita quando si fa clic sul pulsante **OK** della finestra di dialogo **Sincronizza con database** .  
  
## <a name="scripting-objects"></a>Oggetti di scripting  
Per salvare le definizioni [!INCLUDE[tsql](../../includes/tsql-md.md)] degli oggetti di database convertiti o per modificare le definizioni degli oggetti ed eseguire manualmente gli script, è possibile salvare le definizioni degli oggetti di database convertiti in [!INCLUDE[tsql](../../includes/tsql-md.md)] script.  
  
**Per salvare oggetti come script**  
  
1.  Dopo aver selezionato gli oggetti da salvare in uno script, fare clic con il pulsante destro del mouse su **database** e quindi scegliere **Salva come script**.  
  
    È anche possibile creare script per singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o sulla cartella padre, quindi scegliendo **Salva come script**.  
  
2.  Nella finestra di dialogo **Salva con** nome individuare la cartella in cui si desidera salvare lo script, immettere un nome di file nella casella **nome file** , quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)] SSMA aggiungerà l'estensione di file SQL.  
  
### <a name="modifying-scripts"></a>Modifica di script  
Dopo aver salvato il SQL Server o SQL Azure le definizioni degli oggetti come uno script, è possibile utilizzare SQL Server Management Studio per modificare lo script.  
  
**Per modificare uno script**  
  
1.  Scegliere **Apri** dal menu **file** Management Studio, quindi fare clic su **file**.  
  
2.  Nella finestra di dialogo Apri individuare e selezionare il file di script, quindi fare clic su **OK**.  
  
3.  Modificare il file script utilizzando l'editor di query di. Per ulteriori informazioni sull'editor di query, vedere "comandi pratici e funzionalità dell'editor" in documentazione online di SQL Server.  
  
4.  Per salvare lo script, scegliere **Salva** dal menu file.  
  
### <a name="running-scripts"></a>Esecuzione di script  
È possibile eseguire uno script o singole istruzioni in SQL Server Management Studio.  
  
**Per eseguire uno script**  
  
1.  Scegliere **Apri** dal menu **file** SQL Server Management Studio, quindi fare clic su **file**.  
  
2.  Nella finestra di dialogo Apri individuare e selezionare il file di script, quindi fare clic su **OK**.  
  
3.  Per eseguire lo script completo, premere il tasto **F5** .  
  
4.  Per eseguire un set di istruzioni, selezionare le istruzioni nella finestra dell'editor di query, quindi premere il tasto **F5** .  
  
5.  Per ulteriori informazioni sull'utilizzo dell'editor di query per eseguire gli script, vedere la sezione relativa alla query Transact-SQL SQL Server Management Studio in documentazione online di SQL Server.  
  
6.  È inoltre possibile eseguire script dalla riga di comando tramite l'utilità **SQLCMD** e da SQL Server Agent. Per ulteriori informazioni su **SQLCMD**, vedere l'argomento relativo all'utilità sqlcmd in documentazione online di SQL Server. Per ulteriori informazioni su SQL Server Agent, vedere l'argomento relativo all'automazione delle attività amministrative (SQL Server Agent) in documentazione online di SQL Server.  
  
## <a name="securing-objects-in-sql-server"></a>Protezione di oggetti in SQL Server  
Dopo aver caricato gli oggetti di database convertiti in SQL Server, è possibile concedere e negare autorizzazioni per questi oggetti. È consigliabile eseguire questa operazione prima di eseguire la migrazione dei dati a SQL Server. Per informazioni su come proteggere gli oggetti in SQL Server, vedere "Considerazioni sulla protezione per database e applicazioni di database" in documentazione online di SQL Server.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [migrare i dati MySQL nel database SQL di Azure SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
