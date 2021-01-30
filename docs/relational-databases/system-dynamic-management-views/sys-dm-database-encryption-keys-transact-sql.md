---
description: sys.dm_database_encryption_keys (Transact-SQL)
title: sys.dm_database_encryption_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_database_encryption_keys
- sys.dm_database_encryption_keys_TSQL
- dm_database_encryption_keys
- dm_database_encryption_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_database_encryption_keys dynamic management view
ms.assetid: 56fee8f3-06eb-4fff-969e-abeaa0c4b8e4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96bf6756f26e0bc8b82c6f7f008203fcb206c22f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184340"
---
# <a name="sysdm_database_encryption_keys-transact-sql"></a>sys.dm_database_encryption_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sullo stato di crittografia di un database e sulle chiavi di crittografia a esso associate. Per altre informazioni sulla crittografia del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID del database.|  
|encryption_state|**int**|Indica se il database è crittografato o meno.<br /><br /> 0 = Nessuna chiave di crittografia del database presente, nessuna crittografia<br /><br /> 1 = Non crittografato<br /><br /> 2 = Crittografia in corso<br /><br /> 3 = Crittografato<br /><br /> 4 = Modifica chiave in corso<br /><br /> 5 = Decrittografia in corso<br /><br /> 6 = Modifica della protezione in corso. È in corso la modifica della chiave asimmetrica o del certificato usato per crittografare la chiave di crittografia del database.|  
|create_date|**datetime**|Visualizza la data (in formato UTC) in cui è stata creata la chiave di crittografia.|  
|regenerate_date|**datetime**|Visualizza la data (in formato UTC) in cui è stata rigenerata la chiave di crittografia.|  
|modify_date|**datetime**|Visualizza la data (in formato UTC) in cui è stata modificata la chiave di crittografia.|  
|set_date|**datetime**|Visualizza la data (in formato UTC) in cui è stata applicata la chiave di crittografia al database.|  
|opened_date|**datetime**|Indica la data e l'ora dell'ultima apertura della chiave del database.|  
|key_algorithm|**nvarchar(32)**|Visualizza l'algoritmo usato per la chiave.|  
|key_length|**int**|Visualizza la lunghezza della chiave.|  
|encryptor_thumbprint|**varbinary(20)**|Mostra l'identificazione digitale della crittografia.|  
|encryptor_type|**nvarchar(32)**|**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).<br /><br /> Descrive il componente di crittografia.|  
|percent_complete|**real**|Percentuale di completamento del cambiamento di stato della crittografia del database. In assenza di un cambiamento di stato il valore sarà 0.|
|encryption_state_desc|**nvarchar(32)**|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e versioni successive.<br><br> Stringa che indica se il database è crittografato o non crittografato.<br><br>NONE<br><br>NON crittografati<br><br>CRITTOGRAFATI<br><br>DECRYPTION_IN_PROGRESS<br><br>ENCRYPTION_IN_PROGRESS<br><br>KEY_CHANGE_IN_PROGRESS<br><br>PROTECTION_CHANGE_IN_PROGRESS|
|encryption_scan_state|**int**|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e versioni successive.<br><br>Indica lo stato corrente dell'analisi della crittografia. <br><br>0 = non è stata avviata alcuna analisi, Transparent Data Encryption non è abilitato<br><br>1 = l'analisi è in corso.<br><br>2 = l'analisi è in corso ma è stata sospesa, l'utente può riprendere.<br><br>3 = l'analisi è stata interrotta per qualche motivo, è necessario un intervento manuale. Per ulteriori informazioni, contattare supporto tecnico Microsoft.<br><br>4 = l'analisi è stata completata correttamente, Transparent Data Encryption è abilitato e la crittografia è stata completata.|
|encryption_scan_state_desc|**nvarchar(32)**|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e versioni successive.<br><br>Stringa che indica lo stato corrente dell'analisi della crittografia.<br><br> NONE<br><br>RUNNING<br><br>SUSPENDED<br><br>ABORTED<br><br>COMPLETARE|
|encryption_scan_modify_date|**datetime**|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e versioni successive.<br><br> Visualizza la data (in formato UTC) dell'Ultima modifica dello stato di analisi della crittografia.|
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  

 [Funzioni e viste a gestione dinamica relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [Crittografia di SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Chiavi di crittografia del database e di SQL Server &#40;Motore di database&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)  
  
