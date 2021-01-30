---
title: sys.sp_rda_reconcile_columns (Transact-SQL) | Microsoft Docs
description: Informazioni sulle sys.sp_rda_reconcile_columns. Usare questa stored procedure per riconciliare le colonne nelle tabelle di Azure remote e le tabelle SQL Server abilitate per l'estensione.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_columns
- sys.sp_rda_reconcile_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_columns stored procedure
ms.assetid: 60d9cc4e-1828-450b-9d88-5b8485800d73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8eb33cbb1fd2975d96a727f6a7fde457c9827cc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211782"
---
# <a name="syssp_rda_reconcile_columns-transact-sql"></a>sys.sp_rda_reconcile_columns (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Consente di riconciliare le colonne della tabella remota di Azure con le colonne nella tabella SQL Server abilitata per l'estensione.  
    
  **sp_rda_reconcile_columns** aggiunge colonne alla tabella remota presenti nella tabella SQL Server abilitata per l'estensione, ma non nella tabella remota. Queste colonne possono essere colonne eliminate accidentalmente dalla tabella remota. Tuttavia, **sp_rda_reconcile_columns** non elimina le colonne dalla tabella remota presenti nella tabella remota, ma non nella tabella SQL Server.
  
  > [!IMPORTANT]
  > Quando **sp_rda_reconcile_columns** crea nuovamente le colonne accidentalmente eliminate dalla tabella remota, non ripristina i dati che erano presenti nelle colonne eliminate.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_rda_reconcile_columns @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>Argomenti  
 \@objname ='*\@ ObjName*'  
 Nome della tabella SQL Server abilitata per l'estensione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o >0 (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede autorizzazioni db_owner.  
   
## <a name="remarks"></a>Commenti  
 Se sono presenti colonne nella tabella di Azure remota che non esistono più nella tabella SQL Server con estensione abilitata, queste colonne aggiuntive non impediscono a Stretch Database di funzionare normalmente. È possibile rimuovere le colonne aggiuntive manualmente.  
  
## <a name="example"></a>Esempio  
 Per riconciliare le colonne nella tabella remota di Azure, eseguire l'istruzione seguente.  
  
```sql  
EXEC sp_rda_reconcile_columns @objname = N'StretchEnabledTableName';  
```  
  
  
