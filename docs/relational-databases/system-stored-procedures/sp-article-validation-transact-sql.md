---
description: sp_article_validation (Transact-SQL)
title: sp_article_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_article_validation_TSQL
- sp_article_validation
helpviewer_keywords:
- sp_article_validation
ms.assetid: 44e7abcd-778c-4728-a03e-7e7e78d3ce22
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1b2dda5e359b062716c9c0fead89b4ad2628488a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203224"
---
# <a name="sp_article_validation-transact-sql"></a>sp_article_validation (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inizializza una richiesta di convalida dei dati per l'articolo specificato. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione e nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_article_validation [ @publication = ] 'publication'  
    [ , [ @article = ] 'article' ]  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @subscription_level = ] subscription_level ]  
    [ , [ @reserved = ] reserved ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione in cui è presente l'articolo. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo da convalidare. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @rowcount_only = ] type_of_check_requested` Specifica se viene restituito solo il conteggio delle righe per la tabella. *type_of_check_requested* è di **smallint** e il valore predefinito è **1**.  
  
 Se è **0**, eseguire un conteggio delle righe e un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] checksum compatibile con 7,0.  
  
 Se è **1**, eseguire solo un controllo RowCount.  
  
 Se è **2**, eseguire un conteggio delle righe e un checksum binario.  
  
`[ @full_or_fast = ] full_or_fast` Metodo utilizzato per calcolare il conteggio delle righe. *full_or_fast* è di **tinyint**. i possibili valori sono i seguenti.  
  
|**Valore**|**Descrizione**|  
|---------------|---------------------|  
|**0**|Esegue un conteggio completo tramite COUNT(*).|  
|**1**|Esegue un conteggio rapido da **sysindexes. Rows**. Il conteggio delle righe in **sysindexes** è più veloce rispetto al conteggio delle righe nella tabella effettiva. Tuttavia, **sysindexes** viene aggiornato in modalità differita e il conteggio delle righe potrebbe non essere accurato.|  
|**2** (impostazione predefinita)|Esegue un conteggio rapido condizionale eseguendo innanzitutto un tentativo con il metodo rapido. Se il metodo rapido evidenzia delle differenze, viene applicato il metodo completo. Se *expected_rowcount* è null e il stored procedure viene usato per ottenere il valore, viene sempre usato un conteggio completo (*).|  
  
`[ @shutdown_agent = ] shutdown_agent` Specifica se l'agente di distribuzione deve essere chiuso immediatamente dopo il completamento della convalida. *shutdown_agent* è di **bit** e il valore predefinito è **0**. Se è **0**, il agente di distribuzione non viene arrestato. Se è **1**, il agente di distribuzione si arresta dopo la convalida dell'articolo.  
  
`[ @subscription_level = ] subscription_level` Specifica se la convalida viene prelevata o meno da un set di sottoscrittori. *subscription_level* è di **bit** e il valore predefinito è **0**. Se è **0**, la convalida viene applicata a tutti i sottoscrittori. Se è **1**, la convalida viene applicata solo a un subset di sottoscrittori specificato dalle chiamate a **sp_marksubscriptionvalidation** nella transazione aperta corrente.  
  
`[ @reserved = ] reserved` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere utilizzato quando viene richiesta la convalida in un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_article_validation** viene utilizzata nella replica transazionale.  
  
 **sp_article_validation** fa in modo che le informazioni di convalida vengano raccolte nell'articolo specificato e invii una richiesta di convalida al log delle transazioni. Quando l'agente di distribuzione riceve la richiesta, confronta le informazioni di convalida incluse nella richiesta con quelle della tabella del Sottoscrittore. I risultati della convalida vengono visualizzati negli avvisi di Monitoraggio replica e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo gli utenti con autorizzazioni SELECT ALL nella tabella di origine per l'articolo da convalidare possono eseguire **sp_article_validation**.  
  
## <a name="see-also"></a>Vedere anche  
 [Convalida dei dati replicati](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_marksubscriptionvalidation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md)   
 [sp_publication_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [sp_table_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
