---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/18/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: f6ec3bbcc720fe472cc7146d59c30f0f3fc65ab0
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833756"
---
+ RExtension richiede GLIBCXX_3.4.20. Assicurarsi che la versione di **libstdc + +. so. 6** nell'installazione di Red Hat Enterprise Linux (RHEL) fornisca questa operazione.

## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Eseguire il comando seguente per installare [SQL Server Language Extensions](../../../language-extensions/language-extensions-overview.md) in Red Hat Enterprise Linux (RHEL), usato per il runtime di R Custom.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-r"></a>Installare R

1. Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è stato installato, R è già installato in `/opt/microsoft/ropen/3.5.2/lib64/R` . Se si desidera continuare a utilizzare questo percorso come R_HOME, è possibile ignorare questo passaggio.

    Se si vuole usare un runtime diverso di R, prima di continuare con l'installazione di una nuova versione è necessario rimuovere `microsoft-r-open-mro`.

    ```bash
    sudo yum erase microsoft-r-open-mro-3.5.2
    ```

1. Installare [R (3,3 o versione successiva)](https://www.r-project.org/) per Red Hat Enterprise Linux (RHEL). Per impostazione predefinita, R viene installato in **/usr/lib64/R**. Questo percorso corrisponde a **R_HOME**. Se si installa R in un percorso diverso, prendere nota del percorso come **R_HOME**.

    ```bash
    sudo yum install -y R
    ```
