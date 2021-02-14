---
title: Creazione di un assembly di regole personalizzate di analisi del codice statica per SQL Server
description: Informazioni su come creare regole di analisi codice di SQL Server. Configurare una regola per evitare istruzioni WAITFOR DELAY in stored procedure, trigger e funzioni.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f7b6ed8c-a4e0-4e33-9858-a8aa40aef309
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 3cae3183bef3d6bd40a9d30805f3923df8d70679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082342"
---
# <a name="walkthrough-authoring-a-custom-static-code-analysis-rule-assembly-for-sql-server"></a>Procedura dettagliata per la creazione di un assembly di regole personalizzate di analisi del codice statica per SQL Server

Questa procedura dettagliata illustra i passaggi necessari per creare una regola di analisi del codice di SQL Server. La regola creata in questa procedura dettagliata viene usata per evitare le istruzioni WAITFOR DELAY in stored procedure, trigger e funzioni.  
  
In questa procedura dettagliata verrà creata una regola personalizzata per l'analisi del codice statico di Transact\-SQL eseguendo i processi seguenti:  
  
1. Creare una libreria di classi, abilitare la firma per il progetto e aggiungere i riferimenti necessari.  
  
2. Creare una classe di regola personalizzata Visual C\#.  
  
3. Creare due classi helper Visual C\#.  
  
4. Copiare la DLL risultante creata nella directory Extensions per poter eseguire l'installazione.  
  
5. Verificare che la nuova regola di analisi del codice sia attiva.  
  
**Prerequisiti**
  
Per completare questa procedura dettagliata, è necessario disporre dei componenti seguenti:  
  
- È necessario avere installato una versione di Visual Studio che include SQL Server Data Tools e supporti lo sviluppo in C\# o Visual Basic.  
  
- È necessario avere un progetto di SQL Server contenente oggetti di SQL Server.  
  
- Un'istanza di SQL Server a cui sia possibile distribuire un progetto di database.  
  
> [!NOTE]  
> Questa procedura dettagliata è destinata a utenti che hanno già familiarità con le funzionalità di SQL Server di SQL Server Data Tools. È anche necessario che l'utente conosca i concetti di base di Visual Studio, ad esempio come creare una libreria di classi e come usare l'editor di codice per aggiungere codice a una classe.  
  
## <a name="creating-a-custom-code-analysis-rule-for-sql-server"></a>Creazione di una regola di analisi del codice personalizzata per SQL Server  

Per prima cosa creare una libreria di classi. Per creare un progetto libreria di classi:  
  
1. Creare un progetto di libreria di classi di Visual Basic o Visual C\# denominato SampleRules.  
  
2. Rinominare il file Class1. cs in AvoidWaitForDelayRule.cs.  
  
3. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Aggiungi riferimento**.  
  
4. Selezionare System.ComponentModel.Composition nella scheda Framework.  
  
5. Fare clic su **Sfoglia** e passare alla directory C:\Programmi (x86)\\Microsoft SQL Server\120\SDK\Assemblies, selezionare Microsoft.SqlServer.TransactSql.ScriptDom.dll e quindi fare clic su OK.  
  
6. Installare quindi i riferimenti necessari di DACFx. Fare clic su **Sfoglia** e passare alla directory <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120. Scegliere le voci Microsoft.SqlServer.Dac.dll, Microsoft.SqlServer.Dac.Extensions.dll e Microsoft.Data.Tools.Schema.Sql.dll, fare clic su **Aggiungi** e quindi fare clic su **OK**.  
  
    I file binari di DACFx sono ora installati all'interno della directory di installazione di Visual Studio. Per Visual Studio 2012, <Visual Studio Install Dir> sarà in genere C:\Programmi (x86)\\MicrosoftVisual Studio 11.0. Per Visual Studio 2013 questa directory sarà in genere C:\Programmi (x86)\\MicrosoftVisual Studio 12.0.  
  
A questo punto aggiungere le classi di supporto che verranno usate dalla regola.  
  
## <a name="creating-the-custom-code-analysis-rule-supporting-classes"></a>Creazione delle classi supporto per le regole personalizzate di analisi del codice

Prima di creare la classe per la regola stessa, aggiungere al progetto una classe visitor e una classe attribute. Queste classi possono rivelarsi utili per la creazione di altre regole personalizzate.  
  
La prima classe da definire è la classe WaitForDelayVisitor, derivata da [TSqlConcreteFragmentVisitor](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlconcretefragmentvisitor). Questa classe fornisce l'accesso alle istruzioni WAITFOR DELAY nel modello. Le classi visitor usano le API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) fornite da SQL Server. In questa API il codice Transact\-SQL viene rappresentato come un albero sintattico astratto e le classi visitor possono essere utili per trovare oggetti di sintassi specifici quali le istruzioni WAITFORDELAY. Può essere difficile trovarle usando il modello a oggetti, poiché non sono associate a una proprietà o a una relazione di un oggetto specifico, mentre è facile trovarle con il modello visitor e l'API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom).  
  
### <a name="defining-the-waitfordelayvisitor-class"></a>Definizione della classe WaitForDelayVisitor  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRules.  
  
2. Nel menu **Progetto** selezionare **Aggiungi classe**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** .  
  
3. Nella casella di testo **Nome** digitare WaitForDelayVisitor.cs e fare clic sul pulsante **Aggiungi**. Il file WaitForDelayVisitor.cs viene aggiunto al progetto in **Esplora soluzioni**.  
  
4. Aprire il file WaitForDelayVisitor.cs e aggiornare il contenuto in modo che corrisponda al codice seguente:  
  
    ```  
    using System.Collections.Generic;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    namespace SampleRules {  
        class WaitForDelayVistor {}  
    }  
    ```  
  
5. Nella dichiarazione di classe impostare il modificatore di accesso su internal e derivare la classe da TSqlConcreteFragmentVisitor:  
  
    ```  
    internal class WaitForDelayVisitor : TSqlConcreteFragmentVisitor {}  
    ```  
  
6. Aggiungere il codice seguente per definire la variabile membro dell'elenco:  
  
    ```  
    public IList<WaitForStatement> WaitForDelayStatements { get; private set; }  
    ```  
  
7. Definire il costruttore della classe aggiungendo il codice seguente:  
  
    ```  
    public WaitForDelayVisitor() {  
       WaitForDelayStatments = new List<WaitForStatement>();  
    }  
    ```  
  
8. Eseguire l'override del metodo ExplicitVisit aggiungendo il codice seguente:  
  
    ```  
    public override void ExplicitVisit(WaitForStatement node) {  
       // We are only interested in WAITFOR DELAY occurrences  
       if (node.WaitForOption == WaitForOption.Delay)  
          WaitForDelayStatments.Add(node);  
    }  
    ```  
  
    Questo metodo visita le istruzioni WAITFOR nel modello e aggiunge quelle per cui è specificata l'opzione DELAY all'elenco di istruzioni WAITFOR DELAY. La classe principale a cui si fa riferimento qui è [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement).  
  
9. Nel menu **File** scegliere **Salva**.  
  
La seconda classe è LocalizedExportCodeAnalysisRuleAttribute.cs. Si tratta di un'estensione della classe Microsoft.SqlServer.Dac.CodeAnalysis.ExportCodeAnalysisRuleAttribute predefinita, specificata dal framework, che supporta la lettura delle classi DisplayName e Description usate dalla regola da un file di risorse. È una classe utile se si prevede di usare le regole in più lingue.  
  
### <a name="defining-the-localizedexportcodeanalysisruleattribute-class"></a>Definizione della classe LocalizedExportCodeAnalysisRuleAttribute  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRules.  
  
2. Nel menu **Progetto** selezionare **Aggiungi classe**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** .  
  
3. Nella casella di testo **Nome** digitare LocalizedExportCodeAnalysisRuleAttribute.cs e fare clic sul pulsante **Aggiungi**. Il file viene aggiunto al progetto in **Esplora soluzioni**.  
  
4. Aprire il file e aggiornare il contenuto in modo che corrisponda al codice seguente:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using System;  
    using System.Globalization;  
    using System.Reflection;  
    using System.Resources;  
  
    namespace SampleRules  
    {  
  
        internal class LocalizedExportCodeAnalysisRuleAttribute : ExportCodeAnalysisRuleAttribute  
        {  
            private readonly string _resourceBaseName;  
            private readonly string _displayNameResourceId;  
            private readonly string _descriptionResourceId;  
  
            private ResourceManager _resourceManager;  
            private string _displayName;  
            private string _descriptionValue;  
  
            /// <summary>  
            /// Creates the attribute, with the specified rule ID, the fully qualified  
            /// name of the resource file that will be used for looking up display name  
            /// and description, and the Ids of those resources inside the resource file.  
            /// </summary>  
            public LocalizedExportCodeAnalysisRuleAttribute(  
                string id,  
                string resourceBaseName,  
                string displayNameResourceId,  
                string descriptionResourceId)  
                : base(id, null)  
            {  
                _resourceBaseName = resourceBaseName;  
                _displayNameResourceId = displayNameResourceId;  
                _descriptionResourceId = descriptionResourceId;  
            }  
  
            /// <summary>  
            /// Rules in a different assembly would need to overwrite this  
            /// </summary>  
            /// <returns></returns>  
            protected virtual Assembly GetAssembly()  
            {  
                return GetType().Assembly;  
            }  
  
            private void EnsureResourceManagerInitialized()  
            {  
                var resourceAssembly = GetAssembly();  
  
                try  
                {  
                    _resourceManager = new ResourceManager(_resourceBaseName, resourceAssembly);  
                }  
                catch (Exception ex)  
                {  
                    var msg = String.Format(CultureInfo.CurrentCulture, RuleResources.CannotCreateResourceManager, _resourceBaseName, resourceAssembly);  
                    throw new RuleException(msg, ex);  
                }  
            }  
  
            private string GetResourceString(string resourceId)  
            {  
                EnsureResourceManagerInitialized();  
                return _resourceManager.GetString(resourceId, CultureInfo.CurrentUICulture);  
            }  
  
            /// <summary>  
            /// Overrides the standard DisplayName and looks up its value inside a resources file  
            /// </summary>  
            public override string DisplayName  
            {  
                get  
                {  
                    if (_displayName == null)  
                    {  
                        _displayName = GetResourceString(_displayNameResourceId);  
                    }  
                    return _displayName;  
                }  
            }  
  
            /// <summary>  
            /// Overrides the standard Description and looks up its value inside a resources file  
            /// </summary>  
            public override string Description  
            {  
                get  
                {  
                    if (_descriptionValue == null)  
                    {   
                        _descriptionValue = GetResourceString(_descriptionResourceId);  
                    }  
                    return _descriptionValue;  
                }  
            }  
        }  
    }  
    ```  
  
A questo punto aggiungere un file di risorse che definirà il nome della regola, la descrizione della regola e la categoria in cui la regola verrà visualizzata nell'interfaccia di configurazione delle regole.  
  
### <a name="to-add-a-resource-file-and-three-resource-strings"></a>Per aggiungere un file di risorse e tre stringhe di risorse  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRules.  
  
2. Nel menu **Progetto** selezionare **Aggiungi nuovo elemento**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** .  
  
3. Nell'elenco **Modelli installati** fare clic su **Generale**.  
  
4. Nel riquadro dei dettagli fare clic su **File di risorse**.  
  
5. In **Nome** digitare RuleResources.resx. Viene visualizzato l'editor delle risorse, senza alcuna risorsa definita.  
  
6. Definire quattro stringhe di risorse come segue:  
  
    |Nome|Valore|  
    |--------|---------|  
    |AvoidWaitForDelay_ProblemDescription|L'istruzione WAITFOR DELAY è stata trovata in {0}.|  
    |AvoidWaitForDelay_RuleName|Evitare di usare istruzioni WaitFor Delay in stored procedure, funzioni e trigger.|  
    |CategorySamples|SamplesCategory|  
    |CannotCreateResourceManager|Non è possibile creare l'oggetto ResourceManager per {0} da {1}.|  
  
7. Fare clic su **Salva RuleResources.resx** dal menu **File**.  
  
A questo punto definire una classe che faccia riferimento alle risorse nel file di risorse usate da Visual Studio per visualizzare le informazioni sulla regola nell'interfaccia utente.  
  
### <a name="defining-the-sampleconstants-class"></a>Definizione della classe SampleConstants  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRules.  
  
2. Nel menu **Progetto** selezionare **Aggiungi classe**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** .  
  
3. Nella casella di testo **Nome** digitare SampleRuleConstants.cs e fare clic sul pulsante **Aggiungi**. Il file SampleRuleConstants.cs viene aggiunto al progetto in **Esplora soluzioni**.  
  
4. Aprire il file SampleRuleConstants.cs e aggiungere al file le istruzioni Using seguenti:  
  
    ```  
    namespace SampleRules  
    {   
        internal static class RuleConstants  
        {  
            /// <summary>  
            /// The name of the resources file to use when looking up rule resources  
            /// </summary>  
            public const string ResourceBaseName = "Public.Dac.Samples.Rules.RuleResources";  
  
            /// <summary>  
            /// Lookup name inside the resources file for the select asterisk rule name  
            /// </summary>  
            public const string AvoidWaitForDelay_RuleName = "AvoidWaitForDelay_RuleName";  
            /// <summary>  
            /// Lookup ID inside the resources file for the select asterisk description  
            /// </summary>  
            public const string AvoidWaitForDelay_ProblemDescription = "AvoidWaitForDelay_ProblemDescription";  
  
            /// <summary>  
            /// The design category (should not be localized)  
            /// </summary>  
            public const string CategoryDesign = "Design";  
  
            /// <summary>  
            /// The performance category (should not be localized)  
            /// </summary>  
            public const string CategoryPerformance = "Design";  
        }  
    }  
    ```  
  
5. Fare clic su **File** > **Salva**.  
  
## <a name="creating-the-custom-code-analysis-rule-class"></a>Creazione della classe per le regole personalizzate di analisi del codice

Dopo avere aggiunto le classi helper che verranno usate dalla regola di analisi del codice personalizzata, creare una classe di regola personalizzate e denominarla AvoidWaitForDelayRule. La regola personalizzata AvoidWaitForDelayRule consentirà agli sviluppatori di database di evitare di usare le istruzioni WAITFOR DELAY in stored procedure, trigger e funzioni.  
  
### <a name="creating-the-avoidwaitfordelayrule-class"></a>Creazione della classe AvoidWaitForDelayRule  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRules.  
  
2. Nel menu **Progetto** selezionare **Aggiungi classe**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** .  
  
3. Nella casella di testo **Nome** digitare AvoidWaitForDelayRule.cs e fare clic su **Aggiungi**. Il file AvoidWaitForDelayRule.cs viene aggiunto al progetto in **Esplora soluzioni**.  
  
4. Aprire il file AvoidWaitForDelayRule.cs e aggiungere al file le istruzioni Using seguenti:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;   
    namespace SampleRules {  
        class AvoidWaitForDelayRule {}  
    }  
    ```  
  
5. Nella dichiarazione della classe AvoidWaitForDelayRule impostare il modificatore di accesso su public:  
  
    ```  
    /// <summary>  
    /// This is a rule that returns a warning message   
    /// whenever there is a WAITFOR DELAY statement appears inside a subroutine body.  
    /// This rule only applies to stored procedures, functions and triggers.  
    /// </summary>  
    public sealed class AvoidWaitForDelayRule  
    ```  
  
6. Derivare la classe AvoidWaitForDelayRule dalla classe di base Microsoft.SqlServer.Dac.CodeAnalysis.SqlCodeAnalysisRule:  
  
    ```  
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    ```  
  
7. Aggiungere LocalizedExportCodeAnalysisRuleAttribute alla classe.  
  
    LocalizedExportCodeAnalysisRuleAttribute consente al servizio di analisi del codice di individuare le regole di analisi del codice personalizzate. Solo le classi contrassegnate con ExportCodeAnalysisRuleAttribute (o con un attributo che eredita da questa classe) possono essere usate nell'analisi del codice.  
  
    LocalizedExportCodeAnalysisRuleAttribute fornisce alcuni metadati necessari usati dal servizio, tra cui un ID univoco per questa regola, un nome visualizzato che verrà visualizzato nell'interfaccia utente di Visual Studio e una descrizione che può essere usata dalla regola durante l'identificazione dei problemi.  
  
    ```  
    [LocalizedExportCodeAnalysisRule(AvoidWaitForDelayRule.RuleId,  
        RuleConstants.ResourceBaseName,  
        RuleConstants.AvoidWaitForDelay_RuleName,   
        RuleConstants.AvoidWaitForDelay_ProblemDescription  
        Category = RuleConstants.CategoryPerformance,   
        RuleScope = SqlRuleScope.Element)]   
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    {  
       /// <summary>  
       /// The Rule ID should resemble a fully-qualified class name. In the Visual Studio UI  
       /// rules are grouped by "Namespace + Category", and each rule is shown using "Short ID: DisplayName".  
       /// For this rule, that means the grouping will be "Public.Dac.Samples.Performance", with the rule  
       /// shown as "SR1004: Avoid using WaitFor Delay statements in stored procedures, functions and triggers."  
       /// </summary>  
       public const string RuleId = "RuleSamples.SR1004";  
    }  
    ```  
  
    La proprietà RuleScope deve essere Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Element quando la regola analizza elementi specifici. La regola verrà chiamata una volta per ogni elemento corrispondente nel modello. Se si vuole analizzare un intero modello è possibile usare invece Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Model.  
  
8. Aggiungere un costruttore che consente di impostare Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.SupportedElementTypes. Questa operazione è necessaria per le regole il cui ambito è costituito da elementi. Definisce i tipi di elementi a cui verrà applicata questa regola. In questo caso, la regola verrà applicata a stored procedure, trigger e funzioni. Si noti che la classe Microsoft.SqlServer.Dac.Model.ModelSchema elenca tutti i tipi di elementi disponibili che possono essere analizzati.  
  
    ```  
    public AvoidWaitForDelayRule()  
    {  
       // This rule supports Procedures, Functions and Triggers. Only those objects will be passed to the Analyze method  
       SupportedElementTypes = new[]  
       {  
          // Note: can use the ModelSchema definitions, or access the TypeClass for any of these types  
          ModelSchema.ExtendedProcedure,  
          ModelSchema.Procedure,  
          ModelSchema.TableValuedFunction,  
          ModelSchema.ScalarFunction,  
  
          ModelSchema.DatabaseDdlTrigger,  
          ModelSchema.DmlTrigger,  
          ModelSchema.ServerDdlTrigger  
       };  
    }  
    ```  
  
9. Aggiungere una sostituzione per il metodo Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.Analyze(Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext), che usa Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext come parametri di input. Questo metodo restituisce un elenco di potenziali problemi.  
  
    Il metodo ottiene Microsoft.SqlServer.Dac.Model.TSqlModel, Microsoft.SqlServer.Dac.Model.TSqlObject, e [TSqlFragment](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlfragment) dal parametro di contesto. La classe WaitForDelayVisitor viene quindi usata per ottenere un elenco di tutte le istruzioni WAITFOR DELAY nel modello.  
  
    Per ogni [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement) in elenco, viene creato un oggetto Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleProblem.  
  
    ```  
    /// <summary>  
    /// For element-scoped rules the Analyze method is executed once for every matching   
    /// object in the model.   
    /// </summary>  
    /// <param name="ruleExecutionContext">The context object contains the TSqlObject being   
    /// analyzed, a TSqlFragment  
    /// that's the AST representation of the object, the current rule's descriptor, and a   
    /// reference to the model being  
    /// analyzed.  
    /// </param>  
    /// <returns>A list of problems should be returned. These will be displayed in the Visual   
    /// Studio error list</returns>  
    public override IList<SqlRuleProblem> Analyze(  
        SqlRuleExecutionContext ruleExecutionContext)  
    {  
         IList<SqlRuleProblem> problems = new List<SqlRuleProblem>();  
  
         TSqlObject modelElement = ruleExecutionContext.ModelElement;  
  
         // this rule does not apply to inline table-valued function  
         // we simply do not return any problem in that case.  
         if (IsInlineTableValuedFunction(modelElement))  
         {  
             return problems;  
         }  
  
         string elementName = GetElementName(ruleExecutionContext, modelElement);  
  
         // The rule execution context has all the objects we'll need, including the   
         // fragment representing the object,  
         // and a descriptor that lets us access rule metadata  
         TSqlFragment fragment = ruleExecutionContext.ScriptFragment;  
         RuleDescriptor ruleDescriptor = ruleExecutionContext.RuleDescriptor;  
  
         // To process the fragment and identify WAITFOR DELAY statements we will use a   
         // visitor   
         WaitForDelayVisitor visitor = new WaitForDelayVisitor();  
         fragment.Accept(visitor);  
         IList<WaitForStatement> waitforDelayStatements = visitor.WaitForDelayStatements;  
  
         // Create problems for each WAITFOR DELAY statement found   
         // When creating a rule problem, always include the TSqlObject being analyzed. This   
         // is used to determine  
         // the name of the source this problem was found in and a best guess as to the   
         // line/column the problem was found at.  
         //  
         // In addition if you have a specific TSqlFragment that is related to the problem   
         //also include this  
         // since the most accurate source position information (start line and column) will   
         // be read from the fragment  
         foreach (WaitForStatement waitForStatement in waitforDelayStatements)  
         {  
            SqlRuleProblem problem = new SqlRuleProblem(  
                String.Format(CultureInfo.CurrentCulture,   
                    ruleDescriptor.DisplayDescription, elementName),  
                modelElement,  
                waitForStatement);  
            problems.Add(problem);  
        }  
        return problems;  
    }  
  
    private static string GetElementName(  
        SqlRuleExecutionContext ruleExecutionContext,   
        TSqlObject modelElement)  
    {  
        // Get the element name using the built in DisplayServices. This provides a number of   
        // useful formatting options to  
        // make a name user-readable  
        var displayServices = ruleExecutionContext.SchemaModel.DisplayServices;  
        string elementName = displayServices.GetElementName(  
            modelElement, ElementNameStyle.EscapedFullyQualifiedName);  
        return elementName;  
    }  
  
    private static bool IsInlineTableValuedFunction(TSqlObject modelElement)  
    {  
        return TableValuedFunction.TypeClass.Equals(modelElement.ObjectType)  
                       && FunctionType.InlineTableValuedFunction ==   
            modelElement.GetMetadata<FunctionType>(TableValuedFunction.FunctionType);  
    }  
    ```  
  
10. Fare clic su **File** > **Salva**.  
  
### <a name="building-the-class-library"></a>Compilazione della libreria di classi  
  
1. Scegliere **Proprietà SampleRules** dal menu **Progetto**.  
  
2. Fare clic sulla scheda **Firma** .  
  
3. Fare clic su **Firma assembly**.  
  
4. In **Scegli un file chiave con nome sicuro** fare clic su **<New>** .  
  
5. Nella finestra di dialogo **Crea chiave con nome sicuro** in **Nome file di chiave** digitare MyRefKey.  
  
6. (facoltativo) È possibile specificare una password per il file di chiave con nome sicuro.  
  
7. Fare clic su **OK**.  
  
8. Scegliere **Salva tutti** dal menu **File**.  
  
9. Nel menu **Compila** scegliere **Compila soluzione**.  
  
È quindi necessario installare l'assembly in modo che venga caricato quando si compilano e distribuiscono progetti di SQL Server.  
  
## <a name="install-a-static-code-analysis-rule"></a>Installare una regola di analisi del codice statica

Per installare una regola, è necessario copiare l'assembly e il file con estensione pdb associato nella cartella delle estensioni.  
  
### <a name="to-install-the-samplerules-assembly"></a>Per installare l'assembly SampleRules

Nella prossima esercitazione verrà illustrato come copiare le informazioni sull'assembly nella directory Estensioni. All'avvio di Visual Studio, le eventuali estensioni vengono identificate nella directory <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions e relative sottodirectory, quindi vengono rese disponibili per l'uso.  
  
Per Visual Studio 2012, <Visual Studio Install Dir> sarà in genere C:\Programmi (x86)\\MicrosoftVisual Studio 11.0. Per Visual Studio 2013 questa directory sarà in genere C:\Programmi (x86)\\MicrosoftVisual Studio 12.0.  
  
Copiare il file di assembly SampleRules.dll dalla directory di output nella directory <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions. Per impostazione predefinita, il percorso del file con estensione dll compilato è PercorsoSoluzione\PercorsoProgetto\bin\Debug o PercorsoSoluzione\PercorsoProgetto\bin\Release.  
  
La regola verrà quindi installata e verrà visualizzata al riavvio di Visual Studio. Nella prossima esercitazione verrà illustrato come avviare una nuova sessione di Visual Studio e creare un progetto di database.  
  
### <a name="starting-a-new-visual-studio-session-and-creating-a-database-project"></a>Avvio di una nuova sessione di Visual Studio e creazione di un progetto di database  
  
1. Avviare una seconda sessione di Visual Studio.  
  
2. Fare clic su **File** > **Nuovo** > **Progetto**.  
  
3. Nella finestra di dialogo **Nuovo progetto** nell'elenco **Modelli installati** espandere il nodo **SQL Server** e fare clic su **Progetto di database di SQL Server**.  
  
4. Nella casella di testo **Nome** digitare SampleRulesDB e fare clic su **OK**.  
  
La nuova regola verrà visualizzata nel progetto di SQL Server. Per visualizzare la nuova regola di analisi del codice AvoidWaitForRule:  
  
1. In **Esplora soluzioni** selezionare il progetto SampleRulesDB.  
  
2. Scegliere **Proprietà** dal menu **Progetto**. Viene visualizzata la pagina delle proprietà di SampleRulesDB.  
  
3. Fare clic su **Analisi codice**. Verrà visualizzata una nuova categoria denominata RuleSamples.CategorySamples.  
  
4. Espandere RuleSamples. CategorySamples. L'istruzione SR1004: Avoid WAITFOR DELAY verrà visualizzata in stored procedure, trigger e funzioni.  
  
## <a name="see-also"></a>Vedere anche

[Panoramica dell'estendibilità delle regole di analisi del codice del database](../ssdt/overview-of-extensibility-for-database-code-analysis-rules.md)