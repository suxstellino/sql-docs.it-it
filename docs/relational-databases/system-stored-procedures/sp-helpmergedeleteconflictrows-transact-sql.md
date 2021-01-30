---
description: sp_helpmergedeleteconflictrows (Transact-SQL)
title: sp_helpmergedeleteconflictrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergedeleteconflictrows
- sp_helpmergedeleteconflictrows_TSQL
helpviewer_keywords:
- sp_helpmergedeleteconflictrows
ms.assetid: 222be651-5690-4341-9dfb-f9ec1d80c970
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07ca90c1eb7792ea5d197fe37a3542f526f870b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179249"
---
# <a name="sp_helpmergedeleteconflictrows-transact-sql"></a>sp_helpmergedeleteconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle righe di dati che hanno perso nei conflitti di eliminazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di sottoscrizione del Sottoscrittore quando si utilizza la registrazione dei conflitti decentralizzata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergedeleteconflictrows [ [ @publication = ] 'publication']  
    [ , [ @source_object = ] 'source_object']  
    [ , [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publsher_db'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** . Se la pubblicazione viene specificata, vengono restituiti tutti i conflitti risultanti corrispondenti.  
  
`[ @source_object = ] 'source_object'` Nome dell'oggetto di origine. *source_object* è di **tipo nvarchar (386)** e il valore predefinito è null.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**source_object**|**nvarchar (386)**|Oggetto di origine per il conflitto di eliminazione.|  
|**rowguid**|**uniqueidentifier**|Identificatore di riga per il conflitto di eliminazione.|  
|**conflict_type**|**int**|Codice che indica il tipo di conflitto:<br /><br /> **1** = UpdateConflict: il conflitto viene rilevato a livello di riga.<br /><br /> **2** = ColumnUpdateConflict: conflitto rilevato a livello di colonna.<br /><br /> **3** = UpdateDeleteWinsConflict: il conflitto viene vinto da Delete.<br /><br /> **4** = UpdateWinsDeleteConflict: il ROWGUID eliminato che perde il conflitto viene registrato in questa tabella.<br /><br /> **5** = UploadInsertFailed: Impossibile applicare l'inserimento dal Sottoscrittore al server di pubblicazione.<br /><br /> **6** = DownloadInsertFailed: Impossibile applicare l'inserimento dal server di pubblicazione nel Sottoscrittore.<br /><br /> **7** = UploadDeleteFailed: Impossibile caricare l'eliminazione nel Sottoscrittore nel server di pubblicazione.<br /><br /> **8** = DownloadDeleteFailed: non è stato possibile scaricare l'eliminazione nel server di pubblicazione nel Sottoscrittore.<br /><br /> **9** = UploadUpdateFailed: Impossibile applicare l'aggiornamento nel Sottoscrittore nel server di pubblicazione.<br /><br /> **10** = DownloadUpdateFailed: Impossibile applicare l'aggiornamento nel server di pubblicazione al Sottoscrittore.|  
|**reason_code**|**Int**|Codice di errore che può essere sensibile al contesto.|  
|**reason_text**|**varchar (720)**|Descrizione dell'errore che può essere sensibile al contesto.|  
|**origin_datasource**|**varchar(255)**|Origine del conflitto.|  
|**pubid**|**uniqueidentifier**|Identificatore della pubblicazione.|  
|**MSrepl_create_time**|**datetime**|Ora in cui sono state aggiunte le informazioni sui conflitti.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergedeleteconflictrows** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_helpmergedeleteconflictrows**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
