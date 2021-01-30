---
description: sp_showpendingchanges (Transact-SQL)
title: sp_showpendingchanges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showpendingchanges
- sp_showpendingchanges_TSQL
helpviewer_keywords:
- sp_showpendingchanges
ms.assetid: 8013a792-639d-4550-b262-e65d30f9d291
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac2c44e836a56c6fa503b6144e99761294ab473a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207306"
---
# <a name="sp_showpendingchanges-transact-sql"></a>sp_showpendingchanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un set di risultati che mostra le modifiche in attesa di replicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione e nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  Questa procedura consente di ottenere un'approssimazione del numero di modifiche e delle righe interessate da tali modifiche, recuperando ad esempio informazioni dal Server di pubblicazione o dal Sottoscrittore, ma non da entrambi contemporaneamente. Le informazioni archiviate nell'altro nodo potrebbero produrre un set di modifiche più piccolo da sincronizzare rispetto alle stime della procedura.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_showpendingchanges [ [ @destination_server = ] 'destination_server' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article']  
    [ , [ @show_rows = ] show_rows]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @destination_server = ] 'destination_server'` Nome del server in cui vengono applicate le modifiche replicate. *destination_server* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è null. Quando si specifica *Publication* , i risultati sono limitati solo alla pubblicazione specificata.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e il valore predefinito è null. Quando si specifica *article* , i risultati sono limitati solo all'articolo specificato.  
  
`[ @show_rows = ] 'show_rows'` Specifica se il set di risultati contiene informazioni più specifiche sulle modifiche in sospeso e il valore predefinito è **0**. Se viene specificato il valore **1** , il set di risultati contiene le colonne is_delete e rowguid.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|destination_server|**sysname**|Nome del server nel quale è in corso la replica delle modifiche.|  
|pub_name|**sysname**|Nome della pubblicazione.|  
|destination_db_name|**sysname**|Nome del database nel quale è in corso la replica delle modifiche.|  
|is_dest_subscriber|**bit**|Indica che è in corso la replica delle modifiche in un Sottoscrittore. Il valore **1** indica che le modifiche vengono replicate in un Sottoscrittore. **0** indica che le modifiche vengono replicate in un server di pubblicazione.|  
|article_name|**sysname**|Nome dell'articolo nella tabella di origine delle modifiche.|  
|pending_deletes|**int**|Numero di eliminazioni in attesa della replica.|  
|pending_ins_and_upd|**int**|Numero di inserimenti e aggiornamenti in attesa della replica.|  
|is_delete|**bit**|Indica se la modifica in sospeso è un'eliminazione. Il valore **1** indica che la modifica è un'eliminazione. Richiede il valore **1** per @show_rows .|  
|rowguid|**uniqueidentifier**|GUID che identifica la riga modificata. Richiede il valore **1** per @show_rows .|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_showpendingchanges viene utilizzata per la replica di tipo merge.  
  
 sp_showpendingchanges viene utilizzata per la risoluzione dei problemi relativi alla replica di tipo merge.  
  
 Il risultato della stored procedure sp_showpendingchanges non include righe di generazione 0.  
  
 Quando un articolo specificato per *article* non appartiene alla pubblicazione specificata per la *pubblicazione,* viene restituito un conteggio pari a 0 per pending_deletes e pending_ins_and_upd.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server sysadmin o del ruolo predefinito del database db_owner possono eseguire sp_showpendingchanges.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
