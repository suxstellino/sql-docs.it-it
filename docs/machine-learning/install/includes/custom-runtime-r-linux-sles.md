---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 70bf06deb43e861209e12b6481f3fd414cb9361f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833780"
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
    sudo zypper remove microsoft-r-open-mro-3.4.4
    ```

1. Installare [R (3,3 o versione successiva)](https://www.r-project.org/) per SUSE Linux Enterprise Server (SLES). Per impostazione predefinita, R viene installato in **/usr/lib64/R**. Questo percorso corrisponde a **R_HOME**. Se si installa R in un percorso diverso, prendere nota del percorso come **R_HOME**.

    Per installare R, seguire questa procedura:

    ```bash
    sudo zypper ar -f http://download.opensuse.org/repositories/devel:/languages:/R:/patched/openSUSE_12.3/ R-patched
    sudo zypper --gpg-auto-import-keys ref
    sudo zypper install R-core-libs R-core R-core-doc R-patched
    ```

    È possibile ignorare gli avvisi per **R-tcltk-3.6.1**, a meno che non sia necessario questo pacchetto.

## <a name="install-gcc-c"></a>Installare gcc-c + +

Installare **gcc-c + +** in SUSE Linux Enterprise Server (SLES). Viene usato per **RCPP**, che viene installato in un secondo momento.

```bash
sudo zypper install gcc-c++
```
