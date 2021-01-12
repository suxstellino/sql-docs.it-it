---
description: CREATE FULLTEXT CATALOG (Transact-SQL)
title: CREATE FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CATALOG_TSQL
- CREATE_FULLTEXT_TSQL
- FULLTEXT_TSQL
- FULLTEXT CATALOG
- CREATE FULLTEXT CATALOG
- CREATE_FULLTEXT_CATALOG_TSQL
- CATALOG
- FULLTEXT_CATALOG_TSQL
- CREATE FULLTEXT
- FULLTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- CREATE FULLTEXT CATALOG statement
ms.assetid: d7a8bd93-e2d7-4a40-82ef-39069e65523b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4d3520d2a9d8c1c90507bb52742bd888eb1925cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095839"
---
# <a name="create-fulltext-catalog-transact-sql"></a>CREATE FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un catalogo full-text per un database. Un unico catalogo full-text può contenere numerosi indici full-text, ma un indice full-text può essere incluso in un solo catalogo full-text. Ogni database può contenere zero o più cataloghi full-text.  
  
 Non è consentita la creazione di cataloghi full-text nei database **master**, **model** o **tempdb**.  
  
> [!IMPORTANT]  
>  A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] un catalogo full-text è un oggetto virtuale che non appartiene ad alcun filegroup. Un catalogo full-text è un concetto logico che fa riferimento a un gruppo di indici full-text.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
CREATE FULLTEXT CATALOG catalog_name  
     [ON FILEGROUP filegroup ]  
     [IN PATH 'rootpath']  
     [WITH <catalog_option>]  
     [AS DEFAULT]  
     [AUTHORIZATION owner_name ]  
  
<catalog_option>::=  
     ACCENT_SENSITIVITY = {ON|OFF}  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *catalog_name*  
 Nome del nuovo catalogo. Il nome del catalogo deve essere univoco nel database corrente, ovvero diverso da tutti i nomi di catalogo già esistenti. Anche il nome del file corrispondente al catalogo full-text (vedere ON FILEGROUP) deve essere univoco rispetto a tutti i file nel database. Se il nome specificato per il catalogo è già utilizzato per un altro catalogo nel database, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà restituito un errore.  
  
 La lunghezza del nome del catalogo non deve superare i 120 caratteri.  
  
 ON FILEGROUP *filegroup*  
 A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], questa clausola non ha alcun effetto.  
  
 IN PATH **'** _rootpath_ **'**  
 > [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], questa clausola non ha alcun effetto.  
  
 ACCENT_SENSITIVITY = {ON|OFF}  
 Specifica se per il catalogo viene applicata la distinzione tra caratteri accentati e non accentati per l'indicizzazione full-text. Se si modifica questa proprietà, l'indice deve essere ricompilato. Per impostazione predefinita, viene utilizzata l'impostazione per la distinzione tra caratteri accentati e non accentati specificata nelle regole di confronto del database. Per visualizzare le regole di confronto del database, usare la vista del catalogo **sys.databases**.  
  
 Per determinare l'impostazione corrente della proprietà relativa alla distinzione tra caratteri accentati e non accentati di un catalogo full-text, eseguire la funzione FULLTEXTCATALOGPROPERTY con il valore della proprietà **accentsensitivity** su *catalog_name*. Se il valore restituito è '1', il catalogo full-text supporta la distinzione tra caratteri accentati e non accentati. Se il valore è '0', per il catalogo non è attiva tale distinzione.  
  
 AS DEFAULT  
 Specifica che il catalogo è il catalogo predefinito. Quando si creano indici full-text senza specificare un catalogo full-text in modo esplicito, viene utilizzato il catalogo predefinito. Se un catalogo full-text esistente è già contrassegnato AS DEFAULT, l'impostazione di AS DEFAULT per il nuovo catalogo renderà quest'ultimo il catalogo full-text predefinito.  
  
 AUTHORIZATION *owner_name*  
 Imposta il proprietario del catalogo full-text sul nome di un utente o ruolo di database. Se *owner_name* è un ruolo, è necessario che l'utente corrente sia membro di tale ruolo oppure che l'utente che esegue l'istruzione sia il proprietario del database o un amministratore di sistema.  
  
 Se *owner_name* è un nome utente, il nome utente deve essere uno dei seguenti:  
  
-   Nome dell'utente che esegue l'istruzione.  
  
-   Nome di un utente per il quale l'utente che esegue il comando dispone delle autorizzazioni di rappresentazione.  
  
-   Oppure, l'utente che esegue il comando deve essere il proprietario del database o un amministratore di sistema.  
  
 È anche necessario concedere a *owner_name* l'autorizzazione TAKE OWNERSHIP per il catalogo full-text specificato.  
  
## <a name="remarks"></a>Commenti  
 Gli ID dei cataloghi full-text iniziano da 00005 e vengono incrementati di un'unità per ogni catalogo creato.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve avere l'autorizzazione CREATE FULLTEXT CATALOG per il database oppure essere membro dei ruoli predefiniti del database **db_owner** o **db_ddladmin**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono creati un catalogo full-text e un indice full-text.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume) KEY INDEX PK_JobCandidate_JobCandidateID;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 
  
  
