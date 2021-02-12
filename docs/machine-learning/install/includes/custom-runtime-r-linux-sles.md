---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a10c54f61151835692f2bc05de440062fe2f3cfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072792"
---
## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Eseguire il comando seguente per installare [SQL Server Language Extensions](../../../language-extensions/language-extensions-overview.md) in SUSE Linux Enterprise Server (SLES), usato per il runtime di R Custom.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Installare R

1. Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è stato installato, R è già installato in `/opt/microsoft/ropen/3.5.2/lib64/R` . Se si desidera continuare a utilizzare questo percorso come R_HOME, è possibile ignorare questo passaggio.

    Se si vuole usare un runtime diverso di R, prima di continuare con l'installazione di una nuova versione è necessario rimuovere `microsoft-r-open-mro`.

    ```bash
    sudo zypper remove microsoft-r-open-mro-3.5.2
    ```

1. Installare [R (3,3 o versione successiva)](https://www.r-project.org/) per SUSE Linux Enterprise Server (SLES). Per impostazione predefinita, R viene installato in **/usr/lib/R**. Questo percorso corrisponde a **R_HOME**. Se si installa R in un percorso diverso, prendere nota del percorso come **R_HOME**.
