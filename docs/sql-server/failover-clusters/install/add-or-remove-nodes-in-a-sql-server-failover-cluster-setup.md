---
title: Aggiungere e rimuovere nodi in un'istanza del cluster di failover
description: Questo articolo illustra come aggiungere o rimuovere nodi in un'istanza del cluster di failover Always On di SQL Server esistente.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- failover clustering [SQL Server], nodes
- deleting nodes
- cluster maintenance [SQL Server]
- removing nodes
ms.assetid: fe20dca9-a4c1-4d32-813d-42f1782dfdd3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2d1bbc562b90804a7478ce7d5b866cd17dfbc1df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346150"
---
# <a name="add-or-remove-nodes-in-a-failover-cluster-instance-setup"></a>Aggiungere o rimuovere nodi in un'istanza del cluster di failover (programma di installazione)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 Utilizzare questa procedura per gestire i nodi in un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistente.  
  
 L'aggiornamento o la rimozione di un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è riservata agli amministratori locali con autorizzazione di accesso come servizio su tutti i nodi del cluster di failover di Windows Server sottostante. Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.  
  
 Per aggiungere un nodo a un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistente, è necessario eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel nodo che deve essere aggiunto all'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Non eseguire il programma di installazione nel nodo attivo.  
  
 Per rimuovere un nodo da un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistente, è necessario eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel nodo che deve essere rimosso dall'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Per visualizzare i passaggi procedurali per l'aggiunta o la rimozione di nodi, selezionare una delle operazioni seguenti:  
  
-   [Aggiungere un nodo a un'istanza del cluster di failover Always On esistente](#Add)  
  
-   [Rimuovere un nodo da un'istanza del cluster di failover Always On esistente](#Remove)  
  
> [!IMPORTANT]  
>  La lettera di unità del sistema operativo per i percorsi di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve corrispondere per tutti i nodi aggiunti all'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="add-node"></a><a name="Add"></a> Aggiungere un nodo  
  
#### <a name="to-add-a-node-to-an-existing-ssnoversion-failover-cluster-instance"></a>Per aggiungere un nodo a un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]esistente  
  
1.  Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , quindi nella cartella radice fare doppio clic sul file Setup.exe. Per eseguire l'installazione da una condivisione di rete, accedere alla cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe.  
  
2.  L'Installazione guidata consentirà di avviare il Centro installazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per aggiungere un nodo a un'istanza del cluster di failover esistente, fare clic su **Installazione** nel riquadro a sinistra. Selezionare quindi **Aggiungi nodo a cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** .  
  
3.  Controllo configurazione sistema consentirà di eseguire un'operazione di individuazione nel computer. Per continuare, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
4.  Nella pagina Selezione lingua è possibile specificare la lingua per l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se si sta eseguendo l'installazione in un sistema operativo localizzato e se nei supporti di installazione sono inclusi i Language Pack sia per l'inglese sia per la lingua corrispondente al sistema operativo. Per altre informazioni sul supporto di lingue diverse e sulle considerazioni relative all'installazione, vedere le [versioni della lingua locale in SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Scegliere **Avanti** per continuare.  
  
5.  Nella pagina Codice Product Key specificare la chiave PID relativa a una versione di produzione del prodotto. Il codice Product Key immesso per questa installazione deve riferirsi alla stessa edizione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installata nel nodo attivo.  
  
6.  Nella pagina relativa alle condizioni di licenza leggere il contratto di licenza, quindi selezionare la casella di controllo per accettarne i termini e le condizioni. Per migliorare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], è inoltre possibile abilitare l'opzione relativa all'utilizzo delle funzionalità e inviare report a [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Scegliere **Avanti** per continuare. Per terminare l'installazione, fare clic su **Annulla**.  
  
7.  Controllo configurazione sistema consentirà di verificare lo stato del sistema del computer prima che l'installazione continui. Al termine della verifica, fare clic su **Avanti** per continuare.  
  
8.  Nella pagina Configurazione nodi del cluster utilizzare la casella a discesa per specificare il nome dell'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che verrà modificata durante questa operazione di installazione.  
  
9. Nella pagina Configurazione server - Account di servizio specificare gli account di accesso per i servizi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. I servizi effettivamente configurati in questa pagina dipendono dalle caratteristiche selezionate per l'installazione. Per le installazioni di istanze del cluster di failover, le informazioni relative al nome dell'account e al tipo di avvio vengono inserite automaticamente in questa pagina in base alle impostazioni specificate per il nodo attivo. È necessario fornire password per ogni account. Per altre informazioni, vedere [Configurazione Server - Account di servizio](../../../database-engine/install-windows/install-sql-server.md) e [Configurare account di servizio e autorizzazioni di Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     **Nota sulla sicurezza** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Dopo aver specificato le informazioni di accesso per i servizi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , fare clic su **Avanti**.  
  
10. Nella pagina Segnalazione errori specificare le informazioni che si desidera inviare a [!INCLUDE[msCoName](../../../includes/msconame-md.md)] per migliorare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per impostazione predefinita, l'opzione Segnalazione errori è abilitata.  
  
11. Controllo configurazione sistema eseguirà uno o più set di regole per convalidare la configurazione del computer con le caratteristiche di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] specificate.  
  
12. Nella pagina Inizio aggiunta nodo è presente una visualizzazione albero delle opzioni specificate durante l'installazione.  
  
13. Nella pagina Stato aggiunta nodo è possibile monitorare lo stato di avanzamento del processo di installazione.  
  
14. Al termine dell'installazione nella pagina Operazione completata viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti. Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , fare clic su **Chiudi**.  
  
15. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni sui file di log del programma di installazione, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
##  <a name="remove-node"></a><a name="Remove"></a> Rimuovere un nodo  
  
#### <a name="to-remove-a-node-from-an-existing-ssnoversion-failover-cluster-instance"></a>Per rimuovere un nodo da un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistente  
  
1.  Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file setup.exe. Per eseguire l'installazione da una condivisione di rete, accedere alla cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe.  
  
2.  L'Installazione guidata consentirà di avviare il Centro installazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per rimuovere un nodo da un'istanza del cluster di failover esistente, fare clic su **Manutenzione** nel riquadro a sinistra, quindi selezionare **Rimuovi nodo da un cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**.  
  
3.  Controllo configurazione sistema consentirà di eseguire un'operazione di individuazione nel computer. Per continuare, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
4.  Dopo aver selezionato l'installazione nella pagina File di supporto per l'installazione, tramite Controllo configurazione sistema è possibile verificare lo stato del sistema del computer prima che l'installazione continui. Al termine della verifica, fare clic su **Avanti** per continuare.  
  
5.  Nella pagina Configurazione nodi del cluster utilizzare la casella a discesa per specificare il nome dell'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che verrà modificata durante questa operazione di installazione. Il nodo da rimuovere è elencato nel campo **Nome del nodo** .  
  
6.  La pagina Inizio rimozione nodo contiene una visualizzazione albero delle opzioni specificate durante l'installazione. Per continuare, fare clic su **Rimuovi**.  
  
7.  Durante l'operazione di rimozione, lo stato di avanzamento del processo verrà indicato nella pagina Stato rimozione nodo.  
  
8.  Nella pagina Operazione completata viene visualizzato un collegamento al file di log di riepilogo del processo di rimozione del nodo e ad altre note importanti. Per completare la rimozione del nodo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , fare clic su **Chiudi**. Per altre informazioni sui file di log del programma di installazione, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
