---
title: Script di informazioni su database di distribuzione e server di pubblicazione | Microsoft Docs
description: Usare questo script per rispondere alle domande frequenti sugli oggetti nel server di distribuzione e nel server di pubblicazione in SQL Server.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Publishers [SQL Server replication], information scripts
- Distributors [SQL Server replication], information scripts
ms.assetid: 8622db47-c223-48fa-87ff-0b4362cd069a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3681a4241ef4e3ed58072eadb9f24b388d6fb670
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467472"
---
# <a name="distributor-and-publisher-information-script"></a>Script di informazioni sui server di distribuzione e di pubblicazione
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Questo script utilizza le tabelle di sistema e le stored procedure di replica per rispondere a domande frequenti su oggetti presenti nei server di distribuzione e pubblicazione. Può essere utilizzato senza alcuna variazione e può inoltre costituire la base per la generazione di script personalizzati. Per eseguire lo script nel proprio ambiente potrebbe essere necessario apportare due modifiche:  
  
-   Modificare la riga `use AdventureWorks2012` per utilizzare il nome del database di pubblicazione in uso.  
  
-   Rimuovere i commenti (`--`) dalla riga `exec sp_helparticle @publication='<PublicationName>'` e sostituire \<PublicationName> con il nome di una pubblicazione.  
  
```  
--********** Execute at the Distributor in the master database **********--  
  
USE master;  
go  
  
--Is the current server a Distributor?  
--Is the distribution database installed?  
--Are there other Publishers using this Distributor?  
EXEC sp_get_distributor  
  
--Is the current server a Distributor?  
SELECT is_distributor FROM sys.servers WHERE name='repl_distributor' AND data_source=@@servername;  
  
--Which databases on the Distributor are distribution databases?  
SELECT name FROM sys.databases WHERE is_distributor = 1  
  
--What are the Distributor and distribution database properties?  
EXEC sp_helpdistributor;  
EXEC sp_helpdistributiondb;  
EXEC sp_helpdistpublisher;  
  
--********** Execute at the Publisher in the master database **********--  
  
--Which databases are published for replication and what type of replication?  
EXEC sp_helpreplicationdboption;  
  
--Which databases are published using snapshot replication or transactional replication?  
SELECT name as tran_published_db FROM sys.databases WHERE is_published = 1;  
--Which databases are published using merge replication?  
SELECT name as merge_published_db FROM sys.databases WHERE is_merge_published = 1;  
  
--What are the properties for Subscribers that subscribe to publications at this Publisher?  
EXEC sp_helpsubscriberinfo;  
  
--********** Execute at the Publisher in the publication database **********--  
  
USE AdventureWorks2012;  
go  
  
--What are the snapshot and transactional publications in this database?   
EXEC sp_helppublication;  
--What are the articles in snapshot and transactional publications in this database?  
--REMOVE COMMENTS FROM NEXT LINE AND REPLACE <PublicationName> with the name of a publication  
--EXEC sp_helparticle @publication='<PublicationName>';  
  
--What are the merge publications in this database?   
EXEC sp_helpmergepublication;  
--What are the articles in merge publications in this database?  
EXEC sp_helpmergearticle; -- to return information on articles for a single publication, specify @publication='<PublicationName>'  
  
--Which objects in the database are published?  
SELECT name AS published_object, schema_id, is_published AS is_tran_published, is_merge_published, is_schema_published  
FROM sys.tables WHERE is_published = 1 or is_merge_published = 1 or is_schema_published = 1  
UNION  
SELECT name AS published_object, schema_id, 0, 0, is_schema_published  
FROM sys.procedures WHERE is_schema_published = 1  
UNION  
SELECT name AS published_object, schema_id, 0, 0, is_schema_published  
FROM sys.views WHERE is_schema_published = 1;  
  
--Which columns are published in snapshot or transactional publications in this database?  
SELECT object_name(object_id) AS tran_published_table, name AS published_column FROM sys.columns WHERE is_replicated = 1;  
  
--Which columns are published in merge publications in this database?  
SELECT object_name(object_id) AS merge_published_table, name AS published_column FROM sys.columns WHERE is_merge_published = 1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Domande frequenti per gli amministratori di replica](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [sp_get_distributor &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_helpreplicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpreplicationdboption-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)   
 [sys.servers &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-servers-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-views-transact-sql.md)  
  
  
