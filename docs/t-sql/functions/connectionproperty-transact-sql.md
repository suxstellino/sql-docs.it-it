---
description: CONNECTIONPROPERTY (Transact-SQL)
title: CONNECTIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONNECTIONPROPERTY_TSQL
- CONNECTIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- CONNECTIONPROPERTY statement
ms.assetid: 6bd9ccae-af77-4a05-b97f-f8ab41cfde42
author: cawrites
ms.author: chadam
ms.openlocfilehash: 428201b41c42342df355df571763265e64a5da11
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092336"
---
# <a name="connectionproperty-transact-sql"></a>CONNECTIONPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Per una richiesta in arrivo al server, questa funzione restituisce informazioni sulle proprietà della connessione univoca che supporta tale richiesta.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CONNECTIONPROPERTY ( property )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*property*  
Proprietà della connessione. *property* può avere uno dei valori seguenti:
  
|valore|Tipo di dati|Descrizione|  
|---|---|---|
|net_transport|**nvarchar(40)**|Restituisce il protocollo di trasporto fisico usato dalla connessione. Questo valore non ammette i valori Null. Valori restituiti possibili:<br /><br /> **HTTP**<br /> **Named pipe**<br /> **Sessione**<br /> **Shared memory**<br /> **SSL**<br /> **TCP**<br /><br /> e<br /><br /> **VIA**<br /><br /> Nota: viene restituito sempre **Session** quando per una connessione sono abilitati sia la funzionalità MARS (Multiple Active Result Set) che il pool di connessioni.|  
|protocol_type|**nvarchar(40)**|Restituisce il tipo di protocollo del payload. Attualmente distingue tra TDS (TSQL) e SOAP. Ammette i valori Null.|  
|auth_scheme|**nvarchar(40)**|Restituisce lo schema di autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per una connessione. Lo schema di autenticazione può essere relativo all'autenticazione di Windows (NTLM, KERBEROS, DIGEST, BASIC, NEGOTIATE) o all'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori Null.|  
|local_net_address|**varchar(48)**|Restituisce l'indirizzo IP del server di destinazione della connessione specifica. Disponibile solo per le connessioni che usano il provider del trasporto TCP. Ammette i valori Null.|  
|local_tcp_port|**int**|Restituisce la porta TCP del server che verrebbe impiegata nel caso in cui la connessione usasse il trasporto TCP. Ammette i valori Null.|  
|client_net_address|**varchar(48)**|Richiede l'indirizzo host del client che prova a connettersi al server. Ammette i valori Null.|  
|physical_net_transport|**nvarchar(40)**|Restituisce il protocollo di trasporto fisico usato dalla connessione. È accurato quando per una connessione è abilitata la funzionalità MARS (Multiple Active Result Set).|  
|\<Any other string>||Restituisce NULL per l'input non valido.|  
  
## <a name="remarks"></a>Osservazioni  
**local_net_address** e **local_tcp_port** restituiscono NULL in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
I valori restituiti corrispondono alle opzioni mostrate per le colonne corrispondenti nella DMV [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md). Ad esempio:
  
```sql
SELECT   
ConnectionProperty('net_transport') AS 'Net transport',   
ConnectionProperty('protocol_type') AS 'Protocol type';  
```  
  
## <a name="see-also"></a>Vedere anche
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
  
  
