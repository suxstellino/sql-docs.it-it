---
description: sys.database_recovery_status (Transact-SQL)
title: sys.database_recovery_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_recovery_status_TSQL
- database_recovery_status
- sys.database_recovery_status
- sys.database_recovery_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_recovery_status catalog view
ms.assetid: 46fab234-1542-49be-8edf-aa101e728acf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 91698461a94705f43da35b72339d1ed71c37bcce
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464044"
---
# <a name="sysdatabase_recovery_status-transact-sql"></a>sys.database_recovery_status (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Include una riga per database. Se il database non è aperto, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] tenta di avviarlo.  
  
 Per visualizzare la riga relativa a un database diverso da **Master** o **tempdb**, è necessario applicare uno dei seguenti elementi:  
  
-   Essere proprietario del database.  
  
-   Disporre delle autorizzazioni ALTER ANY DATABASE o VIEW ANY DATABASE a livello di server.  
  
-   Disporre dell'autorizzazione CREATE DATABASE nel database **Master** .    
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database, univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**database_guid**|**uniqueidentifier**|Utilizzato per mettere il relazione tra loro tutti i file di un database. È necessario che tutti i file includano questo GUID nella pagina di intestazione per essere avviati come previsto. Solo un database dovrebbe includere questo GUID, ma è possibile creare duplicati copiando o collegando i database. RESTORE genera sempre un nuovo GUID quando si ripristina un database non ancora esistente.<br /><br /> NULL= Il database è offline o non può essere avviato.|  
|**family_guid**|**uniqueidentifier**|Identificatore del "gruppo di backup" del database per l'individuazione di stati di ripristino corrispondenti.<br /><br /> NULL= Il database è offline o non può essere avviato.|  
|**last_log_backup_lsn**|**numeric(25,0)**|Numero di sequenza del file di log iniziale del backup del log successivo.<br /><br /> Se è NULL, non è possibile eseguire il backup del log delle transazioni perché il database è in modalità di recupero semplice o non è presente alcun backup del database corrente.|  
|**recovery_fork_guid**|**uniqueidentifier**|Identifica il fork di recupero corrente nel quale il database è attualmente attivo.<br /><br /> NULL= Il database è offline o non può essere avviato.|  
|**first_recovery_fork_guid**|**uniqueidentifier**|Identificatore del fork di recupero di inizio.<br /><br /> NULL= Il database è offline o non può essere avviato.|  
|**fork_point_lsn**|**numeric(25,0)**|Se **first_recovery_fork_guid** non è uguale a (! =) **recovery_fork_guid**, **fork_point_lsn** è il numero di sequenza del file di log del punto di fork corrente. Negli altri casi il valore è NULL.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo di database e file &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
