---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 83b39be5be128ba4fbda17765a7b67deead2c80c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072789"
---
## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Eseguire i comandi seguenti per installare le [estensioni del linguaggio SQL Server](../../../language-extensions/language-extensions-overview.md) in Ubuntu Linux, che viene usato per il runtime personalizzato di R.

1. Se possibile, eseguire questo comando per aggiornare i pacchetti nel sistema prima dell'installazione.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Installare **mssql-server-extensibility** con questo comando.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-r"></a>Installare R

1. Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è stato installato, R è già installato in `/opt/microsoft/ropen/3.5.2/lib64/R` . Se si desidera continuare a utilizzare questo percorso come R_HOME, è possibile ignorare questo passaggio.

    Se si vuole usare un runtime diverso di R, prima di continuare con l'installazione di una nuova versione è necessario rimuovere `microsoft-r-open-mro`.

    ```bash
    sudo apt remove microsoft-r-open-mro-3.5.2
    ```

1. Installare [R (3,3 o versione successiva)](https://www.r-project.org/) per Ubuntu. Per impostazione predefinita, R viene installato in **/usr/lib/R**. Questo percorso corrisponde a **R_HOME**. Se si installa R in un percorso diverso, prendere nota del percorso come **R_HOME**.

    Di seguito sono riportate istruzioni di esempio per Ubuntu. Modificare l'URL del repository seguente per la versione di R.

    ```bash
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6
    
    # Add R CRAN repository. This repository works for R 4.0.x.
    #
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
    sudo apt-get update
    
    # Install R runtime.
    #
    sudo apt-get -y install r-base-core
    ```
