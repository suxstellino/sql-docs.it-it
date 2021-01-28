---
title: Supporto FILESTREAM | Microsoft Docs
description: SQL Server supporta la funzionalità FILESTREAM avanzata, che consente di archiviare e accedere a valori binari di grandi dimensioni, tramite SQL Server o file system.
ms.custom: ''
ms.date: 09/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- FILESTREAM [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server [FILESTREAM support]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1fcc048d51186289d13cbe8918b5328f8c604f8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766134"
---
# <a name="filestream-support-in-ole-db-driver-for-sql-server"></a>Supporto di FILESTREAM in OLE DB Driver per SQL Server
[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

A partire da [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] OLE DB Driver per SQL Server supporta la funzionalità avanzata FILESTREAM. Per gli esempi, vedere [FILESTREAM e OLE DB](../../oledb/ole-db-how-to/filestream/filestream-and-ole-db.md).  

FILESTREAM consente di archiviare e accedere a valori binari di grandi dimensioni mediante [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o accesso diretto al file system di Windows. Un valore binario di grandi dimensioni è un valore superiore a 2 gigabyte (GB). Per altre informazioni sul supporto FILESTREAM avanzato, vedere [FILESTREAM &#40;SQL Server&#41;](../../../relational-databases/blob/filestream-sql-server.md).  
  
Quando si apre una connessione al database, per impostazione predefinita **\@\@TEXTSIZE** verrà impostato su -1 (senza limiti).  
  
È anche possibile accedere alle colonne FILESTREAM e aggiornarle utilizzando l'API del file system di Windows.  
  
Per altre informazioni, vedere [Accedere a dati FILESTREAM con OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
## <a name="querying-for-filestream-columns"></a>Esecuzione di una query sulle colonne FILESTREAM  
I set di righe degli schemi in OLE DB non indicano se una colonna è di tipo FILESTREAM. Impossibile utilizzare ITableDefinition in OLE DB per creare una colonna FILESTREAM.    
  
Per creare colonne FILESTREAM o per individuare le colonne di tipo FILESTREAM esistenti, è possibile utilizzare la colonna **is_filestream** della vista di catalogo [sys.columns](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).  
  
Di seguito è riportato un esempio:  
  
```sql  
-- Create a table with a FILESTREAM column.  
CREATE TABLE Bob_01 (GuidCol1 uniqueidentifier ROWGUIDCOL NOT NULL UNIQUE DEFAULT NEWID(), IntCol2 int, varbinaryCol3 varbinary(max) FILESTREAM);  
  
-- Find FILESTREAM columns.  
SELECT name FROM sys.columns WHERE is_filestream=1;  
  
-- Determine whether a column is a FILESTREAM column.  
SELECT is_filestream FROM sys.columns WHERE name = 'varbinaryCol3' AND object_id IN (SELECT object_id FROM sys.tables WHERE name='Bob_01');  
```  
  
## <a name="down-level-compatibility"></a>Compatibilità con le versioni precedenti  
Se il client è stato compilato utilizzando OLE DB driver per SQL Server e l'applicazione si connette a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] o versione successiva), il comportamento **varbinary (max)** sarà compatibile con il comportamento introdotto da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] . Questo significa che i dati restituiti avranno come dimensione massima 2 GB. Per valori di dimensioni superiori a 2 GB, verrà eseguito un troncamento e restituito l'avviso "Troncamento a destra dei dati della stringa". 
  
Quando la compatibilità con il tipo di dati è impostata su 80, il comportamento client sarà coerente con il comportamento del client legacy.  
  
Per i client che utilizzano SQLOLEDB o altri provider rilasciati prima di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], verrà eseguito il mapping di **varbinary(max)** all'immagine.  
  
## <a name="comments"></a>Commenti
- Per inviare e ricevere valori **varbinary(max)** maggiori di 2 GB, un'applicazione usa **DBTYPE_IUNKNOWN** in associazioni di parametri e di risultati. Per i parametri il provider deve chiamare IUnknown::QueryInterface per ISequentialStream e per i risultati che restituiscono ISequentialStream.  

-  Per OLE DB il controllo relativo ai valori ISequentialStream diventa meno rigido. Se *wType* è **DBTYPE_IUNKNOWN** nello struct **DBBINDING**, il controllo della lunghezza può essere disabilitato omettendo **DBPART_LENGTH** in *dwPart* o impostando la lunghezza dei dati (in corrispondenza dell'offset *obLength* nel buffer dei dati) su ~0. In questo caso, il provider non controllerà la lunghezza del valore e richiederà e restituirà tutti i dati disponibili tramite il flusso. Questa modifica verrà applicata a tutti i tipi LOB (Large Object) e XML, ma solo in caso di connessione a server [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o versione successiva. In questo modo, gli sviluppatori disporranno di maggiore flessibilità, mantenendo la coerenza e la compatibilità con le versioni precedenti per applicazioni esistenti e server legacy.  Questa modifica ha effetto su tutte le interfacce che trasferiscono dati, principalmente IRowset::GetData, ICommand::Execute e IRowsetFastLoad::InsertRow.
 

## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
