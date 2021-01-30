---
description: sys.sp_cdc_disable_db (Transact-SQL)
title: sys.sp_cdc_disable_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db_TSQL
- sp_cdc_disable_db_TSQL
- sys.sp_cdc_disable_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db
- change data capture [SQL Server], disabling databases
ms.assetid: 420fb99e-e60f-445b-b568-da96471f1e8f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5964b470b527874b16e9341522bd96e9a4110455
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182033"
---
# <a name="syssp_cdc_disable_db-transact-sql"></a>sys.sp_cdc_disable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Disabilita Change Data Capture per il database corrente. Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
sys.sp_cdc_disable_db  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sys.sp_cdc_disable_db** Disabilita Change Data Capture per tutte le tabelle nel database attualmente abilitato. Vengono eliminati tutti gli oggetti di sistema correlati all'acquisizione dei dati delle modifiche, ad esempio tabelle delle modifiche, processi, stored procedure e funzioni. La colonna **is_cdc_enabled** per la voce del database nella vista del catalogo [sys. databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) è impostata su 0.  
  
> [!NOTE]  
>  Se sono presenti molte istanze di acquisizione definite per il database quando Change Data Capture è disabilitato, una transazione con esecuzione prolungata può generare un errore nell'esecuzione di sys.sp_cdc_disable_db. È possibile evitare questo problema disabilitando le singole istanze di acquisizione mediante sys.sp_cdc_disable_table prima dell'esecuzione di sys.sp_cdc_disable_db.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene disabilitata l'acquisizione dei dati delle modifiche per il database `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_db;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_enable_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)   
 [sys.sp_cdc_disable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)  
  
