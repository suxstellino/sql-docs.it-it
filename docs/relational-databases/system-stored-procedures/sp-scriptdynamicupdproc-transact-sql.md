---
description: sp_scriptdynamicupdproc (Transact-SQL)
title: sp_scriptdynamicupdproc (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptdynamicupdproc_TSQL
- sp_scriptdynamicupdproc
helpviewer_keywords:
- sp_scriptdynamicupdproc
ms.assetid: b4c18863-ed92-4aa2-a04f-7ed832fc9e07
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab8af33b865570067301df85355f13e0112979ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189989"
---
# <a name="sp_scriptdynamicupdproc-transact-sql"></a>sp_scriptdynamicupdproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Genera l'istruzione CREATE PROCEDURE, che crea una stored procedure ad aggiornamento dinamico. L'istruzione UPDATE all'interno della stored procedure personalizzata viene compilata in modo dinamico in base alla sintassi MCALL che indica le colonne da modificare. Utilizzare questa stored procedure se il numero di indici nella tabella di sottoscrizione è in aumento e il numero di colonne in fase di modifica è ridotto. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_scriptdynamicupdproc [ @artid =] artid  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @artid = ] artid` ID dell'articolo. *artid* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce un set di risultati costituito da una singola colonna **nvarchar (4000)** . Il set di risultati forma l'istruzione CREATE PROCEDURE completa utilizzata per creare la stored procedure personalizzata.  
  
## <a name="remarks"></a>Commenti  
 **sp_scriptdynamicupdproc** viene utilizzata nella replica transazionale. La logica di scripting MCALL predefinita prevede l'inserimento di tutte le colonne all'interno dell'istruzione UPDATE e l'utilizzo di una mappa di bit per determinare le colonne modificate. Se una colonna non è stata modificata, viene eseguita un'operazione di reimpostazione del valore originale della colonna. Questa operazione in genere non causa problemi. Se la colonna è indicizzata, sono necessari altri processi di elaborazione. L'approccio dinamico prevede l'inserimento delle sole colonne modificate e consente pertanto di creare una stringa UPDATE ottimale. Per la compilazione dell'istruzione UPDATE dinamica in fase di esecuzione sono tuttavia necessari processi di elaborazione aggiuntivi. È consigliabile verificare gli approcci dinamico e statico e quindi scegliere la soluzione ottimale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_scriptdynamicupdproc**.  
  
## <a name="examples"></a>Esempio  
 In questo esempio viene creato un articolo (con *artid* impostato su **1**) nella tabella **authors** nel database **pubs** e viene specificato che l'istruzione Update è la procedura personalizzata da eseguire:  
  
```  
'MCALL sp_mupd_authors'  
```  
  
 Generare le stored procedure personalizzate che dovranno essere eseguite dall'agente di distribuzione nel Sottoscrittore mediante l'esecuzione della stored procedure seguente nel server di pubblicazione:  
  
```  
EXEC sp_scriptdynamicupdproc @artid = '1'  
  
The statement returns:  
  
CREATE PROCEDURE [sp_mupd_authors]   
  @c1 varchar(11),@c2 varchar(40),@c3 varchar(20),@c4 char(12),@c5 varchar(40),@c6 varchar(20),  
  @c7 char(2),@c8 char(5),@c9 bit,@pkc1 varchar(11),@bitmap binary(2)  
as  
  
declare @stmt nvarchar(4000), @spacer nvarchar(1)  
SELECT @spacer =N''  
SELECT @stmt = N'UPDATE [authors] SET '  
  
if substring(@bitmap,1,1) & 2 = 2  
begin  
  select @stmt = @stmt + @spacer + N'[au_lname]' + N'=@2'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 4 = 4  
begin  
  select @stmt = @stmt + @spacer + N'[au_fname]' + N'=@3'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 8 = 8  
begin  
  select @stmt = @stmt + @spacer + N'[phone]' + N'=@4'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 16 = 16  
begin  
  select @stmt = @stmt + @spacer + N'[address]' + N'=@5'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 32 = 32  
begin  
  select @stmt = @stmt + @spacer + N'[city]' + N'=@6'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 64 = 64  
begin  
  select @stmt = @stmt + @spacer + N'[state]' + N'=@7'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 128 = 128  
begin  
  select @stmt = @stmt + @spacer + N'[zip]' + N'=@8'  
  select @spacer = N','  
end  
if substring(@bitmap,2,1) & 1 = 1  
begin  
  select @stmt = @stmt + @spacer + N'[contract]' + N'=@9'  
  select @spacer = N','  
end  
select @stmt = @stmt + N' where [au_id] = @1'  
exec sp_executesql @stmt, N' @1 varchar(11),@2 varchar(40),@3 varchar(20),@4 char(12),@5 varchar(40),  
                             @6 varchar(20),@7 char(2),@8 char(5),@9 bit',@pkc1,@c2,@c3,@c4,@c5,@c6,@c7,@c8,@c9  
  
if @@rowcount = 0  
   if @@microsoftversion>0x07320000  
      exec sp_MSreplraiserror 20598  
```  
  
 Dopo avere eseguito questa stored procedure, è possibile utilizzare lo script risultante per creare manualmente la stored procedure nei Sottoscrittori.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
