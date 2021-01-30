---
description: sp_createstats (Transact-SQL)
title: sp_createstats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a0fb35bd43125370dc4123425cc919be3033951f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205211"
---
# <a name="sp_createstats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Chiama l'istruzione [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) per creare statistiche a colonna singola su colonne che non sono già la prima colonna in un oggetto statistiche. La creazione di statistiche di colonna singola aumenta il numero di istogrammi, con un conseguente miglioramento delle stime della cardinalità, dei piani di query e delle prestazioni di esecuzione delle query. La prima colonna di un oggetto statistiche include un istogramma, mentre le altre colonne non dispongono istogrammi.  
  
 sp_createstats è utile per applicazioni quali quelle di benchmarking quando l'ora di esecuzione delle query è critica e non è possibile attendere la generazione delle statistiche di colonna singola da parte di Query Optimizer. Nella maggior parte dei casi non è necessario usare sp_createstats; il Query Optimizer genera le statistiche di colonna singola necessarie per migliorare i piani di query quando l'opzione **AUTO_CREATE_STATISTICS** è impostata su on.  
  
 Per altre informazioni sulle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md). Per ulteriori informazioni sulla generazione di statistiche a colonna singola, vedere l'opzione **AUTO_CREATE_STATISTICS** in [Opzioni ALTER DATABASE set &#40;&#41;Transact-SQL](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @indexonly = ] 'indexonly'` Crea statistiche solo sulle colonne presenti in un indice esistente e non è la prima colonna in una definizione di indice. **si specifica indexonly** è **char (9)**. e il valore predefinito è NO.  
  
`[ @fullscan = ] 'fullscan'` Usa l'istruzione [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con l'opzione **FULLSCAN** . **FULLSCAN** è **char (9)**.  e il valore predefinito è NO.  
  
`[ @norecompute = ] 'norecompute'` Usa l'istruzione [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con l'opzione **NORECOMPUTE** . **NORECOMPUTE** è di **carattere (12)**.  e il valore predefinito è NO.  
  
`[ @incremental = ] 'incremental'` Usa l'istruzione [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con l'opzione **Incremental = on** . **Incremental** è **char (12)**.  e il valore predefinito è NO.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Il nome dei nuovi oggetti statistiche corrisponde a quello delle colonne su cui sono stati creati.  
  
## <a name="remarks"></a>Commenti  
 sp_createstats non crea o Aggiorna statistiche sulle colonne che sono la prima colonna di un oggetto statistiche esistente.  Include la prima colonna di statistiche create per gli indici, le colonne con statistiche a colonna singola generate con AUTO_CREATE_STATISTICS opzione e la prima colonna di statistiche create con l'istruzione CREATE STATISTICs. sp_createstats non crea statistiche sulle prime colonne degli indici disabilitati, a meno che tale colonna non venga utilizzata in un altro indice attivato. sp_createstats non crea statistiche sulle tabelle con un indice cluster disabilitato.  
  
 Quando la tabella contiene un set di colonne, sp_createstats non crea statistiche sulle colonne di tipo sparse. Per ulteriori informazioni sui set di colonne e sulle colonne di tipo sparse, vedere [utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md) e [utilizzare colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>R. Creazione di statistiche di colonna singola su tutte le colonne idonee  
 Nell'esempio seguente vengono create statistiche di colonna singola su tutte le colonne idonee del database corrente.  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. Creazione di statistiche di colonna singola su tutte le colonne di indice idonee  
 Nell'esempio seguente vengono create statistiche di colonna singola su tutte le colonne idonee già presenti in un indice e che non sono prime colonne dell'indice.  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Statistiche](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
