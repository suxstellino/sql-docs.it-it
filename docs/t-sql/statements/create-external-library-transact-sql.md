---
description: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server
title: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: aa02e00f5ebe848d1fa8567e671f7c912456b852
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049208"
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Carica file di pacchetti R, Python o Java in un database dal flusso di byte o dal percorso di file specificato. Questa istruzione funge da meccanismo generico per l'amministratore del database per il caricamento degli elementi necessari per i runtime dei nuovi linguaggi esterni e per le piattaforme del sistema operativo supportate da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. 

> [!NOTE]
> In SQL Server 2017 sono supportati il linguaggio R e la piattaforma Windows. In SQL Server 2019 e versioni successive sono supportati R, Python e linguaggi esterni nelle piattaforme Windows e Linux.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Carica file di pacchetti R o Python in un database dal flusso di byte o dal percorso di file specificato. Questa istruzione funge da meccanismo generico per l'amministratore del database per il caricamento degli elementi necessari. 

> [!NOTE]
> In Istanza gestita di SQL di Azure è possibile usare **sqlmlutils** per installare una libreria. Per informazioni dettagliate, vedere [Installare pacchetti Python con sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) e [Installare nuovi pacchetti R con sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Sintassi per SQL Server 2019

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = <platform> ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'  
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<platform> :: = 
{
      WINDOWS
    | LINUX
}

<language> :: = 
{
      'R'
    | 'Python'
    | <external_language>
}

```
::: moniker-end
::: moniker range=">=sql-server-2017 <=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Sintassi per SQL Server 2017

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> })  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}
```
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
## <a name="syntax-for-azure-sql-managed-instance"></a>Sintassi per Istanza gestita di SQL di Azure

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = <library_bits>)  
}  

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<language> :: = 
{
      'R'
    | 'Python'
}
```
::: moniker-end

### <a name="arguments"></a>Argomenti

**library_name**

Le librerie caricate nell'istanza possono essere pubbliche o private. Se la libreria viene creata da un membro di `dbo`, la libreria è pubblica e può essere condivisa da tutti gli utenti. In caso contrario, la libreria è privata e disponibile solo per tale utente.

I nomi delle librerie devono essere univoci nel contesto di un utente o proprietario specifico. I due utenti **RUser1** e **RUser2**, ad esempio, possono entrambi caricare la libreria R `ggplot2` individualmente e separatamente. Tuttavia, se **RUser1** vuole caricare una versione più recente di `ggplot2`, la seconda istanza deve essere denominata in modo diverso o deve sostituire la libreria esistente.

I nomi delle librerie non possono essere assegnati in modo arbitrario. Il nome di una libreria deve corrispondere al nome necessario per caricare la libreria nello script esterno.

**owner_name**

Specifica il nome dell'utente o del ruolo che è proprietario della libreria esterna. Se viene omesso, la proprietà viene assegnata all'utente corrente.

Le librerie di proprietà del proprietario del database sono considerate globali per il database e il runtime. In altre parole, i proprietari di database possono creare librerie contenenti un set comune di librerie o pacchetti condiviso da molti utenti. Quando viene creata una libreria esterna da un utente diverso dall'utente `dbo`, la libreria esterna è privata e disponibile solo per tale utente.

Quando l'utente **RUser1** esegue uno script esterno, il valore di `libPath` può contenere più percorsi. Il primo percorso è sempre il percorso della libreria condivisa creata dal proprietario del database. La seconda parte di `libPath` specifica il percorso che contiene i pacchetti caricati individualmente da **RUser1**.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Specifica il contenuto del pacchetto per una piattaforma specifica. È supportato soltanto un elemento di tipo file per piattaforma.

Il file può essere specificato usando il percorso locale o il percorso di rete.

Quando si prova ad accedere al file specificato in **<client_library_specifier>** , SQL Server rappresenta il contesto di sicurezza dell'account di accesso di Windows corrente. Se **<client_library_specifier>** specifica un percorso di rete (percorso UNC), la rappresentazione dell'account di accesso corrente non viene riportata nel percorso di rete a causa dei limiti di delega. In questo caso, l'accesso viene eseguito tramite il contesto di sicurezza dell'account del servizio SQL Server. Per altre informazioni, vedere [Credenziali (motore di database)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Facoltativamente, è possibile specificare una piattaforma del sistema operativo per il file. È consentito un solo elemento di tipo file o un contenuto per piattaforma del sistema operativo per un linguaggio o un runtime specifico.
::: moniker-end

**library_bits**

Specifica il contenuto del pacchetto come valore letterale esadecimale, analogamente agli assembly. 

Questa opzione è utile se è necessario creare una libreria o modificare una libreria esistente (e si hanno le autorizzazioni necessarie a tale scopo), ma il file system del server è soggetto a restrizioni e non è possibile copiare i file di libreria in un percorso a cui il server può accedere.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**PIATTAFORMA**

Specifica la piattaforma per il contenuto della libreria. Il valore predefinito corrisponde alla piattaforma host in cui è in esecuzione SQL Server. Per l'utente, quindi, non è necessario specificare questo valore. È necessario nel caso in cui siano supportate più piattaforme o l'utente debba specificare una piattaforma diversa.
In SQL Server 2019 Windows e Linux sono le piattaforme supportate.
::: moniker-end

::: moniker range="=sql-server-2017"
**LANGUAGE = 'R'**

Specifica il linguaggio del pacchetto. Il linguaggio R è supportato in SQL Server 2017.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

Specifica il linguaggio del pacchetto. Il valore può essere `R` o `Python` in Istanza gestita di SQL di Azure.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

Specifica il linguaggio del pacchetto. Il valore può essere `R`, `Python` o il nome di un linguaggio esterno (vedere [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

## <a name="remarks"></a>Osservazioni

::: moniker range=">=sql-server-2017 <=sql-server-2017"
Per il linguaggio R, quando si usa un file, è necessario preparare i pacchetti sotto forma di file di archivio compressi usando l'estensione zip di Windows. 
In SQL Server 2017 Windows è l'unica piattaforma supportata.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio R, quando si usa un file è necessario preparare i pacchetti sotto forma di file di archivio compressi usando l'estensione zip.  

Per il linguaggio Python, è necessario preparare il pacchetto in un file WHL o ZIP sotto forma di file di archivio compresso. Se il pacchetto è già un file ZIP, deve essere incluso in un nuovo file ZIP. Il caricamento di un pacchetto direttamente come file WHL o ZIP attualmente non è supportato.
::: moniker-end

L'istruzione `CREATE EXTERNAL LIBRARY` carica i bit della libreria nel database. La libreria viene installata quando un utente esegue uno script esterno tramite [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) e chiama il pacchetto o la libreria.

Le librerie caricate nell'istanza possono essere pubbliche o private. Se la libreria viene creata da un membro di `dbo`, la libreria è pubblica e può essere condivisa da tutti gli utenti. In caso contrario, la libreria è privata e disponibile solo per tale utente.

Una serie di pacchetti, detti *pacchetti di sistema*, sono preinstallati in un'istanza di SQL. L'utente non può aggiungere, aggiornare o rimuovere i pacchetti di sistema.

## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione `CREATE EXTERNAL LIBRARY`. Per impostazione predefinita, ogni utente con **dbo** che è membro del ruolo **db_owner** ha le autorizzazioni per creare una libreria esterna. Per tutti gli altri utenti, è necessario concedere in modo esplicito le autorizzazioni usando un'istruzione [GRANT](./grant-database-permissions-transact-sql.md) specificando CREATE EXTERNAL LIBRARY come privilegio.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In SQL Server 2019, oltre all'autorizzazione "CREATE EXTERNAL LIBRARY", l'utente deve anche avere l'autorizzazione REFERENCES su un linguaggio esterno per poter creare librerie esterne per il linguaggio esterno.

```sql
GRANT REFERENCES ON EXTERNAL LANGUAGE::Java to user
GRANT CREATE EXTERNAL LIBRARY to user
```

::: moniker-end

Per modificare qualsiasi libreria è inoltre necessaria l'autorizzazione `ALTER ANY EXTERNAL LIBRARY`.

Per creare una libreria esterna usando un percorso di file, l'utente corrente deve avere un account di accesso autenticato in Windows o essere un membro del ruolo predefinito del server sysadmin.

## <a name="examples"></a>Esempi

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="add-an-external-library-to-a-database"></a>Aggiungere una libreria esterna a un database  

L'esempio seguente aggiunge una libreria esterna denominata `customPackage` a un database.

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

Dopo il caricamento della libreria nell'istanza, l'utente esegue la procedura `sp_execute_external_script` per installare la libreria.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio Python in SQL Server 2019, l'esempio funziona anche sostituendo `'R'` con `'Python'`.
::: moniker-end

### <a name="installing-packages-with-dependencies"></a>Installare pacchetti con dipendenze

Se il pacchetto che si vuole installare presenta dipendenze, è fondamentale analizzare le dipendenze sia di primo che di secondo livello e assicurarsi che tutti i pacchetti necessari siano disponibili _prima_ di tentare l'installazione del pacchetto di destinazione.

Si supponga ad esempio di voler installare un nuovo pacchetto, `packageA`:

+ `packageA` ha una dipendenza da `packageB`
+ `packageB` ha una dipendenza da `packageC`

Perché l'installazione di `packageA` sia possibile, è necessario creare librerie per `packageB` e `packageC` contemporaneamente all'aggiunta di `packageA` a SQL Server. Assicurarsi di controllare anche le versioni dei pacchetti richiesti.

In pratica, le dipendenze dei pacchetti dai pacchetti più diffusi sono in genere molto più complesse rispetto a questo semplice esempio. Ad esempio, **ggplot2** può richiedere oltre 30 pacchetti e questi ultimi possono richiedere pacchetti aggiuntivi non disponibili nel server. La mancanza o una versione non corretta di uno qualsiasi di questi pacchetti può causare la mancata riuscita dell'installazione.

Poiché può essere difficile determinare tutte le dipendenze semplicemente esaminando il manifesto del pacchetto, è consigliabile usare un pacchetto come [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) per identificare tutti i pacchetti che possono essere necessari per eseguire l'installazione.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"

+ Caricare il pacchetto di destinazione e le sue dipendenze. Tutti i file devono essere in una cartella accessibile al server.

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ Installare per primi i pacchetti richiesti.

    Se un pacchetto richiesto è già stato caricato nell'istanza, non è necessario aggiungerlo nuovamente. È sufficiente assicurarsi che la versione del pacchetto esistente sia corretta. 
    
    I pacchetti richiesti `packageC` e `packageB` vengono installati, nell'ordine corretto, quando `sp_execute_external_script` viene eseguita per la prima volta per installare il pacchetto `packageA`.

    Se tuttavia un qualsiasi pacchetto richiesto non è disponibile, l'installazione del pacchetto di destinazione `packageA` non riesce.

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    '
    ```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio Python in SQL Server 2019, l'esempio funziona anche sostituendo `'R'` con `'Python'`.
::: moniker-end

### <a name="create-a-library-from-a-byte-stream"></a>Creare una libreria da un flusso di byte

Se non si ha la possibilità di salvare i file del pacchetto in un percorso nel server, è possibile passare il contenuto del pacchetto in una variabile. L'esempio seguente crea una libreria passando i bit come valore letterale esadecimale.

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio Python SQL Server 2019, l'esempio funziona anche sostituendo **R** con **Python**.
::: moniker-end

> [!NOTE]
> Questo esempio di codice illustra solo la sintassi. Il valore binario in `CONTENT =` è stato troncato per migliorare la leggibilità e non crea una libreria di lavoro. Il contenuto effettivo della variabile binaria sarebbe molto più lungo.

### <a name="change-an-existing-package-library"></a>Modificare una libreria di pacchetti esistente

È possibile usare l'istruzione DDL `ALTER EXTERNAL LIBRARY` per aggiungere nuovo contenuto a una libreria o modificare il contenuto esistente della libreria stessa. Per modificare una libreria esistente è necessaria l'autorizzazione `ALTER ANY EXTERNAL LIBRARY`.

Per altre informazioni, vedere [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="add-a-java-jar-file-to-a-database"></a>Aggiungere un file JAR Java a un database  

L'esempio seguente aggiunge un file JAR esterno denominato `customJar` a un database.

```sql
CREATE EXTERNAL LIBRARY customJar
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\customJar.jar') 
WITH (LANGUAGE = 'Java');
```

Dopo il caricamento della libreria nell'istanza, l'utente esegue la procedura `sp_execute_external_script` per installare la libreria.

```sql
EXEC sp_execute_external_script
    @language = N'Java'
    , @script = N'customJar.MyCLass.myMethod'
    , @input_data_1 = N'SELECT * FROM dbo.MyTable'
WITH RESULT SETS ((column1 int))
```

### <a name="add-an-external-package-for-both-windows-and-linux"></a>Aggiungere un pacchetto esterno per Windows e Linux

È possibile specificare fino a due `<file_spec>`, uno per Windows e uno per Linux.

```sql
CREATE EXTERNAL LIBRARY lazyeval 
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.zip', PLATFORM = WINDOWS),
(CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.tar.gz', PLATFORM = LINUX)
WITH (LANGUAGE = 'R')
```

Quando si usa `sp_execute_external_script` per installare il pacchetto, a seconda della piattaforma in cui è in esecuzione l'istanza di SQL Server verrà usato il contenuto della libreria per quella piattaforma.

```sql
EXECUTE sp_execute_external_script 
    @LANGUAGE = N'R',
    @SCRIPT = N'
library(packageA)
```
::: moniker-end

## <a name="see-also"></a>Vedere anche

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)
