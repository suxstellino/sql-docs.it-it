---
title: Configurazione della visibilità dei metadati | Microsoft Docs
description: Informazioni su come configurare la visibilità dei metadati per le entità a protezione diretta di cui l'utente è proprietario o per le quali dispone dell'autorizzazione in SQL Server.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- subcomponents visibility [SQL Server]
- metadata [SQL Server], visibility
- permissions [SQL Server], metadata access
- viewing metadata
- objects [SQL Server], metadata
- displaying metadata
- database metadata [SQL Server]
- metadata [SQL Server], permissions
ms.assetid: 50d2e015-05ae-4014-a1cd-4de7866ad651
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65eeb429fbef85f1e23a79a8f06299a64a5179d2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755861"
---
# <a name="metadata-visibility-configuration"></a>Configurazione della visibilità dei metadati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La visibilità dei metadati è limitata alle entità a protezione diretta di cui l'utente è proprietario o per le quali dispone di autorizzazioni. Ad esempio, la query seguente restituisce una riga se all'utente è stata concessa un'autorizzazione SELECT o INSERT per la tabella `myTable`.  
  
```  
SELECT name, object_id  
FROM sys.tables  
WHERE name = N'myTable';  
GO  
```  
  
 Se invece l'utente non dispone di alcuna autorizzazione per `myTable`, la query restituisce un set di risultati vuoto.  
  
## <a name="scope-and-impact-of-metadata-visibility-configuration"></a>Ambito e impatto della configurazione della visibilità dei metadati  
 È possibile configurare la visibilità dei metadati unicamente per le entità a protezione diretta seguenti:  

:::row:::
    :::column:::
        Viste del catalogo

        Metadati che espongono funzioni predefinite

        Viste di compatibilità
    :::column-end:::
    :::column:::
         Stored procedure **sp_help** del [!INCLUDE[ssDE](../../includes/ssde-md.md)]

        Viste degli schemi delle informazioni

        Proprietà estese
    :::column-end:::
:::row-end:::
  
 Non è possibile configurare la visibilità dei metadati per le entità a protezione diretta seguenti:  

:::row:::
    :::column:::
        Tabelle di sistema per il log shipping

        Tabelle di sistema del piano di manutenzione database

        Tabelle di sistema di replica
    :::column-end:::
    :::column:::
         [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Tabelle di sistema dell'agente

        Tabelle di sistema di backup

        Replica e stored procedure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_help **dell'agente**
    :::column-end:::
:::row-end:::

 Un'accessibilità limitata ai metadati comporta quanto segue:  
  
-   Le applicazioni per cui è impostato l'accesso **pubblico** ai metadati verranno interrotte.  
  
-   È possibile che le query eseguite su viste di sistema restituiscano solo un subset di righe o a volte un set di risultati vuoto.  
  
-   È possibile che le funzioni predefinite per la creazione di metadati quali OBJECTPROPERTYEX restituiscano NULL.  
  
-   È possibile che le stored procedure **sp_help** del [!INCLUDE[ssDE](../../includes/ssde-md.md)] restituiscano solo un subset di righe o NULL.  
  
 I moduli SQL, ad esempio le stored procedure e i trigger, vengono eseguiti nel contesto di sicurezza del chiamante e pertanto la relativa accessibilità ai metadati è limitata. Nel codice di esempio seguente, quando la stored procedure tenta di accedere ai metadati relativi alla tabella `myTable` per la quale il chiamante non dispone di alcun diritto, viene restituito un set di risultati vuoto. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], viene invece restituita una riga.  
  
```  
CREATE PROCEDURE assumes_caller_can_access_metadata  
BEGIN  
SELECT name, object_id   
FROM sys.objects   
WHERE name = N'myTable';  
END;  
GO  
```  
  
 Per consentire ai chiamanti di visualizzare i metadati, è possibile concedere loro l'autorizzazione VIEW DEFINITION in un ambito appropriato, che può essere a livello di oggetto, di database o di server. Nell'esempio precedente, se il chiamante dispone dell'autorizzazione VIEW DEFINITION per `myTable`, la stored procedure restituisce pertanto una riga. Per altre informazioni, vedere [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md) e [Autorizzazioni di database GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
 È inoltre possibile modificare la stored procedure in modo che venga eseguita con le credenziali del proprietario. Se il proprietario della stored procedure è anche proprietario della tabella, verrà applicata la concatenazione della proprietà e il contesto di sicurezza del proprietario della procedura consentirà di accedere ai metadati relativi a `myTable`. In questo scenario, il codice seguente restituisce una riga di metadati al chiamante.  
  
> [!NOTE]  
>  Nell'esempio seguente viene usata la vista del catalogo [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) anziché la vista di compatibilità [sys.sysobjects](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md) .  
  
```  
CREATE PROCEDURE does_not_assume_caller_can_access_metadata  
WITH EXECUTE AS OWNER  
AS  
BEGIN  
SELECT name, object_id  
FROM sys.objects   
WHERE name = N'myTable'   
END;  
GO  
```  
  
> [!NOTE]  
>  Per passare temporaneamente al contesto di sicurezza del chiamante, è possibile utilizzare EXECUTE AS. Per altre informazioni, vedere [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md).  
  
## <a name="benefits-and-limits-of-metadata-visibility-configuration"></a>Vantaggi e limiti della configurazione della visibilità dei metadati  
 La configurazione della visibilità dei metadati svolge un ruolo importante per il piano di sicurezza globale. In alcuni casi, tuttavia, un utente esperto potrebbe essere in grado di forzare la diffusione di alcuni metadati. È pertanto consigliabile prevedere un ulteriore livello di protezione tramite l'implementazione di autorizzazioni per i metadati.  
  
 In teoria, è possibile forzare la creazione di metadati nei messaggi di errore modificando l'ordine di valutazione del predicato nelle query. La possibilità di *attacchi trial-and-error* di questo tipo non è specifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ma è implicita nelle trasformazioni associative e commutative consentite nell'algebra relazionale. È possibile ridurre questo rischio limitando le informazioni restituite nei messaggi di errore. Per limitare ulteriormente la visibilità dei metadati in questo modo, è possibile avviare il server con il flag di traccia 3625, che limita la quantità di informazioni visualizzate nei messaggi di errore. Questa soluzione contribuisce a limitare la diffusione forzata di informazioni, ma è controbilanciata dal fatto che i messaggi di errore saranno concisi e potrebbero essere difficili da utilizzare a scopi di debug. Per altre informazioni, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md) e [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
 I metadati seguenti non sono soggetti alla diffusione forzata:  
  
-   Valore archiviato nella colonna **provider_string** di **sys.servers**. Un utente che non dispone dell'autorizzazione ALTER ANY LINKED SERVER sarà in grado di visualizzare unicamente un valore NULL in questa colonna.  
  
-   La definizione dell'origine di un oggetto definito dall'utente, ad esempio una stored procedure o un trigger. Il codice sorgente è visibile solo se è vera una delle condizioni seguenti:  
  
    -   L'utente dispone dell'autorizzazione VIEW DEFINITION per l'oggetto.  
  
    -   All'utente non è stata negata l'autorizzazione VIEW DEFINITION per l'oggetto ed è stata concessa l'autorizzazione CONTROL, ALTER o TAKE OWNERSHIP per l'oggetto. Per tutti gli altri utenti verranno visualizzati valori NULL.  
  
-   Le colonne di definizione delle viste del catalogo seguenti:  

    - **sys.all_sql_modules**  
    - **sys.server_sql_modules**  
    - **sys.default_constraints**
    - **sys.numbered_procedures**
    - **sys.sql_modules**
    - **sys.check_constraints**
    - **sys.computed_columns**

-   Colonna **ctext** nella vista di compatibilità **syscomments** .  
  
-   Output della procedura **sp_helptext** .  
  
-   Le colonne seguenti nelle viste degli schemi delle informazioni:

    - INFORMATION_SCHEMA.CHECK_CONSTRAINTS.CHECK_CLAUSE
    - INFORMATION_SCHEMA.DOMAINS.DOMAIN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINES.ROUTINE_DEFINITION
    - INFORMATION_SCHEMA.COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINE_COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.VIEWS.VIEW_DEFINITION

-   La funzione OBJECT_DEFINITION()  
  
-   Valore archiviato nella colonna password_hash di **sys.sql_logins**.  Un utente che non dispone dell'autorizzazione CONTROL SERVER sarà in grado di visualizzare unicamente un valore NULL in questa colonna.  
  
> [!NOTE]  
>  Le definizioni SQL delle procedure e delle funzioni di sistema predefinite sono visibili a livello pubblico tramite la vista del catalogo **sys.system_sql_modules** , la stored procedure **sp_helptext** e la funzione OBJECT_DEFINITION().  
  
## <a name="general-principles-of-metadata-visibility"></a>Principi generali della visibilità dei metadati  
 Di seguito sono illustrati alcuni principi generali da tenere in considerazione per la visibilità dei metadati:  
  
-   Autorizzazioni implicite dei ruoli predefiniti  
  
-   Ambito delle autorizzazioni  
  
-   Precedenza di DENY  
  
-   Visibilità dei metadati dei sottocomponenti  
  
### <a name="fixed-roles-and-implicit-permissions"></a>Autorizzazioni implicite dei ruoli predefiniti  
 I metadati accessibili ai ruoli predefiniti variano in base alle autorizzazioni implicite corrispondenti.  
  
### <a name="scope-of-permissions"></a>Ambito delle autorizzazioni  
 Le autorizzazioni valide in un ambito implicano la possibilità di visualizzare i metadati dell'ambito specifico e di tutti gli ambiti dipendenti. Ad esempio, l'autorizzazione SELECT per uno schema implica che il beneficiario dispone dell'autorizzazione SELECT per tutte le entità a protezione diretta contenute nello schema specifico. Se si concede a un utente l'autorizzazione SELECT per uno schema, l'utente potrà pertanto visualizzare i metadati dello schema e tutte le tabelle, le viste, le funzioni, le procedure, le code nonché i sinonimi, i tipi e le raccolte XML Schema in esso contenuti. Per altre informazioni, vedere [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
### <a name="precedence-of-deny"></a>Precedenza di DENY  
 DENY ha in genere la precedenza sulle altre autorizzazioni. Ad esempio, se a un utente del database viene concessa l'autorizzazione EXECUTE per uno schema, ma gli viene negata l'autorizzazione EXECUTE per una stored procedure dello schema specifico, l'utente non potrà visualizzare i metadati relativi alla stored procedure.  
  
 Inoltre, se a un utente viene negata l'autorizzazione EXECUTE per uno schema ma viene concessa l'autorizzazione EXECUTE per una stored procedure dello schema specifico, l'utente non potrà visualizzare i metadati relativi alla stored procedure.  
  
 Infine, se a un utente viene concessa e negata l'autorizzazione EXECUTE per una stored procedure, cosa che può accadere nel caso di appartenenza a diversi ruoli, DENY ha la precedenza e l'utente non potrà visualizzare i metadati della stored procedure.  
  
### <a name="visibility-of-subcomponent-metadata"></a>Visibilità dei metadati dei sottocomponenti  
 La visibilità dei sottocomponenti, quali gli indici, i vincoli CHECK e i trigger, è determinata dalle autorizzazioni per il componente padre. A questi sottocomponenti non è possibile concedere autorizzazioni. Ad esempio, se a un utente sono state concesse autorizzazioni per una tabella, l'utente potrà visualizzare i metadati relativi a tabelle, colonne, indici, vincoli CHECK, trigger e altri sottocomponenti.  
  
#### <a name="metadata-that-is-accessible-to-all-database-users"></a>Metadati accessibili a tutti gli utenti del database  
 Alcuni metadati devono essere accessibili a tutti gli utenti di un database specifico. Ad esempio, non è possibile concedere autorizzazioni per i filegroup e pertanto non è possibile concedere a un utente l'autorizzazione per la visualizzazione dei metadati di un filegroup. Tutti gli utenti che possono creare una tabella devono tuttavia essere in grado di accedere ai metadati dei filegroup per poter usare le clausole *filegroup* ON o *filegroup* TEXTIMAGE_ON dell'istruzione CREATE TABLE.  
  
 I metadati restituiti dalle funzioni DB_ID() e DB_NAME() sono visibili a tutti gli utenti.  
  
 Si tratta di un elenco delle viste del catalogo visibili per il ruolo **pubblico**.  

:::row:::
    :::column:::
        **sys.partition_functions**

        **sys.partition_schemes**

        **sys.filegroups**

        **sys.database_files**

        **sys.partitions**

        **sys.schemas**

        **sys.sql_dependencies**

        **sys.parameter_type_usages**
    :::column-end:::
    :::column:::
        **sys.partition_range_values**

        **sys.data_spaces**

        **sys.destination_data_spaces**

        **sys.allocation_units**

        **sys.messages**

        **sys.configurations**

        **sys.type_assembly_usages**

        **sys.column_type_usages**
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
