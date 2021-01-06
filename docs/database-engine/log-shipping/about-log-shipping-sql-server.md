---
title: Informazioni sul log shipping (SQL Server) | Microsoft Docs
description: Informazioni sul log shipping di SQL Server che invia i backup del log delle transazioni da un database primario in un'istanza del server primario a database secondari.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- secondary servers [SQL Server]
- log shipping [SQL Server], jobs
- copy jobs [SQL Server]
- primary databases [SQL Server]
- log shipping [SQL Server], monitoring
- log shipping [SQL Server], about log shipping
- alert jobs [SQL Server]
- availability [SQL Server]
- jobs [SQL Server], log shipping
- monitor servers [SQL Server]
- restore jobs [SQL Server]
- log shipping [SQL Server]
- backup jobs [SQL Server]
- primary servers [SQL Server]
ms.assetid: 55da6b94-3a4b-4bae-850f-4bf7f6e918ca
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6ca3b4d6ce2a994f277161d481906dc4835ee48e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643463"
---
# <a name="about-log-shipping-sql-server"></a>Informazioni sul log shipping (SQL Server)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di inviare automaticamente i backup del log delle transazioni da un *database primario* in un'istanza del *server primario* a uno o più *database secondari* in istanze separate del *server secondario* . I backup del log delle transazioni vengono applicati singolarmente a ogni database secondario. Un terzo server facoltativo, noto come *server di monitoraggio*, registra la cronologia e lo stato delle operazioni di backup e di ripristino e genera avvisi nel caso in cui tali operazioni non vengano eseguite come previsto.  
  
 **Contenuto dell'argomento**  
  
-   [Vantaggi](#Benefits)  
  
-   [Termini e definizioni](#TermsAndDefinitions)  
  
-   [Panoramica del log shipping](#ComponentsAndConcepts)  
  
-   [Interoperabilità](#Interoperability)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="benefits"></a><a name="Benefits"></a> Vantaggi  
  
-   Fornisce una soluzione di recupero di emergenza per un solo database primario e uno o più database secondari, ognuno in un'istanza separata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Supporta l'accesso in sola lettura limitato ai database secondari (durante l'intervallo tra processi di ripristino).  
  
-   Consente un ritardo specificato dall'utente tra l'esecuzione del backup del log del database primario da parte del server primario e il momento in cui è necessario che i server secondari eseguano il ripristino del backup del log. Un ritardo maggiore può essere utile, ad esempio, se i dati vengono modificati per errore nel database primario. Se la modifica accidentale viene identificata rapidamente, un ritardo può consentire il recupero dei dati precedenti alla modifica da un database secondario prima che la modifica venga estesa anche a questo database.  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termini e definizioni  
 server primario  
 Istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che costituisce il server di produzione.  
  
 database primario  
 Il database nel server primario per il quale si desidera eseguire il backup su un altro server. Tutte le procedure di amministrazione della configurazione per il log shipping tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengono eseguite dal database primario.  
  
 server secondario  
 L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dove si desidera tenere una copia in modalità warm standby del database primario.  
  
 database secondario  
 La copia in modalità warm standby del database primario. Il database secondario potrebbe trovarsi nello stato RECOVERING o nello stato STANDBY, in cui rimane disponibile per l'accesso in sola lettura limitato.  
  
 server di monitoraggio  
 Istanza facoltativa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che tiene traccia di tutti i dettagli del log shipping, tra cui:  
  
-   Quando è stato eseguito l'ultimo backup del log delle transazioni nel database primario.  
  
-   Quando i server secondari hanno eseguito per l'ultima volta la copia e il ripristino dei file di backup.  
  
-   Informazioni sugli avvisi di backup non riusciti.  
  
> [!IMPORTANT]  
>  Una volta configurato il server di monitoraggio, non sarà possibile modificarlo senza prima rimuovere il log shipping.  
  
 processo di backup  
 Processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che esegue il backup, registra la cronologia nel server locale e nel server di monitoraggio ed elimina i file di backup e le informazioni sulla cronologia precedenti. Se il log shipping è abilitato, nell'istanza del server primario viene creata la categoria di processi relativa al backup per il log shipping.  
  
 processo di copia  
 Processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che copia i file di backup del server primario in una destinazione configurabile del server secondario e registra la cronologia nel server secondario e nel server di monitoraggio. Se il log shipping è abilitato in un database, la categoria di processi relativa alla copia per il log shipping viene creata in ciascun server secondario in una configurazione per il log shipping.  
  
 processo di ripristino  
 Processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che ripristina i file di backup copiati nei database secondari. Registra la cronologia nel server locale e nel server di monitoraggio ed elimina i file e le informazioni sulla cronologia precedenti. Se il log shipping è abilitato in un database, la categoria di processi relativa al ripristino per il log shipping viene creata per l'istanza del server secondario.  
  
 processo gestione avvisi  
 Processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che genera avvisi per i database primario e secondario nel caso in cui le operazioni di backup e ripristino non vengano completate entro la soglia stabilita. Se il log shipping è abilitato in un database, la categoria relativa alla gestione degli avvisi per il log shipping viene creata sull'istanza del server di monitoraggio.  
  
> [!TIP]  
>  Per ogni avviso, è necessario specificare un numero. Assicurarsi inoltre di configurare l'avviso in modo che un operatore venga notificato quando viene generato un avviso.  
  
##  <a name="log-shipping-overview"></a><a name="ComponentsAndConcepts"></a> Panoramica del log shipping  
 Il log shipping prevede tre operazioni:  
  
1.  Backup del log delle transazioni nell'istanza del server primario.  
  
2.  Copia del file di log delle transazioni nell'istanza del server secondario.  
  
3.  Ripristino del backup del log nell'istanza del server secondario.  
  
 È possibile distribuire il log a più istanze del server secondario e in questo caso sarà necessario ripetere le operazioni 2 e 3 per ognuna delle istanze.  
  
 Il failover di una configurazione per il log shipping dal server primario al server secondario non viene eseguito automaticamente. Se il database primario non è più disponibile, è possibile portare online manualmente uno qualsiasi dei database secondari.  
  
 È possibile utilizzare un database secondario per la generazione di report.  
  
 È inoltre possibile configurare avvisi per la configurazione per il log shipping.  
  
### <a name="a-typical-log-shipping-configuration"></a>Configurazione tipica per il log shipping  
 Nella figura seguente viene illustrata una configurazione per il log shipping con l'istanza del server primario, tre istanze del server secondario e un'istanza del server di monitoraggio. Nella figura vengono illustrati i passaggi eseguiti dai processi di backup, copia e ripristino:  
  
1.  L'istanza del server primario esegue il processo di backup per eseguire il backup del log delle transazioni nel database primario. Il backup del log viene quindi inserito in un file di backup del log primario, che viene inviato alla cartella di backup.  In questa figura, la cartella di backup risiede in una directory condivisa, la *condivisione di backup*.  
  
2.  Ognuna delle tre istanze del server secondario esegue un processo di copia per copiare il file di backup del log primario nella propria cartella di destinazione locale.  
  
3.  Ognuna delle istanze del server secondario esegue un processo di ripristino per ripristinare il backup del log dalla cartella di destinazione locale al database secondario locale.  
  
 Tramite le istanze dei server primario e secondario vengono inviati la propria cronologia e il proprio stato all'istanza del server di monitoraggio.  
  
 ![Configurazione che include processi di backup, copia e ripristino](../../database-engine/log-shipping/media/ls-typical-configuration.gif "Configurazione che include processi di backup, copia e ripristino")  
  
##  <a name="interoperability"></a><a name="Interoperability"></a> Interoperabilità  
 Il log shipping può essere utilizzato con le funzionalità o i componenti seguenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
-   [Mirroring del database e log shipping &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)  
  
-   [Log shipping e replica &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] e il mirroring del database si escludono a vicenda. Un database configurato per una di queste funzionalità non può essere configurato per l'altra.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Aggiornamento del log shipping a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Aggiungere un database secondario a una configurazione per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Rimuovere un database secondario da una configurazione per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Rimuovere il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Visualizzare il report di log shipping &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Monitorare il log shipping &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
