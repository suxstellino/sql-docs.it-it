---
title: sys.sp_rda_reconcile_batch (Transact-SQL) | Microsoft Docs
description: Informazioni su come usare sys.sp_rda_reconcile_batch per riconciliare l'ID batch nella tabella SQL Server abilitata per l'estensione con l'ID batch archiviato nella tabella remota di Azure.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_batch
- sys.sp_rda_reconcile_batch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_batch stored procedure
ms.assetid: 6d21eac3-7b6c-4fe0-8bc4-bf503f3948a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 858da83b7529198da29cc5665219e9b83dfe3a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211790"
---
# <a name="syssp_rda_reconcile_batch-transact-sql"></a>sys.sp_rda_reconcile_batch (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Risolve l'ID batch archiviato nella tabella SQL Server abilitata per l'estensione con l'ID batch archiviato nella tabella remota di Azure.  
  
 In genere è necessario eseguire **sp_rda_reconcile_batch** solo se sono stati eliminati manualmente i dati migrati più di recente dalla tabella remota. Quando si eliminano manualmente i dati remoti che includono il batch più recente, gli ID batch non sono sincronizzati e la migrazione viene arrestata.  
 
 Per eliminare i dati di cui è già stata eseguita la migrazione in Azure, vedere la sezione Osservazioni in questa pagina.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_rda_reconcile_batch @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>Argomenti  
 \@objname ='*\@ ObjName*'  
 Nome della tabella SQL Server abilitata per l'estensione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede autorizzazioni db_owner.  
  
## <a name="remarks"></a>Commenti  
 Se si desidera eliminare i dati di cui è già stata eseguita la migrazione in Azure, eseguire le operazioni seguenti.  
  
1.  Sospendere la migrazione dei dati. Per altre informazioni, vedere [Sospendere e riprendere la migrazione dei dati &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
2.  Eliminare i dati dalla tabella di staging SQL Server eseguendo un comando DELETE con l'hint STAGE_ONLY. Per altre informazioni, vedere [eseguire aggiornamenti amministrativi ed eliminazioni](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md#adminHints).
  
3.  Eliminare gli stessi dati dalla tabella remota di Azure eseguendo un comando DELETE con l'hint REMOTE_ONLY.  
  
4.  Eseguire **sp_rda_reconcile_batch**.  
  
5.  Riprendere la migrazione dei dati. Per altre informazioni, vedere [Sospendere e riprendere la migrazione dei dati &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
## <a name="example"></a>Esempio  
 Per riconciliare gli ID batch, eseguire l'istruzione seguente.  
  
```sql  
EXEC sp_rda_reconcile_batch @objname = N'StretchEnabledTableName';  
```  
  
  
