---
title: sp_validate_replica_hosts_as_publishers (T-SQL)
description: Descrive il sp_validate_replica_hosts_as_publishers stored procedure che consente la convalida di tutte le repliche secondarie.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validate_replica_hosts_as_publishers_TSQL
- sp_validate_replica_hosts_as_publishers
helpviewer_keywords:
- sp_validate_replica_hosts_as_publishers
ms.assetid: 45001fc9-2dbd-463c-af1d-aa8982d8c813
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f00ca4f972b7fccfa09f9c36c8e5b7fa09420fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205514"
---
# <a name="sp_validate_replica_hosts_as_publishers-transact-sql"></a>sp_validate_replica_hosts_as_publishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **sp_validate_replica_hosts_as_publishers** è un'estensione di **sp_validate_redirected_publisher** che consente la convalida di tutte le repliche secondarie piuttosto che solo la replica primaria corrente. **sp_validate_replicat_hosts_as_publisher** convalida un'intera topologia di replica di always on. **sp_validate_replica_hosts_as_publishers** necessario eseguire direttamente sul server di distribuzione utilizzando una sessione Desktop remoto per evitare un errore di sicurezza a doppio hop (21892).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_validate_replica_hosts_as_publishers   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @original_publisher = ] 'original_publisher'` Nome dell'istanza di che ha [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicato originariamente il database. *original_publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database da pubblicare. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @redirected_publisher = ] 'redirected_publisher'` Destinazione del reindirizzamento quando **sp_redirect_publisher** è stato chiamato per la coppia originale del server di pubblicazione/database pubblicato. *redirected_publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 No.  
  
## <a name="remarks"></a>Osservazioni  
 Se non esiste alcuna voce per il server di pubblicazione e il database di pubblicazione, **sp_validate_redirected_publisher** restituisce null per il parametro di output *\@ redirected_publisher*. In caso contrario, viene restituito il server di pubblicazione reindirizzato associato, sia in caso di esito positivo che di esito negativo.  
  
 Se la convalida ha esito positivo, **sp_validate_redirected_publisher** restituisce un'indicazione di esito positivo.  
  
 Se la convalida non riesce, vengono generati gli errori appropriati.  **sp_validate_redirected_publisher** esegue il massimo sforzo per generare tutti i problemi e non solo il primo rilevato.  
  
> [!NOTE]  
>  **sp_validate_replica_hosts_as_publishers** avrà esito negativo e verrà visualizzato il messaggio di errore seguente durante la convalida degli host della replica secondaria che non consentono l'accesso in lettura o richiedono che venga specificata la finalità di lettura.  
>   
>  Msg 21899, Livello 11, Stato 1, Procedura **sp_hadr_verify_subscribers_at_publisher**, Riga 109  
>   
>  Impossibile eseguire la query sul server di pubblicazione reindirizzato "MyReplicaHostName" per determinare la presenza di voci sysserver per i sottoscrittori del server di pubblicazione originale "MyOriginalPublisher", errore "976", messaggio di errore "Errore 976, Livello 14, Stato 1". Messaggio: Il database di destinazione, "MyPublishedDB", fa parte di un gruppo di disponibilità e non è attualmente accessibile per le query. Lo spostamento dei dati è sospeso o la replica di disponibilità non è abilitata per l'accesso in lettura. Per consentire l'accesso in sola lettura a questo e ad altri database nel gruppo di disponibilità, abilitare l'accesso in lettura a una o più repliche di disponibilità secondarie nel gruppo.  Per altre informazioni, vedere l'istruzione **ALTER AVAILABILITY GROUP** nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
>   
>  Sono stati rilevati uno o più errori di convalida del server di pubblicazione per l'host della replica 'MyReplicaHostName'.  
  
## <a name="permissions"></a>Autorizzazioni  
 Il chiamante deve essere un membro del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database per il database di distribuzione o un membro di un elenco di accesso alla pubblicazione per una pubblicazione definita associata al database del server di pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)  
  
  
