---
description: sp_addmergealternatepublisher (Transact-SQL)
title: sp_addmergealternatepublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergealternatepublisher_TSQL
- sp_addmergealternatepublisher
helpviewer_keywords:
- sp_addmergealternatepublisher
ms.assetid: de46e0b1-d946-4021-bff6-2d8e3187656d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7c0caf60fb3b1323c24924f177ca4d8cdf1105ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202357"
---
# <a name="sp_addmergealternatepublisher-transact-sql"></a>sp_addmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Consente a un Sottoscrittore di utilizzare un partner di sincronizzazione alternativo. È necessario che le proprietà della pubblicazione consentano ai Sottoscrittori di eseguire la sincronizzazione con altri server di pubblicazione. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergealternatepublisher [ @publisher= ] 'publisher'  
          , [ @publisher_db= ] 'publisher_db'  
          , [ @publication= ] 'publication'  
          , [ @alternate_publisher= ] 'alternate_synchronization_partner'  
          , [ @alternate_publisher_db= ] 'alternate_publisher_db'  
          , [ @alternate_publication= ] 'alternate_synchronization_partner'  
     , [ @alternate_distributor= ] 'alternate_distributor'   
    [ , [ @friendly_name= ] 'friendly_name' ]   
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publisher = ] 'alternate_synchronization_partner'` Nome del server di pubblicazione alternativo. *alternate_synchronization_partner* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publisher_db = ] 'alternate_publisher_db'` Nome del database di pubblicazione nel server di pubblicazione alternativo. *alternate_publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publication = ] 'alternate_synchronization_partner'` Nome della pubblicazione nel partner di sincronizzazione alternativo. *alternate_synchronization_partner* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_distributor = ] 'alternate_distributor'` Nome del server di distribuzione per il partner di sincronizzazione alternativo. *alternate_distributor* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @friendly_name = ] 'friendly_name'` Nome visualizzato in base al quale è possibile identificare l'associazione del server di pubblicazione, della pubblicazione e del server di distribuzione che costituisce un partner di sincronizzazione alternativo. *friendly_name* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergealternatepublisher** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergealternatepublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_dropmergealternatepublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergealternatepublisher-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
