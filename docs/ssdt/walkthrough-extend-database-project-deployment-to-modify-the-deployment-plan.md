---
title: Estendere la distribuzione del progetto di database per modificare il piano di distribuzione
description: Creare un collaboratore alla distribuzione di tipo DeploymentPlanModifier che programma i batch dello script di distribuzione da eseguire di nuovo se si verificano errori durante l'esecuzione.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 22b077b1-fa25-49ff-94f6-6d0d196d870a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bdb9831caf2145b43340fdca9af3108ba20d3c34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064956"
---
# <a name="walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan"></a>Procedura dettagliata: Estendere la distribuzione del progetto di database per modificare il piano di distribuzione

È possibile creare collaboratori alla distribuzione per eseguire azioni personalizzate quando si distribuisce un progetto SQL. È possibile creare un elemento [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) o [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). Usare [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) per modificare il piano prima di eseguirlo e [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) per eseguire operazioni mentre il piano è in esecuzione. In questa procedura dettaglia, si crea un elemento [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) denominato SqlRestartableScriptContributor che aggiunge istruzioni IF ai batch nello script di distribuzione per consentire di eseguire nuovamente lo script finché non sono completi se si verifica un errore durante l'esecuzione.  
  
In questa procedura dettagliata, vengono eseguite le attività principali seguenti:  
  
-   [Creare il tipo DeploymentPlanModifier del collaboratore alla distribuzione](#CreateDeploymentContributor)  
  
-   [Installare il collaboratore alla distribuzione](#InstallDeploymentContributor)  
  
-   [Eseguire o testare il collaboratore alla distribuzione](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>Prerequisiti  
Per completare questa procedura dettagliata, è necessario disporre dei componenti seguenti:  
  
-   È necessario avere installata una versione di Visual Studio che includa SQL Server Data Tools e supporti lo sviluppo in C# o VB.  
  
-   È necessario disporre di un progetto SQL contenente oggetti SQL.  
  
-   Un'istanza di SQL Server a cui sia possibile distribuire un progetto di database.  
  
> [!NOTE]  
> Questa procedura dettagliata è destinata ad utenti che abbiano già familiarità con le funzionalità SQL di SQL Server Data Tools. È inoltre necessario che l'utente conosca i concetti di base di Visual Studio, ad esempio come creare una libreria di classi e come utilizzare l'editor di codice per aggiungere codice a una classe.  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>Creare un collaboratore alla distribuzione  
Per creare un collaboratore alla distribuzione, è necessario effettuare le attività seguenti:  
  
-   Creare un progetto Libreria di classi e aggiungere i riferimenti richiesti.  
  
-   Definire una classe denominata SqlRestartableScriptContributor che eredita da [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier).  
  
-   Eseguire l'override del metodo [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute).  
  
-   Aggiungere metodi helper privati.  
  
-   Compilare l'assembly risultante.  
  
#### <a name="to-create-a-class-library-project"></a>Per creare un progetto Libreria di classi  
  
1.  Creare un progetto Libreria di classi di Visual C# o Visual Basic denominato MyOtherDeploymentContributor.  
  
2.  Rinominare il file "Class1.cs" in "SqlRestartableScriptContributor.cs".  
  
3.  In Esplora soluzioni fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Aggiungi riferimento**.  
  
4.  Selezionare **System.ComponentModel.Composition** nella scheda Framework.  
  
5.  Fare clic su **Sfoglia** e passare alla directory **C:\Programmi (x86)\Microsoft SQL Server\110\SDK\Assemblies**, selezionare **Microsoft.SqlServer.TransactSql.ScriptDom.dll** e quindi fare clic su **OK**.  
  
6.  Aggiungere i riferimenti SQL richiesti: fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Aggiungi riferimento**. Fare clic su **Sfoglia** e passare alla cartella **C:\Programmi (x86)\Microsoft SQL Server\110\DAC\Bin**. Scegliere le voci **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** e **Microsoft.Data.Tools.Schema.Sql.dll**, fare clic su **Aggiungi** e quindi fare clic su **OK**.  
  
Iniziare ad aggiungere codice alla classe.  
  
#### <a name="to-define-the-sqlrestartablescriptcontributor-class"></a>Per definire la classe SqlRestartableScriptContributor  
  
1.  Nell'editor del codice aggiornare il file class1.cs in modo che corrisponda alle istruzioni **using** seguenti:  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;  
    using System.Text;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    ```  
  
2.  Aggiornare la definizione della classe in modo che corrisponda all'esempio seguente:  
  
    ```csharp  
        /// <summary>  
    /// This deployment contributor modifies a deployment plan by adding if statements  
    /// to the existing batches in order to make a deployment script able to be rerun to completion  
    /// if an error is encountered during execution  
    /// </summary>  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
    public class SqlRestartableScriptContributor : DeploymentPlanModifier  
    {  
    }  
  
    ```  
  
    A questo punto si è definito il collaboratore alla distribuzione che eredita da [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier). Durante i processi di compilazione e distribuzione, i collaboratori personalizzati vengono caricati da una directory di estensioni standard. I collaboratori alla modifica del piano di distribuzione sono identificati da un attributo [ExportDeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanmodifierattribute). Questo attributo è richiesto per consentire di individuare i collaboratori. Dovrebbe essere simile al seguente:  
  
    ```csharp  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
  
    ```  
  
3.  Aggiungere le dichiarazioni di membro seguenti:  
  
    ```vb  
         private const string BatchIdColumnName = "BatchId";  
            private const string DescriptionColumnName = "Description";  
  
            private const string CompletedBatchesVariableName = "CompletedBatches";  
            private const string CompletedBatchesVariable = "$(CompletedBatches)";  
            private const string CompletedBatchesSqlCmd = @":setvar " + CompletedBatchesVariableName + " __completedBatches_{0}_{1}";  
            private const string TotalBatchCountSqlCmd = @":setvar TotalBatchCount {0}";  
            private const string CreateCompletedBatchesTable = @"  
    if OBJECT_ID(N'tempdb.dbo." + CompletedBatchesVariable + @"', N'U') is null  
    begin  
    use tempdb  
    create table [dbo].[$(CompletedBatches)]  
    (  
    BatchId int primary key,  
    Description nvarchar(300)  
    )  
    use [$(DatabaseName)]  
    end  
    ";  
  
    ```  
  
    Quindi, eseguire l'override del metodo OnExecute per aggiungere il codice che si desidera eseguire quando viene distribuito un progetto di database.  
  
#### <a name="to-override-onexecute"></a>Per eseguire l'override di OnExecute  
  
1.  Aggiungere il metodo seguente alla classe SqlRestartableScriptContributor:  
  
    ```csharp  
    /// <summary>  
    /// You override the OnExecute method to do the real work of the contributor.  
    /// </summary>  
    /// <param name="context"></param>  
    protected override void OnExecute(DeploymentPlanContributorContext context)  
    {  
         // Replace this with the method body  
    }  
  
    ```  
  
    Eseguire l'override del metodo [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute) dalla classe di base, [DeploymentPlanContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor) che è la classe di base per [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) e [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). Al metodo OnExecute viene passato un oggetto [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) che fornisce accesso a qualsiasi argomento specificato, al modello di database di origine e di destinazione, al piano di distribuzione e alle opzioni di distribuzione. In questo esempio, viene ottenuto il piano di distribuzione e il nome del database di destinazione.  
  
2.  Ora aggiungere l'inizio di un corpo al metodo OnExecute:  
  
    ```vb  
    // Obtain the first step in the Plan from the provided context  
    DeploymentStep nextStep = context.PlanHandle.Head;  
    int batchId = 0;  
    BeginPreDeploymentScriptStep beforePreDeploy = null;  
  
    // Loop through all steps in the deployment plan  
    while (nextStep != null)  
    {  
        // Increment the step pointer, saving both the current and next steps  
        DeploymentStep currentStep = nextStep;  
        nextStep = currentStep.Next;  
  
        // Add additional step processing here  
    }  
  
    // if we found steps that required processing, set up a temporary table to track the work that you are doing  
    if (beforePreDeploy != null)  
    {  
        // Add additional post-processing here  
    }  
  
    // Cleanup and drop the table   
    DeploymentScriptStep dropStep = new DeploymentScriptStep(DropCompletedBatchesTable);  
    base.AddAfter(context.PlanHandle, context.PlanHandle.Tail, dropStep);  
  
    ```  
  
    In questo codice, vengono definite alcune variabili locali e impostato il ciclo che gestirà l'elaborazione di tutti i passaggi del piano di distribuzione. Una volta completato il ciclo, è necessario eseguire alcune attività di post-elaborazione e quindi eliminare la tabella temporanea creata durante la distribuzione per tenere traccia dell'avanzamento durante l'esecuzione del piano. I tipi chiave sono: [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep) e [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep). Un metodo chiave è AddAfter.  
  
3.  Ora aggiungere l'elaborazione di passaggi aggiuntivi, in sostituzione del commento "Add additional step processing here":  
  
    ```csharp  
    // Look for steps that mark the pre/post deployment scripts  
    // These steps will always be in the deployment plan even if the  
    // user's project does not have a pre/post deployment script  
    if (currentStep is BeginPreDeploymentScriptStep)  
    {  
        // This step marks the beginning of the predeployment script.  
        // Save the step and move on.  
        beforePreDeploy = (BeginPreDeploymentScriptStep)currentStep;  
        continue;  
    }  
    if (currentStep is BeginPostDeploymentScriptStep)  
    {  
        // This is the step that marks the beginning of the post deployment script.    
        // We do not continue processing after this point.  
        break;  
    }  
    if (currentStep is SqlPrintStep)  
    {  
        // We do not need to put if statements around these  
        continue;  
    }  
  
    // if we have not yet found the beginning of the pre-deployment script steps,   
    // skip to the next step.  
    if (beforePreDeploy == null)  
    {  
        // We only surround the "main" statement block with conditional  
        // statements  
        continue;  
    }  
  
    // Determine if this is a step that we need to surround with a conditional statement  
    DeploymentScriptDomStep domStep = currentStep as DeploymentScriptDomStep;  
    if (domStep == null)  
    {  
        // This step is not a step that we know how to modify,  
        // so skip to the next step.  
        continue;  
    }  
  
    TSqlScript script = domStep.Script as TSqlScript;  
    if (script == null)  
    {  
        // The script dom step does not have a script with batches - skip  
        continue;  
    }  
  
        // Loop through all the batches in the script for this step.  All the statements  
        // in the batch will be enclosed in an if statement that will check the  
        // table to ensure that the batch has not already been executed  
        TSqlObject sqlObject;  
        string stepDescription;  
        GetStepInfo(domStep, out stepDescription, out sqlObject);  
        int batchCount = script.Batches.Count;  
  
    for (int batchIndex = 0; batchIndex < batchCount; batchIndex++)  
    {  
        // Add batch processing here  
    }  
  
    ```  
  
    I commenti del codice spiegano l'elaborazione. A un livello elevato, questo codice cerca i passaggi di interesse, ignorando gli altri e interrompendosi quando si raggiunge l'inizio dei passaggi di post-elaborazione. Se il passaggio contiene istruzioni che è necessario racchiudere tra condizionali, verrà eseguita elaborazione aggiuntiva. Tipi, metodi e proprietà chiave includono i seguenti: [BeginPreDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpredeploymentscriptstep), [BeginPostDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpostdeploymentscriptstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [TSqlScript](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlscript), Script, [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep) e [SqlPrintStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlprintstep).  
  
4.  Ora, aggiungere il codice di elaborazione batch sostituendo il commento "Add batch processing here":  
  
    ```csharp  
        // Create the if statement that will contain the batch's contents  
        IfStatement ifBatchNotExecutedStatement = CreateIfNotExecutedStatement(batchId);  
        BeginEndBlockStatement statementBlock = new BeginEndBlockStatement();  
        ifBatchNotExecutedStatement.ThenStatement = statementBlock;  
        statementBlock.StatementList = new StatementList();  
  
        TSqlBatch batch = script.Batches[batchIndex];  
        int statementCount = batch.Statements.Count;  
  
        // Loop through all statements in the batch, embedding those in an sp_execsql  
        // statement that must be handled this way (schemas, stored procedures,   
        // views, functions, and triggers).  
        for (int statementIndex = 0; statementIndex < statementCount; statementIndex++)  
        {  
            // Add additional statement processing here  
        }  
  
        // Add an insert statement to track that all the statements in this  
        // batch were executed.  Turn on nocount to improve performance by  
        // avoiding row inserted messages from the server  
        string batchDescription = string.Format(CultureInfo.InvariantCulture,  
            "{0} batch {1}", stepDescription, batchIndex);  
  
        PredicateSetStatement noCountOff = new PredicateSetStatement();  
        noCountOff.IsOn = false;  
        noCountOff.Options = SetOptions.NoCount;  
  
        PredicateSetStatement noCountOn = new PredicateSetStatement();  
        noCountOn.IsOn = true;  
        noCountOn.Options = SetOptions.NoCount;   
        InsertStatement batchCompleteInsert = CreateBatchCompleteInsert(batchId, batchDescription);  
        statementBlock.StatementList.Statements.Add(noCountOn);  
    statementBlock.StatementList.Statements.Add(batchCompleteInsert);  
        statementBlock.StatementList.Statements.Add(noCountOff);  
  
        // Remove all the statements from the batch (they are now in the if block) and add the if statement  
        // as the sole statement in the batch  
        batch.Statements.Clear();  
        batch.Statements.Add(ifBatchNotExecutedStatement);  
  
        // Next batch  
        batchId++;  
  
    ```  
  
    Questo codice crea un'istruzione IF insieme a un blocco BEGIN/END. Quindi, viene eseguita elaborazione aggiuntiva sulle istruzioni nel batch. Al completamento, viene aggiunta un'istruzione INSERT per aggiungere informazioni alla tabella temporanea che tiene traccia dell'avanzamento dell'esecuzione dello script. Infine, aggiornare il batch, sostituendo le istruzioni precedenti con la nuova istruzione IF che contiene tali istruzioni al suo interno. Tipi, metodi e proprietà chiave includono: [IfStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.ifstatement), [BeginEndBlockStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.beginendblockstatement), [StatementList](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.statementlist), [TSqlBatch](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlbatch), [PredicateSetStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.predicatesetstatement), [SetOptions](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.setoptions) e [InsertStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.insertstatement).  
  
5.  Ora, aggiungere il corpo del ciclo di elaborazione dell'istruzione. Sostituire il commento "Add additional statement processing here":  
  
    ```csharp  
    TSqlStatement smnt = batch.Statements[statementIndex];  
  
    if (IsStatementEscaped(sqlObject))  
    {  
        // "escape" this statement by embedding it in a sp_executesql statement  
        string statementScript;  
        domStep.ScriptGenerator.GenerateScript(smnt, out statementScript);  
        ExecuteStatement spExecuteSql = CreateExecuteSql(statementScript);  
        smnt = spExecuteSql;  
    }  
  
    statementBlock.StatementList.Statements.Add(smnt);  
  
    ```  
  
    Per ogni istruzione nel batch, se l'istruzione è di un tipo che deve essere racchiuso in un'istruzione sp_executesql, modificare l'istruzione di conseguenza. Il codice, quindi, aggiunge l'istruzione all'elenco di istruzioni per il blocco di BEGIN/END creato. Tipi, metodi e proprietà chiave includono [TSqlStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlstatement) e [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement).  
  
6.  Infine, aggiungere la sezione di post-elaborazione in sostituzione del commento "Add additional post-processing here":  
  
    ```csharp  
    // Declare a SqlCmd variables.  
    //  
    // CompletedBatches variable - defines the name of the table in tempdb that will track  
    // all the completed batches.  The temporary table's name has the target database name and  
    // a guid embedded in it so that:  
    // * Multiple deployment scripts targeting different DBs on the same server  
    // * Failed deployments with old tables do not conflict with more recent deployments  
    //  
    // TotalBatchCount variable - the total number of batches surrounded by if statements.  Using this  
    // variable pre/post deployment scripts can also use the CompletedBatches table to make their  
    // script rerunnable if there is an error during execution  
    StringBuilder sqlcmdVars = new StringBuilder();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, CompletedBatchesSqlCmd,  
        context.Options.TargetDatabaseName, Guid.NewGuid().ToString("D"));  
    sqlcmdVars.AppendLine();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, TotalBatchCountSqlCmd, batchId);  
  
    DeploymentScriptStep completedBatchesSetVarStep = new DeploymentScriptStep(sqlcmdVars.ToString());  
    base.AddBefore(context.PlanHandle, beforePreDeploy, completedBatchesSetVarStep);  
  
    // Create the temporary table we will use to track the work that we are doing  
    DeploymentScriptStep createStatusTableStep = new DeploymentScriptStep(CreateCompletedBatchesTable);  
    base.AddBefore(context.PlanHandle, beforePreDeploy, createStatusTableStep);  
  
    ```  
  
    Se l'elaborazione rileva uno o più passaggi racchiusi in un'istruzione condizionale, è necessario aggiungere istruzioni allo script di distribuzione per definire variabili SQLCMD. La prima variabile, CompletedBatches, contiene un nome univoco per la tabella temporanea che lo script di distribuzione utilizza per tenere traccia dei batch completati correttamente all'esecuzione dello script. La seconda variabile, TotalBatchCount, contiene il numero totale di batch nello script di distribuzione.  
  
    Tipi, proprietà e metodi di interesse aggiuntivi includono:  
  
    StringBuilder, [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep) e AddBefore.  
  
    Successivamente, si definiscono i metodi helper chiamati da questo metodo.  
  
#### <a name="to-add-the-helper-methods"></a>Per aggiungere metodi helper  
  
-   È necessario definire diversi metodi helper. I metodi importanti includono:  
  
    |**Metodo**|**Descrizione**|  
    |--------------|-------------------|  
    |CreateExecuteSQL|Definire il metodo CreateExecuteSQL per racchiudere un'istruzione fornita in un'istruzione EXEC sp_executesql. Tipi, metodi e proprietà chiave includono i seguenti: [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement), [ExecutableProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executableprocedurereference), [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname), [ProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.procedurereference) e [ExecuteParameter](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executeparameter).|  
    |CreateCompletedBatchesName|Definire il metodo CreateCompletedBatchesName. Questo metodo crea il nome che verrà inserito nella tabella temporanea per un batch. Tipi, metodi e proprietà chiave includono quanto segue: [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname).|  
    |IsStatementEscaped|Definire il metodo IsStatementEscaped. Questo metodo determina se il tipo di elemento del modello richiede che l'istruzione sia racchiusa in un'istruzione EXEC sp_executesql prima di poter essere inclusa nell'istruzione IF. Tipi, metodi e proprietà chiave includono quanto segue: TSqlObject.ObjectType, ModelTypeClass e la proprietà TypeClass per i tipi di modello seguenti: Schema, Procedure, View,  TableValuedFunction, ScalarFunction, DatabaseDdlTrigger, DmlTrigger, ServerDdlTrigger.|  
    |CreateBatchCompleteInsert|Definire il metodo CreateBatchCompleteInsert. Questo metodo crea l'istruzione INSERT che verrà aggiunta allo script di distribuzione per tenere traccia dell'avanzamento dell'esecuzione dello script. Tipi, metodi e proprietà chiave includono: InsertStatement, NamedTableReference, ColumnReferenceExpression, ValuesInsertSource e RowValue.|  
    |CreateIfNotExecutedStatement|Definire il metodo CreateIfNotExecutedStatement. Questo metodo genera un'istruzione IF che verifica se la tabella temporanea di esecuzione dei batch indica che questo batch è già stato eseguito. Tipi, metodi e proprietà chiave includono quanto segue: IfStatement, ExistsPredicate, ScalarSubquery, NamedTableReference, WhereClause, ColumnReferenceExpression, IntegerLiteral, BooleanComparisonExpression e BooleanNotExpression.|  
    |GetStepInfo|Definire il metodo GetStepInfo. Questo metodo estrae informazioni sull'elemento del modello usato per creare lo script del passaggio, oltre al nome del passaggio. Tipi e metodi di interesse includono i seguenti: [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext), [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep), [AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep) e [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep).|  
    |GetElementName|Crea un nome formattato per un elemento TSqlObject.|  
  
1.  Aggiungere il codice seguente per definire i metodi helper:  
  
    ```csharp  
  
    /// <summary>  
    /// The CreateExecuteSql method "wraps" the provided statement script in an "sp_executesql" statement  
    /// Examples of statements that must be so wrapped include: stored procedures, views, and functions  
    /// </summary>  
    private static ExecuteStatement CreateExecuteSql(string statementScript)  
    {  
        // define a new Exec statement  
        ExecuteStatement executeSp = new ExecuteStatement();  
        ExecutableProcedureReference spExecute = new ExecutableProcedureReference();  
        executeSp.ExecuteSpecification = new ExecuteSpecification { ExecutableEntity = spExecute };  
  
        // define the name of the procedure that you want to execute, in this case sp_executesql  
        SchemaObjectName procName = new SchemaObjectName();  
        procName.Identifiers.Add(CreateIdentifier("sp_executesql", QuoteType.NotQuoted));  
        ProcedureReference procRef = new ProcedureReference { Name = procName };  
  
        spExecute.ProcedureReference = new ProcedureReferenceName { ProcedureReference = procRef };  
  
        // add the script parameter, constructed from the provided statement script  
        ExecuteParameter scriptParam = new ExecuteParameter();  
        spExecute.Parameters.Add(scriptParam);  
        scriptParam.ParameterValue = new StringLiteral { Value = statementScript };  
        scriptParam.Variable = new VariableReference { Name = "@stmt" };  
        return executeSp;  
    }  
  
    /// <summary>  
    /// The CreateIdentifier method returns a Identifier with the specified value and quoting type  
    /// </summary>  
    private static Identifier CreateIdentifier(string value, QuoteType quoteType)  
    {  
        return new Identifier { Value = value, QuoteType = quoteType };  
    }  
  
    /// <summary>  
    /// The CreateCompletedBatchesName method creates the name that will be inserted  
    /// into the temporary table for a batch.  
    /// </summary>  
    private static SchemaObjectName CreateCompletedBatchesName()  
    {  
        SchemaObjectName name = new SchemaObjectName();  
        name.Identifiers.Add(CreateIdentifier("tempdb", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier("dbo", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier(CompletedBatchesVariable, QuoteType.SquareBracket));  
        return name;  
    }  
  
    /// <summary>  
    /// Helper method that determins whether the specified statement needs to  
    /// be escaped  
    /// </summary>  
    /// <param name="sqlObject"></param>  
    /// <returns></returns>  
    private static bool IsStatementEscaped(TSqlObject sqlObject)  
    {  
        HashSet<ModelTypeClass> escapedTypes = new HashSet<ModelTypeClass>  
        {  
            Schema.TypeClass,  
            Procedure.TypeClass,  
            View.TypeClass,  
            TableValuedFunction.TypeClass,  
            ScalarFunction.TypeClass,  
            DatabaseDdlTrigger.TypeClass,  
            DmlTrigger.TypeClass,  
            ServerDdlTrigger.TypeClass  
        };  
        return escapedTypes.Contains(sqlObject.ObjectType);  
    }  
  
    /// <summary>  
    /// Helper method that creates an INSERT statement to track a batch being completed  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <param name="batchDescription"></param>  
    /// <returns></returns>  
    private static InsertStatement CreateBatchCompleteInsert(int batchId, string batchDescription)  
    {  
        InsertStatement insert = new InsertStatement();  
        NamedTableReference batchesCompleted = new NamedTableReference();  
        insert.InsertSpecification = new InsertSpecification();  
        insert.InsertSpecification.Target = batchesCompleted;  
        batchesCompleted.SchemaObject = CreateCompletedBatchesName();  
  
        // Build the columns inserted into  
        ColumnReferenceExpression batchIdColumn = new ColumnReferenceExpression();  
        batchIdColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        batchIdColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(BatchIdColumnName, QuoteType.NotQuoted));  
  
        ColumnReferenceExpression descriptionColumn = new ColumnReferenceExpression();  
        descriptionColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        descriptionColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(DescriptionColumnName, QuoteType.NotQuoted));  
  
        insert.InsertSpecification.Columns.Add(batchIdColumn);  
        insert.InsertSpecification.Columns.Add(descriptionColumn);  
  
        // Build the values inserted  
        ValuesInsertSource valueSource = new ValuesInsertSource();  
        insert.InsertSpecification.InsertSource = valueSource;  
  
        RowValue values = new RowValue();  
        values.ColumnValues.Add(new IntegerLiteral { Value = batchId.ToString() });  
        values.ColumnValues.Add(new StringLiteral { Value = batchDescription });  
        valueSource.RowValues.Add(values);  
  
        return insert;  
    }  
  
    /// <summary>  
    /// This is a helper method that generates an if statement that checks the batches executed  
    /// table to see if the current batch has been executed.  The if statement will look like this  
    ///   
    /// if not exists(select 1 from [tempdb].[dbo].[$(CompletedBatches)]   
    ///                where BatchId = batchId)  
    /// begin  
    /// end  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <returns></returns>  
    private static IfStatement CreateIfNotExecutedStatement(int batchId)  
    {  
        // Create the exists/select statement  
        ExistsPredicate existsExp = new ExistsPredicate();  
        ScalarSubquery subQuery = new ScalarSubquery();  
        existsExp.Subquery = subQuery;  
  
        subQuery.QueryExpression = new QuerySpecification  
        {  
            SelectElements =  
            {  
                new SelectScalarExpression  { Expression = new IntegerLiteral { Value ="1" } }  
            },  
            FromClause = new FromClause  
            {  
                TableReferences =  
                    {  
                        new NamedTableReference() { SchemaObject = CreateCompletedBatchesName() }  
                    }  
            },  
            WhereClause = new WhereClause  
            {  
                SearchCondition = new BooleanComparisonExpression  
                {  
                    ComparisonType = BooleanComparisonType.Equals,  
                    FirstExpression = new ColumnReferenceExpression  
                    {  
                        MultiPartIdentifier = new MultiPartIdentifier  
                        {  
                            Identifiers = { CreateIdentifier(BatchIdColumnName, QuoteType.SquareBracket) }  
                        }  
                    },  
                    SecondExpression = new IntegerLiteral { Value = batchId.ToString() }  
                }  
            }  
        };  
  
        // Put together the rest of the statement  
        IfStatement ifNotExists = new IfStatement  
        {  
            Predicate = new BooleanNotExpression  
            {  
                Expression = existsExp  
            }  
        };  
  
        return ifNotExists;  
    }  
  
    /// <summary>  
    /// Helper method that generates a useful description of the step.  
    /// </summary>  
    private static void GetStepInfo(  
        DeploymentScriptDomStep domStep,  
        out string stepDescription,  
        out TSqlObject element)  
    {  
        element = null;  
  
        // figure out what type of step we've got, and retrieve  
        // either the source or target element.  
        if (domStep is CreateElementStep)  
        {  
            element = ((CreateElementStep)domStep).SourceElement;  
        }  
        else if (domStep is AlterElementStep)  
        {  
            element = ((AlterElementStep)domStep).SourceElement;  
        }  
        else if (domStep is DropElementStep)  
        {  
            element = ((DropElementStep)domStep).TargetElement;  
        }  
  
        // construct the step description by concatenating the type and the fully qualified  
        // name of the associated element.  
        string stepTypeName = domStep.GetType().Name;  
        if (element != null)  
        {  
            string elementName = GetElementName(element);  
  
            stepDescription = string.Format(CultureInfo.InvariantCulture, "{0} {1}",  
                stepTypeName, elementName);  
        }  
        else  
        {  
            // if the step has no associated element, just use the step type as the description  
            stepDescription = stepTypeName;  
        }  
    }  
  
    private static string GetElementName(TSqlObject element)  
    {  
        StringBuilder name = new StringBuilder();  
        if (element.Name.HasExternalParts)  
        {  
            foreach (string part in element.Name.ExternalParts)  
            {  
                if (name.Length > 0)  
                {  
                    name.Append('.');  
                }  
                name.AppendFormat("[{0}]", part);  
            }  
        }  
  
        foreach (string part in element.Name.Parts)  
        {  
            if (name.Length > 0)  
            {  
                name.Append('.');  
            }  
            name.AppendFormat("[{0}]", part);  
        }  
  
        return name.ToString();  
    }  
  
    ```  
  
2.  Salvare le modifiche a SqlRestartableScriptContributor.cs.  
  
Compilare la libreria di classi.  
  
#### <a name="to-sign-and-build-the-assembly"></a>Per firmare e compilare l'assembly  
  
1.  Scegliere **Proprietà di MyOtherDeploymentContributor** dal menu **Progetto**.  
  
2.  Fare clic sulla scheda **Firma** .  
  
3.  Fare clic su **Firma assembly**.  
  
4.  In **Scegli un file chiave con nome sicuro** fare clic su **<New>** .  
  
5.  Nella finestra di dialogo **Crea chiave con nome sicuro** , in **Nome file di chiave**, digitare **MyRefKey**.  
  
6.  (facoltativo) È possibile specificare una password per il file di chiave con nome sicuro.  
  
7.  Fare clic su **OK**.  
  
8.  Scegliere **Salva tutti** dal menu **File**.  
  
9. Nel menu **Compila** scegliere **Compila soluzione**.  
  
    È quindi necessario installare l'assembly in modo che venga caricato quando si distribuiscono progetti SQL.  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>Installare un collaboratore alla distribuzione  
Per installare un collaboratore alla distribuzione, è necessario copiare l'assembly e il file con estensione pdb associato nella cartella Extensions.  
  
#### <a name="to-install-the-myotherdeploymentcontributor-assembly"></a>Per installare l'assembly MyOtherDeploymentContributor  
  
1.  Nella prossima esercitazione verrà illustrato come copiare le informazioni sull'assembly nella directory Estensioni. Quando si avvia Visual Studio, le estensioni vengono identificate nella directory %Programmi%\Microsoft SQL Server\110\DAC\Bin\Extensions e relative sottodirectory, quindi vengono rese disponibili per l'utilizzo.  
  
2.  Copiare il file di assembly **MyOtherDeploymentContributor.dll** dalla directory di output alla directory %Programmi%\Microsoft SQL Server\110\DAC\Bin\Extensions. Per impostazione predefinita, il percorso del file con estensione dll compilato è PercorsoSoluzione\PercorsoProgetto\bin\Debug o PercorsoSoluzione\PercorsoProgetto\bin\Release.  
  
## <a name="run-or-test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>Eseguire o testare il collaboratore alla distribuzione  
Per eseguire o testare il collaboratore alla distribuzione, è necessario effettuare le attività seguenti:  
  
-   Aggiungere proprietà al file con estensione sqlproj che si intende compilare.  
  
-   Distribuire il progetto di database utilizzando MSBuild e fornendo i parametri appropriati.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Aggiungere proprietà al file del progetto SQL (.sqlproj)  
È necessario aggiornare sempre il file del progetto SQL per specificare l'ID dei collaboratori che si desidera eseguire. Questa operazione può essere eseguita in due modi:  
  
1.  È possibile modificare manualmente il file con estensione sqlproj per aggiungere gli argomenti richiesti. È possibile scegliere di eseguire questa operazione se il collaboratore non contiene gli argomenti del collaboratore necessari per la configurazione o se non si prevede di riutilizzare il collaboratore alla compilazione in molti progetti. Se si sceglie questa opzione, aggiungere le istruzioni seguenti nel file con estensione sqlproj dopo il primo nodo Import nel file:  
  
    ```  
    <PropertyGroup>  
      <DeploymentContributors>  
        $(DeploymentContributors); MyOtherDeploymentContributor.RestartableScriptContributor  
      </DeploymentContributors>  
    </PropertyGroup>  
    ```  
  
2.  Il secondo metodo consiste nel creare un file targets contenente gli argomenti del collaboratore richiesti. È utile se si utilizza lo stesso collaboratore per più progetti e si dispone degli argomenti del collaboratore richiesti, dal momento che include i valori predefiniti. In questo caso, creare un file targets nel percorso delle estensioni MSBuild:  
  
    1.  Passare a %Programmi%\MSBuild.  
  
    2.  Creare una nuova cartella "MyContributors" in cui archiviare i file TARGETS.  
  
    3.  Creare un nuovo file "MyContributors.targets" in questa directory, aggiungere il testo seguente e salvare il file:  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <DeploymentContributors>$(DeploymentContributors);MyOtherDeploymentContributor.RestartableScriptContributor</DeploymentContributors>  
          </PropertyGroup>  
        </Project>  
  
        ```  
  
    4.  All'interno del file con estensione sqlproj per tutti i progetti per i quali si vogliono eseguire i collaboratori, importare il file targets aggiungendo l'istruzione seguente al file con estensione sqlproj dopo il nodo \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> nel file:  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
  
        ```  
  
Dopo aver seguito uno di questi approcci, è possibile utilizzare MSBuild per passare i parametri per compilazioni della riga di comando.  
  
> [!NOTE]  
> È necessario aggiornare sempre la proprietà "DeploymentContributors" per specificare il proprio ID collaboratore. Si tratta dello stesso ID usato nell'attributo "ExportDeploymentPlanModifier" nel file di origine del collaboratore. Senza questo il collaboratore non verrà eseguito durante la compilazione del progetto. La proprietà "ContributorArguments" deve essere aggiornata solo se si hanno argomenti richiesti per l'esecuzione del collaboratore.  
  
## <a name="deploy-the-database-project"></a>Distribuire il progetto di database  
  
#### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>Per distribuire il progetto SQL e generare un report di distribuzione  
  
-   Il progetto può essere pubblicato o distribuito normalmente in Visual Studio. È sufficiente aprire una soluzione contenente il progetto SQL e scegliere l'opzione Pubblica... dal menu di scelta rapida per il progetto o usare F5 per una distribuzione di debug in Local DB. In questo esempio verrà usata la finestra di dialogo "Pubblica..." per generare uno script di distribuzione.  
  
    1.  Aprire Visual Studio e aprire la soluzione che contiene il progetto SQL.  
  
    2.  Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere l'opzione **Pubblica...** .  
  
    3.  Impostare il nome del server e il nome del database in cui pubblicare.  
  
    4.  Scegliere **Genera script** dalle opzioni nella parte inferiore della finestra di dialogo. Verrà creato uno script che potrà essere utilizzato per la distribuzione. Questo verrà esaminato per verificare se sono state aggiunte le istruzioni IF per rendere riavviabile lo script.  
  
    5.  Esaminare lo script di distribuzione risultante. Immediatamente prima della sezione con etichetta "Pre-Deployment Script Template", deve essere presente una sintassi Transact-SQL analoga alla seguente:  
  
        ```  
        :setvar CompletedBatches __completedBatches_CompareProjectDB_cd1e348a-8f92-44e0-9a96-d25d65900fca  
        :setvar TotalBatchCount 17  
        GO  
  
        if OBJECT_ID(N'tempdb.dbo.$(CompletedBatches)', N'U') is null  
        begin  
        use tempdb  
        create table [dbo].[$(CompletedBatches)]  
        (  
        BatchId int primary key,  
        Description nvarchar(300)  
        )  
        use [$(DatabaseName)]  
        end  
  
        ```  
  
        Più avanti nello script di distribuzione, attorno a ogni batch è presente un'istruzione IF che racchiude l'istruzione originale. Ad esempio, per un'istruzione CREATE SCHEMA può essere presente quanto segue:  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 0)  
            BEGIN  
                EXECUTE sp_executesql @stmt = N'CREATE SCHEMA [Sales]  
            AUTHORIZATION [dbo]';  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (0, N'CreateElementStep Sales batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        Si noti che CREATE SCHEMA è una delle istruzioni da racchiudere in un'istruzione EXECUTE sp_executesql all'interno di un'istruzione IF. Istruzioni quali CREATE TABLE non richiedono l'istruzione EXECUTE sp_executesql e sono simili all'esempio seguente:  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 1)  
            BEGIN  
                CREATE TABLE [Sales].[Customer] (  
                    [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
                    [CustomerName] NVARCHAR (40) NOT NULL,  
                    [YTDOrders]    INT           NOT NULL,  
                    [YTDSales]     INT           NOT NULL  
                );  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (1, N'CreateElementStep Sales.Customer batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        > [!NOTE]  
        > Se si distribuisce un progetto di database identico al database di destinazione, il report risultante non sarà molto significativo. Per risultati più significativi, distribuire modifiche a un database o distribuire un nuovo database.  
  
## <a name="command-line-deployment-using-generated-dacpac-file"></a>Distribuzione della riga di comando mediante il file dacpac generato  
Dopo che un progetto SQL viene compilato, viene creato un file dacpac che può essere utilizzato per distribuire lo schema dalla riga di comando e che consente la distribuzione da un computer diverso, ad esempio da un computer di compilazione. SqlPackage è un'utilità della riga di comando che consente la distribuzione di dacpac con una gamma completa di opzioni che consentono agli utenti di distribuire un dacpac o di generare uno script di distribuzione, tra le altre azioni. Per altre informazioni, vedere [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md).  
  
> [!NOTE]  
> Per distribuire dacpac creati da progetti con la proprietà DeploymentContributors definita, e necessario che le DLL che contengono propri collaboratori siano installati nel computer in uso. Ciò è dovuto al fatto che sono stati contrassegnati come necessari per il corretto completamento della distribuzione.  
>   
> Per evitare questo requisito, escludere il collaboratore alla distribuzione dal file con estensione sqlproj. Viceversa, specificare che i collaboratori vengano eseguiti durante la distribuzione usando SqlPackage con il parametro **AdditionalDeploymentContributors**. Questo è utile nei casi in cui si desidera utilizzare solo un collaboratore in circostanze speciali, come la distribuzione in un server specifico.  
  
## <a name="next-steps"></a>Passaggi successivi  
È possibile provare a utilizzare altri tipi di modifiche ai piani di distribuzione prima di eseguirli. Altri tipi di modifiche che potrebbe essere opportuno eseguire includono:  
  
-   Aggiunta di una proprietà estesa a tutti gli oggetti di database a cui è associato un numero di versione.  
  
-   Aggiunta o rimozione di istruzioni PRINT di diagnostica aggiuntive o di commenti dagli script di distribuzione.  
  
## <a name="see-also"></a>Vedere anche  
[Personalizzare la compilazione e la distribuzione del database tramite collaboratori alla compilazione e distribuzione](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Procedura dettagliata: Estendere la compilazione del progetto di database per generare statistiche del modello](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)  
[Procedura dettagliata: Estendere la distribuzione del progetto di database per analizzare il piano di distribuzione](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
