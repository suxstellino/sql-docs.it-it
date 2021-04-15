---
title: Compilazione nativa di tabelle e stored procedure
description: Informazioni su come OLTP in memoria può compilare tabelle ottimizzate per la memoria e stored procedure che accedono alle tabelle ottimizzate per la memoria nel codice nativo per migliorare le prestazioni.
ms.custom: seo-dt-2019
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 5880fbd9-a23e-464a-8b44-09750eeb2dad
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 629f5fa5c8b3a6638ed7e18c58bb7073e03a6039
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492508"
---
# <a name="native-compilation-of-tables-and-stored-procedures"></a>Compilazione nativa di tabelle e stored procedure
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Con OLTP in memoria viene introdotto il concetto di compilazione nativa. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può compilare in modo nativo stored procedure che accedono alle tabelle ottimizzate per la memoria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è anche in grado di compilare in modo nativo tabelle ottimizzate per la memoria. Con la compilazione nativa si accede ai dati più velocemente e si eseguono le query in modo più efficiente rispetto al tradizionale [!INCLUDE[tsql](../../includes/tsql-md.md)]interpretato. La compilazione nativa di tabelle e stored procedure produce DLL.

È anche supportata la compilazione nativa dei tipi di tabella con ottimizzazione per la memoria. Per altre informazioni, vedere [Tabella temporanea più rapida e variabile di tabella tramite l'ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md).

La compilazione nativa si riferisce al processo di conversione dei costrutti di programmazione in codice nativo, costituito da istruzioni del processore senza la necessità di ulteriore compilazione o interpretazione.

OLTP in memoria compila le tabelle ottimizzate per la memoria quando vengono create e le stored procedure compilate in modo nativo quando vengono caricate nelle DLL native. Inoltre, le DLL vengono ricompilate dopo il riavvio di un database o di un server. Le informazioni necessarie per ricreare le DLL vengono archiviate nei metadati del database. Le DLL non fanno parte del database, sebbene siano associate al database. Ad esempio, le DLL non sono incluse nei backup del database.

> [!NOTE]
> Le tabelle con ottimizzazione per la memoria vengono ricompilate durante un riavvio del server. Per velocizzare il recupero del database, le stored procedure compilate in modo nativo non vengono ricompilate durante un riavvio del server, ma vengono compilate al momento della prima esecuzione. A causa di questa compilazione posticipata, le stored procedure compilate in modo nativo vengono visualizzate solo quando si chiama [sys.dm_os_loaded_modules &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-loaded-modules-transact-sql.md) dopo la prima esecuzione.

## <a name="maintenance-of-in-memory-oltp-dlls"></a>Manutenzione delle DLL di OLTP in memoria

La query seguente indica che tutte le DLL di tabelle e stored procedure vengono attualmente caricate in memoria nel server:

```sql
SELECT
        mod1.name,
        mod1.description
    from
        sys.dm_os_loaded_modules  as mod1
    where
        mod1.description = 'XTP Native DLL';
```

Gli amministratori di database non devono gestire i file generati da una compilazione nativa. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimuove automaticamente i file generati non più necessari. Ad esempio, i file generati verranno eliminati quando una tabella e una stored procedure vengono eliminate o nel caso in cui un database venga eliminato.

> [!NOTE]
> Se la compilazione ha esito negativo o viene interrotta, alcuni file generati non vengono rimossi. Questi file vengono lasciati intenzionalmente per motivi di supporto e vengono rimossi quando il database viene eliminato.

> [!NOTE]
> SQL Server compila file DLL per tutte le tabelle necessarie per il ripristino del database. Se una tabella è stata eliminata immediatamente prima di un riavvio del database, è possibile che i file checkpoint o i log delle transazioni includano ancora residui della tabella. È quindi possibile ricompilare il file DLL della tabella durante l'avvio del database. Dopo il riavvio, il file DLL verrà scaricato e i file verranno rimossi durante il normale processo di pulitura.

## <a name="native-compilation-of-tables"></a>Compilazione nativa di tabelle

La creazione di una tabella ottimizzata per la memoria tramite un'istruzione **CREATE TABLE** restituisce le informazioni della tabella scritte nei metadati del database e le strutture di indice e di tabella create in memoria. La tabella verrà compilata in una DLL.

Considerare il seguente script di esempio che crea un database e una tabella ottimizzata per la memoria:

```sql
USE master;
GO

CREATE DATABASE DbMemopt3;
GO

ALTER DATABASE DbMemopt3
    add filegroup DbMemopt3_mod_memopt_1_fg
        contains memory_optimized_data
;
GO

-- You must edit the front portion of filename= path, to where your DATA\ subdirectory is,
-- keeping only the trailing portion '\DATA\DbMemopt3_mod_memopt_1_fn'!

ALTER DATABASE DbMemopt3
    add file
    (
        name     = 'DbMemopt3_mod_memopt_1_name',
        filename = 'C:\DATA\DbMemopt3_mod_memopt_1_fn'

        --filename = 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQLSVR2016ID\MSSQL\DATA\DbMemopt3_mod_memopt_1_fn'
    )
        to filegroup DbMemopt3_mod_memopt_1_fg
;
GO

USE DbMemopt3;
GO

CREATE TABLE dbo.t1
(
    c1 int not null primary key nonclustered,
    c2 int
)
    with (memory_optimized = on)
;
GO



-- You can safely rerun from here to the end.

-- Retrieve the path of the DLL for table t1.


DECLARE @moduleName  nvarchar(256);

SET @moduleName =
    (
        '%xtp_t_' +
        cast(db_id() as nvarchar(16)) +
        '_' +
        cast(object_id('dbo.t1') as nvarchar(16)) +
        '%.dll'
    )
;


-- SEARCHED FOR NAME EXAMPLE:  mod1.name LIKE '%xtp_t_8_565577053%.dll'
PRINT @moduleName;


SELECT
        mod1.name,
        mod1.description
    from
        sys.dm_os_loaded_modules  as mod1
    where
        mod1.name LIKE @moduleName
    order by
        mod1.name
;
-- ACTUAL NAME EXAMPLE:  mod1.name = 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQLSVR2016ID\MSSQL\DATA\xtp\8\xtp_t_8_565577053_184009305855461.dll'
GO

--   DROP DATABASE DbMemopt3;  -- Clean up.
GO
```

La creazione della tabella comporta anche la creazione della corrispondente DLL e il caricamento della DLL in memoria. La query DMV immediatamente dopo l'istruzione CREATE TABLE recupera il percorso della DLL della tabella.

La DLL della tabella interpreta le strutture di indice e il formato di riga della tabella. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usano la DLL per attraversare indici, recuperare righe e archiviare il contenuto delle righe.

## <a name="native-compilation-of-stored-procedures"></a>Compilazione nativa di stored procedure

Le stored procedure che sono contrassegnate con NATIVE_COMPILATION vengono compilate in modo nativo. Pertanto, le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] nella procedura vengono tutte compilate nel codice nativo per l'esecuzione efficiente della logica di business critica per le prestazioni.

Per altre informazioni sulle stored procedure compilate in modo nativo, vedere [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).

Considerare la seguente stored procedure di esempio che inserisce righe nella tabella t1 dell'esempio precedente:

```sql
CREATE PROCEDURE dbo.native_sp
    with native_compilation,
         schemabinding,
         execute as owner
as
begin atomic
    with (transaction isolation level = snapshot,
          language = N'us_english')

    DECLARE @i int = 1000000;

    WHILE @i > 0
    begin
        INSERT dbo.t1 values (@i, @i+1);
        SET @i -= 1;
    end
end;
GO

EXECUTE dbo.native_sp;
GO

-- Reset.

DELETE from dbo.t1;
GO
```

La DLL di native_sp può interagire direttamente con la DLL di t1 e con il motore di archiviazione di OLTP in memoria per inserire le righe il più rapidamente possibile.

Il compilatore di OLTP in memoria usano Query Optimizer per creare un piano di esecuzione efficiente per ogni query della stored procedure. Si noti che le stored procedure compilate in modo nativo non vengono automaticamente ricompilate se i dati della tabella cambiano. Per altre informazioni sulla gestione delle statistiche e delle stored procedure con OLTP in memoria, vedere [Statistiche per tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).

## <a name="security-considerations-for-native-compilation"></a>Considerazioni sulla sicurezza della compilazione nativa

Per la compilazione nativa di tabelle e stored procedure viene usato il compilatore di OLTP in memoria. Il compilatore genera file che vengono scritti su disco e caricati in memoria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono usati i seguenti meccanismi per limitare l'accesso a tali file.

### <a name="native-compiler"></a>Compilatore nativo

Il file eseguibile del compilatore, nonché i file binari e i file di intestazione necessari per la compilazione nativa, vengono installati come parte dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella cartella MSSQL\Binn\Xtp. Se l'istanza predefinita viene installata in C:\Programmi, i file del compilatore vengono quindi installati in C:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL13.MSSQLSERVER\MSSQL\Binn\Xtp.

Per limitare l'accesso al compilatore, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono usati gli elenchi di controllo di accesso (ACL) per limitare l'accesso ai file binari. Tutti i file binari di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono protetti dalla modifica o dalla manomissione tramite gli ACL. Gli ACL del compilatore nativo limitano anche l'utilizzo del compilatore; solo gli amministratori di sistema e l'account del servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispongono delle autorizzazioni di lettura ed esecuzione per i file del compilatore nativo.

### <a name="files-generated-by-a-native-compilation"></a>File generati da una compilazione nativa

I file generati quando una tabella o una stored procedure viene compilata includono file DLL e file intermedi compresi i file con le estensioni seguenti: c, obj, xml e pdb. I file generati vengono salvati in una sottocartella della cartella dati predefinita. La sottocartella viene denominata Xtp. Quando si installa l'istanza predefinita con la cartella dati predefinita, i file generati vengono installati in C:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL13.MSSQLSERVER\MSSQL\DATA\Xtp.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la manomissione dei file DLL generati viene impedita in tre modi:

- Quando una tabella o una stored procedure viene compilata in una DLL, tale DLL viene immediatamente caricata in memoria e collegata al processo sqlserver.exe. Non è possibile modificare una DLL mentre è collegata a un processo.

- Quando un database viene riavviato, tutte le tabelle e le stored procedure vengono ricompilate (rimosse e ricreate) in base ai metadati del database. In questo modo verranno rimosse tutte le modifiche apportate a un file generato da un agente dannoso.

- I file generati sono considerati parte dei dati utente e dispongono delle stesse restrizioni di sicurezza, tramite ACL, dei file di database: solo gli amministratori di sistema e l'account del servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono accedere a tali file.

Per gestire tali file, non sono necessarie interazioni dell'utente. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno creati e rimossi automaticamente in base alle necessità.

## <a name="see-also"></a>Vedere anche

[Tabelle ottimizzate per la memoria](./sample-database-for-in-memory-oltp.md)

[Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)