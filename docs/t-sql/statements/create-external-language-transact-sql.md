---
description: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server
title: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a299c47b9d0d2fccde5c44c26d9f78e8bc1cca2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070677"
---
# <a name="create-external-language-transact-sql"></a>CREATE EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Registra estensioni di linguaggio esterno nel database partendo dal percorso file specificato o dal flusso di byte. Questa istruzione funge da meccanismo generico per l'amministratore del database per la registrazione di nuove estensioni di linguaggio esterno in tutte le piattaforme del sistema operativo supportate da SQL Server. Per altre informazioni, vedere [Estensioni del linguaggio](../../language-extensions/language-extensions-overview.md).

> [!NOTE]
> **R** e **Python** sono nomi riservati e nessun linguaggio esterno può essere creato con tali nomi specifici. Per altre informazioni su come usare **R** e **Python**, vedere [SQL Server Machine Learning Services](../../machine-learning/index.yml).

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = { <external_lang_specifier> | <content_bits> },
    FILE_NAME = <external_lang_file_name>
    [ , PLATFORM = <platform> ]
    [ , PARAMETERS = <external_lang_parameters> ]
    [ , ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
    | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'


<platform> :: =
{
    WINDOWS
  | LINUX
}

<external_lang_parameters> :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>Argomenti

**language_name**

I linguaggi sono oggetti con ambito di database. I nomi dei linguaggi devono essere univoci all'interno del database.

**owner_name**

Specifica il nome dell'utente o del ruolo che è proprietario del linguaggio esterno. Se viene omesso, la proprietà viene assegnata all'utente corrente. A seconda delle autorizzazioni, è possibile che sia necessario concedere ad altri utenti autorizzazione esplicita per eseguire script con un linguaggio specifico.

**file_spec**

Specifica il contenuto dell'estensione del linguaggio. È consentito un solo filespec per un linguaggio specifico, per ogni piattaforma.

**external_lang_specifier**

Percorso file completo al file con estensione zip o al file tar.gz che contiene il codice delle estensioni. Può essere un percorso a un file con estensione zip (in Windows) o al file tar.gz (in Linux).

**content_bits**

Specifica il contenuto del linguaggio come valore letterale esadecimale, analogamente agli assembly.

Questa opzione è utile se è necessario creare un linguaggio o modificare un linguaggio esistente (e si hanno le autorizzazioni necessarie a tale scopo), ma il file system del server è soggetto a restrizioni e non è possibile copiare i file di libreria in un percorso a cui il server può accedere.

**external_lang_file_name**

Nome del file con estensione dll o so. Questo nome è necessario per identificare il file corretto, nel caso in cui il file <external_lang_specifier> con estensione zip o il file tar.gz. contenga diversi file con estensione dll o so.

**external_lang_parameters**

Consente di assegnare un set di parametri al runtime del linguaggio esterno. I valori dei parametri sono disponibili per il runtime esterno dopo che il processo esterno è stato avviato. Le variabili di ambiente sono invece accessibili all'estensione del linguaggio prima dell'avvio del processo esterno.

**external_lang_env_variables**

Consente di assegnare un set di variabile di ambiente al runtime del linguaggio esterno prima dell'avvio del processo esterno. Un esempio di variabile di ambiente è ad esempio la home directory del runtime stesso. Ad esempio: JRE_HOME.

**platform**

Questo parametro è necessario per gli scenari ibridi del sistema operativo. In un'architettura ibrida il linguaggio deve essere registrato una volta per ogni piattaforma. Il nome di piattaforma e linguaggio sarà la chiave univoca per ogni linguaggio esterno. Se non viene specificata una piattaforma, viene usato il sistema operativo corrente.

## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione `CREATE EXTERNAL LANGUAGE`. Per impostazione predefinita, ogni utente con **dbo** che è membro del ruolo **db_owner** ha le autorizzazioni per creare un linguaggio esterno. Per tutti gli altri utenti, è necessario concedere in modo esplicito l'autorizzazione usando un'istruzione [GRANT](./grant-database-permissions-transact-sql.md), specificando CREATE EXTERNAL LANGUAGE come privilegio.

Per modificare una libreria è necessaria un'altra autorizzazione, `ALTER ANY EXTERNAL LANGUAGE`.

### <a name="execute-external-script-permission"></a>Autorizzazione EXECUTE EXTERNAL SCRIPT

È possibile usare le autorizzazioni EXECUTE EXTERNAL SCRIPT in modo da concedere l'esecuzione dello script esterno in linguaggi specifici. Questa autorizzazione è diversa dall'autorizzazione di database EXECUTE ANY EXTERNAL SCRIPT, che non concede l'autorizzazione di esecuzione in un linguaggio specifico.

In questo modo gli utenti non-**dbo** devono avere l'autorizzazione per eseguire un linguaggio specifico:

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::language_name 
TO database_principal_name;
```

### <a name="reference-permissions-to-external-libraries"></a>Autorizzazioni di riferimento a librerie esterne

Analogamente agli assembly, sono necessarie autorizzazioni di riferimento per i linguaggi esterni, in modo da creare un collegamento tra le librerie esterne e i linguaggi esterni. Ad esempio, se un linguaggio esterno sarà eliminato, l'utente deve prima assicurarsi che tutte le librerie esterne che fanno riferimento a quel linguaggio siano eliminate. È ora possibile visualizzare il linguaggio esterno come oggetto di un livello superiore rispetto alle librerie esterne, all'interno di una gerarchia.

## <a name="examples"></a>Esempi

### <a name="a-create-an-external-language-in-a-database"></a>R. Creare un linguaggio esterno in un database  

L'esempio seguente aggiunge un linguaggio esterno denominato Java a un database in SQL Server in Windows.

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

### <a name="b-create-an-external-language-for-both-windows-and-linux"></a>B. Aggiungere un linguaggio esterno per Windows e Linux

È possibile specificare fino a due `<file_spec>`, uno per Windows e uno per Linux.

```sql
CREATE EXTERNAL LANGUAGE Java
FROM
(CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll', PLATFORM = WINDOWS),
(CONTENT = N'<path-to-tar.gz>', FILE_NAME = 'javaextension.so', PLATFORM = LINUX);
GO
```
### <a name="c-grant-permissions-to-execute-external-script"></a>C. Concedere le autorizzazioni per eseguire script esterni

Nell'esempio seguente viene concesso l'accesso all'entità di sicurezza **mylogin** per l'esecuzione di script usando il linguaggio esterno **Java**.

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::Java 
TO mylogin;
```


## <a name="see-also"></a>Vedere anche

[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)