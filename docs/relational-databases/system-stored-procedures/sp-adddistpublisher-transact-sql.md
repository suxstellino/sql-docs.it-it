---
description: sp_adddistpublisher (Transact-SQL)
title: sp_adddistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: c25bc50edbbf1c6826f091d2ea1570b423ddaf30
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937819"
---
# <a name="sp_adddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Configura un server di pubblicazione per l'utilizzo del database di distribuzione specificato. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione. Si noti che è necessario eseguire le stored procedure [sp_adddistributor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) e [sp_adddistributiondb &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) prima di utilizzare questa stored procedure.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @storage_connection_string= ] 'storage_connection_string']
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato. L'uso di numeri di porta personalizzati per il server di distribuzione remoto si applica solo a SQL Server 2019.

::: moniker-end
  
`[ @distribution_db = ] 'distribution_db'` Nome del database di distribuzione. *distributor_db* è di **tipo sysname** e non prevede alcun valore predefinito. Questo parametro viene utilizzato dagli agenti di replica per la connessione al server di pubblicazione.  
  
`[ @security_mode = ] security_mode` Modalità di sicurezza implementata. Questo parametro viene utilizzato solo dagli agenti di replica per connettersi al server di pubblicazione per le sottoscrizioni ad aggiornamento in coda o con un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione non. *security_mode* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Gli agenti di replica nel server di distribuzione si connettono al server di pubblicazione tramite l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**1** (impostazione predefinita)|Gli agenti di replica nel server di distribuzione si connettono al server di pubblicazione tramite l'autenticazione di Windows.|  
  
`[ @login = ] 'login'` Account di accesso. Questo parametro è obbligatorio se *security_mode* è **0**. *login* è di tipo **sysname** e il valore predefinito è NULL. Questo parametro viene utilizzato dagli agenti di replica per la connessione al server di pubblicazione.  
  
`[ @password = ] 'password']` Password. *password* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene utilizzato dagli agenti di replica per la connessione al server di pubblicazione.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa.  
  
`[ @working_directory = ] 'working_directory'` Nome della directory di lavoro utilizzata per archiviare i file di dati e di schema per la pubblicazione. *working_directory* è di **tipo nvarchar (255)** e il valore predefinito è la cartella Repldata per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio `C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData` . Il nome deve essere specificato in formato UNC.  

 Per il database SQL di Azure, usare `\\<storage_account>.file.core.windows.net\<share>` .

`[ @storage_connection_string = ] 'storage_connection_string'` È obbligatorio per il database SQL. Usare la chiave di accesso dal portale di Azure in impostazioni > di archiviazione.

 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]

`[ @trusted = ] 'trusted'` Questo parametro è deprecato ed è disponibile solo per compatibilità con le versioni precedenti. *Trusted* è di **tipo nvarchar (5)** e l'impostazione su qualsiasi valore, ma **false** , comporterà un errore.  
  
`[ @encrypted_password = ] encrypted_password` L'impostazione di *encrypted_password* non è più supportata. Se si tenta di impostare questo parametro di **bit** su **1** , verrà generato un errore.  
  
`[ @thirdparty_flag = ] thirdparty_flag` È quando il server di pubblicazione è [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *thirdparty_flag* è di **bit**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**1**|Database non di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
`[ @publisher_type = ] 'publisher_type'` Specifica il tipo di server di pubblicazione quando il server di pubblicazione non lo è [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* è di tipo sysname. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> (predefinito)|Specifica un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Specifica un server di pubblicazione Oracle standard.|  
|**ORACLE GATEWAY**|Specifica un server di pubblicazione Oracle Gateway.|  
  
 Per ulteriori informazioni sulle differenze tra un server di pubblicazione Oracle e un server di pubblicazione Oracle Gateway, vedere [configurare un server di pubblicazione Oracle](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_adddistpublisher** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_adddistpublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurare la distribuzione](../../relational-databases/replication/configure-distribution.md)  
  
  
