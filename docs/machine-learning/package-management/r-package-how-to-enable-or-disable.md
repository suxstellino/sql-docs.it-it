---
title: Abilitare o disabilitare la gestione remota dei pacchetti R
description: Abilitare la gestione remota dei pacchetti R in SQL Server 2016 R Services o Machine Learning Services (In-Database) di SQL Server
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 728dc656d3fdb94abae0a5c2c0e2665af2c4807c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073922"
---
# <a name="enable-or-disable-remote-package-management-for-sql-server"></a>Abilitare o disabilitare la gestione remota dei pacchetti per SQL Server
[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Questo articolo descrive come abilitare la gestione remota dei pacchetti R da una workstation client o da un'istanza diversa di Machine Learning Server. Dopo aver abilitato la funzionalità di gestione dei pacchetti in SQL Server, è possibile usare i comandi RevoScaleR in un client per installare i pacchetti in SQL Server.

Per impostazione predefinita, la funzionalità di gestione esterna dei pacchetti per SQL Server è disabilitata. È necessario eseguire uno script separato per abilitare la funzionalità come descritto nella sezione successiva.

## <a name="overview-of-process-and-tools"></a>Panoramica del processo e degli strumenti

Per abilitare o disabilitare la gestione dei pacchetti in SQL Server, usare l'utilità della riga di comando **RegisterRExt.exe** inclusa nel pacchetto **RevoScaleR**.

L'[abilitazione](#bkmk_enable) di questa funzionalità è un processo che consiste di due passaggi e richiede un amministratore del database. La gestione dei pacchetti viene abilitata nell'istanza di SQL Server (una volta per ogni istanza di SQL Server) e poi nel database SQL (una volta per ogni database di SQL Server).

Anche per [disabilitare](#bkmk_disable) la funzionalità di gestione dei pacchetti sono necessari più passaggi. Si rimuovono le autorizzazioni e i pacchetti a livello di database (una volta per ogni database), poi si rimuovono i ruoli dal server (una volta per ogni istanza).

## <a name="enable-package-management"></a><a name="bkmk_enable"></a> Abilitare la gestione dei pacchetti

1. In SQL Server aprire un prompt dei comandi con privilegi elevati e passare alla cartella che contiene l'utilità RegisterRExt.exe. Il percorso predefinito è `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExe.exe`.

2. Eseguire il comando seguente, specificando gli argomenti appropriati per l'ambiente:

    `RegisterRExt.exe /install pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Questo comando crea oggetti a livello di istanza nel computer con SQL Server che sono necessari per la gestione dei pacchetti. Riavvia anche il servizio Launchpad per l'istanza.

    Se non si specifica un'istanza, viene usata l'istanza predefinita. Se non si specifica un utente, viene usato il contesto di sicurezza corrente. Il comando seguente, ad esempio, abilita la gestione dei pacchetti nell'istanza predefinita, usando le credenziali dell'utente che ha aperto il prompt dei comandi:

    `REgisterRExt.exe /install pkgmgmt`

3. Per aggiungere la gestione dei pacchetti a un database specifico, eseguire il comando seguente da un prompt dei comandi con privilegi elevati:

    `RegisterRExt.exe /install pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`
   
    Questo comando crea alcuni artefatti del database, tra cui i ruoli del database seguenti necessari per il controllo delle autorizzazioni utente: `rpkgs-users`, `rpkgs-private` e `rpkgs-shared`.

    Il comando seguente, ad esempio, abilita la gestione dei pacchetti nel database, nell'istanza predefinita. Se non si specifica un utente, viene usato il contesto di sicurezza corrente.

    `RegisterRExt.exe /install pkgmgmt /database:TestDB`

4. Ripetere il comando per ogni database in cui devono essere installati i pacchetti.

5. Per verificare che i nuovi ruoli siano stati creati, fare clic sul database in SQL Server Management Studio, espandere **Sicurezza**, quindi **Ruoli database**.

    È anche possibile eseguire una query su sys.database_principals, ad esempio la seguente:

    ```sql
    SELECT pr.principal_id, pr.name, pr.type_desc,   
        pr.authentication_type_desc, pe.state_desc,   
        pe.permission_name, s.name + '.' + o.name AS ObjectName  
    FROM sys.database_principals AS pr  
    JOIN sys.database_permissions AS pe  
        ON pe.grantee_principal_id = pr.principal_id  
    JOIN sys.objects AS o  
        ON pe.major_id = o.object_id  
    JOIN sys.schemas AS s  
        ON o.schema_id = s.schema_id;
    ```

Dopo aver abilitato questa funzionalità, è possibile usare la funzione RevoScaleR per installare o disinstallare i pacchetti da un client R remoto.

## <a name="disable-package-management"></a><a name="bkmk_disable"></a> Disabilitare la gestione dei pacchetti

1. Da un prompt dei comandi con privilegi elevati, eseguire di nuovo l'utilità RegisterRExt per disabilitare la gestione dei pacchetti a livello di database:

    `RegisterRExt.exe /uninstall pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`

    Questo comando rimuove gli oggetti di database correlati alla gestione dei pacchetti dal database specificato. Rimuove anche tutti i pacchetti installati dal percorso del file di sistema protetto nel computer con SQL Server.

2. Ripetere il comando in ogni database in cui è stata usata la gestione dei pacchetti.

3.  (Facoltativo) Dopo aver cancellato i pacchetti da tutti i database usando il passaggio precedente, eseguire il comando seguente da un prompt dei comandi con privilegi elevati:

    `RegisterRExt.exe /uninstall pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Questo comando rimuove la funzionalità di gestione dei pacchetti dall'istanza. Per visualizzare le modifiche, potrebbe essere necessario riavviare manualmente il servizio Launchpad.

## <a name="next-steps"></a>Passaggi successivi

+ [Usare RevoScaleR per installare i pacchetti R](install-r-packages-with-revoscaler.md)
+ [Ottenere informazioni sui pacchetti R](r-package-information.md)
+ [Suggerimenti per l'uso di pacchetti R](tips-for-using-r-packages.md)
