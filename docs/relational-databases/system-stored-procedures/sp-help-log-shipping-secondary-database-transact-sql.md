---
description: sp_help_log_shipping_secondary_database (Transact-SQL)
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5fc81b903438d874a80399563527747ada9ef43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199629"
---
# <a name="sp_help_log_shipping_secondary_database-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa stored procedure recupera le impostazioni per uno o più database secondari.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @secondary_database = ] 'secondary_database'` Nome del database secondario. *secondary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @secondary_id = ] 'secondary_id'` ID del server secondario nella configurazione del log shipping. *secondary_id* è di tipo **uniqueidentifier** e non può essere null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**secondary_id**|ID del server secondario nella configurazione per il log shipping.|  
|**primary_server**|Nome dell'istanza primaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione per il log shipping.|  
|**primary_database**|Nome del database primario nella configurazione di log shipping.|  
|**backup_source_directory**|Directory in cui vengono archiviati i file di backup del log delle transazioni dal server primario.|  
|**backup_destination_directory**|Directory nel server secondario in cui vengono copiati i file di backup.|  
|**file_retention_period**|Durata di tempo, in minuti, durante la quale un file di backup viene mantenuto nel server secondario prima di essere eliminato.|  
|**copy_job_id**|ID associato al processo di copia nel server secondario.|  
|**restore_job_id**|ID associato al processo di ripristino nel server secondario.|  
|**monitor_server**|Nome dell'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilizzata come server di monitoraggio nella configurazione del log shipping.|  
|**monitor_server_security_mode**|Modalità di sicurezza utilizzata per connettersi al server di monitoraggio.<br /><br /> 1 = Autenticazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di.|  
|**secondary_database**|Nome del database secondario nella configurazione di log shipping.|  
|**restore_delay**|Indica per quanti minuti il server secondario deve attendere prima di ripristinare un file di backup specifico. Il valore predefinito è 0 minuti.|  
|**restore_all**|Se impostato su 1, il server secondario ripristina tutti i backup del log delle transazioni disponibili al momento dell'esecuzione del processo di ripristino. In caso contrario, l'operazione viene arrestata dopo il ripristino di un file.|  
|**restore_mode**|Modalità di ripristino per il database secondario.<br /><br /> 0 = Ripristina log con NORECOVERY.<br /><br /> 1 = Ripristina log con STANDBY.|  
|**disconnect_users**|Se impostato su 1, gli utenti vengono disconnessi dal database secondario quando viene eseguita un'operazione di ripristino. Valore predefinito = 0.|  
|**block_size**|Dimensioni, in byte, per il blocco del dispositivo di backup.|  
|**buffer_count**|Numero totale di buffer utilizzati dall'operazione di backup o di ripristino.|  
|**max_transfer_size**|Dimensione, espressa in byte, della richiesta di input o output massimo emessa da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il dispositivo di backup.|  
|**restore_threshold**|Numero di minuti che può trascorrere tra operazioni di ripristino prima che venga generato un avviso.|  
|**threshold_alert**|Avviso da generare quando viene superata la soglia di ripristino.|  
|**threshold_alert_enabled**|Determina se sono abilitati gli avvisi di soglia di ripristino.<br /><br /> 1 = abilitati.<br /><br /> 0 = disabilitati.|  
|**last_copied_file**|Nome dell'ultimo file di backup copiato nel server secondario.|  
|**last_copied_date**|Data e ora dell'ultima operazione di copia nel server secondario.|  
|**last_copied_date_utc**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di copia nel server secondario.|  
|**last_restored_file**|Nome dell'ultimo file di backup ripristinato nel database secondario.|  
|**last_restored_date**|Data e ora dell'ultima operazione di ripristino nel database secondario.|  
|**last_restored_date_utc**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di ripristino nel database secondario.|  
|**history_retention_period**|Periodo di tempo, in minuti, durante il quale i record della cronologia di log shipping vengono mantenuti per un database secondario specificato prima di essere eliminati.|  
|**last_restored_latency**|Intervallo, in minuti, intercorso tra la creazione del backup del log nel server primario e il relativo ripristino nel server secondario.<br /><br /> Il valore iniziale è NULL.|  
  
## <a name="remarks"></a>Commenti  
 Se si include il parametro *secondary_database* , il set di risultati conterrà informazioni sul database secondario. Se si include il parametro *secondary_id* , il set di risultati conterrà informazioni su tutti i database secondari associati a tale ID secondario.  
  
 **sp_help_log_shipping_secondary_database** deve essere eseguito dal database **Master** nel server secondario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [sp_help_log_shipping_secondary_primary &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
