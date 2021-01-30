---
description: sys.column_master_keys (Transact-SQL)
title: sys.column_master_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_master_key_definitions_TSQL
- column_master_key_definitions
- sys.column_master_key_definitions_TSQL
- sys.column_master_key_definitions
- column_master_keys_TSQL
- column_master_keys
- sys.column_master_keys_TSQL
- sys.column_master_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_master_key_definitions catalog view
- sys.column_master_keys catalog view
ms.assetid: fbec2efa-5fe9-4121-9b34-60497b0b2aca
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33682106df449dbdd1dd1c4244dffe89118a2964
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161415"
---
# <a name="syscolumn_master_keys-transact-sql"></a>sys.column_master_keys (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce una riga per ogni chiave master del database aggiunta usando l'istruzione [create Master Key](../../t-sql/statements/create-column-master-key-transact-sql.md) . Ogni riga rappresenta una singola chiave master della colonna (CMK).  
    
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del CMK.|  
|**column_master_key_id**|**int**|ID della chiave master della colonna.|  
|**create_date**|**datetime**|Data di creazione della chiave master della colonna.|  
|**modify_date**|**datetime**|Data dell'Ultima modifica della chiave master della colonna.|  
|**key_store_provider_name**|**sysname**|Nome del provider per l'archivio delle chiavi master della colonna contenente CMK. I valori consentiti sono i seguenti:<br /><br /> MSSQL_CERTIFICATE_STORE: se l'archivio della chiave master della colonna è un archivio certificati.<br /><br /> Valore definito dall'utente, se l'archivio della chiave master della colonna è di un tipo personalizzato.|  
|**key_path**|**nvarchar(4000)**|Percorso specifico dell'archivio della chiave master della colonna. Il formato del percorso dipende dal tipo di archivio delle chiavi master della colonna. Esempio:<br /><br /> `'CurrentUser/Personal/'<thumbprint>`<br /><br /> Per un archivio chiavi master di colonna personalizzato, lo sviluppatore è responsabile della definizione di un percorso chiave per l'archivio delle chiavi master della colonna personalizzata.|  
|**allow_enclave_computations**|**bit**|Indica se la chiave master della colonna è abilitata per l'enclave, (se le chiavi di crittografia della colonna, crittografate con la chiave master, possono essere usate per i calcoli all'interno di enclave protette sul lato server). Per altre informazioni, vedere [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).|  
|**URL REST**|**varbinary(max)**|Firma digitale di **key_path** e **allow_enclave_computations**, prodotti usando la chiave master della colonna, a cui fa riferimento **key_path**.|


  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **View any Column Master Key** .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Gestire le chiavi per Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
 
  
  
