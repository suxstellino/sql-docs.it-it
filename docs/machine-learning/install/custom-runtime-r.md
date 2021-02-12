---
title: Installare il runtime personalizzato di R
description: Informazioni su come installare un runtime personalizzato R per SQL Server usando le estensioni del linguaggio. Il runtime personalizzato di Python può eseguire script di machine learning.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 17e4a885281cf428e8a5051b4060199b2824bd90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072756"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Installare un runtime personalizzato di R per SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni su come installare un runtime personalizzato R per l'esecuzione di script R esterni con SQL Server in:

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

Il runtime personalizzato può eseguire script di machine learning e usa le [estensioni del linguaggio SQL Server](../../language-extensions/language-extensions-overview.md).

Usare la propria versione del runtime di R con SQL Server, anziché la versione di runtime predefinita installata con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [R custom runtime - Windows](includes/custom-runtime-r-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-r-linux-ubuntu.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - RHEL specific steps](includes/custom-runtime-r-linux-rhel.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - SLES specific steps](includes/custom-runtime-r-linux-sles.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Abilitare lo script esterno

È possibile eseguire uno script esterno di Python con la stored procedure [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Per abilitare gli script esterni, usare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) per eseguire l'istruzione seguente.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Verificare l'installazione

Usare lo script SQL seguente per verificare l'installazione e la funzionalità del runtime personalizzato di Python.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="next-steps"></a>Passaggi successivi

+ [Installare un runtime personalizzato di Python per SQL Server](custom-runtime-python.md)
+ [Framework di estendibilità in SQL Server](../concepts/extensibility-framework.md)
+ [Panoramica delle estensioni del linguaggio](../../language-extensions/language-extensions-overview.md)