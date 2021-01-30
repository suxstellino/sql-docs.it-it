---
description: sp_updatestats (Transact-SQL)
title: sp_updatestats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_updatestats_TSQL
- sp_updatestats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updatestats
ms.assetid: 01184651-6e61-45d9-a502-366fecca0ee4
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0cfaffcf1df13da39a093043d84e28f4433d3a71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209270"
---
# <a name="sp_updatestats-transact-sql"></a>sp_updatestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Viene eseguito `UPDATE STATISTICS` su tutte le tabelle interne e definite dall'utente nel database corrente.  
  
Per ulteriori informazioni su `UPDATE STATISTICS` , vedere [update statistics &#40;&#41;Transact-SQL ](../../t-sql/statements/update-statistics-transact-sql.md). Per altre informazioni sulle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
    
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_updatestats [ [ @resample = ] 'resample']  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="arguments"></a>Argomenti  
`[ @resample = ] 'resample'` Specifica che **sp_updatestats** utilizzerà l'opzione RESAMPLE dell'istruzione [Update Statistics](../../t-sql/statements/update-statistics-transact-sql.md) . Se **' resample '** non è specificato, **sp_updatestats** aggiorna le statistiche usando il campionamento predefinito. il **ricampionamento** è **varchar (8)** e il valore predefinito è no.  
  
## <a name="remarks"></a>Commenti  
 **sp_updatestats** viene eseguito `UPDATE STATISTICS` , specificando la `ALL` parola chiave, in tutte le tabelle interne e definite dall'utente nel database. sp_updatestats Visualizza i messaggi che indicano lo stato di avanzamento. Al termine dell'aggiornamento, viene segnalato che sono state aggiornate le statistiche di tutte le tabelle.  
  
**sp_updatestats** aggiorna le statistiche per gli indici non cluster disabilitati e non aggiorna le statistiche sugli indici cluster disabilitati.  
  
Per le tabelle basate su disco, **sp_updatestats** aggiorna le statistiche in base alle informazioni **modification_counter** nella vista del catalogo **sys.dm_db_stats_properties** , aggiornando le statistiche in cui è stata modificata almeno una riga. Le statistiche sulle tabelle ottimizzate per la memoria vengono sempre aggiornate quando si eseguono **sp_updatestats**. Non eseguire pertanto **sp_updatestats** più del necessario.  
  
**sp_updatestats** possibile attivare una ricompilazione di stored procedure o altro codice compilato. Tuttavia, **sp_updatestats** potrebbe non provocare una ricompilazione, se è possibile solo un piano di query per le tabelle a cui si fa riferimento e gli indici. Una ricompilazione non sarebbe necessaria in tali casi anche se le statistiche vengono aggiornate.  
  
Per i database con un livello di compatibilità inferiore a 90, l'esecuzione di **sp_updatestats** non mantiene l'impostazione più recente di NORECOMPUTE per statistiche specifiche. Per i database con un livello di compatibilità pari o superiore a 90, sp_updatestats mantiene l'opzione NORECOMPUTE più recente per statistiche specifiche. Per altre informazioni sulla disabilitazione e sulla riabilitazione degli aggiornamenti delle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
## <a name="permissions"></a>Autorizzazioni  

Per eseguire **sp_updatestats**, l'utente deve essere il proprietario del database ( `dbo` , non solo membro del ruolo `db_owner` ) o essere membro del ruolo predefinito del server sysadmin.

## <a name="examples"></a>Esempio  
Nell'esempio seguente vengono aggiornate le statistiche per le tabelle del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_updatestats;   
```  

## <a name="automatic-index-and-statistics-management"></a>Gestione automatica dell'indice e delle statistiche
Sfruttare le soluzioni, ad esempio la [deframmentazione dell'indice adattativo](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), per gestire automaticamente la deframmentazione dell'indice e gli aggiornamenti delle statistiche per uno o più database. Questa procedura sceglie automaticamente se ricompilare o riorganizzare un indice in base al relativo livello di frammentazione, tra gli altri parametri, e aggiornare le statistiche con una soglia lineare.

## <a name="see-also"></a>Vedere anche  
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Stored procedure di sistema](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
 
