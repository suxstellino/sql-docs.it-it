---
description: sp_copysnapshot (Transact-SQL)
title: sp_copysnapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysnapshot
- sp_copysnapshot_TSQL
helpviewer_keywords:
- sp_copysnapshot
ms.assetid: a012a32f-6f26-45bf-8046-b51cd7fec455
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 76e8f1bead2aaf10289f724b300a79eacd3ca11f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185769"
---
# <a name="sp_copysnapshot-transact-sql"></a>sp_copysnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Copia la cartella snapshot della pubblicazione specificata nella cartella elencata nel **\@ destination_folder**. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione. Risulta utile per la copia di uno snapshot su supporti rimovibili, quali un CD-ROM.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_copysnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione di cui si desidera copiare il contenuto dello snapshot. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @destination_folder = ] 'destination_folder'` Nome della cartella in cui verranno copiati i contenuti dello snapshot di pubblicazione. *destination_folder* è di **tipo nvarchar (255)** e non prevede alcun valore predefinito. Il *destination_folder* può essere un percorso alternativo, ad esempio in un altro server, in un'unità di rete o su un supporto rimovibile, ad esempio CD-ROMs o dischi rimovibili.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di tipo sysname e il valore predefinito è null.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di tipo sysname e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_copysnapshot** viene utilizzato in tutti i tipi di replica. I Sottoscrittori che eseguono [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la versione 7,0 e versioni precedenti non possono utilizzare la posizione alternativa dello snapshot.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_copysnapshot**.  
  
## <a name="see-also"></a>Vedere anche  
 [Percorsi alternativi cartella snapshot](../../relational-databases/replication/snapshot-options.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
