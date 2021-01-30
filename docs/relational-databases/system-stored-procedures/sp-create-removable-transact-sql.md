---
description: sp_create_removable (Transact-SQL)
title: sp_create_removable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_removable
- sp_create_removable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_removable
ms.assetid: 06e36ae5-f70d-4a26-9a7f-ee4b9360b355
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ffe2273f7bd2ca63143f77a4d0c5726709d8440
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205200"
---
# <a name="sp_create_removable-transact-sql"></a>sp_create_removable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene creato un database su supporti rimovibili. Vengono creati tre o più file, uno per le tabelle del catalogo di sistema, uno per il log delle transazioni e uno o più per le tabelle di dati, quindi viene inserito il database in questi file.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] È invece consigliabile utilizzare [create database](../../t-sql/statements/create-database-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_create_removable   
   [ @dbname = ] 'dbname',   
   [ @syslogical= ] 'syslogical',   
   [ @sysphysical = ] 'sysphysical',   
   [ @syssize = ] syssize,   
   [ @loglogical = ] 'loglogical',   
   [ @logphysical = ] 'logphysical',   
   [ @logsize = ] logsize,   
   [ @datalogical1 = ] 'datalogical1',   
   [ @dataphysical1 = ] 'dataphysical1',   
   [ @datasize1 = ] datasize1 ,   
   [ @datalogical16 = ] 'datalogical16',   
   [ @dataphysical16 = ] 'dataphysical16',   
   [ @datasize16 = ] datasize16 ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Nome del database da creare per l'utilizzo su supporti rimovibili. *dbname* è di **tipo sysname**.  
  
`[ @syslogical = ] 'syslogical'` Nome logico del file contenente le tabelle del catalogo di sistema. *syslogical* è di **tipo sysname**.  
  
`[ @sysphysical = ] 'sysphysical'` È il nome fisico. In esso è incluso un percorso completo del file contenente le tabelle del catalogo di sistema. *sysphysical* è di **tipo nvarchar (260)**.  
  
`[ @syssize = ] syssize` Dimensioni in megabyte del file contenente le tabelle del catalogo di sistema. *syssize* è di **tipo int**. Il valore minimo di *syssize* è 1.  
  
`[ @loglogical = ] 'loglogical'` Nome logico del file contenente il log delle transazioni. *loglogical* è di **tipo sysname**.  
  
`[ @logphysical = ] 'logphysical'` È il nome fisico. In esso è incluso un percorso completo del file contenente il log delle transazioni. *logphysical* è di **tipo nvarchar (260)**.  
  
`[ @logsize = ] logsize` Dimensioni in megabyte del file contenente il log delle transazioni. *logsize* è di **tipo int**. Il valore minimo di *logsize* è 1.  
  
`[ @datalogical1 = ] 'datalogical'` Nome logico di un file contenente le tabelle di dati. *datalogicalal* è di **tipo sysname**.  
  
 Il numero dei file di dati è compreso tra 1 e 16. Vengono in genere creati più file di dati quando si prevede che il database sia di grandi dimensioni e debba essere pertanto suddiviso su più dischi.  
  
`[ @dataphysical1 = ] 'dataphysical'` È il nome fisico. In esso è incluso un percorso completo di un file contenente le tabelle di dati. *dataphysical* è di **tipo nvarchar (260)**.  
  
`[ @datasize1 = ] 'datasize'` Dimensioni in megabyte di un file contenente tabelle di dati. *DataSize* è di **tipo int**. Il valore di *DataSize* minimo è 1.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare questa stored procedure se si desidera creare una copia del database su un supporto rimovibile, ad esempio un CD, e distribuire il database ad altri utenti.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE DATABASE, CREATE ANY DATABASE o ALTER ANY DATABASE.  
  
 Per mantenere il controllo sull'utilizzo del disco per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'autorizzazione per la creazione dei database è in genere limitata a pochi account di accesso.  
  
### <a name="permissions-on-data-and-log-files"></a>Autorizzazioni per i file di dati e di log  
 Ogni volta che si eseguono determinate operazioni in un database, vengono impostate le autorizzazioni corrispondenti per i relativi file di dati e di log. Con le autorizzazioni è possibile evitare che vengano accidentalmente alterati i file che si trovano in una directory con autorizzazioni aperte.  
  
|Operazione nel database|Autorizzazioni impostate per i file|  
|---------------------------|------------------------------|  
|Modifica per l'aggiunta di un nuovo file|Data di creazione|  
|Esecuzione del backup|Collegamento|  
|Ripristino|Scollegamento|  
  
> [!NOTE]  
>  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono impostate autorizzazioni per i file di dati e di log.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il database `inventory` viene creato come database rimovibile.  
  
```  
EXEC sp_create_removable 'inventory',   
   'invsys',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invsys.mdf'  
, 2,   
   'invlog',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invlog.ldf', 4,  
   'invdata',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invdata.ndf',   
10;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_certify_removable &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-certify-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [sp_detach_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
