---
description: sp_mergearticlecolumn (Transact-SQL)
title: sp_mergearticlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergearticlecolumn
- sp_mergearticlecolumn_TSQL
helpviewer_keywords:
- sp_mergearticlecolumn
ms.assetid: b4f2b888-e094-4759-a472-d893638995eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5d8d2f63afd421081058dd1b53ec8084f4b5e13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185311"
---
# <a name="sp_mergearticlecolumn-transact-sql"></a>sp_mergearticlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Suddivide una pubblicazione di tipo merge in partizioni verticali. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_mergearticlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column'  
    [ , [ @operation = ] 'operation'   
    [ , [ @schema_replication = ] 'schema_replication' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo della pubblicazione. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @column = ] 'column'` Identifica le colonne su cui creare la partizione verticale. *Column* è di **tipo sysname** e il valore predefinito è null. Se NULL e `@operation = N'add'`, per impostazione predefinita all'articolo vengono aggiunte tutte le colonne della tabella di origine. la *colonna* non può essere null quando l' *operazione* è impostata su **Drop**. Per escludere colonne da un articolo, eseguire **sp_mergearticlecolumn** e specificare la *colonna* e `@operation = N'drop'` per ogni colonna da rimuovere dall' *articolo* specificato.  
  
`[ @operation = ] 'operation'` Stato della replica. *Operation* è di **tipo nvarchar (4)** e il valore predefinito è Add. **Aggiungi** contrassegna la colonna per la replica. **Drop** Cancella la colonna.  
  
`[ @schema_replication = ] 'schema_replication'` Specifica che una modifica dello schema verrà propagata durante l'esecuzione del agente di merge. *schema_replication* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
> [!NOTE]  
>  Per *schema_replication* è supportato solo il valore **false** .  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Abilita o Disabilita la possibilità di invalidare uno snapshot. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non saranno valide per lo snapshot.  
  
 **1** specifica che le modifiche apportate all'articolo di merge potrebbero invalidare lo snapshot. in tal caso, il valore **1** consente di eseguire il nuovo snapshot.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Abilita o Disabilita la possibilità di reinizializzare della sottoscrizione. *force_reinit_subscription* è un bit e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non comporteranno la reinizializzazione della sottoscrizione.  
  
 **1** specifica che le modifiche apportate all'articolo di merge possono causare la reinizializzazione della sottoscrizione. in tal caso, il valore **1** consente la reinizializzazione della sottoscrizione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_mergearticlecolumn** viene utilizzata nella replica di tipo merge.  
  
 Non è possibile eliminare una colonna Identity dall'articolo se viene utilizzata la gestione automatica degli intervalli di valori Identity. Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Se un'applicazione imposta una nuova partizione verticale dopo la creazione dello snapshot iniziale, è necessario generare un nuovo snapshot e riassociarlo a ogni sottoscrizione. Gli snapshot vengono associati alla successiva esecuzione pianificata dell'agente snapshot e di distribuzione o di merge.  
  
 Se si utilizza il rilevamento a livello di riga per il rilevamento dei conflitti (impostazione predefinita), la tabella di base può includere fino a 1.024 colonne, che devono tuttavia essere filtrate dall'articolo in modo da pubblicare un massimo di 246 colonne. Se viene utilizzato il rilevamento a livello di colonna, nella tabella di base possono essere incluse al massimo 246 colonne.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-mergearticlecolumn-tr_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_mergearticlecolumn**.  
  
## <a name="see-also"></a>Vedere anche  
 [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtrare i dati pubblicati](../../relational-databases/replication/publish/filter-published-data.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
