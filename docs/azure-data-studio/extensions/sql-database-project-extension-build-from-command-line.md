---
title: Compilare un progetto dalla riga di comando
description: Compilare progetti di database di SQL Server dalla riga di comando
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 08/07/2020
ms.openlocfilehash: 1233b8319c4fbd893159ba88fbcd1c3401f05ce0
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495719"
---
# <a name="build-a-database-project-from-command-line"></a>Compilare un progetto di database dalla riga di comando

Mentre l'estensione di progetto di database SQL (anteprima) per Azure Data Studio fornisce un'interfaccia utente grafica per la [compilazione di un progetto di database](sql-database-project-extension-build.md), è disponibile anche un'esperienza di compilazione da riga di comando per ambienti Windows, MacOS e Linux. Questo articolo illustra la sintassi e i prerequisiti necessari per compilare un progetto SQL nel file DACPAC dalla riga di comando.

## <a name="prerequisites"></a>Prerequisiti

1. Installare e configurare l'[estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension.md).

2. I file DLL di .NET Core seguenti e il file di destinazione `Microsoft.Data.Tools.Schema.SqlTasks.targets` sono necessari per compilare un progetto di database SQL dalla riga di comando supportata da tutte le piattaforme supportate dall'estensione di Azure Data Studio per i progetti di database SQL. Questi file vengono creati dall'estensione durante la prima compilazione completata nell'interfaccia di Azure Data Studio e vengono salvati nella cartella dell'estensione in `BuildDirectory`.  In Linux questi file sono salvati in `~\.azuredatastudio\extensions\microsoft.sql-database-projects-x.x.x\BuildDirectory\`.  Copiare questi 10 file in una cartella nuova e accessibile o annotarne il percorso.  Il percorso verrà indicato come `DotNet Core build folder` in questo documento.

    - Microsoft.Data.Tools.Schema.Sql.dll
    - Microsoft.Data.Tools.Schema.Tasks.Sql.dll
    - Microsoft.Data.Tools.Utilities.dll
    - System.Io.Packaging.dll
    - Microsoft.SqlServer.Dac.dll
    - Microsoft.SqlServer.Dac.Extensions.dll
    - Microsoft.SqlServer.TransactSql.ScriptDom.dll
    - Microsoft.SqlServer.Types.dll
    - Microsoft.Data.Tools.Schema.SqlTasks.targets
    - System.ComponentModel.Composition.dll
    - Microsoft.Data.SqlClient.dll

3. Se il progetto è stato creato in Azure Data Studio, passare a [Compilare il progetto dalla riga di comando](#build-the-project-from-the-command-line). Se il progetto è stato creato in SQL Server Data Tools (SSDT), aprire il progetto nell'estensione Progetto di database SQL di Azure Data Studio.  Se si apre un progetto in Azure Data Studio, il file `sqlproj` verrà aggiornato automaticamente con tre modifiche, riportate di seguito per finalità informative:

    1. Condizioni di importazione

    ```console
    <Import Condition="'$(NetCoreBuild)' == 'true'" Project="$(NETCoreTargetsPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/> 
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' != ''" Project="$(SQLDBExtensionsRefPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' == ''" Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    ```

    2. Informazioni di riferimento sui pacchetti

    ```console
    <ItemGroup>
        <PackageReference Condition="'$(NetCoreBuild)' == 'true'" Include="Microsoft.NETFramework.ReferenceAssemblies" Version="1.0.0" PrivateAssets="All"/>
    </ItemGroup>
    ```

    3. Destinazione pulita, necessaria per il supporto della modifica doppia in SQL Server Data Tools (SSDT) e Azure Data Studio

        ```console
        <Target Name="AfterClean">
            <Delete Files="$(BaseIntermediateOutputPath)\project.assets.json"/>
        </Target>
        ```

## <a name="build-the-project-from-the-command-line"></a>Compilare il progetto dalla riga di comando

Dalla cartella .NET completa usare il comando seguente:

```console
dotnet build "<sqlproj file path>" /p:NetCoreBuild=true /p:NETCoreTargetsPath="<DotNet Core build folder>"
```

Ad esempio, da `/usr/share/dotnet` in Linux:

```console
dotnet build "/home/myuser/Documents/DatabaseProject1/DatabaseProject1.sqlproj" /p:NetCoreBuild=true /p:NETCoreTargetsPath="/home/myuser/.azuredatastudio-insiders/extensions/microsoft.sql-database-projects-0.1.2/BuildDirectory"  
```

## <a name="next-steps"></a>Passaggi successivi

- [Estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension.md)
- [Pubblicare i progetti di database SQL](sql-database-project-extension-build.md#publish-a-database-project)
