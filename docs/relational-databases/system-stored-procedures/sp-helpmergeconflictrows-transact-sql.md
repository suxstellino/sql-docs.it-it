---
description: sp_helpmergeconflictrows (Transact-SQL)
title: sp_helpmergeconflictrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergeconflictrows_TSQL
- sp_helpmergeconflictrows
helpviewer_keywords:
- sp_helpmergeconflictrows
ms.assetid: 131395a5-cb18-4795-a7ae-fa09d8ff347f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4aeb1205f3d929eed7b2717ff388d8e03891d9e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179255"
---
# <a name="sp_helpmergeconflictrows-transact-sql"></a>sp_helpmergeconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le righe nella tabella dei conflitti specificata. Questa stored procedure viene eseguita nel computer in cui è archiviata la tabella dei conflitti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergeconflictrows [ [ @publication = ] 'publication' ]  
        , [ @conflict_table = ] 'conflict_table'  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publsher_db' ]   
    [ , [ @logical_record_conflicts = ] logical_record_conflicts ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** . Se la pubblicazione viene specificata, vengono restituiti tutti i conflitti risultanti corrispondenti. Se, ad esempio, nella tabella **MSmerge_conflict_Customers** sono presenti righe con conflitti per le pubblicazioni **WA** e **CA** , il passaggio di un nome di pubblicazione **CA** consente di recuperare i conflitti relativi alla pubblicazione della **CA** .  
  
`[ @conflict_table = ] 'conflict_table'` Nome della tabella dei conflitti. *conflict_table* è di **tipo sysname** e non prevede alcun valore predefinito. In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive, le tabelle dei conflitti vengono denominate usando **l' \_ _\_ articolo_** formato nomi con MSmerge_conflict pubblicazione, con una tabella per ogni articolo pubblicato.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @logical_record_conflicts = ] logical_record_conflicts` Indica se il set di risultati contiene informazioni sui conflitti tra record logici. *logical_record_conflicts* è di **tipo int** e il valore predefinito è 0. **1** indica che vengono restituite informazioni sui conflitti tra record logici.  
  
## <a name="result-sets"></a>Set di risultati  
 **sp_helpmergeconflictrows** restituisce un set di risultati costituito dalla struttura della tabella di base e da queste colonne aggiuntive.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**origin_datasource**|**varchar(255)**|Origine del conflitto.|  
|**conflict_type**|**int**|Codice che indica il tipo di conflitto:<br /><br /> **1** = conflitto di aggiornamento: il conflitto viene rilevato a livello di riga.<br /><br /> **2** = conflitto di aggiornamento della colonna: conflitto rilevato a livello di colonna.<br /><br /> **3** = conflitto di aggiornamento di eliminazione WINS: il conflitto viene eliminato.<br /><br /> **4** = conflitto di eliminazione di aggiornamento WINS: il ROWGUID eliminato che perde il conflitto viene registrato in questa tabella.<br /><br /> **5** = inserimento di caricamento non riuscito: Impossibile applicare l'inserimento dal Sottoscrittore al server di pubblicazione.<br /><br /> **6** = inserimento di download non riuscito: Impossibile applicare l'inserimento dal server di pubblicazione nel Sottoscrittore.<br /><br /> **7** = eliminazione del caricamento non riuscita: non è stato possibile caricare l'eliminazione nel Sottoscrittore nel server di pubblicazione.<br /><br /> **8** = eliminazione del download non riuscita: non è stato possibile scaricare l'eliminazione nel server di pubblicazione nel Sottoscrittore.<br /><br /> **9** = aggiornamento di caricamento non riuscito: non è stato possibile applicare l'aggiornamento nel Sottoscrittore al server di pubblicazione.<br /><br /> **10** = aggiornamento di download non riuscito: non è stato possibile applicare l'aggiornamento nel server di pubblicazione al Sottoscrittore.<br /><br /> **12** = eliminazione WINS aggiornamento record logico: il record logico eliminato che perde il conflitto viene registrato in questa tabella.<br /><br /> **13** = conflitto di inserimento dei record logici: l'inserimento in un record logico è in conflitto con un aggiornamento.<br /><br /> **14** = conflitto di aggiornamento WINS di eliminazione di record logici: il record logico aggiornato che perde il conflitto viene registrato in questa tabella.|  
|**reason_code**|**int**|Codice di errore che può essere sensibile al contesto.|  
|**reason_text**|**varchar (720)**|Descrizione dell'errore che può essere sensibile al contesto.|  
|**pubid**|**uniqueidentifier**|Identificatore della pubblicazione.|  
|**MSrepl_create_time**|**datetime**|Ora in cui sono state aggiunte le informazioni sui conflitti.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergeconflictrows** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database e il ruolo **replmonitor** nel database di distribuzione possono eseguire **sp_helpmergeconflictrows**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzazione delle informazioni sui conflitti per le pubblicazioni di tipo merge &#40;la programmazione Transact-SQL della replica&#41;](../replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
