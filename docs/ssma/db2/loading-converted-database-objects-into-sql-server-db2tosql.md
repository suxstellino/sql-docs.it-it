---
description: Caricamento di oggetti di database convertiti in SQL Server (DB2ToSQL)
title: Caricamento di oggetti di database convertiti in SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f4ea1ced-9f9f-4a9d-88ab-81dbab64adc3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 209b49e3719400b92a5425bdd680d8bed548e285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072475"
---
# <a name="loading-converted-database-objects-into-sql-server-db2tosql"></a>Caricamento di oggetti di database convertiti in SQL Server (DB2ToSQL)
Una volta convertiti gli schemi DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile caricare gli oggetti di database risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile fare in modo che SSMA crei gli oggetti oppure è possibile creare script per gli oggetti ed eseguire gli script manualmente. SSMA consente inoltre di aggiornare i metadati di destinazione con il contenuto effettivo del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Scelta tra sincronizzazione e script  
Se si desidera caricare gli oggetti di database convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza alcuna modifica, è possibile fare in modo che SSMA crei o ricrei direttamente gli oggetti di database. Questo metodo è rapido e semplice, ma non consente la personalizzazione del [!INCLUDE[tsql](../../includes/tsql-md.md)] codice che definisce gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti, diversi dalle stored procedure.  
  
Se si desidera modificare l'oggetto [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzato per creare oggetti o se si desidera un maggiore controllo sulla creazione degli oggetti, utilizzare SSMA per creare gli script. È quindi possibile modificare tali script, creare singolarmente ogni oggetto e utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per pianificare la creazione di tali oggetti.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Utilizzo di SSMA per sincronizzare oggetti con SQL Server  
Per utilizzare SSMA per creare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti di database, selezionare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, quindi sincronizzare gli oggetti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , come illustrato nella procedura seguente. Per impostazione predefinita, se gli oggetti sono già presenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e se i metadati SSMA sono più recenti rispetto all'oggetto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA modificherà le definizioni degli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile modificare il comportamento predefinito modificando **le impostazioni del progetto**.  
  
> [!NOTE]  
> È possibile selezionare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli oggetti di database esistenti che non sono stati convertiti da database DB2. Tuttavia, tali oggetti non verranno ricreati o modificati da SSMA.  
  
**Per sincronizzare oggetti con SQL Server**  
  
1.  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora Metadati espandere il nodo principale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quindi espandere **database**.  
  
2.  Selezionare gli oggetti da elaborare:  
  
    -   Per sincronizzare un database completo, selezionare la casella di controllo accanto al nome del database.  
  
    -   Per sincronizzare o omettere singoli oggetti o categorie di oggetti, selezionare o deselezionare la casella di controllo accanto all'oggetto o alla cartella.  
  
3.  Dopo aver selezionato gli oggetti da elaborare in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Sincronizza con database**.  
  
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
  
2.  Nella finestra di dialogo **Salva con** nome individuare la cartella in cui si desidera salvare lo script, immettere un nome di file nella casella **nome file** e quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)] . SSMA aggiungerà l'estensione del nome di file SQL.  
  
### <a name="modifying-scripts"></a>Modifica di script  
Dopo aver salvato le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni degli oggetti come uno o più script, è possibile utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per visualizzare e modificare gli script.  
  
**Per modificare uno script**  
  
1.  Scegliere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Apri** dal menu **File** e quindi fare clic su **File**.  
  
2.  Nella finestra di dialogo **Apri** selezionare il file di script, quindi fare clic su OK.
  
3.  Modificare il file script utilizzando l'editor di query di.  
  
    Per ulteriori informazioni sull'editor di query, vedere "comandi pratici e funzionalità dell'editor" nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di.  
  
4.  Per salvare lo script, scegliere **Salva** dal menu file.  
  
### <a name="running-scripts"></a>Esecuzione di script  
È possibile eseguire uno script o singole istruzioni in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Per eseguire uno script**  
  
1.  Scegliere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Apri** dal menu **File** e quindi fare clic su **File**.  
  
2.  Nella finestra di dialogo **Apri** selezionare il file di script, quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Per eseguire lo script completo, premere il tasto **F5** .  
  
4.  Per eseguire un set di istruzioni, selezionare le istruzioni nella finestra dell'editor di query, quindi premere il tasto **F5** .  
  
Per ulteriori informazioni sull'utilizzo dell'editor di query per eseguire gli script, vedere l'argomento " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] query" nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione in linea.  
  
È inoltre possibile eseguire script dalla riga di comando tramite l'utilità **SQLCMD** e dall' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente. Per ulteriori informazioni su **SQLCMD**, vedere l'argomento relativo all'utilità sqlcmd nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di. Per ulteriori informazioni su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vedere "automazione delle attività amministrative ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent)" nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di.  
  
## <a name="securing-objects-in-sql-server"></a>Protezione di oggetti in SQL Server  
Dopo aver caricato gli oggetti di database convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile concedere e negare autorizzazioni per tali oggetti. È consigliabile eseguire questa operazione prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per informazioni su come proteggere gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere "Considerazioni sulla protezione per database e applicazioni di database" nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [migrare i dati DB2 in SQL Server](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati DB2 in SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
