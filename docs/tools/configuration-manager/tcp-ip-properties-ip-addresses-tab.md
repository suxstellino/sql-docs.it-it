---
title: Proprietà TCP/IP
description: Di seguito viene descritto come usare la scheda Indirizzi IP della finestra di dialogo Proprietà TCP/IP per configurare le opzioni del protocollo TCP/IP per un indirizzo IP specifico in SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], listening on
- listening [SQL Server], on ports
ms.assetid: 4c17ed45-9da7-4bec-bce6-970109fe7365
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56055b7d3537413398980579342884ab6a58c142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345087"
---
# <a name="tcpip-properties-ip-addresses-tab"></a>Proprietà TCP/IP
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  La finestra di dialogo **Proprietà TCP/IP** (scheda Indirizzi IP) consente di configurare le opzioni del protocollo TCP/IP per un indirizzo IP specifico. Solo le opzioni **Porte dinamiche TCP** e **Porta TCP** possono essere configurate contemporaneamente per tutti gli indirizzi selezionando **IPAll**.  
  
 Le modifiche hanno effetto dopo il riavvio di SQL Server. Per informazioni sull'avvio e l'arresto del servizio SQL Server Browser, vedere [Avvio e arresto del servizio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
## <a name="static-vs-dynamic-ports"></a>Porte statiche e Porte dinamiche  
 L'istanza predefinita di SQL Server resta in attesa di connessioni in ingresso sulla porta 1433. È possibile cambiare la porta per ragioni di sicurezza oppure per soddisfare i requisiti di un'applicazione client. Per impostazione predefinita, le istanze denominate, incluso SQL Server Express, sono configurate per l'attesa su porte dinamiche. Per configurare una porta statica, lasciare vuota la casella **Porte dinamiche TCP** e fornire un numero di porta disponibile nella casella **Porta TCP** . Per ulteriori informazioni sull'apertura di porte nel firewall, vedere Configurazione di Windows Firewall per consentire l'accesso a SQL Server nella documentazione online.  
  
## <a name="dynamic-ports"></a>Porte dinamiche  
 All'avvio, quando un'istanza di SQL Server è configurata per restare in attesa sulle porte dinamiche, verifica che nel sistema operativo sia disponibile una porta e apre un endpoint per la porta. Per eseguire la connessione, le connessioni in ingresso devono specificare il numero di quella porta. Poiché il numero di porta può variare a ogni avvio di SQL Server, SQL Server offre il servizio SQL Server Browser, che esegue il monitoraggio delle porte e indirizza le connessioni in ingresso alla porta corrente per l'istanza. L'uso di porte dinamiche rende più complessa la connessione a SQL Server tramite un firewall, perché il numero di porta può cambiare al riavvio di SQL Server con la conseguente necessità di modificare le impostazioni del firewall. Per evitare problemi di connessione attraverso un firewall, configurare SQL Server per l'uso di una porta statica.  
  
## <a name="options"></a>Opzioni  
 **Attivo**  
 Indica che l'indirizzo IP è attivo sul computer. Non disponibile per **IPAll**.  
  
 **Enabled**  
 Se la proprietà **Attesa su tutti** nella finestra di dialogo **Proprietà TCP/IP** (scheda Protocollo) è impostata su **No**, questa proprietà indica se SQL Server è in attesa sull'indirizzo IP. Se la proprietà **Attesa su tutti** nella finestra di dialogo **Proprietà TCP/IP** (scheda Protocollo) è impostata su **Sì**, questa proprietà viene ignorata. Non disponibile per **IPAll**.  
  
 **Indirizzo IP**  
 Consente di visualizzare o modificare l'indirizzo IP utilizzato per la connessione. Elenca gli indirizzi IP utilizzati dal computer e l'indirizzo IP di loopback, 127.0.0.1. Non disponibile per **IPAll**. L'indirizzo IP può essere in formato IPv4 o IPv6.  
  
 **Porte dinamiche TCP**  
 Se le porte dinamiche non sono abilitate, la casella è vuota. Per attivare le porte dinamiche, impostare su 0.  
  
 Per **IPAll**, visualizza il numero della porta dinamica usata.  
  
 **Porta TCP**  
 Permette di visualizzare o modificare la porta sulla quale SQL Server è in attesa. Per impostazione predefinita, l'istanza predefinita di SQL Server resta in attesa sulla porta 1433.  
  
 Il motore di database può restare in attesa su più porte sullo stesso indirizzo IP. Elencare le porte separandole con una virgola, nel formato 1433,1500,1501. La lunghezza massima del campo è 2047 caratteri.  
  
 Per configurare un unico indirizzo IP per l'attesa su più porte è inoltre necessario impostare il parametro **Attesa su tutti** su **No** nella **scheda Protocolli** della finestra di dialogo **Proprietà TCP/IP** . Per altre informazioni, vedere "Procedura: Configurare il motore di database per l'attesa su più porte TCP" nella documentazione online di SQL Server.  
  
## <a name="adding-or-removing-ip-addresses"></a>Aggiunta o rimozione di indirizzi IP  
 Gestione configurazione SQL Server visualizza gli indirizzi IP disponibili al momento dell'installazione di SQL Server. Aggiungendo o modificando schede di rete, alla scadenza di un indirizzo IP assegnato dinamicamente, quando si riconfigura la struttura della rete o quando si sposta fisicamente il computer, ad esempio quando un computer portatile si connette alla rete in un edificio diverso, gli indirizzi IP disponibili cambiano. Per modificare un indirizzo IP, modificare la casella **Indirizzo IP** e quindi riavviare SQL Server.  
  
## <a name="additional-topics-in-books-online"></a>Altri argomenti nella documentazione online  
 Cercare in MSDN argomenti come quelli relativi alla **configurazione di un server per l'ascolto su una porta TCP specifica (Gestione configurazione SQL Server)** e alla **configurazione del motore di database per l'ascolto su più porte TCP**.  
  
## <a name="see-also"></a>Vedere anche  
 [Scelta di un protocollo di rete](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Creazione di una stringa di connessione valida con TCP/IP](creating-a-valid-connection-string-using-tcp-ip.md)   
 [Servizio SQL Server Browser](sql-server-browser-service.md)  
  
