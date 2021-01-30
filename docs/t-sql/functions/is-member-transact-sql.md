---
description: IS_MEMBER (Transact-SQL)
title: IS_MEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_MEMBER
- IS_MEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], members
- current member status
- roles [SQL Server], members
- testing member status
- members [SQL Server]
- checking member status
- IS_MEMBER function
- verifying member status
- groups [SQL Server], members
- members [SQL Server], verifying
ms.assetid: 77cb68a0-19b7-4fe1-ab17-e5587699631b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e938e2cfbe93c137ce24d77d7f942b1764f521be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179671"
---
# <a name="is_member-transact-sql"></a>IS_MEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indica se l'utente corrente è membro del gruppo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows o del ruolo di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificato. La funzione IS_MEMBER non è supportata per i gruppi di Azure Active Directory.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IS_MEMBER ( { 'group' | 'role' } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *group* **'**  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Nome del gruppo di Windows sottoposto al controllo; deve essere nel formato *Dominio*\\*Gruppo*. *group* è di tipo **sysname**.  
  
 **'** *role* **'**  
 Nome del ruolo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottoposto al controllo. *role* è di tipo **sysname** e può includere i ruoli predefiniti del database o quelli definiti dall'utente, ma non i ruoli del server.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 IS_MEMBER restituisce i valori seguenti.  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0|L'utente corrente non è membro di *group* o *role*.|  
|1|L'utente corrente è membro di *group* o *role*.|  
|NULL|*group* o *role* non valido. Quando la query viene eseguita da un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o da un account di accesso che utilizza un ruolo applicazione esegue una query, restituisce NULL per un gruppo di Windows.|  
  
 IS_MEMBER determina l'appartenenza al gruppo di Windows tramite l'analisi di un token di accesso creato da Windows. Il token di accesso non riflette le modifiche a livello di appartenenza al gruppo apportate dopo la connessione da parte di un utente a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un ruolo applicazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non possono eseguire una query sull'appartenenza del gruppo di Windows.  
  
 Per aggiungere e rimuovere membri da un ruolo del database, usare [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Per aggiungere e rimuovere membri da un ruolo del server, usare [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Questa funzione valuta l'appartenenza al ruolo, non l'autorizzazione sottostante. Il ruolo predefinito del database **db_owner** dispone ad esempio dell'autorizzazione **CONTROL DATABASE**. Se l'utente ha l'autorizzazione **CONTROL DATABASE** ma non è membro del ruolo, questa funzione indicherà correttamente che l'utente non è membro del ruolo **db_owner**, anche se ha le stesse autorizzazioni.  
  
 I membri del ruolo predefinito del server **sysadmin** accedono a ogni database come utenti **dbo**. Durante la verifica dell'autorizzazione di membro del ruolo predefinito del server **sysadmin** vengono controllate le autorizzazioni per **dbo**, non l'account di accesso originale. Poiché **dbo** non può essere aggiunto a un ruolo del database e non esiste nei gruppi di Windows, **dbo** restituirà sempre 0 (o NULL se il ruolo non esiste).  
  
## <a name="related-functions"></a>Funzioni correlate  
 Per determinare se un altro account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è membro di un ruolo del database, usare [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md). Per determinare se un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è membro di un ruolo del server, usare [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene verificato se l'utente corrente è membro di un ruolo di database oppure di un gruppo di domini di Windows.  
  
```sql  
-- Test membership in db_owner and print appropriate message.  
IF IS_MEMBER ('db_owner') = 1  
   PRINT 'Current user is a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') = 0  
   PRINT 'Current user is NOT a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') IS NULL  
   PRINT 'ERROR: Invalid group / role specified';  
GO  
  
-- Execute SELECT if user is a member of ADVWORKS\Shipping.  
IF IS_MEMBER ('ADVWORKS\Shipping') = 1  
   SELECT 'User ' + USER + ' is a member of ADVWORKS\Shipping.';   
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
