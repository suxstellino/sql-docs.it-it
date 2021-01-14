---
title: Eseguire l'aggiornamento a un'edizione diversa
description: Il programma di installazione di SQL Server supporta l'aggiornamento dell'edizione tra varie edizioni di SQL Server. Prima di iniziare un aggiornamento di edizione, vedere le risorse in questo articolo.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 31d16820-d126-4c57-82cc-27701e4091bc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 87f1521927518446b355fea828e50901d6e35bc1
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975532"
---
# <a name="upgrade-to-a-different-edition-of-sql-server-setup"></a>Eseguire l'aggiornamento a un'edizione diversa di SQL Server (programma di installazione)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta l'aggiornamento dell'edizione fra le varie edizioni di [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]. Per informazioni sui percorsi di aggiornamento supportati, vedere [Aggiornamenti di versione ed edizione supportati](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md). Prima di iniziare l'aggiornamento dell'edizione di un'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], rivedere gli articoli seguenti:  

  [Edizioni e funzionalità supportate di SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
- [Edizioni e le funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
- [Edizioni e le funzionalità supportate di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  
- [Limiti della capacità di calcolo per edizione di SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)  
- [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
> [!NOTE]  
> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un'istanza cluster di failover:** è sufficiente eseguire l'aggiornamento dell'edizione in uno dei nodi dell'istanza del cluster di failover [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo nodo può essere attivo o passivo e il motore non imposta le risorse offline durante l'aggiornamento dell'edizione. Dopo l'aggiornamento dell'edizione è necessario per riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o eseguire il failover a un nodo diverso.  
  
## <a name="prerequisites"></a>Prerequisiti  
Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura per tale condivisione.  
  
> [!IMPORTANT]  
> Per rendere effettiva la modifica dell'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario riavviare i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ciò determina un periodo di inattività mentre i servizi sono offline.  
  
## <a name="procedure"></a>Procedura  
  
### <a name="to-upgrade-to-a-different-edition-of-ssnoversion"></a>Per eseguire l'aggiornamento a un'edizione diversa di [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]  
  
1.  Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic su setup.exe o avviare Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da Strumenti di configurazione. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe.  
  
2.  Per aggiornare un'istanza esistente di [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] a un'edizione diversa, nel Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic su **Manutenzione**, quindi selezionare **Aggiornamento edizione**.  
  
3.  Se sono necessari, i file di supporto per l'installazione verranno installati dal programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se viene richiesto, riavviare il computer prima di continuare.  
  
4.  Controllo configurazione sistema consente di eseguire un'operazione di individuazione nel computer. Per continuare, fare clic su **OK**.  
  
5.  Nella pagina codice Product Key fare clic su un pulsante di opzione per indicare se si intende eseguire l'aggiornamento a un'edizione gratuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o se si dispone di una chiave PID per una versione di produzione del prodotto. Per altre informazioni, vedere [Edizioni e componenti di SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md) e [Aggiornamenti di versione ed edizione supportati](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
6.  Nella pagina relativa alle condizioni di licenza leggere il contratto di licenza, quindi selezionare la casella di controllo per accettarne i termini e le condizioni. Scegliere **Avanti** per continuare. Per terminare l'installazione, fare clic su **Annulla**.  
  
7.  Nella pagina Seleziona istanza specificare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da aggiornare.  
  
8.  Nella pagina Regole aggiornamento edizione viene eseguita la convalida della configurazione del computer prima dell'inizio dell'operazione di aggiornamento dell'edizione.  
  
9. Nella pagina Aggiornamento dell'edizione è presente una visualizzazione albero delle opzioni specificate durante l'installazione. Per continuare, fare clic su **Aggiorna**.  
  
10. Durante il processo di aggiornamento dell'edizione, è necessario riavviare i servizi per scegliere la nuova impostazione. Al termine dell'aggiornamento, nella pagina Operazione completata viene visualizzato un collegamento al file di log di riepilogo relativo all'aggiornamento dell'edizione stessa. Per chiudere la procedura guidata, fare clic su **Chiudi**.  
  
11. Nella pagina Operazione completata viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti.  
  
12. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni sui file di log del programma di installazione, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
13. Se è stato eseguito l'aggiornamento da [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], è necessario eseguire ulteriori passaggi prima che sia possibile utilizzare l'istanza aggiornata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    -   Abilitare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in Gestione controllo servizi di Windows.  
  
    -   Utilizzare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per effettuare il provisioning dell'account di servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 Oltre ai passaggi precedenti, potrebbe essere necessario eseguire le operazioni seguenti se è stato eseguito l'aggiornamento da [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]:  
  
-   Gli utenti per i quali è stato effettuato il provisioning in [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] rimarranno in tale stato dopo l'aggiornamento. In particolare, gli utenti presenti nel gruppo BUILTIN\Users continueranno a essere sottoposti a provisioning. Disabilitare, rimuovere o effettuare di nuovo il provisioning di questi account in base alle esigenze. Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
-   Le dimensioni e la modalità di recupero per i database di sistema tempdb e modello rimarranno invariate dopo l'aggiornamento. Riconfigurare queste impostazioni in base alle esigenze. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
-   I database modello rimarranno nel computer dopo l'aggiornamento.  

> [!NOTE]  
> Se la procedura ha esito negativo a causa della regola Engine_SqlEngineHealthCheck, è possibile usare l'opzione di installazione della riga di comando per ignorare questa regola specifica per consentire il completamento del processo di aggiornamento. Per ignorare la verifica della regola, aprire un prompt dei comandi e passare al percorso che contiene il file di installazione di SQL Server (Setup.exe). Digitare quindi il comando seguente: 

```console
setup.exe /q /ACTION=editionupgrade /InstanceName=MSSQLSERVER /PID=<appropriatePid> /SkipRules=Engine_SqlEngineHealthCheck
```


## <a name="see-also"></a>Vedere anche  
 [Eseguire l'aggiornamento di SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Backward Compatibility_deleted](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
