---
title: Concedere le autorizzazioni per l'esecuzione di script Python e R
description: Informazioni su come concedere agli utenti l'autorizzazione per l'esecuzione di script Python e R esterni in Machine Learning Services per SQL Server e le autorizzazioni di lettura, scrittura o DDL (Data Definition Language) per i database.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/19/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: bbfc55b4c0460d948d78d72e628033d195a1f027
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103718"
---
# <a name="grant-database-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Concedere agli utenti del database l'autorizzazione per l'esecuzione di script Python e R con SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Informazioni su come concedere a un [utente di database](../../relational-databases/security/authentication-access/create-a-database-user.md) l'autorizzazione per l'esecuzione di script Python e R esterni in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) e per concedere autorizzazioni di lettura, scrittura o Data Definition Language (DDL) ai database.

Per altre informazioni, vedere la sezione sulle autorizzazioni in [Panoramica della sicurezza per il framework di estendibilità](../../machine-learning/concepts/security.md#permissions).

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>Autorizzazione per l'esecuzione di script

Per ogni utente che esegue script Python o R con Machine Learning Services per SQL Server e che non è un amministratore, è necessario concedere l'autorizzazione per l'esecuzione di script esterni in ogni database in cui viene usato il linguaggio.

Per concedere l'autorizzazione a un [utente del database](../../relational-databases/security/authentication-access/create-a-database-user.md) per l'esecuzione di script esterni, eseguire lo script seguente:

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> Le autorizzazioni non sono specifiche del linguaggio di scripting supportato. In altre parole, non esistono livelli di autorizzazione separati per script R e script Python.

<a name="permissions-db"></a>

## <a name="grant-database-permissions"></a>GRANT-autorizzazioni per database

Mentre un utente del database esegue gli script, potrebbe essere necessario che l'utente del database legga i dati da altri database. L'utente del database potrebbe inoltre dover creare nuove tabelle per archiviare i risultati e scrivere dati nelle tabelle.

Per ogni account utente del database o account di accesso SQL che esegue script R o Python, assicurarsi che disponga delle autorizzazioni appropriate per il database specifico: 

+ `db_datareader` per la lettura dei dati.
+ `db_datawriter` per il salvataggio degli oggetti nel database.
+ `db_ddladmin` per la creazione di oggetti come stored procedure o tabelle contenenti dati sottoposti a training e serializzati.

L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente, ad esempio, concede all'account di accesso SQL *MySQLLogin* i diritti per l'esecuzione di query T-SQL nel database *ML_Samples*. Per eseguire questa istruzione, l'account di accesso SQL deve essere già presente nel contesto di sicurezza del server. Per altre informazioni, vedere [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle autorizzazioni incluse in ogni ruolo, vedere [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md).
