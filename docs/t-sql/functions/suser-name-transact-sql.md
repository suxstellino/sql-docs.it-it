---
description: SUSER_NAME (Transact-SQL)
title: SUSER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/12/2020
ms.prod: sql
ms.prod_service: synapse-analytics, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_NAME
- SUSER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- security identification names [SQL Server]
- logins [SQL Server], users
- identification names for logins [SQL Server]
- users [SQL Server], logins
- SUSER_NAME function
- logins [SQL Server], names
- names [SQL Server], logins
ms.assetid: ae598d9f-9baa-49b8-b1c1-042854206de4
author: VanMSFT
ms.author: vanto
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f9a204175b54b5349a975ade8926844eced687bd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755431"
---
# <a name="suser_name-transact-sql"></a>SUSER_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Restituisce il nome di identificazione dell'account di accesso dell'utente.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SUSER_NAME ( [ server_user_id ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
_server\_user\_id_  
Numero di identificazione dell'account di accesso dell'utente. _server\_user\_id_ è facoltativo ed è di tipo **int**. _server\_user\_id_ può essere il numero di identificazione di qualsiasi account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure di qualsiasi utente o gruppo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows autorizzato a connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se _server\_user\_id_ viene omesso, viene restituito il nome di identificazione dell'account di accesso dell'utente corrente. Se nel parametro è inclusa la parola NULL, verrà restituito NULL.  
  
## <a name="return-types"></a>Tipi restituiti  
**nvarchar(128)**  
  
## <a name="remarks"></a>Commenti  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7.0 l'ID dell'utente del server (SUID) è stato sostituito con l'ID di sicurezza (SID).  
  
SUSER_NAME restituisce un nome di account di accesso solo per gli account a cui corrisponde una voce nella tabella di sistema **syslogins**.  
  
È possibile usare SUSER_NAME nell'elenco di selezione, nella clausola WHERE e in tutti i casi in cui è consentita un'espressione. Usare le parentesi dopo SUSER_NAME, anche se non viene specificato alcun parametro.  

> [!NOTE]
> Anche se la funzione SUSER_NAME è supportata nel database SQL di Azure, l'uso di *Execute As* con SUSER_NAME non è supportato nel database SQL di Azure. 
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene restituito il nome di identificazione dell'account di accesso dell'utente il cui numero di identificazione dell'account di accesso è `1`.  
  
```sql
SELECT SUSER_NAME(1);  
```  
  
## <a name="see-also"></a>Vedere anche  
[SUSER_ID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-id-transact-sql.md)   
[Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
