---
title: Gestire sessioni di eventi in Esplora oggetti
description: In Esplora oggetti è possibile eseguire azioni che hanno effetto sugli eventi estesi, ad esempio creare, avviare o arrestare, esportare, importare, modificare o eliminare le sessioni di eventi estesi.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: 16849e38-d3fb-414d-8dcb-797b5ffce6ee
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0ada75d2aa8c96c1013d1a91f0117120299d214b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492407"
---
# <a name="manage-event-sessions-in-the-object-explorer"></a>Gestire sessioni di eventi in Esplora oggetti

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento vengono illustrate le azioni eseguibili in **Esplora oggetti** che influiscono su Eventi estesi:  
  
-   Creare una sessione Eventi estesi  
  
-   Avviare o arrestare una sessione Eventi estesi  
  
-   Esportare una sessione Eventi estesi  
  
-   Importare un modello di sessione Eventi estesi  
  
-   Modificare una sessione Eventi estesi  
  
-   Eliminare una sessione Eventi estesi  
  
## <a name="create-an-extended-events-session"></a>Creare una sessione Eventi estesi  
 Per altre informazioni sulla creazione di una sessione Eventi estesi, vedere [Create an Extended Events Session](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))(Creare una sessione Eventi estesi).  
  
## <a name="starting-or-stopping-an-extended-events-session"></a>Avviare o arrestare una sessione Eventi estesi  
 È possibile avviare o arrestare una sessione Eventi estesi tramite l' **Editor di query** usando l'istruzione **ALTER EVENT SESSION** oppure il nodo **Eventi estesi** di **Esplora oggetti**.  
  
 Quando si arresta una sessione eventi, questa non viene più elencata come sessione attiva nella DMV sys.dm_xe_sessions. La definizione della sessione rimane tuttavia intatta ed è possibile riavviare la sessione. Per rimuovere completamente una definizione di sessione, è necessario eliminare la sessione.  
  
 Per avviare o arrestare una sessione Eventi estesi, è necessario disporre dell'autorizzazione ALTER ANY EVENT SESSION.  
  
 Quando si arresta una sessione che prevede l'uso di una destinazione in memoria, ad esempio destinazioni del contatore degli eventi sincroni, di abbinamento degli eventi, bucket o buffer circolare, tutte le informazioni archiviate nel buffer della sessione (colonna target_data della DMV sys.dm_xe_session_targets) andranno perse. Per accedere ai dati degli eventi dopo aver arrestato la sessione, è necessario salvare i dati prima dell'arresto della sessione oppure configurare la sessione in modo che venga utilizzata la destinazione file.  
  
### <a name="start-or-stop-an-extended-events-session-using-query-editor"></a>Avviare o arrestare una sessione Eventi estesi tramite l'Editor di query  
 Per avviare una sessione, eseguire le istruzioni seguenti sostituendo *session_name* con il nome della sessione Eventi estesi:  
  
```  
ALTER EVENT SESSION [session_name]  
ON SERVER  
STATE = START  
```  
  
 Per arrestare una sessione, eseguire le istruzioni seguenti sostituendo *session_name* con il nome della sessione Eventi estesi:  
  
```  
ALTER EVENT SESSION [session_name]  
ON SERVER  
STATE = STOP  
```  
  
### <a name="start-or-stop-an-extended-events-session-in-object-explorer"></a>Avviare o arrestare una sessione Eventi estesi in Esplora oggetti  
 Per avviare o arrestare una sessione Eventi estesi in **Esplora oggetti**, espandere i nodi **Gestione**, **Eventi estesi** e **Sessioni** , fare clic con il pulsante destro del mouse su una sessione, quindi scegliere **Avvia sessione** o **Arresta sessione**.  
  
## <a name="export-an-extended-events-session-template"></a>Esportare un modello di sessione Eventi estesi  
 È possibile esportare una sessione Eventi estesi usando **Esplora oggetti** e salvarla come file modello xml. Ad esempio, è possibile esportare una sessione, quindi applicare il modello a una nuova sessione eventi usando la **Creazione guidata nuova sessione** o la procedura guidata **Nuova sessione** .  
  
 Quando si esporta una sessione, assicurarsi di salvare il file modello in un percorso che utilizza il file system NTFS e di limitare l'accesso agli utenti autorizzati a visualizzare le informazioni.  
  
 Per esportare una sessione Eventi estesi in **Esplora oggetti**:  
  
1.  Espandere i nodi **Gestione**, **Eventi estesi**, quindi **Sessioni** .  
  
2.  Fare clic con il pulsante destro del mouse sulla sessione che si vuole esportare e selezionare **Esporta sessione**.  
  
3.  Nella finestra di dialogo **Salva con nome** selezionare un percorso in cui salvare il file, digitare il nome del file nella casella **Nome file** , quindi fare clic su **Salva**.  
  
     Se si salva il file nel percorso predefinito dei modelli di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , il modello sarà visualizzato nell'elenco a discesa di modelli predefiniti quando si usano la **Creazione guidata nuova sessione** e la finestra di dialogo **Nuova sessione** .  
  
## <a name="import-an-extended-events-session-template"></a>Importare un modello di sessione Eventi estesi  
 Usando **Esplora oggetti** è possibile importare un modello per una sessione Eventi estesi. È possibile, ad esempio, effettuare questa operazione per creare una sessione da un modello esportato da un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per creare una sessione Eventi estesi, è necessario disporre dell'autorizzazione **ALTER ANY EVENT SESSION** .  
  
 Prima di importare un file modello, verificare che il file provenga da una fonte attendibile. È necessario salvare i file modello in un percorso che utilizza il file system NTFS e in cui l'accesso è limitato agli utenti autorizzati a visualizzare le informazioni.  
  
 Per importare una sessione Eventi estesi:  
  
1.  In **Esplora oggetti** espandere i nodi **Gestione**, quindi **Eventi estesi** .  
  
2.  Fare clic con il pulsante destro del mouse su **Sessioni** e selezionare **Nuova sessione**.  
  
3.  Consente di specificare un nome per la sessione.  
  
4.  Espandere la casella di riepilogo a discesa **Modello** .  
  
5.  Fare clic su **\<File From ...>Apri** e individuare la sessione (file XML) che si desidera importare.  
  
 La sessione verrà visualizzata nel nodo **Sessioni** . Per impostazione predefinita,la sessione non viene avviata.  
  
## <a name="edit-an-extended-events-session"></a>Modificare una sessione Eventi estesi  
 È possibile modificare una sessione Eventi estesi in Esplora oggetti.  
  
 Per modificare una sessione Eventi estesi:  
  
1.  In **Esplora oggetti** espandere i nodi **Gestione**, **Eventi estesi**, quindi **Sessioni** .  
  
2.  Fare clic con il pulsante destro del mouse su una sessione e selezionare **Proprietà**.  
  
3.  Nella sezione **Selezione pagina** selezionare la pagina o le pagine che si vuole modificare.  
  
4.  Dopo avere completato l'esame della sessione eventi, fare clic su **OK**.  
  
## <a name="script-an-event-session-definition-using-tsql"></a>Creare uno script per la definizione di una sessione eventi tramite [!INCLUDE[tsql](../../includes/tsql-md.md)]  
 Sia nella Creazione guidata nuova sessione che nella finestra di dialogo Nuova sessione è presente un'opzione Script che consente di generare codice [!INCLUDE[tsql](../../includes/tsql-md.md)] per definire la sessione Eventi estesi.  
  
 Per accedere a [!INCLUDE[tsql](../../includes/tsql-md.md)] per una sessione Eventi estesi esistente, fare clic con il pulsante destro del mouse sul nome della sessione, scegliere **Crea script per sessione** e selezionare quindi **Genera codice per istruzione CREATE in**.  
  
## <a name="delete-an-extended-events-session"></a>Eliminare una sessione Eventi estesi  
 È possibile eliminare una sessione Eventi estesi:  
  
-   Nell'Editor di query usando **DROP EVENT SESSION**.  
  
-   In **Esplora oggetti**.  
  
 Quando si elimina una sessione eventi, tutte le informazioni di configurazione vengono rimosse e la definizione della sessione non viene più visualizzata nella vista del catalogo sys.server_event_sessions.  
  
> [!NOTE]  
>  system_health e Always On_health sono incluse con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; non eliminarle. system_health è eliminata per impostazione predefinita (per altre informazioni, vedere [Utilizzare la sessione system_health](../../relational-databases/extended-events/use-the-system-health-session.md)). Always On_health è disabilitata per impostazione predefinita. Tramite queste sessioni vengono raccolti dati che possono essere utili per la diagnosi dei problemi di prestazioni.  
  
 Per eliminare una sessione Eventi estesi, è necessario disporre dell'autorizzazione ALTER ANY EVENT SESSION.  
  
 Per eliminare una sessione Eventi estesi in **Esplora oggetti**:  
  
1.  Espandere i nodi **Gestione**, **Eventi estesi**, quindi **Sessioni** .  
  
2.  Fare clic con il pulsante destro del mouse su una sessione e selezionare **Elimina**.  
  
3.  Nella finestra di dialogo **Elimina oggetto** fare clic su **OK**.  
  
4.  Dopo avere completato l'esame della sessione eventi, fare clic su **OK**.  
  
 Per eliminare una sessione Eventi estesi nell' **Editor di query**, eseguire le istruzioni seguenti, sostituendo *session_name* con il nome della sessione Eventi estesi da eliminare:  
  
```  
DROP EVENT SESSION [session_name]  
ON SERVER  
```  
  
