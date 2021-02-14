---
title: Estendere la compilazione del progetto di database per generare statistiche del modello
description: Informazioni su come creare, installare e testare un collaboratore alla compilazione che restituisce statistiche dal modello di database SQL quando si compila un progetto di database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: d44935ce-63bf-46df-976a-5a54866c8119
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5e3905a3ded61ac6ef99f9ada4e94ffaab77a36c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016681"
---
# <a name="walkthrough-extend-database-project-build-to-generate-model-statistics"></a>Procedura dettagliata: Estendere la compilazione del progetto del database per generare statistiche del modello

È possibile creare un collaboratore alla compilazione per eseguire azioni personalizzate quando si compila un progetto di database. In questa procedura dettagliata, si crea un collaboratore alla compilazione denominato ModelStatistics che restituisce statistiche dal modello di database SQL quando si compila un progetto di database. Poiché questo collaboratore alla compilazione accetta i parametri durante la compilazione, sono richiesti alcuni passaggi aggiuntivi.  
  
In questa procedura dettagliata, vengono eseguite le attività principali seguenti:  
  
-   [Creare un collaboratore alla compilazione](#CreateBuildContributor)  
  
-   [Installare il collaboratore alla compilazione](#InstallBuildContributor)  
  
-   [Testare il collaboratore alla compilazione](#TestBuildContributor)  
  
## <a name="prerequisites"></a>Prerequisiti  
Per completare questa procedura dettagliata, è necessario disporre dei componenti seguenti:  
  
-   È necessario avere installata una versione di Visual Studio che includa SQL Server Data Tools (SSDT) e supporti lo sviluppo in C# o VB.  
  
-   È necessario disporre di un progetto SQL contenente oggetti SQL.  
  
> [!NOTE]  
> Questa procedura dettagliata è destinata ad utenti che abbiano già familiarità con le funzionalità SQL di SSDT. È inoltre necessario che l'utente conosca i concetti di base di Visual Studio, ad esempio come creare una libreria di classi e come utilizzare l'editor di codice per aggiungere codice a una classe.  
  
## <a name="build-contributor-background"></a>Informazioni di supporto sui collaboratori alla compilazione  
I collaboratori alla compilazione vengono eseguiti durante la compilazione del progetto, dopo che il modello che rappresenta il progetto viene generato ma prima che il progetto venga salvato su disco. Possono essere utilizzati per diversi scenari, ad esempio  
  
-   Convalida del contenuto del modello e segnalazione degli errori di convalida al chiamante. Questa operazione può essere eseguita aggiungendo gli errori a un elenco passato come parametro al metodo OnExecute.  
  
-   Generazione di statistiche del modello e segnalazione all'utente. Questo è l'esempio qui riportato.  
  
Il punto di ingresso principale per i collaboratori alla compilazione è il metodo OnExecute. Tutte le classi che ereditano da BuildContributor devono implementare questo metodo. A questo metodo viene passato un oggetto BuildContributorContext che contiene tutti i dati pertinenti per la compilazione, ad esempio il modello del database, le proprietà della compilazione e gli argomenti o file che dovranno essere usati da chi collabora alla compilazione.  
  
**TSqlModel e API del modello di database**  
  
L'oggetto più utile è il modello di database, rappresentato da un oggetto TSqlModel. Si tratta di una rappresentazione logica di un database che include tutte le tabelle, la vista e altri elementi, oltre alle reazioni tra di essi. È disponibile uno schema fortemente tipizzato che è possibile utilizzare per eseguire query per tipi specifici di elementi e attraversare le relazioni interessanti. Nel codice della procedura dettagliata verranno riportati esempi di utilizzo.  
  
Di seguito sono riportati alcuni dei comandi utilizzati dal collaboratore di esempio della procedura dettagliata:  
  
|**Classe**|**Metodo/proprietà**|**Descrizione**|  
|-------------|------------------------|-------------------|  
|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)|GetObjects()|Esegue una query al modello per gli oggetti ed è il punto di ingresso principale all'API del modello. È possibile eseguire query solo sui tipi di livello superiore, ad esempio, le tabelle o le viste. Altri tipi, ad esempio le colonne, si possono individuare solo con l'attraversamento del modello. Se non è specificato alcun filtro ModelTypeClass, verranno restituiti tutti i tipi di livello superiore.|  
|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|GetReferencedRelationshipInstances()|Trova le relazioni agli elementi a cui l'elemento TSqlObject corrente fa riferimento. Ad esempio, per una tabella vengono restituiti oggetti come le colonne della tabella. In questo caso è possibile usare un filtro ModelRelationshipClass per specificare le relazioni esatte su cui eseguire la query. Ad esempio, usando il filtro "Table.Columns" si assicura che vengano restituite solo le colonne.<br /><br />Sono presenti vari metodi simili, quali GetReferencingRelationshipInstances, GetChildren e GetParent. Per ulteriori informazioni, vedere la documentazione dell'API.|  
  
**Identificazione univoca del collaboratore**  
  
Durante il processo di compilazione, i collaboratori personalizzati vengono caricati da una directory di estensioni standard. I collaboratori alla compilazione sono identificati da un attributo [ExportBuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportbuildcontributorattribute). Questo attributo è richiesto per consentire di individuare i collaboratori. Dovrebbe essere simile al seguente:  
  
```  
[ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
  
```  
  
In questo caso il primo parametro all'attributo deve essere un identificatore univoco, che verrà usato per identificare il collaboratore nei file del progetto. È consigliabile combinare lo spazio dei nomi della libreria (in questa procedura dettagliata "ExampleContributors") con il nome della classe (in questa procedura dettagliata "ModelStatistics"), per produrre l'identificatore. Verrà illustrato come questo spazio dei nomi viene utilizzato per specificare che il collaboratore deve essere eseguito più avanti nella procedura dettagliata.  
  
## <a name="create-a-build-contributor"></a><a name="CreateBuildContributor"></a>Creare un collaboratore alla compilazione  
Per creare un collaboratore alla compilazione, è necessario effettuare le attività seguenti:  
  
-   Creare un progetto Libreria di classi e aggiungere i riferimenti richiesti.  
  
-   Definire una classe denominata ModelStatistics che erediti da [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor).  
  
-   Eseguire l'override del metodo OnExecute.  
  
-   Aggiungere alcuni metodi helper privati.  
  
-   Compilare l'assembly risultante.  
  
#### <a name="to-create-a-class-library-project"></a>Per creare un progetto Libreria di classi  
  
1.  Creare un progetto Libreria di classi di Visual Basic o Visual C# denominato MyBuildContributor.  
  
2.  Rinominare il file "Class1.cs" in "ModelStatistics.cs".  
  
3.  In Esplora soluzioni fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Aggiungi riferimento**.  
  
4.  Selezionare la voce **System.ComponentModel.Composition** e fare clic su **OK**.  
  
5.  Aggiungere i riferimenti SQL richiesti: fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Aggiungi riferimento**. Fare clic sul pulsante **Sfoglia** . Passare alla cartella **C:\Programmi (x86)\Microsoft SQL Server\110\DAC\Bin**. Scegliere le voci **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** e **Microsoft.Data.Tools.Schema.Sql.dll** , quindi fare clic su **OK**.  
  
    Iniziare quindi ad aggiungere codice alla classe.  
  
#### <a name="to-define-the-modelstatistics-class"></a>Per definire la classe ModelStatistics  
  
1.  La classe ModelStatistics elabora il modello di database passato al metodo OnExecute e produce e un report XML che illustra in dettaglio il contenuto del modello.  
  
    Nell'editor del codice, aggiornare il file ModelStatistics.cs affinché corrisponda a quanto segue:  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.IO;  
    using System.Linq;  
    using System.Xml.Linq;  
    using Microsoft.Data.Schema;  
    using Microsoft.Data.Schema.Build;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema.SchemaModel;  
    using Microsoft.Data.Schema.Sql;  
  
    namespace ExampleContributors  
    {  
    /// <summary>  
        /// A BuildContributor that generates statistics about a model and saves this to the output directory.  
        /// Will only run if a "GenerateModelStatistics=true" contributor argument is set in the project file, or a targets file.   
        /// Statistics can be sorted by "none, "name" or "value", with "none" being the default sort behavior.  
        ///   
        /// To set contributor arguments in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
        ///     </ContributorArguments>  
        /// <PropertyGroup>      
        ///   
        /// This will generate model statistics when building in Debug mode only - remove the condition to generate in all build modes.  
        /// </summary>  
        [ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
        public class ModelStatistics : BuildContributor  
        {  
            public const string GenerateModelStatistics = "ModelStatistics.GenerateModelStatistics";  
            public const string SortModelStatisticsBy = "ModelStatistics.SortModelStatisticsBy";  
            public const string OutDir = "ModelStatistics.OutDir";  
            public const string ModelStatisticsFilename = "ModelStatistics.xml";  
            private enum SortBy { None, Name, Value };  
            private static Dictionary<string, SortBy> SortByMap = new Dictionary<string, SortBy>(StringComparer.OrdinalIgnoreCase)  
            {  
                { "none", SortBy.None },  
                { "name", SortBy.Name },  
                { "value", SortBy.Value },  
            };  
  
            private SortBy _sortBy = SortBy.None;  
  
            /// <summary>  
            /// Override the OnExecute method to perform actions when you build a database project.  
            /// </summary>  
            protected override void OnExecute(BuildContributorContext context, IList<ExtensibilityError> errors)  
            {  
                // handle related arguments, passed in as part of  
                // the context information.  
                bool generateModelStatistics;  
                ParseArguments(context.Arguments, errors, out generateModelStatistics);  
  
                // Only generate statistics if requested to do so  
                if (generateModelStatistics)  
                {  
                    // First, output model-wide information, such  
                    // as the type of database schema provider (DSP)  
                    // and the collation.  
                    StringBuilder statisticsMsg = new StringBuilder();  
                    statisticsMsg.AppendLine(" ")  
                                 .AppendLine("Model Statistics:")  
                                 .AppendLine("===")  
                                 .AppendLine(" ");  
                    errors.Add(new ExtensibilityError(statisticsMsg.ToString(), Severity.Message));  
  
                    var model = context.Model;  
  
                    // Start building up the XML that will later  
                    // be serialized.  
                    var xRoot = new XElement("ModelStatistics");  
  
                    SummarizeModelInfo(model, xRoot, errors);  
  
                    // First, count the elements that are contained   
                    // in this model.  
                    IList<TSqlObject> elements = model.GetObjects(DacQueryScopes.UserDefined).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "UserDefinedElements", xRoot, errors);  
  
                    // Now, count the elements that are defined in  
                    // another model. Examples include built-in types,  
                    // roles, filegroups, assemblies, and any   
                    // referenced objects from another database.  
                    elements = model.GetObjects(DacQueryScopes.BuiltIn | DacQueryScopes.SameDatabase | DacQueryScopes.System).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "OtherElements", xRoot, errors);  
  
                    // Now, count the number of each type  
                    // of relationship in the model.  
                    SurveyRelationships(model, xRoot, errors);  
  
                    // Determine where the user wants to save  
                    // the serialized XML file.  
                    string outDir;  
                    if (context.Arguments.TryGetValue(OutDir, out outDir) == false)  
                    {  
                        outDir = ".";  
                    }  
                    string filePath = Path.Combine(outDir, ModelStatisticsFilename);  
                    // Save the XML file and tell the user  
                    // where it was saved.  
                    xRoot.Save(filePath);  
                    ExtensibilityError resultArg = new ExtensibilityError("Result was saved to " + filePath, Severity.Message);  
                    errors.Add(resultArg);  
                }  
            }  
  
            /// <summary>  
            /// Examine the arguments provided by the user  
            /// to determine if model statistics should be generated  
            /// and, if so, how the results should be sorted.  
            /// </summary>  
            private void ParseArguments(IDictionary<string, string> arguments, IList<ExtensibilityError> errors, out bool generateModelStatistics)  
            {  
                // By default, we don't generate model statistics  
                generateModelStatistics = false;  
  
                // see if the user provided the GenerateModelStatistics   
                // option and if so, what value was it given.  
                string valueString;  
                arguments.TryGetValue(GenerateModelStatistics, out valueString);  
                if (string.IsNullOrWhiteSpace(valueString) == false)  
                {  
                    if (bool.TryParse(valueString, out generateModelStatistics) == false)  
                    {  
                        generateModelStatistics = false;  
  
                        // The value was not valid from the end user  
                        ExtensibilityError invalidArg = new ExtensibilityError(  
                            GenerateModelStatistics + "=" + valueString + " was not valid.  It can be true or false", Severity.Error);  
                        errors.Add(invalidArg);  
                        return;  
                    }  
                }  
  
                // Only worry about sort order if the user requested  
                // that we generate model statistics.  
                if (generateModelStatistics)  
                {  
                    // see if the user provided the sort option and  
                    // if so, what value was provided.  
                    arguments.TryGetValue(SortModelStatisticsBy, out valueString);  
                    if (string.IsNullOrWhiteSpace(valueString) == false)  
                    {  
                        SortBy sortBy;  
                        if (SortByMap.TryGetValue(valueString, out sortBy))  
                        {  
                            _sortBy = sortBy;  
                        }  
                        else  
                        {  
                            // The value was not valid from the end user  
                            ExtensibilityError invalidArg = new ExtensibilityError(  
                                SortModelStatisticsBy + "=" + valueString + " was not valid.  It can be none, name, or value", Severity.Error);  
                            errors.Add(invalidArg);  
                        }  
                    }  
                }  
            }  
  
            /// <summary>  
            /// Retrieve the database schema provider for the  
            /// model and the collation of that model.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SummarizeModelInfo(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // use a Dictionary to accumulate the information  
                // that will later be output.  
                var info = new Dictionary<string, string>();  
  
                // Two things of interest: the database schema  
                // provider for the model, and the language id and  
                // case sensitivity of the collation of that  
                // model  
                info.Add("Version", model.Version.ToString());  
  
                TSqlObject options = model.GetObjects(DacQueryScopes.UserDefined, DatabaseOptions.TypeClass).FirstOrDefault();  
                if (options != null)  
                {  
                    info.Add("Collation", options.GetProperty<string>(DatabaseOptions.Collation));  
                }  
  
                // Output the accumulated information and add it to   
                // the XML.  
                OutputResult("Basic model info", info, xContainer, errors);  
            }  
  
            /// <summary>  
            /// For a provided list of model elements, count the number  
            /// of elements for each class name, sorted as specified  
            /// by the user.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private void Summarize<T>(IList<T> set, Func<T, string> groupValue, string category, XElement xContainer, IList<ExtensibilityError> errors)  
            { // Use a Dictionary to keep all summarized information  
                var statistics = new Dictionary<string, int>();  
  
                // For each element in the provided list,  
                // count items based on the specified grouping  
                var groups =  
                    from item in set  
                    group item by groupValue(item) into g  
                    select new { g.Key, Count = g.Count() };  
  
                // order the groups as requested by the user  
                if (this._sortBy == SortBy.Name)  
                {  
                    groups = groups.OrderBy(group => group.Key);  
                }  
                else if (this._sortBy == SortBy.Value)  
                {  
                    groups = groups.OrderBy(group => group.Count);  
                }  
  
                // build the Dictionary of accumulated statistics  
                // that will be passed along to the OutputResult method.  
                foreach (var item in groups)  
                {  
                    statistics.Add(item.Key, item.Count);  
                }  
  
                statistics.Add("subtotal", set.Count);  
                statistics.Add("total items", groups.Count());  
  
                // output the results, and build up the XML  
                OutputResult(category, statistics, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Iterate over all model elements, counting the  
            /// styles and types for relationships that reference each   
            /// element  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SurveyRelationships(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // get a list that contains all elements in the model  
                var elements = model.GetObjects(DacQueryScopes.All);  
                // We are interested in all relationships that  
                // reference each element.  
                var entries =  
                    from element in elements  
                    from entry in element.GetReferencedRelationshipInstances(DacExternalQueryScopes.All)  
                    select entry;  
  
                // initialize our counting buckets  
                var composing = 0;  
                var hierachical = 0;  
                var peer = 0;  
  
                // process each relationship, adding to the   
                // appropriate bucket for style and type.  
                foreach (var entry in entries)  
                {  
                    switch (entry.Relationship.Type)  
                    {  
                        case RelationshipType.Composing:  
                            ++composing;  
                            break;  
                        case RelationshipType.Hierarchical:  
                            ++hierachical;  
                            break;  
                        case RelationshipType.Peer:  
                            ++peer;  
                            break;  
                        default:  
                            break;  
                    }  
                }  
  
                // build a dictionary of data to pass along  
                // to the OutputResult method.  
                var stat = new Dictionary<string, int>  
                {  
                    {"Composing", composing},  
                    {"Hierarchical", hierachical},  
                    {"Peer", peer},  
                    {"subtotal", entries.Count()}  
                };  
  
                OutputResult("Relationships", stat, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Performs the actual output for this contributor,  
            /// writing the specified set of statistics, and adding any   
            /// output information to the XML being constructed.  
            /// </summary>  
            private static void OutputResult<T>(string category, Dictionary<string, T> statistics, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                var maxLen = statistics.Max(stat => stat.Key.Length) + 2;  
                var format = string.Format("{{0, {0}}}: {{1}}", maxLen);  
  
                StringBuilder resultMessage = new StringBuilder();  
                //List<ExtensibilityError> args = new List<ExtensibilityError>();  
                resultMessage.AppendLine(category);  
                resultMessage.AppendLine("-----------------");  
  
                // Remove any blank spaces from the category name  
                var xCategory = new XElement(category.Replace(" ", ""));  
                xContainer.Add(xCategory);  
  
                foreach (var item in statistics)  
                {  
                    //Console.WriteLine(format, item.Key, item.Value);  
                    var entry = string.Format(format, item.Key, item.Value);  
                    resultMessage.AppendLine(entry);  
                    // Replace any blank spaces in the element key with  
                    // underscores.  
                    xCategory.Add(new XElement(item.Key.Replace(' ', '_'), item.Value));  
                }  
                resultMessage.AppendLine(" ");  
                errors.Add(new ExtensibilityError(resultMessage.ToString(), Severity.Message));  
            }  
        }  
    }  
  
    ```  
  
    Quindi, si compilerà la libreria di classi.  
  
### <a name="to-sign-and-build-the-assembly"></a>Per firmare e compilare l'assembly  
  
1.  Scegliere **Proprietà di MyBuildContributor** dal menu **Progetto**.  
  
2.  Fare clic sulla scheda **Firma** .  
  
3.  Fare clic su **Firma assembly**.  
  
4.  In **Scegli un file chiave con nome sicuro** fare clic su **<New>** .  
  
5.  Nella finestra di dialogo **Crea chiave con nome sicuro** , in **Nome file di chiave**, digitare **MyRefKey**.  
  
6.  (facoltativo) È possibile specificare una password per il file di chiave con nome sicuro.  
  
7.  Fare clic su **OK**.  
  
8.  Scegliere **Salva tutti** dal menu **File**.  
  
9. Nel menu **Compila** scegliere **Compila soluzione**.  
  
    È quindi necessario installare l'assembly in modo che venga caricato quando si compilano progetti SQL.  
  
## <a name="install-a-build-contributor"></a><a name="InstallBuildContributor"></a>Installare un collaboratore alla compilazione  
Per installare un collaboratore alla compilazione, è necessario copiare l'assembly e il file con estensione pdb associato nella cartella Extensions.  
  
#### <a name="to-install-the-mybuildcontributor-assembly"></a>Per installare l'assembly MyBuildContributor  
  
1.  Nella prossima esercitazione verrà illustrato come copiare le informazioni sull'assembly nella directory Estensioni. Quando si avvia Visual Studio, le estensioni vengono identificate nella directory %Programmi%\Microsoft SQL Server\110\DAC\Bin\Extensions e relative sottodirectory, quindi vengono rese disponibili per l'utilizzo.  
  
2.  Copiare il file di assembly **MyBuildContributor.dll** dalla directory di output alla directory %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions.  
  
    > [!NOTE]  
    > Per impostazione predefinita, il percorso del file con estensione dll compilato è PercorsoSoluzione\PercorsoProgetto\bin\Debug o PercorsoSoluzione\PercorsoProgetto\bin\Release.  
  
## <a name="run-or-test-your-build-contributor"></a><a name="TestBuildContributor"></a>Eseguire o testare il collaboratore alla compilazione  
Per eseguire o testare il collaboratore alla compilazione, è necessario effettuare le attività seguenti:  
  
-   Aggiungere proprietà al file con estensione sqlproj che si intende compilare.  
  
-   Compilare il progetto di database utilizzando MSBuild e fornendo i parametri appropriati.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Aggiungere proprietà al file del progetto SQL (.sqlproj)  
È necessario aggiornare sempre il file del progetto SQL per specificare l'ID dei collaboratori che si desidera eseguire. Inoltre, poiché questo collaboratore alla compilazione accetta parametri della riga di comando da MSBuild, è necessario modificare il progetto SQL per consentire agli utenti di passare tali parametri tramite MSBuild.  
  
Questa operazione può essere eseguita in due modi:  
  
-   È possibile modificare manualmente il file con estensione sqlproj per aggiungere gli argomenti richiesti. È possibile scegliere di eseguire questa operazione se non si desidera riutilizzare il collaboratore alla compilazione in un numero elevato di progetti. Se si sceglie questa opzione, aggiungere le istruzioni seguenti nel file con estensione sqlproj dopo il primo nodo Import nel file  
  
    ```  
    /// <PropertyGroup>  
    ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
    ///         $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
    ///     </ContributorArguments>  
    /// <PropertyGroup>  
  
    ```  
  
-   Il secondo metodo consiste nel creare un file targets contenente gli argomenti del collaboratore richiesti. È utile se si utilizza lo stesso collaboratore per più progetti, dal momento che include i valori predefiniti.  
  
    In questo caso, creare un file targets nel percorso delle estensioni MSBuild:  
  
    1.  Passare a %Program Files%\MSBuild\\.  
  
    2.  Creare una nuova cartella "MyContributors" in cui archiviare i file TARGETS.  
  
    3.  Creare un nuovo file "MyContributors.targets" in questa directory, aggiungere il testo seguente e quindi salvare il file:  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <BuildContributors>$(BuildContributors);ExampleContributors.ModelStatistics</BuildContributors>  
            <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy=name;</ContributorArguments>  
          </PropertyGroup>  
        </Project>  
        ```  
  
    4.  All'interno del file con estensione sqlproj per tutti i progetti per i quali si vogliono eseguire i collaboratori, importare il file targets aggiungendo l'istruzione seguente al file con estensione sqlproj dopo il nodo \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> nel file:  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
        ```  
  
Dopo aver seguito uno di questi approcci, è possibile utilizzare MSBuild per passare i parametri per compilazioni della riga di comando.  
  
> [!NOTE]  
> È necessario aggiornare sempre la proprietà "BuildContributors" per specificare il proprio ID collaboratore. Si tratta dello stesso ID usato nell'attributo "ExportBuildContributor" nel file di origine del collaboratore. Senza questo il collaboratore non verrà eseguito durante la compilazione del progetto. La proprietà "ContributorArguments" deve essere aggiornata solo se si hanno argomenti richiesti per l'esecuzione del collaboratore.  
  
### <a name="build-the-sql-project"></a>Compilare il progetto SQL.  
  
##### <a name="to-rebuild-your-database-project-by-using-msbuild-and-generate-statistics"></a>Per ricompilare il progetto di database utilizzando MSBuild e generare statistiche  
  
1.  In Visual Studio fare clic con il pulsante destro del mouse sul progetto e selezionare "Ricompila". In questo modo il progetto verrà ricompilato ed è necessario verificare le statistiche del modello generate, con l'output incluso nell'output di compilazione e salvato in ModelStatistics.xml. Si noti che può essere necessario scegliere "Mostra tutti i file" in Esplora soluzioni per visualizzare il file XML.  
  
2.  Aprire un prompt dei comandi di Visual Studio: dal menu **Start** scegliere **Tutti i programmi**, fare clic su **Microsoft Visual Studio <Visual Studio Version>**, **Strumenti di Visual Studio** e quindi scegliere **Prompt dei comandi di Visual Studio (<Visual Studio Version>)**.  
  
3.  Al prompt dei comandi, passare alla cartella contenente il progetto SQL.  
  
4.  Al prompt dei comandi digitare il comando seguente:  
  
    ```  
    MSBuild /t:Rebuild MyDatabaseProject.sqlproj /p:BuildContributors=$(BuildContributors);ExampleContributors.ModelStatistics /p:ContributorArguments=$(ContributorArguments);GenerateModelStatistics=true;SortModelStatisticsBy=name;OutDir=.\;  
    ```  
  
    Sostituire *MyDatabaseProject* con il nome del progetto di database che si vuole compilare. Se si è modificato il progetto dopo l'ultima compilazione, è possibile utilizzare /t:Build anziché /t:Rebuild.  
  
    Nell'output vengono visualizzate informazioni sulla compilazione simile alle seguenti:  
  
```  
Model Statistics:  
===  
  
Basic model info  
-----------------  
    Version: Sql110  
  Collation: SQL_Latin1_General_CP1_CI_AS  
  
UserDefinedElements  
-----------------  
  DatabaseOptions: 1  
         subtotal: 1  
      total items: 1  
  
OtherElements  
-----------------  
                Assembly: 1  
       BuiltInServerRole: 9  
           ClrTypeMethod: 218  
  ClrTypeMethodParameter: 197  
         ClrTypeProperty: 20  
                Contract: 6  
                DataType: 34  
                Endpoint: 5  
               Filegroup: 1  
             MessageType: 14  
                   Queue: 3  
                    Role: 10  
                  Schema: 13  
                 Service: 3  
                    User: 4  
         UserDefinedType: 3  
                subtotal: 541  
             total items: 16  
  
Relationships  
-----------------  
     Composing: 477  
  Hierarchical: 6  
          Peer: 19  
      subtotal: 502  
  
```  
  
1.  Aprire ModelStatistics.xml ed esaminarne il contenuto.  
  
    I risultati segnalati sono anche mantenuti nel file XML.  
  
## <a name="next-steps"></a>Passaggi successivi  
È possibile creare strumenti aggiuntivi per eseguire l'elaborazione del file XML di output. Questo è solo un esempio di collaboratore alla compilazione È possibile, ad esempio, creare un collaboratore alla compilazione per restituire un file del dizionario dei dati come parte della compilazione.  
  
## <a name="see-also"></a>Vedere anche  
[Personalizzare la compilazione e la distribuzione del database tramite collaboratori alla compilazione e distribuzione](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Procedura dettagliata: Estendere la distribuzione del progetto di database per analizzare il piano di distribuzione](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
