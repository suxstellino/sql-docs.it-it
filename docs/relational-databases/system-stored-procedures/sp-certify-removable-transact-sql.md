---
description: sp_certify_removable (Transact-SQL)
title: sp_certify_removable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_certify_removable_TSQL
- sp_certify_removable
dev_langs:
- TSQL
helpviewer_keywords:
- sp_certify_removable
ms.assetid: ca12767f-0ae5-4652-b523-c23473f100a1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69a0c6e1331144fc340ad15bb103e593d3ba8f65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197674"
---
# <a name="sp_certify_removable-transact-sql"></a>sp_certify_removable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verifica che un database sia configurato correttamente per la distribuzione su supporti rimovibili e segnala eventuali problemi.  
  
> **IMPORTANTE** [! In alternativa, includere[ssNoteDepFutureAvoid](../../t-sql/statements/create-database-transact-sql.md) .  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_certify_removable [ @dbname= ] 'dbname'  
     [ , [ @autofix = ] 'auto' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Specifica il database da verificare. *dbname* è di **tipo sysname**.  
  
`[ @autofix = ] 'auto'` Assegna la proprietà del database e di tutti gli oggetti di database all'amministratore di sistema ed elimina eventuali utenti del database creati dall'utente e le autorizzazioni non predefinite. *auto* è di **tipo nvarchar (4)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Se il database è configurato correttamente, **sp_certify_removable** esegue le operazioni seguenti:  
  
-   Imposta il database offline per consentire la copia dei file.  
  
-   Aggiorna i dati statistici di tutte le tabelle e segnala gli eventuali problemi relativi alla proprietà o a un utente.  
  
-   Contrassegna i filegroup di dati come filegroup di sola lettura per consentire la copia dei file su supporti di sola lettura.  
  
 L'amministratore di sistema deve essere il proprietario del database e di tutti i relativi oggetti. L'amministratore di sistema è un utente noto presente in tutti i server in cui è in esecuzione [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e può prevedere l'esistenza quando il database viene distribuito e installato in un secondo momento.  
  
 Se si esegue **sp_certify_removable** senza il valore **auto** e vengono restituite informazioni su una delle condizioni seguenti:  
  
-   L'amministratore di sistema non è il proprietario del database.  
  
-   Esistono utenti creati da un altro utente.  
  
-   L'amministratore di sistema non è il proprietario di tutti gli oggetti del database.  
  
-   Sono state concesse autorizzazioni non predefinite.  
  
 È possibile correggere queste condizioni come indicato di seguito:  
  
-   Utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] strumenti e procedure, quindi eseguire di nuovo **sp_certify_removable** .  
  
-   È sufficiente eseguire **sp_certify_removable** con il valore **auto** .  
  
 Si noti che questa stored procedure controlla solo gli utenti e le autorizzazioni degli utenti. È consentito aggiungere gruppi al database e concedere autorizzazioni a tali gruppi. Per altre informazioni, vedere [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni Execute sono limitate ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene confermato che il database `inventory` è pronto per la rimozione.  
  
```  
EXEC sp_certify_removable inventory, AUTO;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_create_removable &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-create-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
