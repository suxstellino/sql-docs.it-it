---
title: sys.sp_rda_set_rpo_duration (Transact-SQL) | Microsoft Docs
description: Informazioni sulle sys.sp_rda_set_rpo_duration. Utilizzare questa stored procedure per impostare il numero di ore di dati migrati che SQL Server conserva in una tabella di staging.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_set_rpo_duration
- sys.sp_rda_set_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_rpo_duration stored procedure
ms.assetid: 95c80c5b-9252-4612-9ea7-544c48834fd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cc7bbf5ddcde440bb359616201602a9c463d87e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186622"
---
# <a name="syssp_rda_set_rpo_duration-transact-sql"></a>sys.sp_rda_set_rpo_duration (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Imposta il numero di ore di dati migrati che SQL Server conserva in una tabella di staging per garantire un ripristino completo del database di Azure remoto, se è necessario un ripristino temporizzato.    
    
 Per altre informazioni, vedere [Stretch database riduce il rischio di perdita di dati per i dati di Azure mantenendo temporaneamente le righe migrate](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO).  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
     
## <a name="syntax"></a>Sintassi    
    
```    
    
sp_rda_set_rpo_duration [ @duration_hrs = ] duration_hrs    
    
```    
    
## <a name="arguments"></a>Argomenti    
 [ @duration_hrs =] *duration_hrs*    
 Numero di ore (valore intero non null) dei dati migrati che si desidera mantenere SQL Server per il database abilitato per l'estensione corrente. Il valore predefinito e il valore minimo è 8 ore.    
 
 > [!NOTE]
 > I valori più elevati richiedono più spazio di archiviazione in SQL Server.
    
## <a name="permissions"></a>Autorizzazioni    
 Richiede autorizzazioni db_owner.    
    
## <a name="remarks"></a>Commenti    
 Per ottenere il valore corrente, eseguire [sys.sp_rda_get_rpo_duration &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md).    
    
## <a name="see-also"></a>Vedere anche    
 [sys.sp_rda_get_rpo_duration &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md)     
 [Ripristinare i database abilitati per Stretch (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)     
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)    
    
  
