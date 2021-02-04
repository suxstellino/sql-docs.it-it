---
description: Opzioni (Esplora oggetti di SQL Server - pagina Generazione script)
title: Opzioni (Esplora oggetti di SQL Server - pagina Generazione script)
ms.custom: seo-lt-2019
ms.date: 08/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.ObjectExplorerScripting
- VS.ToolsOptionsPages.Sql_Server_Object_Explorer.ObjectExplorerScripting
ms.assetid: 6105aec9-1b72-4cb2-bd24-fc35f6d95240
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cbaf09a88e5856bfd5c8f36826d8894b947e3c0d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250500"
---
# <a name="options-sql-server-object-explorer---scripting-page"></a>Opzioni (Esplora oggetti di SQL Server - pagina Generazione script)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Usare questa pagina per impostare le opzioni di scripting valide per i comandi seguenti dei menu di scelta rapida degli oggetti in **Esplora oggetti**:  
  
-   Comandi **Modifica** per tabelle e viste utente.  
  
-   Comandi **Crea script<object> per** per oggetti creati dall'utente.  
  
-   Comandi **Modifica** per oggetti creati dall'utente.  
  
-   Questa pagina consente inoltre di impostare i valori predefiniti per le opzioni di creazione di script per **Generazione guidata script di SQL Server**.  
  
## <a name="remarks"></a>Osservazioni  
I comandi **Modifica** e **Cambia** potrebbero produrre risultati diversi rispetto al comando **Crea script <object> per** con la stessa impostazione dell'opzione. I comandi **Modifica** e **Cambia** sono infatti stati creati per modificare oggetti nel database corrente durante una sessione dell'editor di query. Il comando **Crea script<object> per** è invece stato creato per generare uno script affinché possa essere usato in seguito per la creazione di oggetti.  
  
## <a name="options"></a>Opzioni  
Specificare le opzioni di scripting selezionando le impostazioni desiderate tra quelle disponibili nell'elenco visualizzato a destra di ciascuna opzione.

> [!NOTE]
> Le impostazioni predefinite elencate sono valide solo per l'opzione **Genera script per intero database e tutti gli oggetti di database** e possono variare quando si usa l'opzione **Seleziona oggetti di database specifici**.
  
### <a name="general-scripting-options"></a>Opzioni generali di scripting  
**Delimita singole istruzioni**  
Consente di separare istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] singole utilizzando un separatore batch. Per modificare il separatore batch predefinito per l' **editor di query**, scegliere **Strumenti**/**Opzioni**/**Esecuzione query**/**SQL Server**/**Generale**/**Separatore batch**. Il valore predefinito è False. Per altre informazioni, vedere [GO (Transact-SQL)](../../t-sql/language-elements/sql-server-utilities-statements-go.md).  
  
**Includi intestazioni descrittive**  
Consente di aggiungere commenti descrittivi allo script dividendolo in sezioni per ogni oggetto. Il valore predefinito è true. Per altre informazioni, vedere [/ *...* / (Comment) (Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
**Includi abilitazione compressione VarDecimal**  
Consente di includere le opzioni per l'archiviazione vardecimal. Il valore predefinito è False. Per altre informazioni, vedere [sp_db_vardecimal_storage_format (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
**Genera script per il rilevamento modifiche**  
Consente di includere nello script le informazioni sul rilevamento delle modifiche.  
  
**Script per cataloghi full-text**  
Consente di includere uno script per cataloghi full-text. Il valore predefinito è False. Per altre informazioni, vedere [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md).  
  
**Script per USE <database>**  
Aggiunge l'istruzione USE DATABASE allo script per creare oggetti di database nel contesto del database di **Esplora oggetti** corrente. Se si prevede di utilizzare lo script in un database diverso, selezionare False per omettere tale istruzione. Il valore predefinito è true. Per altre informazioni, vedere [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md).  
  
### <a name="object-scripting-options"></a>Opzioni di scripting per gli oggetti  

**Verifica esistenza oggetto**: verifica che un oggetto con il nome specificato esista prima dell'eliminazione o della modifica oppure che un oggetto con il nome specificato non esista prima della creazione. Per altre informazioni, vedere [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md) e [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md).

**Genera script per oggetti dipendenti**  
Consente di generare uno script per oggetti aggiuntivi, richiesti quando viene eseguito lo script per l'oggetto selezionato. Il valore predefinito è False.  
  
**Schema per qualifica dei nomi degli oggetti**  
Consente di qualificare il nome degli oggetti con lo schema dell'oggetto. Il valore predefinito è False. Per altre informazioni, vedere [Creazione di uno schema di database](../../relational-databases/security/authentication-access/create-a-database-schema.md).  

**Genera script per le opzioni di compressione dati**: include opzioni di compressione dati nello script. Il valore predefinito è False.

**Script per proprietà estese**  
Consente di includere le proprietà estese nello script qualora l'oggetto disponga di proprietà estese. Il valore predefinito è False. Per altre informazioni, vedere [sp_addextendedproperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md).  
  
**Proprietario script**  
Consente di includere il proprietario nello script generato. Il valore predefinito è False.  
  
**Script per autorizzazioni**  
Consente di includere autorizzazioni sugli oggetti di database nello script. Il valore predefinito è true. Per altre informazioni, vedere [Autorizzazioni](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="tableview-options"></a>Opzioni tabella/vista  
Le opzioni seguenti si applicano solo agli script per tabelle o viste.  
  
**Converti tipi di dati definiti dall'utente in tipi di base**  
Consente di convertire i tipi di dati definiti dall'utente nei tipi di base da cui sono stati creati. Utilizzare True se il tipo di dati definito dall'utente nel database di origine non è presente nel database in cui lo script verrà eseguito. Utilizzare False per mantenere i tipi di dati definiti dall'utente. Il valore predefinito è False. Per altre informazioni, vedere [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md).  
  
**Genera comandi SET ANSI PADDING**  
Consente di aggiungere l'istruzione SET ANSI_PADDING prima e dopo ogni istruzione CREATE TABLE. Il valore predefinito è true. Per altre informazioni, vedere [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
**Includi regole di confronto**  
Consente di includere le regole di confronto nella definizione della colonna. Il valore predefinito è true. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
**Includi proprietà IDENTITY**  
Consente di includere definizioni per il valore di inizializzazione di IDENTITY e l'incremento di IDENTITY. Il valore predefinito è true. Per altre informazioni, vedere [IDENTITY (Property) (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
**Schema per qualifica dei riferimenti alle chiavi esterne**  
Consente di aggiungere il nome dello schema ai riferimenti alle tabelle per i vincoli FOREIGN KEY. Il valore predefinito è true.  
  
**Script per associazione di valori predefiniti e regole**  
Includere le chiamate alle stored procedure di associazione **sp_bindefault** e **sp_bindrule** . Il valore predefinito è true. Per altre informazioni, vedere [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md) e [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md).  
  
**Script per vincoli CHECK**  
Aggiunge [vincoli CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) allo script. Il valore predefinito è true.  
  
**Script per valori predefiniti**  
Consente di includere i valori predefiniti delle colonne nello script. Il valore predefinito è False. Per altre informazioni, vedere [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md).  
  
**Script per filegroup**  
Consente di specificare il filegroup nella clausola ON per le definizioni di tabella. Il valore predefinito è False. Per altre informazioni, vedere [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
**Script per chiavi esterne**  
Include [vincoli FOREIGN KEY](../../relational-databases/tables/primary-and-foreign-key-constraints.md) nello script. Il valore predefinito è False.  
  
**Script per indici full-text**  
Consente di includere indici full-text nello script. Il valore predefinito è False. Per altre informazioni, vedere [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
**Script per indici**  
Consente di includere indici cluster, non cluster e XML nello script. Il valore predefinito è true. Per altre informazioni, vedere [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
**Script per schemi di partizione**  
Consente di includere schemi di partizione di tabelle nello script. Il valore predefinito è False. Per altre informazioni, vedere [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
  
**Script per chiavi primarie**  
Include [vincoli primari e FOREIGN KEY](../../relational-databases/tables/primary-and-foreign-key-constraints.md) nello script. Il valore predefinito è true.  
  
**Script per statistiche**  
Consente di includere statistiche definite dall'utente nello script. Il valore predefinito è False. Per altre informazioni, vedere [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md).  
  
**Script per trigger**  
Consente di includere trigger nello script. Il valore predefinito è False. Per altre informazioni, vedere [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
**Script per chiavi univoche**  
Include [vincoli UNIQUE e CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) nello script. Il valore predefinito è False.  
  
**Script per colonne vista**  
Consente di dichiarare colonne di viste in intestazioni di viste. Il valore predefinito è False. Per altre informazioni, vedere [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
**Includi nomi di sistema DRI**  
Consente di includere nomi di vincoli generati dal sistema per applicare l'integrità referenziale dichiarativa. Il valore predefinito è False. Per altre informazioni, vedere [REFERENTIAL_CONSTRAINTS (Transact-SQL)](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md).  
  
### <a name="version-options"></a>Opzioni di versione

**Corrispondenza delle impostazioni dello script con l'origine**: se abilitata, la versione di destinazione, l'edizione e il tipo del motore degli script generati saranno impostati sui valori del server da cui viene eseguito l'inserimento dell'oggetto nello script. Vengono così disabilitate e ignorate le altre opzioni di versione. 

**Script per l'edizione del motore di database**: gli script generati saranno destinati all'[edizione del motore](/dotnet/api/microsoft.sqlserver.management.smo.edition) specificata.

**Script per il tipo di motore di database**: gli script generati saranno destinati al [tipo di motore di database](/previous-versions/sql/sql-server-2014/ee642509(v=sql.120)) specificato.

**Script per versione server**  
Gli script saranno destinati alla versione specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è possibile creare script per versioni precedenti per le nuove funzionalità di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Alcuni script creati per [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] non possono essere eseguiti in server con una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o in un database con un' [impostazione del livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)precedente.  

## <a name="see-also"></a>Vedere anche  
[Generazione di script (SQL Server Management Studio)](../scripting/generate-scripts-sql-server-management-studio.md)  
