---
title: Caricamento di oggetti di database convertiti in SQL Server (OracleToSQL) | Microsoft Docs
description: Informazioni su come caricare gli oggetti di database convertiti da Oracle nell'istanza di SQL Server usando SSMA per Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Synchronization, Securing Objects in SQL Server
- Synchronization,Scripting Objects
ms.assetid: a8ae33b2-1883-4785-922b-ea0e31c0b37a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4b315b0b26f771a2e215f03e1abcfeb5f829e86b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080802"
---
# <a name="loading-converted-database-objects-into-sql-server-oracletosql"></a>Caricamento di oggetti di database convertiti in SQL Server (OracleToSQL)
Una volta convertiti gli schemi Oracle in SQL Server, è possibile caricare gli oggetti di database risultanti in SQL Server. È possibile fare in modo che SSMA crei gli oggetti oppure è possibile creare script per gli oggetti ed eseguire gli script manualmente. SSMA consente inoltre di aggiornare i metadati di destinazione con il contenuto effettivo del database SQL Server.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Scelta tra sincronizzazione e script  
Se si desidera caricare gli oggetti di database convertiti in SQL Server senza alcuna modifica, è possibile fare in modo che SSMA crei o ricrei direttamente gli oggetti di database. Questo metodo è rapido e semplice, ma non consente la personalizzazione del [!INCLUDE[tsql](../../includes/tsql-md.md)] codice che definisce gli oggetti SQL Server, oltre alle stored procedure.  
  
Se si desidera modificare l'oggetto [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzato per creare oggetti o se si desidera un maggiore controllo sulla creazione degli oggetti, utilizzare SSMA per creare gli script. È quindi possibile modificare tali script, creare singolarmente ogni oggetto e utilizzare SQL Server Agent per pianificare la creazione di tali oggetti.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Utilizzo di SSMA per sincronizzare oggetti con SQL Server  
Per utilizzare SSMA per creare SQL Server oggetti di database, selezionare gli oggetti in SQL Server Esplora metadati, quindi sincronizzare gli oggetti con SQL Server, come illustrato nella procedura seguente. Per impostazione predefinita, se gli oggetti sono già presenti nel SQL Server e se i metadati SSMA sono più recenti rispetto all'oggetto in SQL Server, SSMA modificherà le definizioni degli oggetti in SQL Server. È possibile modificare il comportamento predefinito modificando **le impostazioni del progetto**.  
  
> [!NOTE]  
> È possibile selezionare gli oggetti di database SQL Server esistenti che non sono stati convertiti da database Oracle. Tuttavia, tali oggetti non verranno ricreati o modificati da SSMA.  
  
**Per sincronizzare oggetti con SQL Server**  
  
1.  In SQL Server Esplora Metadati espandere il nodo SQL Server principale, quindi espandere **database**.  
  
2.  Selezionare gli oggetti da elaborare:  
  
    -   Per sincronizzare un database completo, selezionare la casella di controllo accanto al nome del database.  
  
    -   Per sincronizzare o omettere singoli oggetti o categorie di oggetti, selezionare o deselezionare la casella di controllo accanto all'oggetto o alla cartella.  
  
3.  Dopo aver selezionato gli oggetti da elaborare in SQL Server Esplora metadati, fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Sincronizza con database**.  
  
    È inoltre possibile sincronizzare singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre, quindi scegliendo  **Sincronizza con database**.  
  
    Successivamente, SSMA visualizzerà la finestra di dialogo **Sincronizza con database, in** cui è possibile visualizzare due gruppi di elementi. Sul lato sinistro, SSMA Mostra gli oggetti di database selezionati rappresentati in un albero. Sul lato destro è possibile visualizzare un albero che rappresenta gli stessi oggetti nei metadati SSMA. È possibile espandere l'albero facendo clic sul pulsante a destra o a sinistra. La direzione della sincronizzazione viene visualizzata nella colonna azione posizionata tra i due alberi.  
  
    Un segno di azione può trovarsi in tre stati:  
  
    -   Una freccia sinistra indica che il contenuto dei metadati verrà salvato nel database (impostazione predefinita).  
  
    -   Una freccia destra indica che il contenuto del database sovrascriverà i metadati SSMA.  
  
    -   Un segno incrociato indica che non verrà eseguita alcuna azione.  
  
Fare clic sul segno di azione per modificare lo stato. La sincronizzazione effettiva verrà eseguita quando si fa clic sul pulsante **OK** della finestra di dialogo **Sincronizza con database** .  
  
## <a name="scripting-objects"></a>Oggetti di scripting  
Per salvare le definizioni [!INCLUDE[tsql](../../includes/tsql-md.md)] degli oggetti di database convertiti o per modificare le definizioni degli oggetti ed eseguire manualmente gli script, è possibile salvare le definizioni degli oggetti di database convertiti in [!INCLUDE[tsql](../../includes/tsql-md.md)] script.  
  
**Per salvare oggetti come script**  
  
1.  Dopo aver selezionato gli oggetti da salvare in uno script, fare clic con il pulsante destro del mouse su **database** e quindi scegliere **Salva come script**.  
  
    È anche possibile creare script per singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o sulla cartella padre, quindi scegliendo **Salva come script**.  
  
2.  Nella finestra di dialogo **Salva con** nome individuare la cartella in cui si desidera salvare lo script, immettere un nome di file nella casella **nome file** , quindi fare clic su OK SSMA per aggiungere l'estensione del nome file SQL.  
  
### <a name="modifying-scripts"></a>Modifica di script  
Dopo aver salvato le definizioni degli oggetti SQL Server come uno o più script, è possibile utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per visualizzare e modificare gli script.  
  
**Per modificare uno script**  
  
1.  Scegliere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Apri** dal menu **File** e quindi fare clic su **File**.  
  
2.  Nella finestra di dialogo **Apri** selezionare il file di script, quindi fare clic su OK.
  
3.  Modificare il file script utilizzando l'editor di query di.  
  
    Per ulteriori informazioni sull'editor di query, vedere "comandi pratici e funzionalità dell'editor" in documentazione online di SQL Server.  
  
4.  Per salvare lo script, scegliere **Salva** dal menu file.  
  
### <a name="running-scripts"></a>Esecuzione di script  
È possibile eseguire uno script o singole istruzioni in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Per eseguire uno script**  
  
1.  Scegliere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Apri** dal menu **File** e quindi fare clic su **File**.  
  
2.  Nella finestra di dialogo **Apri** selezionare il file di script e quindi fare clic su OK.  
  
3.  Per eseguire lo script completo, premere il tasto **F5** .  
  
4.  Per eseguire un set di istruzioni, selezionare le istruzioni nella finestra dell'editor di query, quindi premere il tasto **F5** .  
  
Per ulteriori informazioni sull'utilizzo dell'editor di query per eseguire gli script, vedere " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] query" in documentazione online di SQL Server.  
  
È inoltre possibile eseguire script dalla riga di comando tramite l'utilità **SQLCMD** e dal SQL Server Agent. Per ulteriori informazioni su **SQLCMD**, vedere l'argomento relativo all'utilità sqlcmd in documentazione online di SQL Server. Per ulteriori informazioni su SQL Server Agent, vedere l'argomento relativo all'automazione delle attività amministrative (SQL Server Agent) in documentazione online di SQL Server.  
  
## <a name="securing-objects-in-sql-server"></a>Protezione di oggetti in SQL Server  
Dopo aver caricato gli oggetti di database convertiti in SQL Server, è possibile concedere e negare autorizzazioni per tali oggetti. È consigliabile eseguire questa operazione prima di eseguire la migrazione dei dati a SQL Server. Per informazioni su come proteggere gli oggetti in SQL Server, vedere "Considerazioni sulla protezione per database e applicazioni di database" in documentazione online di SQL Server.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [migrare i dati in SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
