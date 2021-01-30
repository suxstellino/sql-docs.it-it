---
description: sys.sp_cdc_enable_db (Transact-SQL)
title: sys.sp_cdc_enable_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_enable_db_TSQL
- sp_cdc_enable_db
- sys.sp_cdc_enable_db
- sys.sp_cdc_enable_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_enable_db
- change data capture [SQL Server], enabling databases
- sp_cdc_enable_db
ms.assetid: 176d83b3-493d-43cd-800e-aa123c3bdf17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a0756c34e9fc914abdb99cddef865d0968767ab3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205997"
---
# <a name="syssp_cdc_enable_db-transact-sql"></a>sys.sp_cdc_enable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Abilita Change Data Capture per il database corrente. È necessario eseguire questa procedura per un database prima di abilitare Change Data Capture per le tabelle presenti nel database. Change Data Capture consente di registrare le attività di inserimento, aggiornamento ed eliminazione applicate alle tabelle abilitate, fornendo i dettagli delle modifiche in un formato relazionale facilmente utilizzabile. Le informazioni sulla colonna che rispecchiano la struttura della colonna di una tabella di origine rilevata vengono acquisite per le righe modificate, insieme ai metadati necessari ad applicare le modifiche a un ambiente di destinazione.  
  
> [!IMPORTANT]
>  Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_enable_db  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Change Data Capture non può essere abilitato nei [database di sistema](../../relational-databases/databases/system-databases.md) o nei database di distribuzione.  
  
 sys.sp_cdc_enable_db crea gli oggetti Change Data Capture con ambito esteso all'intero database, includendo tabelle dei metadati e trigger DDL. Vengono inoltre creati lo schema cdc e l'utente del database CDC e la colonna is_cdc_enabled per la voce del database nella vista del catalogo [sys. databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) viene impostata su 1.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene abilitata l'acquisizione dei dati delle modifiche.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_db;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_disable_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)  
  
  
