---
description: PWDCOMPARE (Transact-SQL)
title: PWDCOMPARE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PWDCOMPARE
- PWDCOMPARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sa account
- passwords [SQL Server], blank
- PWDCOMPARE function [Transact-SQL]
ms.assetid: 5f84ff9e-c1ec-46aa-8501-50f854ebcc3a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 08f67bbdbbcf397291b51fad47357d1dfb060cef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194747"
---
# <a name="pwdcompare-transact-sql"></a>PWDCOMPARE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Esegue l'hashing di una password e confronta l'hash con l'hash di una password esistente. È possibile utilizzare PWDCOMPARE per eseguire la ricerca di password di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuote o di password comuni vulnerabili.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
PWDCOMPARE ( 'clear_text_password'  
   , password_hash   
   [ , version ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *clear_text_password* **'**  
 Password non crittografata. *clear_text_password* è di tipo **sysname** (**nvarchar(128)**).  
  
 *password_hash*  
 Hash di crittografia di una password. *password_hash* è di tipo **varbinary(128)**.  
  
 *version*  
 Parametro obsoleto che può essere impostato su 1 se *password_hash* rappresenta un valore relativo a un account di accesso di una versione precedente a [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] di cui è stata eseguita la migrazione a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versione successiva, ma che non è mai stato convertito nel sistema [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. *version* è di tipo **int**.  
  
> [!CAUTION]  
>  Questo parametro viene fornito per la compatibilità con le versioni precedenti, ma viene ignorato poiché ora i BLOB dell'hash della password contengono le descrizioni delle versioni. [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)]  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
 Restituisce 1 se l'hash di *clear_text_password* corrisponde al parametro *password_hash*. In caso contrario, restituisce 0.  
  
## <a name="remarks"></a>Commenti  
 La funzione PWDCOMPARE non costituisce un rischio per la sicurezza degli hash delle password, in quanto lo stesso test può essere eseguito tentando di accedere con la password fornita come primo parametro.  
  
 **PWDCOMPARE** non può essere usata con le password di utenti del database indipendente. Non esiste un equivalente per i database indipendenti.  
  
## <a name="permissions"></a>Autorizzazioni  
 PWDENCRYPT è disponibile per il ruolo public.  
  
 Per esaminare la colonna password_hash di sys.sql_logins, è richiesta l'autorizzazione CONTROL SERVER.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-identifying-logins-that-have-no-passwords"></a>R. Identificazione degli account di accesso che non dispongono di password  
 Nell'esempio seguente vengono identificati gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non dispongono di password.  
  
```sql  
SELECT name FROM sys.sql_logins   
WHERE PWDCOMPARE('', password_hash) = 1 ;  
```  
  
### <a name="b-searching-for-common-passwords"></a>B. Ricerca di password comuni  
 Per eseguire la ricerca di password comuni che si desidera identificare e cambiare, specificare la password come primo parametro. Eseguire ad esempio l'istruzione seguente per eseguire la ricerca di una password specificata come `password`.  
  
```sql  
SELECT name FROM sys.sql_logins   
WHERE PWDCOMPARE('password', password_hash) = 1 ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [PWDENCRYPT &#40;Transact-SQL&#41;](../../t-sql/functions/pwdencrypt-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
