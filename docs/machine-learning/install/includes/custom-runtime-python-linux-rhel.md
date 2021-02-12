---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: c6d15087150e914e62b5d19951715198ced47512
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072927"
---
## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Eseguire il comando seguente per installare [SQL Server Language Extensions](../../../language-extensions/language-extensions-overview.md) in Red Hat Enterprise Linux (RHEL), che viene usato per il runtime personalizzato di Python.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-python-37-and-pandas"></a>Installare Python 3.7 e pandas

L'estensione del linguaggio Python usata per il runtime Python personalizzato attualmente supporta solo [python 3,7](https://www.python.org/) . Se si vuole usare una versione diversa di Python, seguire le istruzioni nel [repository GitHub Estensione del linguaggio Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) per modificare e ricompilare l'estensione.

1. Eseguire i comandi seguenti per installare Python 3,7.

    ```bash
    # Install python3.7 and the corresponding library:
    yum install gcc openssl-devel bzip2-devel libffi-devel zlib-devel
    
    cd /usr/src
    wget https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tgz
    tar xzf Python-3.7.9.tgz
    
    cd Python-3.7.9
    ./configure --enable-optimizations --prefix=/usr
    make altinstall
    ```

1. Eseguire il comando seguente per installare il pacchetto Pandas

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
