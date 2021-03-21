---
title: Tabelle e indici (OLE DB Driver)
description: Informazioni sulle interfacce IIndexDefinition e ITableDefinition di OLE DB Driver che consentono ai consumer di creare, modificare ed eliminare tabelle e indici di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, indexes
- OLE DB, tables
- ITableDefinition interface
- tables [OLE DB]
- IIndexDefinition interface
- OLE DB Driver for SQL Server, tables
- OLE DB Driver for SQL Server, indexes
- indexes [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6640d06621820d44ab69a44d6b248f97d921650c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754241"
---
# <a name="tables-and-indexes"></a>Tabelle e indici
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server espone le interfacce **IIndexDefinition** e **ITableDefinition**, consentendo ai consumer di creare, modificare ed eliminare tabelle e indici di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La validità delle definizioni di tabelle e indici dipende dalla versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 La possibilità di creare o eliminare tabelle e indici dipende dai diritti di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di cui dispone l'utente dell'applicazione consumer. L'eliminazione di una tabella può essere vincolata ulteriormente dalla presenza di vincoli di integrità referenziale dichiarativa o da altri fattori.  
  
 La maggior parte delle applicazioni destinate a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa SQL-DMO invece di queste interfacce di OLE DB Driver per SQL Server. SQL-DMO è una raccolta di oggetti di automazione OLE che supportano tutte le funzioni amministrative di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le applicazioni destinate a più provider OLE DB utilizzano queste interfacce OLE DB generiche supportate dai diversi provider OLE DB.  
  
 Nel set di proprietà DBPROPSET_SQLSERVERCOLUMN specifico del provider [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] definisce la proprietà seguente.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|SSPROP_COL_COLLATIONNAME|Tipo: VT_BSTR<br /><br /> L/S: Scrittura<br /><br /> Valore predefinito: Null<br /><br /> Descrizione: questa proprietà viene utilizzata solo in **ITableDefinition**. La stringa specificata in questa proprietà viene utilizzata per la creazione di un'istruzione [CREATE TABLE](../../../t-sql/statements/create-table-transact-sql.md)<br /><br /> .|  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Creazione di tabelle di SQL Server](../../oledb/ole-db-tables-indexes/creating-sql-server-tables.md)  
  
-   [Aggiunta di una colonna a una tabella di SQL Server](../../oledb/ole-db-tables-indexes/adding-a-column-to-a-sql-server-table.md)  
  
-   [Rimozione di una colonna da una tabella di SQL Server](../../oledb/ole-db-tables-indexes/removing-a-column-from-a-sql-server-table.md)  
  
-   [Eliminazione di una tabella di SQL Server](../../oledb/ole-db-tables-indexes/dropping-a-sql-server-table.md)  
  
-   [Creazione di indici di SQL Server](../../oledb/ole-db-tables-indexes/creating-sql-server-indexes.md)  
  
-   [Eliminazione di un indice di SQL Server](../../oledb/ole-db-tables-indexes/dropping-a-sql-server-index.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../../t-sql/statements/create-index-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-index-transact-sql.md)  
  
  
