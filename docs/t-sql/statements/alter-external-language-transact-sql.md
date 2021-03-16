---
description: ALTER EXTERNAL LANGUAGE (Transact-SQL) - SQL Server
title: ALTER EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2021
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a86741824e47c51c6a24c39e6dfc5437d12bca45
ms.sourcegitcommit: e2d25f265556af92afcc0acde662929e654bf841
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103489534"
---
# <a name="alter-external-language-transact-sql"></a>ALTER EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Modifica il contenuto in un'estensione di linguaggio esterno esistente nel database.

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]
{
    SET <file_spec>
    | ADD <file_spec>
    | REMOVE PLATFORM <platform> 
}
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = {<external_lang_specifier> | <content_bits>,
    FILE_NAME = <external_lang_file_name>
    [, PLATFORM = <platform> ]
    [, PARAMETERS = <external_lang_parameters> ]
    [, ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
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

< external_lang_parameters > :: =  
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

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni

**PARAMETERS** e **ENVIRONMENT_VARIABLES** non sono attualmente supportati.

## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione `ALTER ANY EXTERNAL LANGUAGE`. Per impostazione predefinita, ogni utente con **dbo** che è membro del ruolo **db_owner** ha le autorizzazioni per modificare un linguaggio esterno. Per tutti gli altri utenti, è necessario concedere in modo esplicito l'autorizzazione usando un'istruzione [GRANT](./grant-database-permissions-transact-sql.md), specificando ALTER ANY EXTERNAL LANGUAGE come privilegio.

## <a name="examples"></a>Esempi

### <a name="alter-an-external-language-in-a-database"></a>Modificare un linguaggio esterno in un database  

L'esempio seguente aggiunge un linguaggio esterno denominato Java a un database in SQL Server in Windows.

```sql
ALTER EXTERNAL LANGUAGE Java 
SET (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

## <a name="see-also"></a>Vedere anche

[CREATE EXTERNAL LANGUAGE (Transact-SQL)](create-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)
