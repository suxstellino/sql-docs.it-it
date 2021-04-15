---
title: Assistente compilazione nativa | Microsoft Docs
description: Informazioni su come usare l'Assistente compilazione nativa per eseguire la migrazione di una stored procedure interpretata alla compilazione nativa come parte della migrazione a OLTP in memoria.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
f1_keywords:
- sql13.swb.nativecompilationwizard.f1
- swb.nativecompilationwizard.f1
ms.assetid: d3898a47-2985-4a08-bc70-fd8331a01b7b
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4275a527ca485a1cfeee35210ed9c3bf6ac89876
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491381"
---
# <a name="native-compilation-advisor"></a>Assistente compilazione nativa
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  I report di analisi delle prestazioni delle transazioni indicano quali stored procedure interpretate nel database traggono vantaggio se vengono trasferite per usare la compilazione nativa. Per altri dettagli, vedere [Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md).  
  
 Una volta identificata la stored procedure da trasferire per usare la compilazione nativa, è possibile usare l'Assistente compilazione nativa per facilitare la migrazione della stored procedure interpretata alla compilazione nativa. Per altre informazioni sulle stored procedure compilate in modo nativo, vedere [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
 In una determinata stored procedure interpretata, l'Assistente compilazione nativa consente di identificare tutte le funzionalità non supportate nei moduli nativi. L'Assistente compilazione nativa fornisce collegamenti alla documentazione relativa a soluzioni alternative e soluzioni.  
  
 Per informazioni sulle metodologie di migrazione, vedere [In-Memory OLTP - Common Workload Patterns and Migration Considerations](/previous-versions/dn673538(v=msdn.10)) (OLTP in memoria: considerazioni sulla migrazione e sui modelli di carico di lavoro comuni).  
  
## <a name="walkthrough-using-the-native-compilation-advisor"></a>Procedura dettagliata per l'utilizzo dell'Assistente compilazione nativa  
 In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla stored procedure che si desidera convertire, quindi selezionare **Assistente compilazione nativa**. Verrà visualizzata la pagina introduttiva di **Assistente compilazione nativa stored procedure**. Fare clic su **Avanti** per continuare.  
  
### <a name="stored-procedure-validation"></a>Convalida stored procedure  
 In questa pagina verrà indicato se la stored procedure utilizza dei costrutti che non sono compatibili con la compilazione nativa. Fare clic su **Avanti** per vedere i dettagli. Se sono presenti costrutti che non sono compatibili con la compilazione nativa, fare clic su **Avanti** per vedere i dettagli.  
  
### <a name="stored-procedure-validation-result"></a>Risultati di convalida stored procedure  
 I dettagli su eventuali costrutti non compatibili con la compilazione nativa saranno visualizzati nella pagina **Risultati di convalida stored procedure** . È possibile generare un report (facendo clic su **Genera report**), uscire da **Assistente compilazione nativa** e aggiornare il codice in modo che risulti compatibile con la compilazione nativa.  
  
## <a name="code-sample"></a>Codice di esempio  
 L'esempio seguente mostra una stored procedure interpretata e la stored procedure *equivalente* per la compilazione nativa. Nell'esempio viene supposto l'utilizzo di una directory denominata c:\data.  
  
> [!NOTE]  
>  Come al solito, l'elemento **FILEGROUP** e l'istruzione mydatabase **USE** si applicano a Microsoft SQL Server, ma non al database SQL di Azure.  
  
```sql  
CREATE DATABASE Demo  
ON  
PRIMARY(NAME = [Demo_data],  
FILENAME = 'C:\DATA\Demo_data.mdf', size=500MB)  
, FILEGROUP [Demo_fg] CONTAINS MEMORY_OPTIMIZED_DATA(  
NAME = [Demo_dir],  
FILENAME = 'C:\DATA\Demo_dir')  
LOG ON (name = [Demo_log], Filename='C:\DATA\Demo_log.ldf', size=500MB)  
COLLATE Latin1_General_100_BIN2;  
go  
  
USE Demo;  
go  
  
CREATE TABLE [dbo].[SalesOrders]  
(  
     [order_id] [int] NOT NULL,  
     [order_date] [datetime] NOT NULL,  
     [order_status] [tinyint] NOT NULL  
     CONSTRAINT [PK_SalesOrders] PRIMARY KEY NONCLUSTERED HASH   
(  
     [order_id]  
) WITH ( BUCKET_COUNT = 2097152)  
) WITH ( MEMORY_OPTIMIZED = ON )  
go  
  
-- Interpreted.  
CREATE PROCEDURE [dbo].[InsertOrder] @id INT, @date DATETIME2, @status TINYINT  
AS   
BEGIN   
  INSERT dbo.SalesOrders VALUES (@id, @date, @status);  
END  
go  
  
-- Natively Compiled.  
CREATE PROCEDURE [dbo].[InsertOrderXTP]  
      @id INT, @date DATETIME2, @status TINYINT  
  WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS   
BEGIN ATOMIC WITH   
     (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
     )  
  INSERT dbo.SalesOrders VALUES (@id, @date, @status);  
END  
go  
  
SELECT * from SalesOrders;  
go  
  
EXECUTE dbo.InsertOrder @id= 10, @date = '1956-01-01 12:00:00', @status = 1;  
EXECUTE dbo.InsertOrderXTP @id= 11, @date = '1956-01-01 12:01:00', @status = 2;  
  
SELECT * from SalesOrders;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)   
 [Requisiti per l'utilizzo di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)  
  
