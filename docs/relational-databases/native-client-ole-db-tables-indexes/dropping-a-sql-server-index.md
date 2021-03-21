---
description: Drop SQL Server index (provider OLE DB di Native Client)
title: Drop SQL Server index (provider OLE DB di Native Client) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- removing indexes
- deleting indexes
- DropIndex function
- dropping indexes
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
ms.assetid: add3ba14-10b1-4723-b7c0-3e83689e9fdd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26c14af7fd452b93df93ef5c59b777b4057e3279
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748641"
---
# <a name="dropping-a-sql-server-native-client-index"></a>Eliminazione di un indice SQL Server Native Client

  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client espone la funzione **IIndexDefinition::D ropindex** . Questo consente ai consumer di rimuovere una colonna da una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client espone alcuni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vincoli PRIMARY KEY e Unique come indici. Il proprietario della tabella, il proprietario del database e alcuni membri del ruolo amministrativo possono modificare una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], eliminando un vincolo. Per impostazione predefinita, solo il proprietario della tabella può eliminare un indice. L'esito positivo o negativo di **DropIndex** dipende quindi non solo dai diritti di accesso dell'utente dell'applicazione, ma anche dal tipo di indice indicato.  
  
 I consumer specificano il nome della tabella come stringa di caratteri Unicode nel membro *pwszName* dell'unione *uName* nel parametro *pTableID*. Il membro *eKind* di *pTableID* deve essere DBKIND_NAME.  
  
 I consumer specificano il nome dell'indice come stringa di caratteri Unicode nel membro *pwszName* dell'unione *uName* nel parametro *pIndexID*. Il membro *eKind* di *pIndexID* deve essere DBKIND_NAME. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non supporta la funzionalità OLE DB di eliminazione di tutti gli indici in una tabella quando *pIndexID* è null. Se *pIndexID* è Null, viene restituito E_INVALIDARG.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle e indici](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
