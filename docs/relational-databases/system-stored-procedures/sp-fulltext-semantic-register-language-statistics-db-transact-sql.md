---
description: sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)
title: sp_fulltext_semantic_register_language_statistics_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_semantic_register_language_statistics_db
- sp_fulltext_semantic_register_language_statistics_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_semantic_register_language_statistics_db
ms.assetid: bef1b104-5a44-4327-9ae4-45eae3000f7e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dba0092fdad469c7ce8d762cfd16efcf5f75d6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193543"
---
# <a name="sp_fulltext_semantic_register_language_statistics_db-transact-sql"></a>sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di registrare un database di statistiche lingua semantica prepopolato nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 È possibile avviare l'estrazione semantica solo dopo avere collegato il database delle statistiche sulla lingua e averlo registrato tramite questa stored procedure. È sufficiente eseguire questa attività una volta per ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db  
    [ @dbname = ] 'database_name';  
GO  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 [ @dbname =]'*database_name*'  
 Nome del database di statistiche lingua semantica da registrare per l'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il database deve essere già collegato. *database_name* è di **tipo sysname** e non può essere null.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-set"></a>Set di risultati  
 Nessuna.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Il database di statistiche lingua semantica contiene statistiche correlate alla lingua necessarie per l'elaborazione semantica di contenuto testuale.  
  
 **sp_fulltext_semantic_register_language_statistics_db** esegue i passaggi seguenti:  
  
1.  Verificare che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia una versione che supporta l'elaborazione semantica.  
  
2.  Verificare che per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sia già stato definito un database di statistiche lingua semantica.  
  
3.  Verificare che il database sia un database di statistiche lingua semantica valido.  
  
4.  Impostare le autorizzazioni per il database di statistiche lingua semantica per limitare l'accesso al database da parte degli utenti.  
  
5.  Inserire i metadati che definiscono il nome del database di statistiche lingua semantica per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Inserire i metadati che definiscono i mapping tra il database di statistiche lingua semantica installato e le tabelle dei modelli di lingua interne.  
  
7.  Verificare che il database sia pronto per l'utilizzo.  
  
 Per altre informazioni, vedere [Installazione e configurazione della ricerca semantica](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sul database Semantic Language Statistics installato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , eseguire una query sulla vista del catalogo [sys.fulltext_semantic_language_statistics_database &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Sono necessarie autorizzazioni CONTROL SERVER.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato come registrare il database di Semantic Language Statistics chiamando **sp_fulltext_semantic_register_language_statistics_db**.  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = 'semanticsDb';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Installazione e configurazione della ricerca semantica](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
