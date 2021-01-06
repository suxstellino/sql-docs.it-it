---
description: 'Transazioni: gruppi di disponibilità Always On e mirroring del database'
title: 'Transazioni: gruppi di disponibilità e mirroring del database'
descripton: Learn about cross-database and distributed transaction support for SQL Server Always On availability groups and database mirroring.
ms.custom: seo-lt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: 9f7ed895-ad65-43e3-ba08-00d7bff1456d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 79e04e64a4fc89bb3e745d67b1cdbe929850b6ec
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642988"
---
# <a name="transactions---availability-groups-and-database-mirroring"></a>Transazioni: gruppi di disponibilità Always On e mirroring del database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Questo articolo descrive il supporto delle transazioni tra database e delle transazioni distribuite per i gruppi di disponibilità Always On e il mirroring del database.  

## <a name="support-for-distributed-transactions"></a>Supporto delle transazioni distribuite

SQL Server 2017 supporta le transazioni distribuite per i database nei gruppi di disponibilità. Questo supporto include i database nella stessa istanza di SQL Server o i database in istanze diverse di SQL Server. Le transazioni distribuite non sono supportate per i database configurati per il mirroring.

> [!NOTE]
> [!INCLUDE[SQL Server 2016](../../../includes/sssql15-md.md)] Nel Service Pack 2 e nelle versioni successive è disponibile un supporto completo per le transazioni distribuite in gruppi di disponibilità. 
> 
> Nelle versioni di [!INCLUDE[SQL Server 2016](../../../includes/sssql15-md.md)] precedenti a Service Pack 2 le transazioni distribuite tra database (ovvero le transazioni che usano database nella stessa istanza di SQL Server) che coinvolgono un database in un gruppo di disponibilità non sono supportate.

Per configurare un gruppo di disponibilità per le transazioni distribuite, vedere [Configurare il gruppo di disponibilità per le transazioni distribuite](configure-availability-group-for-distributed-transactions.md).

Per altre informazioni, vedere:

- [Guida all'amministrazione di DTC](/previous-versions/windows/desktop/ms681291(v=vs.85))
- [Guida per gli sviluppatori di DTC](/previous-versions/windows/desktop/ms679938(v=vs.85))
- [Riferimento per programmatori di DTC](/previous-versions/windows/desktop/ms686108(v=vs.85))

## <a name="sql-server-2016-sp1-and-before-support-for-cross-database-transactions-within-the-same-sql-server-instance"></a>SQL Server 2016 SP1 e versioni precedenti: supporto delle transazioni tra database nella stessa istanza di SQL Server  

In SQL Server 2016 SP1 e versioni precedenti, le transazioni tra database all'interno della stessa istanza di SQL Server non sono supportate per i gruppi di disponibilità. I due database in una transazione tra database non possono essere ospitati dalla stessa istanza di SQL Server, se uno o entrambi si trovano in un gruppo di disponibilità. Questa limitazione si applica anche quando tali database fanno parte dello stesso gruppo di disponibilità.  
  
Anche le transazioni tra database non sono supportate per il mirroring del database.  
  
##  <a name="sql-server-2016-sp1-and-before-support-for-distributed-transactions"></a><a name="dtcsupport"></a> SQL Server 2016 SP1 e versioni precedenti: supporto delle transazioni distribuite  
Le transazioni distribuite sono supportate con i gruppi di disponibilità quando i database sono ospitati da istanze di SQL Server diverse. Questa limitazione si applica anche alle transazioni distribuite tra le istanze di SQL Server e un altro server conforme a DTC.  
 
Microsoft Distributed Transaction Coordinator (MSDTC o DTC) è un servizio Windows che offre l'infrastruttura delle transazioni per i sistemi distribuiti. MSDTC consente alle applicazioni client di includere più origini dati in un'unica transazione di cui viene quindi eseguito il commit in tutti i server inclusi nella transazione. Ad esempio, è possibile usare MSDTC per coordinare le transazioni che interessano più database in server diversi.

Una nuova funzionalità di SQL Server 2016 consente di usare le transazioni distribuite nei casi in cui uno o più database della transazione sono inclusi in un gruppo di disponibilità. Nelle versioni precedenti a SQL Server 2016 le transazioni distribuite non erano supportate per i database inclusi in gruppi di disponibilità. SQL Server 2016 può registrare un gestore risorse per ogni database. Questa nuova funzionalità permette alle transazioni distribuite di includere database di gruppi di disponibilità.
  
 Devono essere soddisfatti i requisiti seguenti:  
  
-   I gruppi di disponibilità devono essere in esecuzione in Windows Server 2012 R2 o versioni successive. Per Windows Server 2012 R2, è necessario installare l'aggiornamento in KB3090973 disponibile all'indirizzo [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  
  
-   I gruppi di disponibilità devono essere creati con il comando **CREATE AVAILABILITY GROUP** e la clausola **WITH DTC\_SUPPORT = PER_DB**. Attualmente non è possibile modificare un gruppo di disponibilità esistente.  

- Tutte le istanze di SQL Server che vengono incluse nel gruppo di disponibilità devono essere SQL Server 2016 o versioni successive.
 
 ## <a name="non-support-for-distributed-transactions"></a>Transazioni distribuite non supportate
 I casi specifici in cui le transazioni distribuite non sono supportate includono:
 
 - In SQL Server 2016 SP1 e versioni precedenti, nei casi in cui più database coinvolti nella transazione sono inclusi nello stesso gruppo di disponibilità.
 
 - In SQL Server 2016 SP1 e versioni precedenti, nei casi in cui è presente almeno un database in un gruppo di disponibilità e un altro database nella stessa istanza di SQL Server. 
 
 - Casi in cui il gruppo di disponibilità non è stato creato con la transazione distribuita abilitata.
 
 - Mirroring del database.
 
 > [!IMPORTANT]
 > Determinare il risultato predefinito appropriato delle transazioni che DTC non riesce a risolvere per il proprio ambiente.  Per informazioni su come configurare il risultato predefinito, vedere [Opzione di configurazione del server in-doubt xact resolution](../../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md).
  
## <a name="example-scenario-with-database-mirroring"></a>Scenario di esempio con mirroring del database  
 Nell'esempio di mirroring del database seguente si illustra come può verificarsi un'incoerenza logica. In questo esempio un'applicazione utilizza una transazione tra database per inserire due righe di dati. Una riga viene inserita in una tabella in un database con mirroring, A, l'altra viene inserita in un altro database, B. Il database A viene sottoposto a mirroring in modalità a protezione elevata con failover automatico. Durante il commit della transazione, il database A diventa non disponibile e viene automaticamente eseguito il failover della sessione di mirroring sul mirror del database A.  
  
 Dopo il failover, è possibile che il commit della transazione tra database abbia esito positivo sul database B, ma non sul database di cui è stato eseguito il failover. Ad esempio, se il server principale originale del database A non ha inviato il log per la transazione tra database al server mirror prima dell'errore. Dopo il failover, tale transazione non è più presente nel nuovo server principale. I database A e B diventano incoerenti perché i dati inseriti nel database B rimangono inalterati, mentre quelli inseriti nel database A sono stati persi.  
  
 Uno scenario analogo può verificarsi quando si usa una transazione MS DTC. Ad esempio, dopo il failover, il nuovo server principale contatta MS DTC. MS DTC, tuttavia, non rileva il nuovo server principale e termina tutte le transazioni di cui si sta preparando il commit e per le quali in altri database il commit viene considerato eseguito.  
  
> [!NOTE]  
>  Non è supportato l'uso del mirroring del database con DTC o di gruppi di disponibilità con DTC in modi non approvati in questo articolo.  Ciò non implica che gli aspetti del prodotto non correlati a DTC non siano supportati. Tuttavia, eventuali problemi causati dall'uso improprio delle transazioni distribuite non sono supportati.  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Gruppi di disponibilità Always On: Interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
