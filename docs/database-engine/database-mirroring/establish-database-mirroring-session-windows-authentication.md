---
title: Configurare il mirroring del database (autenticazione di Windows)
description: Informazioni su come configurare una sessione di mirroring del database con autenticazione di Windows tramite SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], sessions
ms.assetid: 7cb418d6-dce1-4a0d-830e-9c5ccfe3bd72
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f09f731b2d8838c5cdcd4ea9861666c9d89cacf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342351"
---
# <a name="establish-database-mirroring-session---windows-authentication"></a>Stabilire una sessione di mirroring del database tramite autenticazione di Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 Per stabilire una sessione di mirroring del database e modificare le proprietà di mirroring per un database, usare la pagina **Mirroring** della finestra di dialogo **Proprietà database** . Prima di usare la pagina **Mirroring** per configurare il mirroring del database, assicurarsi che siano stati soddisfatti i requisiti seguenti:  
  
-   Nelle istanze del server principale e del server mirror deve essere eseguita la stessa edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ovvero Standard o Enterprise. È inoltre consigliabile che vengano eseguite in sistemi simili in grado di gestire carichi di lavoro identici.  
  
    > [!NOTE]  
    >  L'istanza del server di controllo non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Il database mirror deve essere presente e aggiornato.  
  
     La creazione di un database mirror richiede il ripristino di un backup recente del database principale (usando WITH NORECOVERY) nell'istanza del server mirror. Richiede inoltre l'esecuzione di uno o più backup del log dopo il backup completo e il loro ripristino in sequenza nel database mirror (usando WITH NORECOVERY). Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Se le istanze dei server sono in esecuzione con account utente di dominio diversi, ogni istanza richiede un account di accesso nel database **master** delle altre. Se l'account di accesso non è presente, è necessario crearlo prima di configurare il mirroring. Per altre informazioni, vedere [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
### <a name="to-configure-database-mirroring"></a>Per configurare il mirroring del database  
  
1.  Dopo aver attivato la connessione all'istanza del server principale, in Esplora oggetti fare clic sul nome del server per espandere l'albero.  
  
2.  Espandere **Database** e selezionare il database per il mirroring.  
  
3.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Server mirror**. Viene visualizzata la pagina **Mirroring** della finestra di dialogo **Proprietà database** .  
  
4.  Per iniziare la configurazione del mirroring, fare clic su **Configura sicurezza** . Verrà avviata la Configurazione guidata sicurezza mirroring del database.  
  
    > [!NOTE]  
    >  Durante una sessione di mirroring del database, è possibile usare questa procedura guidata solo per aggiungere o modificare l'istanza del server di controllo del mirroring.  
  
5.  La Configurazione guidata sicurezza mirroring del database crea automaticamente l'endpoint di mirroring del database (se non è presente) in ogni istanza del server e immette gli indirizzi di rete del server in ciascun campo corrispondente al ruolo dell'istanza del server (**Server principale**, **Server mirror** o **Server di controllo del mirroring**).  
  
    > [!IMPORTANT]  
    >  Quando si crea un endpoint, la Configurazione guidata sicurezza mirroring del database usano sempre l'autenticazione di Windows. Prima di poter usare la procedura guidata con l'autenticazione basata sui certificati, l'endpoint del mirroring deve già essere configurato per l'utilizzo dei certificati in ogni istanza del server. Inoltre, tutti i campi della finestra di dialogo **Account di servizio** della procedura guidata devono rimanere vuoti. Per informazioni sulla creazione di un endpoint del mirroring del database per l'uso dei certificati, vedere [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
6.  Facoltativamente, è possibile cambiare modalità operativa. Alcune modalità operative sono disponibili se è stato o meno specificato un indirizzo TCP per un server di controllo del mirroring. descritte di seguito:  
  
    |Opzione|Server di controllo del mirroring?|Spiegazione|  
    |------------|--------------|-----------------|  
    |**Prestazioni elevate (asincrona)**|Null (se presente, non usato ma la sessione richiede un quorum)|Per massimizzare le prestazioni, il database mirror rimane sempre un passo indietro rispetto al database principale. La distanza tra i database è tuttavia solitamente ridotta. La perdita di un partner produce l'effetto seguente:<br /><br /> Se l'istanza del server mirror diventa non disponibile, le attività continuano nel server principale.<br /><br /> Se l'istanza del server principale diventa non disponibile, il server mirror si arresta, ma se la sezione non dispone di un server di controllo del mirroring (come consigliato) o se il server di controllo del mirroring è connesso al server mirror, il server mirror sarà accessibile come server di standby a caldo (warm standby). Il proprietario del database potrà quindi forzare il servizio nell'istanza del server mirror, con possibile perdita di dati.<br /><br /> <br /><br /> Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).|  
    |**Protezione elevata senza failover automatico (sincrona)**|No|Tutte le transazioni di cui è stato eseguito il commit vengono scritte nel disco del server mirror.<br /><br /> Il failover manuale è possibile quando i partner sono connessi tra loro e il database è sincronizzato.<br /><br /> La perdita di un partner produce l'effetto seguente:<br /><br /> Se l'istanza del server mirror diventa non disponibile, le attività continuano nel server principale.<br /><br /> Se l'istanza del server principale diventa non disponibile, il server mirror si arresta, ma rimane accessibile come server di standby a caldo (warm standby). Il proprietario del database potrà quindi forzare il servizio nell'istanza del server mirror, con possibile perdita di dati.<br /><br /> <br /><br /> Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).|  
    |**Protezione elevata con failover automatico (sincrona)**|Sì (obbligatorio)|Tutte le transazioni di cui è stato eseguito il commit vengono scritte nel disco del server mirror.<br /><br /> La disponibilità viene ottimizzata mediante l'utilizzo di un'istanza del server di controllo del mirroring per supportare il failover automatico. Si noti che è possibile selezionare l'opzione **Protezione elevata con failover automatico (sincrona)** solo se è già stato specificato un indirizzo del server di controllo del mirroring.<br /><br /> Il failover manuale è possibile quando i partner sono connessi tra loro e il database è sincronizzato.<br /><br /> In presenza di un server di controllo del mirroring, la perdita di un partner produce l'effetto seguente:<br /><br /> Se l'istanza del server principale diventa non disponibile, si verifica il failover automatico. L'istanza del server mirror passa al ruolo del server principale e il database del server mirror viene considerato come database principale.<br /><br /> Se l'istanza del server mirror diventa non disponibile, le attività continuano nel server principale.<br /><br /> <br /><br /> Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).<br /><br /> **&#42;&#42; Importante &#42;&#42;** Se il server di controllo del mirroring viene disconnesso, è necessario che i partner siano connessi tra loro affinché il database sia disponibile. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).|  
  
7.  Se sussistono tutte le condizioni seguenti, fare clic su **Avvia mirroring** per avviare il mirroring:  
  
    -   Si è attualmente connessi all'istanza del server principale.  
  
    -   La sicurezza è stata configurata correttamente.  
  
    -   Gli indirizzi TCP completi delle istanze del server principale e del server mirror sono specificati nella sezione **Indirizzi di rete del server** .  
  
    -   Se la modalità operativa è impostata su **Protezione elevata con failover automatico (sincrona)** , viene anche specificato l'indirizzo TCP completo dell'istanza del server di controllo del mirroring.  
  
8.  Dopo l'avvio del mirroring, è possibile cambiare la modalità operativa e salvare la modifica scegliendo **OK**. Si noti che è possibile passare alla modalità a protezione elevata con failover automatico solo se prima si è specificato un indirizzo per il server di controllo del mirroring.  
  
    > [!NOTE]  
    >  Per rimuovere il server di controllo del mirroring, eliminare l'indirizzo di rete del server dal campo **Server di controllo del mirroring** . Se si passa dalla modalità a protezione elevata con failover automatico alla modalità a prestazioni elevate, il contenuto del campo **Server di controllo del mirroring** viene automaticamente cancellato.  
  
## <a name="see-also"></a>Vedere anche  
 [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Proprietà del database &#40;Pagina Mirroring&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Sospendere o riprendere una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [Impostazione di un database mirror per l'utilizzo della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)   
 [Rimuovere il mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)   
 [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)   
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Aggiunta o sostituzione di un server di controllo del mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
  
