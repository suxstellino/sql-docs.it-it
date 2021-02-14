---
title: Informazioni sul listener del gruppo di disponibilità
description: 'Panoramica del listener del gruppo di disponibilità Always On e informazioni su come usarlo per indirizzare automaticamente il traffico al server desiderato. '
ms.custom: seodec18
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: e96b57c13cccf07424ce43aab74fde8b4fa43d08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340855"
---
# <a name="what-is-an-availability-group-listener"></a>Informazioni sul listener del gruppo di disponibilità  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Un listener del gruppo di disponibilità è un nome di rete virtuale a cui i client possono connettersi per accedere a un database in una replica primaria o secondaria di un gruppo di disponibilità Always On. Un listener consente a un client di connettersi a una replica senza dover conoscere il nome dell'istanza fisica di SQL Server. Poiché il listener instrada il traffico, non è necessario modificare la stringa di connessione client dopo un failover. 

Un listener del gruppo di disponibilità è composto da un nome listener DNS, dal numero di porta e da uno o più indirizzi IP. Solo il protocollo TCP è supportato dal listener del gruppo di disponibilità.  È necessario che il nome DNS del listener sia univoco nel dominio e in NetBIOS.  Quando si crea un listener, questo diventa una risorsa di un cluster con una dipendenza del gruppo di disponibilità, un IP virtuale (indirizzo VIP) e un nome di rete virtuale associati. Un client utilizza il DNS per risolvere il VNN in più indirizzi IP e quindi tenta di connettersi a ogni indirizzo, fino a che una richiesta di connessione riesce o fino a che le richieste di connessione non scadranno.  
  
Se il routing di sola lettura viene configurato per una o più [repliche secondarie leggibili](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md), le connessioni client con finalità di lettura al listener vengono reindirizzate a una replica secondaria leggibile. 
  
Questo articolo offre una panoramica di un listener del gruppo di disponibilità. Vengono anche fornite informazioni per [configurare il listener](create-or-configure-an-availability-group-listener-sql-server.md) e [connettersi al listener](listeners-client-connectivity-application-failover.md).
  
  
##  <a name="listener-parameters"></a><a name="AGlConfig"></a> Parametri del listener  

 Un listener del gruppo di disponibilità usa gli elementi seguenti:
  
 **Nome DNS univoco**  
 È noto anche come nome di rete virtuale (VNN). Si applicano le regole di denominazione di Active Directory per i nomi host DNS. Per ulteriori informazioni, vedere l'articolo della Knowledge Base [Convenzioni di denominazione di Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264) .  
  
**Uno o più indirizzi IP virtuali (VIP)**  
 Indirizzi IP virtuali configurati per una o più subnet su cui è possibile eseguire il failover del gruppo di disponibilità.  
  
**Configurazione dell'indirizzo IP**  
 Per un listener del gruppo di disponibilità specifico, l'indirizzo IP può usare il protocollo DHCP (Dynamic Host Configuration Protocol) oppure uno o più indirizzi IP statici. L'uso di DHCP può causare ritardi di connettività durante il failover e quindi non è consigliabile usarlo negli ambienti di produzione. I gruppi di disponibilità che si estendono su più subnet o usano configurazioni di rete ibride devono usare un indirizzo IP statico. 
 
  
##  <a name="listener-port"></a><a name="SelectListenerPort"></a> Porta del listener 
 Quando si configura un listener del gruppo di disponibilità, è necessario specificare una porta.  È possibile configurare la porta predefinita su 1433 per non rendere troppo complesse le stringhe di connessione client. Se si intende utilizzare la porta 1433, non è necessario specificare un numero di porta in una stringa di connessione. Inoltre, poiché ogni listener del gruppo di disponibilità dispone di un nome di rete virtuale separato, ogni listener del gruppo di disponibilità configurato su un solo cluster WSFC può essere configurato per fare riferimento alla stessa porta predefinita 1433.  
  
 È inoltre possibile specificare una porta del listener non standard. Ciò significa, tuttavia, che è necessario specificare in modo esplicito una porta di destinazione nella stringa di connessione ogni volta che ci si connette al listener del gruppo di disponibilità.  Sarà inoltre necessario aprire un'autorizzazione sul firewall per la porta non standard.  
  
 Se si utilizza la porta predefinita 1433 per il nome di rete virtuale del listener del gruppo di disponibilità, è necessario assicurarsi che nessuno altro servizio sul nodo cluster utilizzi questa porta, cosa che provocherebbe un conflitto tra porte.  
  
 Se una delle istanze di SQL Server è già in attesa sulla porta TCP 1433 attraverso il listener dell'istanza e nessun altro servizio, incluse istanze aggiuntive di SQL Server, nel computer è in attesa sulla porta 1433, non si verificherà alcun conflitto tra porte con il listener del gruppo di disponibilità.  Il motivo è dato dal fatto che il listener del gruppo di disponibilità può condividere la stessa porta TCP nello stesso processo del servizio.  È tuttavia opportuno non configurare più istanze di SQL Server (side-by-side) per l'ascolto sulla stessa porta.  
  
  
##  <a name="behavior-of-client-connections-on-failover"></a><a name="CCBehaviorOnFailover"></a> Comportamento delle connessioni client in caso di failover  

 Quando si verifica un failover del gruppo di disponibilità, le connessioni persistenti esistenti al gruppo di disponibilità vengono terminate e il client deve stabilire una nuova connessione per poter continuare a utilizzare lo stesso database primario o il database secondario in sola lettura.  Nel corso di un failover sul lato server, la connettività al gruppo di disponibilità potrebbe non essere disponibile, di conseguenza l'applicazione client potrebbe tentare di riconnettersi finché il database primario non viene riportato online.  
  
 Se il gruppo di disponibilità viene riportato online durante un tentativo di connessione dell'applicazione client, ma prima del periodo di timeout della connessione, è possibile che il driver client si connetta correttamente durante uno di questi tentativi interni senza che si verifichi alcun errore.  


## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con il funzionamento di un listener del gruppo di disponibilità, [creare il proprio listener](create-or-configure-an-availability-group-listener-sql-server.md) e quindi configurare l'applicazione per [connettersi al listener](listeners-client-connectivity-application-failover.md). È anche possibile esaminare diverse [strategie di monitoraggio dei gruppi di disponibilità](monitoring-of-availability-groups-sql-server.md) per garantire l'integrità del gruppo di disponibilità. 

Per altre informazioni sui gruppi di disponibilità, vedere [Panoramica dei gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). 
  

  
  
