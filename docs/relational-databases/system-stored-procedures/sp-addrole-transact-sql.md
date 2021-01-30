---
description: sp_addrole (Transact-SQL)
title: sp_addrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addrole
- sp_addrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrole
ms.assetid: e8a21642-8440-419a-8585-93d3d9d44f00
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c6217f78d47e193c8583caca0925f2d4b587c439
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200164"
---
# <a name="sp_addrole-transact-sql"></a>sp_addrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un nuovo ruolo di database nel database corrente.  
  
> [!IMPORTANT]
>  **sp_addrole** è incluso per la compatibilità con le versioni precedenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e potrebbe non essere supportato in una versione futura. In alternativa, usare [create Role](../../t-sql/statements/create-role-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addrole [ @rolename = ] 'role' [ , [ @ownername = ] 'owner' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] 'role'` Nome del nuovo ruolo del database. *Role* è di **tipo sysname** e non prevede alcun valore predefinito. *Role* deve essere un identificatore (ID) valido e non deve essere già presente nel database corrente.  
  
`[ @ownername = ] 'owner'` Proprietario del nuovo ruolo del database. *owner* è di **tipo sysname** e il valore predefinito è l'utente attualmente in esecuzione. il *proprietario* deve essere un utente del database o un ruolo del database nel database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 I nomi dei ruoli di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono includere da 1 a 128 caratteri. Sono consentiti simboli, numeri e lettere. I nomi dei ruoli del database non possono: contenere un carattere barra rovesciata ( \\ ), essere null o una stringa vuota (**''**).  
  
 Dopo l'aggiunta di un ruolo del database, utilizzare [sp_addrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) per aggiungere entità al ruolo. Quando si utilizza l'istruzione GRANT, DENY o REVOKE per applicare autorizzazioni al ruolo, i membri corrispondenti ereditano tali autorizzazioni come se fossero state assegnate direttamente ai relativi account.  
  
> [!NOTE]  
>  Non è possibile creare nuovi ruoli di server. I ruoli possono essere creati solo a livello di database.  
  
 Impossibile utilizzare **sp_addrole** all'interno di una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE ROLE per il database. Per la creazione di uno schema, è richiesta l'autorizzazione CREATE SCHEMA per il database. Se il *proprietario* viene specificato come utente o gruppo, richiede la rappresentazione per tale utente o gruppo. Se il *proprietario* viene specificato come ruolo, è richiesta l'autorizzazione ALTER per tale ruolo o per un membro di tale ruolo. Se owner è specificato come ruolo di applicazione, è richiesta l'autorizzazione ALTER per quel ruolo di applicazione.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiunto un nuovo ruolo `Managers` al database corrente.  
  
```  
EXEC sp_addrole 'Managers';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)  
  
  
