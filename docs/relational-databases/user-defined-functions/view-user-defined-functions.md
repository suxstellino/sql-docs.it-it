---
description: Visualizzare le funzioni definite dall'utente
title: Visualizzare funzioni definite dall'utente | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.udfproperties.general.f1
- sql13.swb.functionproperties.general.f1
helpviewer_keywords:
- displaying user-defined functions
- viewing user-defined functions
- user-defined functions [SQL Server], viewing
- status information [SQL Server], user-defined functions
ms.assetid: a45dfab5-6384-4311-b935-2e23a70c5c10
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ae8eb805b886edbe9c470ee3b10d01679b2f2362
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210714"
---
# <a name="view-user-defined-functions"></a>Visualizzare le funzioni definite dall'utente
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile acquisire informazioni sulla definizione o le proprietà di una funzione definita dall'utente in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Potrebbe essere necessario visualizzare la definizione della funzione per determinare come vengono derivati i dati dalle tabelle di origine o per visualizzare i dati definiti dalla funzione.  
  
> [!IMPORTANT]  
>  Se si cambia il nome di un oggetto a cui viene fatto riferimento da una funzione, è necessario modificare la funzione in modo che per il relativo testo venga fatto riferimento al nuovo nome. Pertanto, prima di rinominare un oggetto, visualizzare le dipendenze dell'oggetto per determinare se la modifica proposta interessa eventuali funzioni.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per acquisire informazioni su una funzione tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'uso di **sys.sql_expression_dependencies** per trovare tutte le dipendenze da una funzione richiede l'autorizzazione VIEW DEFINITION per il database e l'autorizzazione SELECT per **sys.sql_expression_dependencies** per il database. Le definizioni dell'oggetto di sistema, come quelle restituite in OBJECT_DEFINITION sono visibili pubblicamente.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-show-a-user-defined-functions-properties"></a>Per mostrare le proprietà di una funzione definita dall'utente  
  
1.  In **Esplora oggetti** fare clic sul segno più accanto al database contenente la funzione in cui si desidera visualizzare le proprietà, quindi fare di nuovo clic sul segno più per espandere la cartella **Programmabilità** .  
  
2.  Fare clic sul segno più per espandere la cartella **Funzioni** .  
  
3.  Fare clic sul segno più per espandere la cartella che contiene la funzione di cui si desidera visualizzare le proprietà:  
  
    -   Table-valued Function  
  
    -   Funzione a valori scalari  
  
    -   Funzione di aggregazione  
  
4.  Fare clic con il pulsante destro del mouse sulla funzione di cui si vogliono visualizzare le proprietà e scegliere **Proprietà**.  

     Le proprietà seguenti vengono visualizzate nella finestra di dialogo **Proprietà funzione -** _nome_funzione_.  
  
     **Database**  
     Nome del database che contiene la funzione.  
  
     **Server**  
     Nome dell'istanza del server corrente.  
  
     **Utente**  
     Nome dell'utente della connessione.  
  
     **Data creazione**  
     Visualizza la data di creazione della funzione.  
  
     **Esegui come**  
     Contesto di esecuzione per la funzione.  
  
     **Nome**  
     Nome della funzione corrente.  
  
     **Schema**  
     Visualizza lo schema proprietario della funzione.  
  
     **Oggetto di sistema**  
     Indica se la funzione è un oggetto di sistema. I valori sono True e False.  
  
     **ANSI NULLs**  
     Indica se l'oggetto è stato creato con l'opzione ANSI NULLs.  
  
     **Crittografata**  
     Indica se la funzione è crittografata. I valori sono True e False.  
  
     **Tipo di funzione**  
     Tipo della funzione definita dall'utente.  
  
     **Identificatore delimitato**  
     Indica se l'oggetto è stato creato con l'opzione quoted identifier.  
  
     **Associata a schema**  
     Indica se la funzione è associata allo schema. I valori sono True e False. Per informazioni sulle funzioni associate a schema, vedere la sezione relativa a SCHEMABINDING in [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-function"></a>Per acquisire la definizione e le proprietà di una funzione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare uno degli esempi seguenti nella finestra della query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the function name, definition, and relevant properties  
    SELECT sm.object_id,   
       OBJECT_NAME(sm.object_id) AS object_name,   
       o.type,   
       o.type_desc,   
       sm.definition,  
       sm.uses_ansi_nulls,  
       sm.uses_quoted_identifier,  
       sm.is_schema_bound,  
       sm.execute_as_principal_id  
    -- using the two system tables sys.sql_modules and sys.objects  
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id  
    -- from the function 'dbo.ufnGetProductDealerPrice'  
    WHERE sm.object_id = OBJECT_ID('dbo.ufnGetProductDealerPrice')  
    ORDER BY o.type;  
    GO  
  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the definition of the function dbo.ufnGetProductDealerPrice  
    SELECT OBJECT_DEFINITION (OBJECT_ID('dbo.ufnGetProductDealerPrice')) AS ObjectDefinition;  
    GO  
    ```  
  
 Per altre informazioni, vedere [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) e [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md).  
  
#### <a name="to-get-the-dependencies-of-a-function"></a>Per acquisire le dipendenze di una funzione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get all of the dependency information  
    SELECT OBJECT_NAME(sed.referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(sed.referencing_id, sed.referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        sed.referencing_class_desc, sed.referenced_class_desc,  
        sed.referenced_server_name, sed.referenced_database_name, sed.referenced_schema_name,  
        sed.referenced_entity_name,   
        COALESCE(COL_NAME(sed.referenced_id, sed.referenced_minor_id), '(n/a)') AS referenced_column_name,  
        sed.is_caller_dependent, sed.is_ambiguous  
    -- from the two system tables sys.sql_expression_dependencies and sys.object  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    -- on the function dbo.ufnGetProductDealerPrice  
    WHERE sed.referencing_id = OBJECT_ID('dbo.ufnGetProductDealerPrice');  
    GO  
    ```  
  
 Per altre informazioni, vedere [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) e [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
  
