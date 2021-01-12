---
description: PathName (Transact-SQL)
title: PathName (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- PathName_TSQL
- PathName
dev_langs:
- TSQL
helpviewer_keywords:
- PathName FILESTREAM [SQL Server]
ms.assetid: 6b95ad90-6c82-4a23-9294-a2adb74934a3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 85058a3d551a385e4d2de8aed2ea9f56dc3b19c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093901"
---
# <a name="pathname-transact-sql"></a>PathName (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il percorso di un oggetto binario di grandi dimensioni (BLOB) FILESTREAM. L'API OpenSqlFilestream usa questo percorso per restituire un handle che un'applicazione può usare per lavorare con i dati BLOB usando le API Win32. PathName è un valore di sola lettura.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
column_name.PathName ( @option [ , use_replica_computer_name ] )  
```  
  
## <a name="arguments"></a>Argomenti  
 *column_name*  
 Nome di colonna di una colonna di tipo **varbinary (max)** FILESTREAM. *column_name* deve essere un nome di colonna. Non può essere un'espressione né il risultato di un'istruzione CAST o CONVERT.  
  
 La richiesta del percorso per una colonna con qualsiasi altro tipo di dati o per un columnthat **varbinary (max)** non dispone dell'attributo di archiviazione FILESTREAM provocherà un errore in fase di compilazione della query.  
  
 *\@Opzione*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) integer che definisce il modo in cui deve essere formattato il componente server del percorso. l' *\@ opzione* può essere uno dei valori seguenti. Il valore predefinito è 0.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0|Restituisce il nome del server convertito in formato BIOS, ad esempio `\\SERVERNAME\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
|1|Restituisce il nome del server senza conversione, ad esempio `\\ServerName\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F1`|  
|2|Restituisce il percorso completo del server, ad esempio `\\ServerName.MyDomain.com\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
  
 *use_replica_computer_name*  
 Valore di bit che definisce il modo in cui deve essere restituito il nome del server in un gruppo di disponibilità Always On.  
  
 Quando il database non appartiene a un gruppo di disponibilità Always On, il valore di questo argomento viene ignorato. Il nome computer viene utilizzato sempre nel percorso.  
  
 Quando il database appartiene a un gruppo di disponibilità Always On, il valore di *use_replica_computer_name* ha l'effetto seguente sull'output della funzione **pathname** :  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|Non specificato.|La funzione restituisce il nome di rete virtuale nel percorso.|  
|0|La funzione restituisce il nome di rete virtuale nel percorso.|  
|1|La funzione restituisce il nome del computer nel percorso.|  
  
## <a name="return-type"></a>Tipo restituito  
 **nvarchar(max)**  
  
## <a name="return-value"></a>Valore restituito  
 Il valore restituito è il percorso completo logico oppure il percorso NETBIOS dell'oggetto BLOB. PathName non restituisce un indirizzo IP. Quando l'oggetto BLOB FILESTREAM non è stato creato, viene restituito NULL.  
  
## <a name="remarks"></a>Osservazioni  
 La colonna ROWGUID deve essere visibile in tutte le query in cui viene chiamato PathName.  
  
 Un oggetto BLOB FILESTREAM può essere creato solo utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-reading-the-path-for-a-filestream-blob"></a>R. Lettura del percorso di un oggetto BLOB FILESTREAM  
 Nell'esempio seguente viene assegnato `PathName` a una variabile `nvarchar(max)`.  
  
```sql  
DECLARE @PathName nvarchar(max);  
SET @PathName = (  
    SELECT TOP 1 photo.PathName()  
    FROM dbo.Customer  
    WHERE LastName = 'CustomerName'  
    );  
```  
  
### <a name="b-displaying-the-paths-for-filestream-blobs-in-a-table"></a>B. Visualizzazione dei percorsi di oggetti BLOB FILESTREAM in una tabella  
 Nell'esempio seguente vengono creati e visualizzati i percorsi di tre oggetti BLOB FILESTREAM.  
  
```sql  
-- Create a FILESTREAM-enabled database.  
-- The c:\data directory must exist.  
CREATE DATABASE PathNameDB  
ON  
PRIMARY ( NAME = ArchX1,  
    FILENAME = 'c:\data\archdatP1.mdf'),  
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = ArchX3,  
    FILENAME = 'c:\data\filestreamP1')  
LOG ON  ( NAME = ArchlogX1,  
    FILENAME = 'c:\data\archlogP1.ldf');  
GO  
  
USE PathNameDB;  
GO  
  
-- Create a table, add some records, and  
-- create the associated FILESTREAM  
-- BLOB files.  
  
CREATE TABLE TABLE1  
    (  
        ID int,  
        RowGuidColumn UNIQUEIDENTIFIER  
                      NOT NULL UNIQUE ROWGUIDCOL,  
        FILESTREAMColumn varbinary(MAX) FILESTREAM  
    );  
GO  
  
INSERT INTO TABLE1 VALUES  
 (1, NEWID(), 0x00)  
,(2, NEWID(), 0x00)  
,(3, NEWID(), 0x00);  
GO  
  
SELECT FILESTREAMColumn.PathName() AS 'PathName' FROM TABLE1;  
  
--Results  
--PathName  
------------------------------------------------------------------------------------------------------------  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\DD67C792-916E-4A76-8C8A-4A85DC5DB908  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\2907122B-2560-4CB9-86DC-FBE7ABA1843B  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\922BE0E0-CAB9-4403-90BF-945BD258E4BC  
--  
--(3 row(s) affected)  
GO  
  
--Drop the database to clean up.  
USE master;  
GO  
DROP DATABASE PathNameDB;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati BLOB (Binary Large Object) (SQL Server)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [GET_FILESTREAM_TRANSACTION_CONTEXT &#40;&#41;Transact-SQL ](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)   
 [Accesso ai dati FILESTREAM con OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
  
