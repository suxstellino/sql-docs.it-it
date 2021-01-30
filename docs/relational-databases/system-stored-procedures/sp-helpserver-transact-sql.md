---
description: sp_helpserver (Transact-SQL)
title: sp_helpserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpserver
- sp_helpserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpserver
ms.assetid: e8f42de7-c738-41c3-8bf5-dbd559dc7184
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83d212a6cfb817f3aa28c87ab0999d0488cec5e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211919"
---
# <a name="sp_helpserver-transact-sql"></a>sp_helpserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su un particolare server remoto o di replica oppure su tutti i server di entrambi i tipi. Specifica il nome del server, il nome di rete del server, lo stato di replica del server, il numero di identificazione del server e il nome delle regole di confronto nonché i valori di timeout per la connessione a server collegati o l'esecuzione di query su server collegati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpserver [ [ @server = ] 'server' ]   
  [ , [ @optname = ] 'option' ]   
  [ , [ @show_topology = ] 'show_topology' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @server = ] 'server'` È il server in cui vengono segnalate le informazioni. Quando il *Server* non è specificato, segnala tutti i server in **master.sys. Servers**. il *Server* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @optname = ] 'option'` Opzione che descrive il server. *Option* è di tipo **varchar (** 35 **)** e il valore predefinito è null. i valori possibili sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**regole di confronto compatibili**|L'utilizzo di questa opzione influisce sull'esecuzione delle query distribuite in server collegati. Se questa opzione è impostata su true,|  
|**accesso ai dati**|Consente di attivare e disabilitare un server collegato per l'accesso a query distribuite.|  
|**dist**|Server di distribuzione.|  
|**dpub**|Server di pubblicazione remoto associato al server di distribuzione corrente.|  
|**convalida differita dello schema**|Ignora il controllo dello schema delle tabelle remote all'inizio della query.|  
|**pub**|Server di pubblicazione.|  
|**RPC**|Attiva l'esecuzione di chiamate RPC dal server specificato.|  
|**RPC in uscita**|Viene abilitata l'esecuzione di chiamate RPC al server specificato.|  
|**sub**|Sottoscrittore.|  
|**sistema**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Usa regole di confronto remote**|Consente di utilizzare le regole di confronto di una colonna remota anziché quelle del server locale.|  
  
`[ @show_topology = ] 'show_topology'` Relazione tra il server specificato e gli altri server. *show_topology* è di tipo **varchar (** 1 **)** e il valore predefinito è null. Se *show_topology* non è uguale a **t** o è null, **sp_helpserver** restituisce le colonne elencate nella sezione set di risultati. Se *show_topology* è uguale a **t**, oltre alle colonne elencate nei set di risultati, **sp_helpserver** restituisce anche le informazioni su **topx** e **Topy** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o 1 (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del server.|  
|**network_name**|**sysname**|Nome di rete del server.|  
|**Stato**|**varchar (** 70 **)**|Stato del server.|  
|**id**|**char (** 4 **)**|Numero di identificazione del server.|  
|**nome_regole_di_confronto**|**sysname**|Regole di confronto del server.|  
|**connect_timeout**|**int**|Valore di timeout per la connessione al server collegato.|  
|**query_timeout**|**int**|Valore di timeout per le query eseguite sul server collegato.|  
  
## <a name="remarks"></a>Commenti  
 A un server possono essere associati più stati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni non vengono controllate.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-information-about-all-servers"></a>R. Visualizzazione di informazioni su tutti i server  
 Nell'esempio seguente vengono visualizzate informazioni su tutti i server tramite l'esecuzione di `sp_helpserver` senza parametri.  
  
```  
USE master;  
GO  
EXEC sp_helpserver;  
```  
  
### <a name="b-displaying-information-about-a-specific-server"></a>B. Visualizzazione di informazioni su un server specifico  
 Nell'esempio seguente vengono visualizzate tutte le informazioni sul server `SEATTLE2`.  
  
```  
USE master;  
GO  
EXEC sp_helpserver 'SEATTLE2';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_addsubscriber &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_dropsubscriber &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
