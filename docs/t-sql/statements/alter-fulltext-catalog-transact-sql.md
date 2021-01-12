---
description: ALTER FULLTEXT CATALOG (Transact-SQL)
title: ALTER FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_FULLEXT_CATALOG_TSQL
- ALTER FULLEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- modifying full-text catalogs
- full-text catalogs [SQL Server], rebuilding
- accent sensitivity
- ALTER FULLTEXT CATALOG statement
- full-text catalogs [SQL Server], modifying
- full-text catalogs [SQL Server], reorganizing
ms.assetid: 31a47aaf-6c7f-48a4-a86a-d57aec66c9cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a694ce465b53f74e3139a017548e7bfecd52b88c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082852"
---
# <a name="alter-fulltext-catalog-transact-sql"></a>ALTER FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica le proprietà di un catalogo full-text.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
ALTER FULLTEXT CATALOG catalog_name   
{ REBUILD [ WITH ACCENT_SENSITIVITY = { ON | OFF } ]  
| REORGANIZE  
| AS DEFAULT   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *catalog_name*  
 Specifica il nome del catalogo da modificare. Se non esiste un catalogo con il nome specificato, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore e non esegue l'operazione ALTER.  
  
 REBUILD  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di ricompilare l'intero catalogo. Quando viene ricompilato un catalogo, il catalogo esistente viene eliminato e al suo posto viene creato un nuovo catalogo. Tutte le tabelle con riferimenti di indicizzazione full-text vengono associate al nuovo catalogo. La ricompilazione reimposta i metadati full-text nelle tabelle di sistema del database.  
  
 WITH ACCENT_SENSITIVITY = {ON|OFF}  
 Specifica se il catalogo da modificare supporta o meno la distinzione tra caratteri accentati e non accentati per l'esecuzione di query e l'indicizzazione full-text.  
  
 Per determinare l'impostazione corrente della proprietà relativa alla distinzione tra caratteri accentati e non accentati di un catalogo full-text, eseguire la funzione FULLTEXTCATALOGPROPERTY con il valore della proprietà **accentsensitivity** su *catalog_name*. Se la funzione restituisce '1', il catalogo full-text supporta la distinzione tra caratteri accentati e non accentati. Se la funzione restituisce '0', il catalogo non supporta la distinzione tra caratteri accentati e non accentati.  
  
 Il valore predefinito per la distinzione tra caratteri accentati e non accentati è uguale per il catalogo e il database.  
  
 REORGANIZE  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di eseguire un'*unione nell'indice master*, che comporta l'unione degli indici più piccoli creati durante il processo di indicizzazione in un unico indice di grandi dimensioni. L'unione dei frammenti di indici full-text può migliorare le prestazioni e liberare risorse di memoria e spazio su disco. Se il catalogo full-text è soggetto a modifiche frequenti, utilizzare questo comando con regolarità per riorganizzarlo.  
  
 REORGANIZE, inoltre, consente di ottimizzare le strutture interne dell'indice e del catalogo.  
  
 Tenere presente che, a seconda della quantità di dati indicizzati, un'unione nell'indice master può richiedere parecchio tempo. L'unione nell'indice master di una grande quantità di dati può comportare la creazione di una transazione con esecuzione prolungata, con il conseguente ritardo del troncamento del log delle transazioni durante il checkpoint. In questo caso, le dimensioni del log delle transazioni potrebbero aumentare notevolmente, se si utilizza il modello di recupero con registrazione completa. È consigliabile verificare che il log delle transazioni contenga spazio sufficiente per una transazione con esecuzione prolungata prima di riorganizzare un indice full-text di grandi dimensioni in un database in cui viene utilizzato il modello di recupero con registrazione completa. Per altre informazioni, vedere [Gestione delle dimensioni del file di log delle transazioni](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
 AS DEFAULT  
 Specifica che il catalogo è il catalogo predefinito. Quando vengono creati indici full-text senza che siano stati specificati cataloghi, viene utilizzato il catalogo predefinito. Se esiste già un catalogo full-text predefinito e si imposta AS DEFAULT per questo catalogo, il catalogo predefinito esistente verrà ignorato.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione ALTER nel catalogo full-text oppure deve essere membro del ruolo predefinito del server sysadmin o del ruolo predefinito del database **db_owner** o **db_ddladmin**.  
  
> [!NOTE]  
>  Per utilizzare ALTER FULLTEXT CATALOG AS DEFAULT, l'utente deve disporre dell'autorizzazione ALTER nel catalogo full-text e dell'autorizzazione CREATE FULLTEXT CATALOG nel database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente la proprietà `accentsensitivity` del catalogo full-text predefinito viene impostata su `ftCatalog`, a indicare che il catalogo supporta la distinzione tra caratteri accentati e non accentati.  
  
```sql  
--Change to accent insensitive  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog   
REBUILD WITH ACCENT_SENSITIVITY=OFF;  
GO  
-- Check Accentsensitivity  
SELECT FULLTEXTCATALOGPROPERTY('ftCatalog', 'accentsensitivity');  
GO  
--Returned 0, which means the catalog is not accent sensitive.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)  
  
  
