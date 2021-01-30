---
description: managed_backup managed_backup.sp_backup_config_advanced (Transact-SQL)
title: managed_backup managed_backup.sp_backup_config_advanced (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_backup_config_optional
- sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional
dev_langs:
- TSQL
helpviewer_keywords:
- sp_backup_config_optional
- managed_backup.sp_backup_config_optional
ms.assetid: 4fae8193-1f88-48fd-a94a-4786efe8d6af
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 77b56d757d685e30798c8763c948dbb72e89c2d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203726"
---
# <a name="managed_backupsp_backup_config_advanced-transact-sql"></a>managed_backup managed_backup.sp_backup_config_advanced (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Configura le impostazioni avanzate per [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
EXEC managed_backup.sp_backup_config_advanced   
    [@database_name = ] 'database_name'  
    ,[@encryption_algorithm = ] 'name of the encryption algorithm'  
    ,[@encryptor_type = ] {'CERTIFICATE' | 'ASYMMETRIC_KEY'}  
    ,[@encryptor_name = ] 'name of the certificate or asymmetric key'  
    ,[@local_cache_path = ] 'NOT AVAILABLE'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 @database_name  
 Nome del database per l'abilitazione del backup gestito in un database specifico. Se è NULL o *, questo backup gestito si applica a tutti i database nel server.  
  
 @encryption_algorithm  
 Nome dell'algoritmo di crittografia utilizzato durante il backup per crittografare il file di backup. @encryption_algorithmÈ di **tipo sysname**. È un parametro obbligatorio quando si configura il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per la prima volta per il database. Specificare **NO_ENCRYPTION** se non si desidera crittografare il file di backup. Quando si modificano le [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] impostazioni di configurazione, questo parametro è facoltativo. se il parametro non viene specificato, vengono conservati i valori di configurazione esistenti. I valori consentiti per questo parametro sono:  
  
-   AES_128  
  
-   AES_192  
  
-   AES_256  
  
-   TRIPLE_DES_3KEY  
  
-   NO_ENCRYPTION  
  
 Per altre informazioni sugli algoritmi di crittografia, vedere [Choose an Encryption Algorithm](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
 @encryptor_type  
 Tipo di crittografia, che può essere ' CERTIFICAte ' o ' ASYMMETRIC_KEY '. @encryptor_typeÈ di **tipo nvarchar (32)**. Questo parametro è facoltativo se si specifica NO_ENCRYPTION per il @encryption_algorithm parametro.  
  
 @encryptor_name  
 Nome di un certificato o una chiave asimmetrica esistente da utilizzare per crittografare il backup. @encryptor_nameÈ di **tipo sysname**. Se si utilizza una chiave asimmetrica, deve essere configurata con Extensible Key Management (EKM). Questo parametro è facoltativo se si specifica NO_ENCRYPTION per il @encryption_algorithm parametro.  
  
 Per ulteriori informazioni, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 @local_cache_path  
 Questo parametro non è ancora supportato.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **db_backupoperator** database, con autorizzazioni **ALTER ANY CREDENTIAL** e autorizzazioni **Execute** per **sp_delete_backuphistory** stored procedure.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono impostate le opzioni di configurazione avanzate per [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per l'istanza di SQL Server.  
  
```sql
Use msdb;  
Go  
   EXEC managed_backup.sp_backup_config_advanced  
                @encryption_algorithm ='AES_128'  
                ,@encryptor_type = 'CERTIFICATE'  
                ,@encryptor_name = 'MyTestDBBackupEncryptCert'  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [managed_backup managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)   
 [managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)  
  
  
