---
description: DROP DEFAULT (Transact-SQL)
title: DROP DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_DEFAULT_TSQL
- DROP DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- DROP DEFAULT statement
- defaults [SQL Server], removing
ms.assetid: d2d3af25-8877-46ba-95d9-1844961d97ee
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2762305952b7c2e189a9c399f614974e1fc32e11
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688785"
---
# <a name="drop-default-transact-sql"></a>DROP DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove dal database corrente uno o più valori predefiniti creati dall'utente.  
  
> [!IMPORTANT]
>  L'istruzione DROP DEFAULT verrà rimossa a partire dalla prossima versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non utilizzare DROP DEFAULT in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente la utilizzano. In alternativa, usare le definizioni di valori predefiniti che è possibile creare con la parola chiave DEFAULT dell'istruzione [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP DEFAULT [ IF EXISTS ] { [ schema_name . ] default_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *IF EXISTS*  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Rimuove in modo condizionale l'impostazione predefinita solo se esiste già.  
  
 *schema_name*  
 Nome dello schema a cui appartiene il valore predefinito.  
  
 *default_name*  
 Nome di un valore predefinito esistente. Per visualizzare un elenco dei valori predefiniti esistenti, eseguire **sp_help**. I valori predefiniti devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome dello schema predefinito è facoltativo.  
  
## <a name="remarks"></a>Osservazioni  
 Prima di eliminare un valore predefinito associato a una colonna o un tipo di dati alias, è necessario annullarne l'associazione eseguendo **sp_unbindefault**.  
  
 Se si aggiungono nuove righe senza specificare in modo esplicito un valore dopo avere eliminato un valore predefinito da una colonna che ammette valori Null, in tale posizione verrà inserita la stringa NULL. Se si aggiungono nuove righe senza specificare in modo esplicito un valore dopo avere eliminato un valore predefinito da una colonna NOT NULL, verrà visualizzato un messaggio di errore. Queste righe vengono aggiunte successivamente durante la normale esecuzione dell'istruzione INSERT.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione DROP DEFAULT, è necessario disporre almeno dell'autorizzazione ALTER per lo schema a cui appartiene il valore predefinito.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-a-default"></a>R. Eliminazione di un valore predefinito  
 Se un valore predefinito non è associato a una colonna o un tipo di dati alias, è possibile eliminarlo semplicemente tramite l'istruzione DROP DEFAULT. Nell'esempio seguente viene rimosso il valore predefinito `datedflt` creato dall'utente.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
         WHERE name = 'datedflt'   
            AND type = 'D')  
   DROP DEFAULT datedflt;  
GO  
```  
  
 A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] è possibile usare la sintassi seguente.  
  
```sql  
DROP DEFAULT IF EXISTS datedflt;  
GO  
```  
  
### <a name="b-dropping-a-default-that-has-been-bound-to-a-column"></a>B. Eliminazione di un valore predefinito associato a una colonna  
 Nell'esempio seguente viene disassociato e quindi eliminato il valore predefinito `EmergencyContactPhone` associato alla colonna `Contact` della tabella `phonedflt`.  
  
```sql  
USE AdventureWorks2012;  
GO  
   BEGIN   
      EXEC sp_unbindefault 'Person.Contact.Phone'  
      DROP DEFAULT phonedflt  
   END;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
