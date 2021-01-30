---
description: sys.dm_broker_connections (Transact-SQL)
title: sys.dm_broker_connections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_broker_connections
- dm_broker_connections
- sys.dm_broker_connections_TSQL
- dm_broker_connections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_connections dynamic management view
ms.assetid: d9e20433-67fe-4fcc-80e3-b94335b2daef
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9da190e779d70581ea7b5b6d5fc8552b7bda9991
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174561"
---
# <a name="sysdm_broker_connections-transact-sql"></a>sys.dm_broker_connections (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni connessione di rete di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Per ulteriori informazioni, vedere la tabella seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**connection_id**|**uniqueidentifier**|Identificatore della connessione. Ammette valori Null.|  
|**transport_stream_id**|**uniqueidentifier**|Identificatore della [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connessione SNI (Network Interface) utilizzata dalla connessione per le comunicazioni TCP/IP. Ammette valori Null.|  
|**state**|**smallint**|Stato corrente della connessione. Ammette valori Null. Valori possibili:<br /><br /> 1 = NEW<br /><br /> 2 = CONNECTING<br /><br /> 3 = CONNECTED<br /><br /> 4 = LOGGED_IN<br /><br /> 5 = CHIUSO|  
|**state_desc**|**nvarchar(60)**|Stato corrente della connessione. Ammette valori Null. Valori possibili:<br /><br /> NEW<br /><br /> CONNECTING<br /><br /> CONNECTED<br /><br /> LOGGED_IN<br /><br /> CLOSED|  
|**connect_time**|**datetime**|Data e ora di apertura della connessione. Ammette valori Null.|  
|**login_time**|**datetime**|Data e ora in cui è stato eseguito l'accesso per la connessione. Ammette valori Null.|  
|**authentication_method**|**nvarchar(128)**|Nome del metodo di autenticazione di Windows, ad esempio NTLM o KERBEROS. Questo valore proviene da Windows. Ammette valori Null.|  
|**principal_name**|**nvarchar(128)**|Nome dell'account di accesso convalidato per le autorizzazioni di connessione. Per l'autenticazione di Windows, corrisponde al nome dell'utente remoto. Per l'autenticazione basata su certificati, corrisponde al proprietario del certificato. Ammette valori Null.|  
|**remote_user_name**|**nvarchar(128)**|Nome dell'utente peer dell'altro database utilizzato dall'autenticazione di Windows. Ammette valori Null.|  
|**last_activity_time**|**datetime**|Data e ora dell'ultimo utilizzo della connessione per l'invio o la ricezione di informazioni. Ammette valori Null.|  
|**is_accept**|**bit**|Specifica se la connessione ha avuto origine sul lato remoto. Ammette valori Null.<br /><br /> 1 = La connessione è una richiesta accettata dall'istanza remota.<br /><br /> 0 = La connessione è stata avviata dall'istanza locale.|  
|**login_state**|**smallint**|Stato del processo di accesso per la connessione. Valori possibili:<br /><br /> 0 = INITIAL<br /><br /> 1 = WAIT LOGIN NEGOTIATE<br /><br /> 2 = ONE ISC<br /><br /> 3 = ONE ASC<br /><br /> 4 = TWO ISC<br /><br /> 5 = TWO ASC<br /><br /> 6 = WAIT ISC Confirm<br /><br /> 7 = WAIT ASC Confirm<br /><br /> 8 = WAIT REJECT<br /><br /> 9 = WAIT PRE-MASTER SECRET<br /><br /> 10 = WAIT VALIDATION<br /><br /> 11 = WAIT ARBITRATION<br /><br /> 12 = ONLINE<br /><br /> 13 = ERROR|  
|**login_state_desc**|**nvarchar(60)**|Descrizione dello stato corrente dell'accesso dal computer remoto. Valori possibili:<br /><br /> È in corso l'inizializzazione dell'handshake della connessione.<br /><br /> L'handshake della connessione è in attesa del messaggio relativo alla negoziazione dell'accesso.<br /><br /> L'handshake della connessione ha inizializzato e inviato il contesto di sicurezza per l'autenticazione.<br /><br /> L'handshake della connessione ha ricevuto e accettato il contesto di sicurezza per l'autenticazione.<br /><br /> L'handshake della connessione ha inizializzato e inviato il contesto di sicurezza per l'autenticazione. È disponibile un meccanismo facoltativo per l'autenticazione dei peer.<br /><br /> L'handshake della connessione ha ricevuto e inviato il contesto di sicurezza accettato per l'autenticazione. È disponibile un meccanismo facoltativo per l'autenticazione dei peer.<br /><br /> L'handshake della connessione è in attesa del messaggio di conferma dell'inizializzazione del contesto di sicurezza.<br /><br /> L'handshake della connessione è in attesa del messaggio di conferma dell'accettazione del contesto di sicurezza.<br /><br /> L'handshake della connessione è in attesa del messaggio di rifiuto SSPI per l'autenticazione non riuscita.<br /><br /> L'handshake della connessione è in attesa del messaggio relativo al segreto pre-master.<br /><br /> L'handshake della connessione è in attesa del messaggio di convalida.<br /><br /> L'handshake della connessione è in attesa del messaggio relativo all'arbitraggio.<br /><br /> L'handshake della connessione è completo ed è online (pronto) per lo scambio di messaggi.<br /><br /> Errore di connessione.|  
|**peer_certificate_id**|**int**|ID di oggetto locale del certificato utilizzato dall'istanza remota per l'autenticazione. Il proprietario di questo certificato deve disporre delle autorizzazioni CONNECT per l'endpoint di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Ammette valori Null.|  
|**encryption_algorithm**|**smallint**|Algoritmo di crittografia utilizzato per la connessione. Ammette valori Null. Valori possibili:<br /><br /> **Descrizione &#124; valore &#124; opzione DDL corrispondente**<br /><br /> 0 &#124; nessuna &#124; disabilitata<br /><br /> 1 &#124; SOLO LA FIRMA<br /><br /> 2 &#124; AES, RC4 &#124; obbligatorio &#124; algoritmo necessario RC4}<br /><br /> 3 &#124; AES &#124;AES algoritmo necessario<br /><br /> **Nota:** L'algoritmo RC4 è supportato solo per compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Rappresentazione testuale dell'algoritmo di crittografia. Ammette valori Null. I valori possibili sono:<br /><br /> **Descrizione &#124; opzione DDL corrispondente**<br /><br /> NESSUNO &#124; disabilitato<br /><br /> RC4 &#124; {required &#124; algoritmo richiesto RC4}<br /><br /> AES &#124; algoritmo necessario AES<br /><br /> NONE, RC4 &#124; {supported &#124; l'algoritmo supportato RC4}<br /><br /> NESSUNO, AES &#124; algoritmo supportato RC4<br /><br /> RC4, AES &#124; algoritmo obbligatorio RC4 AES<br /><br /> AES, RC4 &#124; algoritmo richiesto AES RC4<br /><br /> NESSUNO, RC4, AES &#124; algoritmo supportato RC4 AES<br /><br /> NESSUNO, AES, RC4 &#124; algoritmo supportato AES RC4|  
|**receives_posted**|**smallint**|Numero di ricezioni di rete asincrone non ancora completate per la connessione. Ammette valori Null.|  
|**is_receive_flow_controlled**|**bit**|Specifica se le ricezioni di rete sono state posticipate a causa del controllo di flusso, poiché la rete è occupata. Ammette valori Null.<br /><br /> 1 = True|  
|**sends_posted**|**smallint**|Numero di invii di rete asincroni non ancora completati per la connessione. Ammette valori Null.|  
|**is_send_flow_controlled**|**bit**|Specifica se gli invii di rete sono stati posticipati a causa del controllo di flusso di rete, poiché la rete è occupata. Ammette valori Null.<br /><br /> 1 = True|  
|**total_bytes_sent**|**bigint**|Numero totale di byte inviati dalla connessione. Ammette valori Null.|  
|**total_bytes_received**|**bigint**|Numero totale di byte ricevuti dalla connessione. Ammette valori Null.|  
|**total_fragments_sent**|**bigint**|Numero totale di frammenti di messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] inviati dalla connessione. Ammette valori Null.|  
|**total_fragments_received**|**bigint**|Numero totale di frammenti di messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] ricevuti dalla connessione. Ammette valori Null.|  
|**total_sends**|**bigint**|Numero totale di richieste di invio in rete generate dalla connessione. Ammette valori Null.|  
|**total_receives**|**bigint**|Numero totale di richieste di ricezione in rete generate dalla connessione. Ammette valori Null.|  
|**peer_arbitration_id**|**uniqueidentifier**|Identificatore interno dell'endpoint. Ammette valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="physical-joins"></a>Join fisici  
 ![Join per sys.dm_broker_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-connections-1.gif "Join per sys.dm_broker_connections")  
  
## <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|**dm_broker_connections.connection_id**|**dm_exec_connections.connection_id**|Uno-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative a Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

