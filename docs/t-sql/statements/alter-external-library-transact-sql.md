---
description: ALTER EXTERNAL LIBRARY (Transact-SQL)
title: ALTER EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- ALTER EXTERNAL LIBRARY
- ALTER_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 7a939a09d107d08ee436d3b1f98c2f74e882ebf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337962"
---
# <a name="alter-external-library-transact-sql"></a>ALTER EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Modifica il contenuto di una libreria di pacchetti esterna esistente.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> In SQL Server 2017 sono supportati il linguaggio R e la piattaforma Windows. In SQL Server 2019 e versioni successive sono supportati R, Python e linguaggi esterni nelle piattaforme Windows e Linux.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> In Istanza gestita di SQL di Azure è possibile modificare una libreria rimuovendola e quindi usando **sqlmlutils** per installare la versione modificata. Per altre informazioni su **sqlmlutils**, vedere [Installare pacchetti Python con sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) e [installare nuovi pacchetti R con sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Sintassi per SQL Server 2019

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = <language> )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = <platform> )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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
::: moniker range="=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Sintassi per SQL Server 2017

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = 'R' )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = WINDOWS )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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

Specifica il nome di una libreria di pacchetti esistente. Le librerie hanno un ambito di tipo utente. I nomi delle librerie devono essere univoci nel contesto di un utente o proprietario specifico.

Il nome della libreria non può essere assegnato in modo arbitrario. È quindi necessario usare il nome previsto dal runtime di chiamata quando viene caricato il pacchetto.

**owner_name**

Specifica il nome dell'utente o del ruolo che è proprietario della libreria esterna.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Specifica il contenuto del pacchetto per una piattaforma specifica. È supportato soltanto un elemento di tipo file per piattaforma.

Il file può essere specificato usando il percorso locale o il percorso di rete. Se l'opzione dell'origine dati è specificata, il nome del file può essere un percorso relativo che riguarda il contenitore a cui si fa riferimento in `EXTERNAL DATA SOURCE`.

Facoltativamente, è possibile specificare una piattaforma del sistema operativo per il file. È consentito un solo elemento di tipo file o un contenuto per piattaforma del sistema operativo per un linguaggio o un runtime specifico.
::: moniker-end

**library_bits**

Specifica il contenuto del pacchetto come valore letterale esadecimale, analogamente agli assembly.

Questa opzione è utile quando si ha l'autorizzazione necessaria a modificare una libreria, ma l'accesso ai file nel server è limitato e non è possibile salvare il contenuto in un percorso accessibile al server.

In alternativa, è possibile passare il contenuto dei pacchetti come variabile in un formato binario.

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**platform = WINDOWS**

Specifica la piattaforma per il contenuto della libreria. Si tratta di un valore obbligatorio quando si modifica una libreria esistente per poter aggiungere una piattaforma diversa.
In SQL Server 2017 Windows è l'unica piattaforma supportata.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**platform**

Specifica la piattaforma per il contenuto della libreria. Si tratta di un valore obbligatorio quando si modifica una libreria esistente per poter aggiungere una piattaforma diversa. 
In SQL Server 2019 Windows e Linux sono le piattaforme supportate.
::: moniker-end

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**LANGUAGE = 'R'**

Specifica il linguaggio del pacchetto. Il linguaggio R è supportato in SQL Server 2017.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

Specifica il linguaggio del pacchetto. Il valore può essere **R** o **Python** in Istanza gestita di SQL di Azure.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

Specifica il linguaggio del pacchetto. Il valore può essere **R**, **Python** o il nome di un linguaggio esterno (vedere [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni

::: moniker range=">=sql-server-2017 <=sql-server-2017"
Per il linguaggio R, è necessario preparare i pacchetti sotto forma di file di archivio compressi usando l'estensione zip di Windows. In SQL Server 2017 Windows è l'unica piattaforma supportata.  
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio R, quando si usa un file è necessario preparare i pacchetti sotto forma di file di archivio compressi usando l'estensione zip. 

Per il linguaggio Python, è necessario preparare il pacchetto in un file WHL o ZIP sotto forma di file di archivio compresso. Se il pacchetto è già un file ZIP, deve essere incluso in un nuovo file ZIP. Il caricamento di un pacchetto direttamente come file WHL o ZIP attualmente non è supportato.
::: moniker-end

L'istruzione `ALTER EXTERNAL LIBRARY` carica solo i bit della libreria nel database. La libreria modificata viene installata quando un utente esegue il codice [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) che chiama la libreria.

Una serie di pacchetti, detti *pacchetti di sistema*, sono preinstallati in un'istanza di SQL. L'utente non può aggiungere, aggiornare o rimuovere i pacchetti di sistema.

## <a name="permissions"></a>Autorizzazioni

Per impostazione predefinita, l'utente **dbo** o qualsiasi membro del ruolo **db_owner** ha l'autorizzazione per eseguire ALTER EXTERNAL LIBRARY. Inoltre, una libreria esterna può essere modificata dall'utente che ha creato la libreria.

## <a name="examples"></a>Esempi

L'esempio seguente modifica una libreria esterna denominata `customPackage`.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="replace-the-contents-of-a-library-using-a-file"></a>Sostituire il contenuto di una libreria usando un file

Nell'esempio seguente viene modificata una libreria esterna denominata `customPackage`, usando un file compresso che contiene i bit aggiornati.

```sql
ALTER EXTERNAL LIBRARY customPackage 
SET 
  (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip')
WITH (LANGUAGE = 'R');
```

Per installare la libreria aggiornata, eseguire la stored procedure `sp_execute_external_script`.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
;
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Per il linguaggio Python, l'esempio funziona anche sostituendo `'R'` con `'Python'`.
::: moniker-end

### <a name="alter-an-existing-library-using-a-byte-stream"></a>Modificare una libreria esistente usando un flusso di byte

L'esempio seguente modifica la libreria esistente passando i nuovi bit come valore letterale esadecimale.

```SQL
ALTER EXTERNAL LIBRARY customLibrary 
SET (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
Per il linguaggio Python, l'esempio funziona anche sostituendo `'R'` con `'Python'`.
::: moniker-end

> [!NOTE]
> Questo esempio di codice illustra solo la sintassi. Il valore binario in `CONTENT =` è stato troncato per migliorare la leggibilità e non crea una libreria di lavoro. Il contenuto effettivo della variabile binaria sarebbe molto più lungo.

## <a name="see-also"></a>Vedere anche

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)