---
description: '&#x40;&#x40;CONNECTIONS (Transact-SQL)'
title: '@@CONNECTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CONNECTIONS'
- '@@CONNECTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CONNECTIONS function'
- connections [SQL Server], number of
- connections [SQL Server], attempted
- number of connection attempts
- attempted connections
ms.assetid: c59836a8-443c-4b9a-8b96-8863ada97ac7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 32275f46ea789f5507fff4a2594d7eb8d84604f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101264"
---
# <a name="x40x40connections-transact-sql"></a>&#x40;&#x40;CONNECTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione restituisce il numero di tentativi di connessione, sia con esito positivo che negativo, dopo l'ultimo avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
@@CONNECTIONS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
**integer**
  
## <a name="remarks"></a>Commenti  
Le connessioni sono distinte dagli utenti. Un'applicazione, ad esempio, può aprire più connessioni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza che siano visibili all'utente.
  
Eseguire **sp_monitor** per un report contenente dati statistici relativi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compreso il conteggio dei tentativi di connessione.
  
@@MAX_CONNECTIONS rappresenta il numero massimo consentito di connessioni simultanee al server. @@CONNECTIONS viene incrementato a ogni tentativo di connessione, quindi @@CONNECTIONS può superare @@MAX_CONNECTIONS.
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce il conteggio dei tentativi di accesso a partire dalla data e ora correnti.
  
```sql
SELECT GETDATE() AS 'Today''s Date and Time',   
@@CONNECTIONS AS 'Login Attempts';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
``` 
Today's Date and Time  Login Attempts  
---------------------- --------------  
12/5/2006 10:32:45 AM  211023         
```  
  
## <a name="see-also"></a>Vedi anche
[Funzioni statistiche di sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
[sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)
  
  
