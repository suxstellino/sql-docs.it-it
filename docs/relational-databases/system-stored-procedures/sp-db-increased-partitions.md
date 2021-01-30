---
description: sp_db_increased_partitions
title: sp_db_increased_partitions | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_increased_partitions_TSQL
- sp_db_increased_partitions
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_increased_partitions
ms.assetid: a8c043ec-b504-4929-ac0e-8babaa99d989
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2241ce80c8537fa16feaedbf5c0f1c316d13821b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201337"
---
# <a name="sp_db_increased_partitions"></a>sp_db_increased_partitions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di abilitare e disabilitare il supporto per partizioni fino a 15.000 per il database specificato.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dp_increased_partitions   
[ @dbname ] = 'database_name'   
[ , [ @increased_partitions = ] 'increased_partitions' ] [;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @dbname =]'*database_name*'  
 Nome del database. *dbname* è di **tipo sysname** e il valore predefinito è null. Se *dbname* non è specificato, viene utilizzato il database corrente.  
  
 [ @increased_partitions =]'*increased_partitions*'  
 Consente di abilitare e disabilitare il supporto per partizioni fino a 15.000 sul database specificato. *increased_partitions* è di tipo **varchar (6)** e il valore predefinito è null. I valori accettati per abilitare il supporto sono 'ON' o 'TRUE' e 'OFF' o 'FALSE' per disabilitarlo. Se *increased_partitions* non viene specificato, la procedura restituisce 1 per indicare che il supporto è abilitato per il database specificato oppure 0 per indicare che il supporto è disabilitato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER DATABASE per il database specificato.  
  
  
