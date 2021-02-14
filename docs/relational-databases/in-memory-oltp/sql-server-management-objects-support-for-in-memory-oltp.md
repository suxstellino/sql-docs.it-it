---
title: Supporto di SQL Server Management Objects - OLTP in memoria
description: Informazioni sugli elementi di SQL Server Management Objects (SMO) che supportano OLTP in memoria. Esaminare i tipi e i membri nello spazio dei nomi Microsoft.SqlServer.Management.Smo.
ms.custom: seo-dt-2019
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2b67292d-6d8e-4016-9063-a97461ffe57a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fcacc0f64b33bedaeda87ef7a7a7b6363d1359a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353652"
---
# <a name="sql-server-management-objects-support-for-in-memory-oltp"></a>Supporto di SQL Server Management Objects per OLTP in memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Questo argomento descrive gli elementi di SQL Server Management Objects (SMO) che supportano OLTP in memoria.  

## <a name="smo-types-and-members"></a>Tipi e membri di SMO

I seguenti tipi e membri sono inclusi nello spazio dei nomi **Microsoft.SqlServer.Management.Smo**, e supportano OLTP in memoria:

- **<xref:Microsoft.SqlServer.Management.Smo.DurabilityType>** (enumerazione)
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.FileGroupType%2A>** (proprietà)
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.%23ctor%2A>** (costruttore)
- **<xref:Microsoft.SqlServer.Management.Smo.FileGroupType>** (enumerazione)
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.BucketCount%2A>** (proprietà)
- IndexType. **<xref:Microsoft.SqlServer.Management.Smo.IndexType.NonClusteredHashIndex>** (membro di enumerazione)
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.IsMemoryOptimized%2A>** (proprietà)
- Server. **<xref:Microsoft.SqlServer.Management.Smo.Server.IsXTPSupported%2A>** (proprietà)
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsNativelyCompiled%2A>** (proprietà)
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsSchemaBound%2A>** (proprietà)
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.Durability%2A>** (proprietà)
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.IsMemoryOptimized%2A>** (proprietà)
- UserDefinedTableType. **<xref:Microsoft.SqlServer.Management.Smo.UserDefinedTableType.IsMemoryOptimized%2A>** (proprietà)

## <a name="c-code-example"></a>Esempio di codice C#

#### <a name="assemblies-referenced-by-the-compiled-code-example"></a>Assembly a cui si fa riferimento nell'esempio di codice compilato

- Microsoft.SqlServer.ConnectionInfo.dll
- Microsoft.SqlServer.Management.Sdk.Sfc.dll
- Microsoft.SqlServer.Smo.dll
- Microsoft.SqlServer.SqlEnum.dll

#### <a name="actions-taken-in-the-code-example"></a>Azioni intraprese nell'esempio di codice

1. Creare un database con filegroup e file con ottimizzazione per la memoria.  
2. Creare una tabella durevole con ottimizzazione per la memoria con una chiave primaria, un indice non cluster e un indice hash non cluster.  
3. Creare colonne e indici.  
4. Creare un tipo di tabella con ottimizzazione per la memoria definito dall'utente  
5. Creare una stored procedure compilata in modo nativo

#### <a name="source-code"></a>Codice sorgente
  
```csharp
using Microsoft.SqlServer.Management.Smo;  
using System;  
  
public class A {  
   static void Main(string[] args) {  
      Server server = new Server("(local)");  
  
      // Create a database with memory-optimized filegroup and memory-optimized file.
      Database db = new Database(server, "MemoryOptimizedDatabase");  
      db.Create();  
      FileGroup fg = new FileGroup(
         db,
         "memOptFilegroup",
         FileGroupType.MemoryOptimizedDataFileGroup);  
      db.FileGroups.Add(fg);  
      fg.Create();  
      // Change this path if needed.
      DataFile file = new DataFile(
         fg,
         "memOptFile",
         @"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\MSSQLmemOptFileName");  
      file.Create();  
  
      // Create a durable memory-optimized table with primary key, nonclustered index and nonclustered hash index.
      // Define the table as memory optimized and set the durability.
      Table table = new Table(db, "memOptTable");  
      table.IsMemoryOptimized = true;  
      table.Durability = DurabilityType.SchemaAndData;  
  
      // Create columns.
      Column col1 = new Column(table, "col1", DataType.Int);  
      col1.Nullable = false;  
      table.Columns.Add(col1);  
      Column col2 = new Column(table, "col2", DataType.Float);  
      col2.Nullable = false;  
      table.Columns.Add(col2);  
      Column col3 = new Column(table, "col3", DataType.Decimal(2, 10));  
      col3.Nullable = false;  
      table.Columns.Add(col3);  
  
      // Create indexes.
      Index pk = new Index(table, "PK_memOptTable");  
      pk.IndexType = IndexType.NonClusteredIndex;  
      pk.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      pk.IndexedColumns.Add(new IndexedColumn(pk, col1.Name));  
      table.Indexes.Add(pk);  
  
      Index ixNonClustered = new Index(table, "ix_nonClustered");  
      ixNonClustered.IndexType = IndexType.NonClusteredIndex;  
      ixNonClustered.IndexKeyType = IndexKeyType.None;  
      ixNonClustered.IndexedColumns.Add(
         new IndexedColumn(ixNonClustered, col2.Name));  
      table.Indexes.Add(ixNonClustered);  
  
      Index ixNonClusteredHash = new Index(table, "ix_nonClustered_Hash");  
      ixNonClusteredHash.IndexType = IndexType.NonClusteredHashIndex;  
      ixNonClusteredHash.IndexKeyType = IndexKeyType.None;  
      ixNonClusteredHash.BucketCount = 1024;  
      ixNonClusteredHash.IndexedColumns.Add(
         new IndexedColumn(ixNonClusteredHash, col3.Name));  
      table.Indexes.Add(ixNonClusteredHash);  
  
      table.Create();  
  
      // Create a user-defined memory-optimized table type.
      UserDefinedTableType uDTT = new UserDefinedTableType(db, "memOptUDTT");  
      uDTT.IsMemoryOptimized = true;  
  
      // Add columns.
      Column udTTCol1 = new Column(uDTT, "udtCol1", DataType.Int);  
      udTTCol1.Nullable = false;  
      uDTT.Columns.Add(udTTCol1);  
      Column udTTCol2 = new Column(uDTT, "udtCol2", DataType.Float);  
      udTTCol2.Nullable = false;  
      uDTT.Columns.Add(udTTCol2);  
      Column udTTCol3 = new Column(uDTT, "udtCol3", DataType.Decimal(2, 10));  
      udTTCol3.Nullable = false;  
      uDTT.Columns.Add(udTTCol3);  
  
      // Add index.
      Index ix = new Index(uDTT, "IX_UDT");  
      ix.IndexType = IndexType.NonClusteredHashIndex;  
      ix.BucketCount = 1024;  
      ix.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      ix.IndexedColumns.Add(new IndexedColumn(ix, udTTCol1.Name));  
      uDTT.Indexes.Add(ix);  
  
      uDTT.Create();  
  
      // Create a natively compiled stored procedure.
      StoredProcedure sProc = new StoredProcedure(db, "nCSProc");  
      sProc.TextMode = false;  
      sProc.TextBody = "--Type body here";  
      sProc.IsNativelyCompiled = true;  
      sProc.IsSchemaBound = true;  
      sProc.ExecutionContext = ExecutionContext.Owner;  
      sProc.Create();  
   }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  

- [Supporto di SQL Server per OLTP in memoria](./transact-sql-support-for-in-memory-oltp.md)
- [Panoramica di SMO](../server-management-objects-smo/overview-smo.md)