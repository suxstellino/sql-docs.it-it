---
description: sp_publication_validation (Transact-SQL)
title: sp_publication_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_publication_validation
- sp_publication_validation_TSQL
helpviewer_keywords:
- sp_publication_validation
ms.assetid: 06be2363-00c0-4936-97c1-7347f294a936
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7e785892f73b45b6e5f6f20944c9c15ee88e8f63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199581"
---
# <a name="sp_publication_validation-transact-sql"></a>sp_publication_validation (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inizializza una richiesta di convalida per ogni articolo nella pubblicazione specificata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_publication_validation [ @publication = ] 'publication'  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @rowcount_only = ] 'rowcount_only'` Indica se restituire solo il conteggio delle righe per la tabella. *rowcount_only* è di **smallint** . i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Esegue un checksum compatibile con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.<br /><br /> Nota: quando un articolo viene filtrato orizzontalmente, viene eseguita un'operazione di conteggio delle righe anziché un'operazione di checksum.|  
|**1** (impostazione predefinita)|Esegue solo la convalida mediante conteggio delle righe.|  
|**2**|Esegue la convalida mediante conteggio delle righe e checksum binario.<br /><br /> Nota: per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottoscrittori della versione 7,0, viene eseguita solo una convalida tramite conteggio delle righe.|  
  
`[ @full_or_fast = ] 'full_or_fast'` Metodo utilizzato per calcolare il conteggio delle righe. *full_or_fast* è di **tinyint** . i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Esegue un conteggio completo con COUNT(*).|  
|**1**|Esegue un conteggio rapido da **sysindexes. Rows**. Il conteggio delle righe negli [ indicisys.sys](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) è molto più veloce rispetto al conteggio delle righe nella tabella effettiva. Tuttavia, poiché gli [ indicisys.sys](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) vengono aggiornati in modo differito, il conteggio delle righe potrebbe non essere accurato.|  
|**2** (impostazione predefinita)|Esegue un conteggio rapido condizionale eseguendo innanzitutto un tentativo con il metodo rapido. Se il metodo rapido evidenzia delle differenze, viene applicato il metodo completo. Se *expected_rowcount* è null e il stored procedure viene usato per ottenere il valore, viene sempre usato un conteggio completo (*).|  
  
`[ @shutdown_agent = ] 'shutdown_agent'` Indica se il agente di distribuzione deve essere arrestato immediatamente dopo il completamento della convalida. *shutdown_agent* è di **bit** e il valore predefinito è **0**. Se è **0**, l'agente di replica non viene arrestato. Se è **1**, l'agente di replica si arresta dopo la convalida dell'ultimo articolo.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere utilizzato quando viene richiesta la convalida in un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_publication_validation** viene utilizzata nella replica transazionale.  
  
 **sp_publication_validation** possibile chiamare in qualsiasi momento dopo l'attivazione degli articoli associati alla pubblicazione. Questa procedura può essere eseguita in modo manuale una sola volta oppure nell'ambito di un processo con pianificazione periodica per la convalida dei dati.  
  
 Se l'applicazione dispone di Sottoscrittori ad aggiornamento immediato, **sp_publication_validation** possibile rilevare errori non corretti. **sp_publication_validation** calcola innanzitutto il conteggio delle righe o il checksum nel server di pubblicazione e quindi nel Sottoscrittore. Dato che il trigger per l'aggiornamento immediato può propagare un aggiornamento dal Sottoscrittore al server di pubblicazione dopo l'esecuzione del conteggio delle righe o del checksum nel server di pubblicazione ma prima del completamento di queste operazioni nel Sottoscrittore, i valori potrebbero cambiare. Per assicurarsi che i valori nel Sottoscrittore e nel server di pubblicazione non vengano modificati durante la convalida di una pubblicazione, arrestare il servizio Microsoft Distributed Transaction Coordinator (MS DTC) nel server di pubblicazione durante l'operazione di convalida.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_publication_validation**.  
  
## <a name="see-also"></a>Vedere anche  
 [Convalida dei dati nel Sottoscrittore](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_article_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_table_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
