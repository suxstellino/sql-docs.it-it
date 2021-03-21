---
description: Ottenere informazioni su una vista
title: Ottenere informazioni su una vista | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.viewproperties.general.f1
helpviewer_keywords:
- views [SQL Server], status information
- metadata [SQL Server], views
- dependencies [SQL Server], views
- displaying view information
- views [SQL Server], metadata
- viewing view information
- status information [SQL Server], views
- view dependencies
ms.assetid: 05a73e33-8f85-4fb6-80c1-1b659e753403
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b969656bdb0ccaeebf1d800f88e6c1989310ceac
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734919"
---
# <a name="get-information-about-a-view"></a>Ottenere informazioni su una vista
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] è possibile acquisire informazioni sulla definizione o sulle proprietà di una vista in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Potrebbe essere necessario visualizzare la definizione della vista per determinare come vengono derivati i dati dalle tabelle di origine o per visualizzare i dati definiti dalla vista.  
  
> [!IMPORTANT]  
>  Se si cambia il nome di un oggetto a cui viene fatto riferimento da una vista, è necessario modificare la vista in modo che per il relativo testo venga fatto riferimento al nuovo nome. Prima di rinominare un oggetto, visualizzare innanzitutto le relative dipendenze per determinare se la modifica proposta interessa le viste.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per acquisire informazioni su una vista tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'utilizzo di `sp_helptext` per restituire la definizione di una vista richiede l'appartenenza al ruolo **pubblico** . L'utilizzo di `sys.sql_expression_dependencies` per individuare tutte le dipendenze in una vista richiede l'autorizzazione VIEW DEFINITION sul database e l'autorizzazione SELECT su `sys.sql_expression_dependencies` per il database. Le definizioni dell'oggetto di sistema, come quelle restituite in SELECT OBJECT_DEFINITION sono visibili pubblicamente.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="get-view-properties-by-using-object-explorer"></a>Acquisire le proprietà di visualizzazione tramite Esplora oggetti  
  
1.  In **Esplora oggetti** fare clic sul segno più accanto al database contenente la vista in cui si desidera visualizzare le proprietà, quindi fare di nuovo clic sul segno più per espandere la cartella **Viste** .  
  
2.  Fare clic con il pulsante destro del mouse sulla vista di cui si vogliono visualizzare le proprietà e scegliere **Proprietà**.  

     Le seguenti proprietà vengono visualizzate nella finestra di dialogo **Proprietà vista** .  
  
     **Database**  
     Nome del database contenente la vista.  
  
     **Server**  
     Nome dell'istanza del server corrente.  
  
     **Utente**  
     Nome dell'utente della connessione.  
  
     **Data creazione**  
     Indica la data di creazione della vista.  
  
     **Nome**  
     Nome della vista corrente.  
  
     **Schema**  
     Consente di visualizzare lo schema proprietario della vista.  
  
     **Oggetto di sistema**  
     Indica se la vista è un oggetto di sistema. I valori sono True e False.  
  
     **ANSI NULLs**  
     Indica se l'oggetto è stato creato con l'opzione ANSI NULLs.  
  
     **Crittografata**  
     Indica se la vista è crittografata. I valori sono True e False.  
  
     **Identificatore delimitato**  
     Indica se l'oggetto è stato creato con l'opzione quoted identifier.  
  
     **Associata a schema**  
     Indica se la vista è associata a uno schema. I valori sono True e False. Per informazioni sulle viste associate a schemi, vedere la sezione SCHEMABINDING di[CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
#### <a name="getting-view-properties-by-using-the-view-designer-tool"></a>Acquisizione di proprietà delle viste tramite lo strumento Progettazione viste  
  
1.  In **Esplora oggetti** espandere il database contenente la vista in cui si desidera visualizzare le proprietà, quindi espandere la cartella **Viste** .  
  
2.  Fare clic con il pulsante destro del mouse sulla vista di cui si vogliono visualizzare le proprietà e scegliere **Progettazione**.  
  
3.  Fare clic con il pulsante destro del mouse sullo spazio vuoto del riquadro Diagramma, quindi scegliere **Proprietà**.  
  
     Le seguenti proprietà vengono visualizzate nel riquadro **Proprietà** .  
  
     **(Nome)**  
     Nome della vista corrente.  
  
     **Nome database**  
     Nome del database contenente la vista.  
  
     **Descrizione**  
     Breve descrizione della vista corrente.  
  
     **Schema**  
     Consente di visualizzare lo schema proprietario della vista.  
  
     **Nome server**  
     Nome dell'istanza del server corrente.  
  
     **Associa a schema**  
     Con questa proprietà gli utenti non possono modificare in alcun modo gli oggetti sottostanti che contribuiscono a questa vista per evitare di invalidare la relativa definizione.  
  
     **Deterministico**  
     Indica se il tipo di dati della colonna selezionata può essere determinato con certezza  
  
     **Valori Distinct**  
     Viene specificato che tramite la query verranno esclusi i duplicati nella vista. Questa opzione risulta utile quando si utilizzano solo alcune colonne di una tabella e in queste colonne potrebbero essere contenuti valori duplicati. L'opzione è inoltre utile quando dalla creazione di un join di due o più tabelle vengono generate righe duplicate nel set di risultati. Selezionare questa opzione equivale a inserire la parola chiave DISTINCT nell'istruzione del riquadro SQL.  
  
     **Estensione GROUP BY**  
     Viene specificato che sono disponibili opzioni aggiuntive per le viste basate su query di aggregazione.  
  
     **Tutte le colonne**  
     Viene mostrato se tutte le colonne vengono restituite dalla vista selezionata. Questa proprietà viene impostata durante la creazione della vista.  
  
     **Commento SQL**  
     Visualizza una descrizione delle istruzioni SQL. Per visualizzare la descrizione completa o modificarla, fare clic su di essa e quindi sui puntini di sospensione **(...)** a destra della proprietà. Nei commenti è possibile specificare, ad esempio, quali utenti utilizzano la vista e quando.  
  
     **Specifica TOP**  
     Si espande per visualizzare le proprietà **Top**, **Espressione**, **Percentuale** e **With Ties**  
  
     **(In alto)**  
     Specifica che la vista includerà una clausola TOP che restituirà soltanto le prime n righe o il primo n% delle righe del set di risultati. Per impostazione predefinita, nella vista vengono restituite le prime 10 righe del set di risultati. Utilizzare questa proprietà per modificare il numero di righe restituito o specificare una percentuale diversa  
  
     **Espressione**  
     Visualizza la percentuale (se l'opzione **Percentuale** è impostata su **Sì**) o i record (se l'opzione **Percentuale** è impostata su **No**) restituiti dalla vista.  
  
     **Percentuale**  
     Specifica che la query includerà una clausola **TOP** che restituirà soltanto il primo n percento di righe del set di risultati  
  
     **Con valori equivalenti**  
     Specifica che la vista includerà una clausola **WITH TIES** . **WITH TIES** è utile se nella vista sono incluse anche una clausola **ORDER BY** e una **TOP** basate sulla percentuale. Se questa opzione è impostata e la percentuale limite specificata rientra in un set di righe con valori identici nella clausola **ORDER BY** , la vista verrà estesa fino a includere tutte queste righe.  
  
     **Specifica aggiornamento**  
     Si espande per visualizzare le proprietà **Aggiorna in base alle regole della vista** e **Opzione Check** .  
  
     **(Aggiorna in base alle regole della vista)**  
     Viene indicato che tutti gli aggiornamenti e gli inserimenti nella vista saranno convertiti da Microsoft Data Access Components (MDAC) in istruzioni SQL che fanno riferimento alla vista piuttosto che alle tabelle di base della vista.  
  
     In alcuni casi, in MDAC le operazioni di aggiornamento e inserimento nella vista vengono presentate come aggiornamenti e inserimenti nelle tabelle di base sottostanti della vista. Selezionando **Aggiorna in base alle regole della vista** è possibile assicurarsi che tramite MDAC le operazioni di aggiornamento e inserimento vengano generate nella vista stessa.  
  
     **Opzione Check**  
     Indica che quando si apre questa vista e si modifica il riquadro **Risultati** l'origine dati verifica se i dati aggiunti o modificati soddisfano la clausola **WHERE** della definizione della vista. Se la modifica non soddisfa la clausola **WHERE** , verrà visualizzato un errore con ulteriori informazioni.  
  
#### <a name="to-get-dependencies-on-the-view"></a>Per acquisire dipendenze dalla vista  
  
1.  In **Esplora oggetti** espandere il database contenente la vista in cui si desidera visualizzare le proprietà, quindi espandere la cartella **Viste** .  
  
2.  Fare clic con il pulsante destro del mouse sulla vista di cui si vogliono visualizzare le proprietà e selezionare **Visualizza dipendenze**.  
  
3.  Selezionare **Oggetti che dipendono da [nome vista]** per visualizzare gli oggetti che fanno riferimento alla vista.  
  
4.  Selezionare **Oggetti da cui dipende [nome vista]** per visualizzare gli oggetti a cui viene fatto riferimento dalla vista.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-view"></a>Per acquisire la definizione e le proprietà di una vista  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare uno degli esempi seguenti nella finestra della query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition, uses_ansi_nulls, uses_quoted_identifier, is_schema_bound  
    FROM sys.sql_modules  
    WHERE object_id = OBJECT_ID('HumanResources.vEmployee');   
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID('HumanResources.vEmployee')) AS ObjectDefinition;   
    GO  
    ```  
  
    ```  
    EXEC sp_helptext 'HumanResources.vEmployee';  
    ```  
  
 Per altre informazioni, vedere [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md), [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md) e [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md).  
  
#### <a name="to-get-the-dependencies-of-a-view"></a>Per acquisire le dipendenze di una vista  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
    GO  
    ```  
  
 Per altre informazioni, vedere [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) e [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
  
