---
title: Installare il runtime personalizzato di Python
description: Informazioni su come installare un runtime personalizzato di Python per SQL Server tramite le estensioni del linguaggio. Il runtime personalizzato di Python può eseguire script di machine learning.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 22ba1ff16d22582be39e7b48a297e9dc65f9f554
ms.sourcegitcommit: e4b71e5d432a29b6c76ea457b00aa0abd4b6c77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2021
ms.locfileid: "106273472"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installare un runtime personalizzato di Python per SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni su come installare un runtime personalizzato Python per l'esecuzione di script Python esterni con SQL Server in:

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

Il runtime personalizzato può eseguire script di machine learning e usa le [estensioni del linguaggio SQL Server](../../language-extensions/language-extensions-overview.md).

Usare la propria versione del runtime di Python con SQL Server, anziché la versione di runtime predefinita installata con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [Python custom runtime - Windows](includes/custom-runtime-python-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-python-linux-ubuntu.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - RHEL specific steps](includes/custom-runtime-python-linux-rhel.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - SLES specific steps](includes/custom-runtime-python-linux-sles.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
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
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Passaggi successivi

+ [Installare un runtime personalizzato di R per SQL Server](custom-runtime-r.md)
+ [Framework di estendibilità in SQL Server](../concepts/extensibility-framework.md)
+ [Panoramica delle estensioni del linguaggio](../../language-extensions/language-extensions-overview.md)
