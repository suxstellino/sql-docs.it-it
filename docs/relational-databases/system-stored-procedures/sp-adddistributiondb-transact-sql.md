---
description: sp_adddistributiondb (Transact-SQL)
title: sp_adddistributiondb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/30/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributiondb_TSQL
- sp_adddistributiondb
helpviewer_keywords:
- sp_adddistributiondb
ms.assetid: e9bad56c-d2b3-44ba-a4d7-ff2fd842e32d
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a5a80b39ca68abd37f3e9e843c2e76e8e71669d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182818"
---
# <a name="sp_adddistributiondb-transact-sql"></a>sp_adddistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo database di distribuzione e installa lo schema del server di distribuzione. Nel database di distribuzione vengono archiviate le procedure, lo schema e i metadati utilizzati nella replica. Questa stored procedure viene eseguita nel database master del server di distribuzione per la creazione del database di distribuzione e per l'installazione delle tabelle e delle stored procedure necessarie per la distribuzione della replica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adddistributiondb [ @database= ] 'database'   
    [ , [ @data_folder= ] 'data_folder' ]   
    [ , [ @data_file= ] 'data_file' ]   
    [ , [ @data_file_size= ] data_file_size ]   
    [ , [ @log_folder= ] 'log_folder' ]   
    [ , [ @log_file= ] 'log_file' ]   
    [ , [ @log_file_size= ] log_file_size ]   
    [ , [ @min_distretention= ] min_distretention ]   
    [ , [ @max_distretention= ] max_distretention ]   
    [ , [ @history_retention= ] history_retention ]   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @createmode= ] createmode ]  
    [ , [ @from_scripting = ] from_scripting ] 
    [ , [ @deletebatchsize_xact = ] deletebatchsize_xact ] 
    [ , [ @deletebatchsize_cmd = ] deletebatchsize_cmd ] 
```  
  
## <a name="arguments"></a>Argomenti  
`[ @database = ] database'` Nome del database di distribuzione da creare. il *database* è di **tipo sysname** e non prevede alcun valore predefinito. Se il database specificato esiste già e non è contrassegnato come database di distribuzione, vengono installati gli oggetti necessari per consentire la distribuzione e il database viene contrassegnato come database di distribuzione. Se il database specificato è già abilitato come database di distribuzione, viene restituito un errore.  
  
`[ @data_folder = ] 'data_folder'_` Nome della directory utilizzata per archiviare il file di dati del database di distribuzione. *data_folder* è di **tipo nvarchar (255)** e il valore predefinito è null. Se è NULL, viene utilizzata la directory dei dati per l'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data` .  
  
`[ @data_file = ] 'data_file'` Nome del file di database. *data_file* è di **tipo nvarchar (255)** e il valore predefinito è **database**. Se il valore è NULL, la stored procedure crea il nome del nuovo file in base al nome del database.  
  
`[ @data_file_size = ] data_file_size` Dimensioni iniziali del file di dati in megabyte (MB). *data_file_size i* s **int** e il valore predefinito è 5 MB.  
  
`[ @log_folder = ] 'log_folder'` Nome della directory per il file di log del database. *log_folder* è di **tipo nvarchar (255)** e il valore predefinito è null. Se il valore è Null, viene utilizzata la directory dei dati di tale istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ad esempio `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`).  
  
`[ @log_file = ] 'log_file'` Nome del file di log. *log_file* è di **tipo nvarchar (255)** e il valore predefinito è null. Se il valore è NULL, la stored procedure crea il nome del nuovo file in base al nome del database.  
  
`[ @log_file_size = ] log_file_size` Dimensioni iniziali del file di log in megabyte (MB). *log_file_size* è di **tipo int** e il valore predefinito è 0 MB, che indica che le dimensioni del file vengono create utilizzando le dimensioni del file di log più piccole consentite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @min_distretention = ] min_distretention` Periodo di memorizzazione minimo, in ore, prima dell'eliminazione delle transazioni dal database di distribuzione. *min_distretention* è di **tipo int** e il valore predefinito è 0 ore.  
  
`[ @max_distretention = ] max_distretention` Periodo di memorizzazione massimo, in ore, prima dell'eliminazione delle transazioni. *max_distretention* è di **tipo int** e il valore predefinito è 72 ore. Le sottoscrizioni che non hanno ricevuto comandi replicati e che hanno superato il periodo di memorizzazione massimo per la distribuzione vengono contrassegnate come inattive e devono essere reinizializzate. Per ogni sottoscrizione disattivata viene eseguita l'istruzione RAISERROR 21011. Il valore **0** indica che le transazioni replicate non vengono archiviate nel database di distribuzione.  
  
`[ @history_retention = ] history_retention` Numero di ore di memorizzazione della cronologia. *history_retention* è di **tipo int** e il valore predefinito è 48 ore.  
  
`[ @security_mode = ] security_mode` Modalità di sicurezza da utilizzare per la connessione al server di distribuzione. *security_mode* è di **tipo int** e il valore predefinito è 1. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione; **1** specifica l'autenticazione integrata di Windows.  
  
`[ @login = ] 'login'` Nome dell'account di accesso utilizzato per la connessione al server di distribuzione per la creazione del database di distribuzione. Questa operazione è necessaria se *security_mode* è impostato su **0**. *login* è di tipo **sysname** e il valore predefinito è NULL.  
  
`[ @password = ] 'password'` Password utilizzata per la connessione al server di distribuzione. Questa operazione è necessaria se *security_mode* è impostato su **0**. *password* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @createmode = ] createmode`*createmode* è di **tipo int** e il valore predefinito è 1. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**1** (impostazione predefinita)|CREARE il DATABASE o utilizzare il database esistente, quindi applicare il file **Instdist. SQL** per creare oggetti di replica nel database di distribuzione.|  
|**2**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
 
`[ @deletebatchsize_xact = ] deletebatchsize_xact` Specifica le dimensioni del batch da utilizzare durante la pulizia delle transazioni scadute dalle tabelle MSRepl_Transactions. *deletebatchsize_xact* è di **tipo int** e il valore predefinito è 5000. Questo parametro è stato introdotto per la prima volta in SQL Server 2017, seguito dalle versioni in SQL Server 2012 SP4 e SQL Server 2016 SP2.  

`[ @deletebatchsize_cmd = ] deletebatchsize_cmd` Specifica le dimensioni del batch da utilizzare durante la pulizia dei comandi scaduti dalle tabelle MSRepl_Commands. *deletebatchsize_cmd* è di **tipo int** e il valore predefinito è 2000. Questo parametro è stato introdotto per la prima volta in SQL Server 2017, seguito dalle versioni in SQL Server 2012 SP4 e SQL Server 2016 SP2. 
 
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_adddistributiondb** viene utilizzato in tutti i tipi di replica. ma viene eseguita solo in un server di distribuzione.  
  
 È necessario configurare il server di distribuzione eseguendo [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) prima di eseguire **sp_adddistributiondb**.  
  
 Eseguire **sp_adddistributor** prima di eseguire **sp_adddistributiondb**.  
  
## <a name="example"></a>Esempio  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_adddistributiondb**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributiondb &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurare la distribuzione](../../relational-databases/replication/configure-distribution.md)  
  
  
