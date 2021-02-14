---
title: mssql-cli
description: mssql-cli è uno strumento di query da riga di comando interattivo per SQL Server eseguito in Windows, macOS o Linux.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein, maghan
ms.custom: tools|mssql-cli
ms.date: 02/22/2018
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 1514c776944b66757a23baf06bb5169f66171f2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033521"
---
# <a name="mssql-cli-command-line-query-tool-for-sql-server-preview"></a>Strumento di query da riga di comando mssql-cli per SQL Server (anteprima)

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

mssql-cli è uno strumento da riga di comando interattivo per l'esecuzione di query su SQL Server, eseguibile in Windows, macOS o Linux.

## <a name="install-mssql-cli"></a>Installare mssql-cli

Per le istruzioni dettagliate sull'installazione, vedere la [Guida all'installazione](https://github.com/dbcli/mssql-cli/tree/master/doc/installation). Gli scenari di installazione più comuni sono riepilogati di seguito.

### <a name="windows-and-macos-installation"></a>Installazione in Windows e in macOS

mssql-cli viene installato in Windows e in macOS tramite `pip`:

```$ pip install mssql-cli```

Per istruzioni più dettagliate, vedere la [Guida all'installazione](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

### <a name="linux-installation"></a>Installazione in Linux

Dopo la registrazione del repository Microsoft, mssql-cli può essere installato e aggiornato tramite gestione pacchetti in diverse distribuzioni di Linux.

L'esempio seguente si applica a Ubuntu 18.04 (Bionic). Altre informazioni ed esempi per altre distribuzioni sono disponibili nella [Guida all'installazione](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

```
# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Ubuntu repository
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod

# Update the list of products
sudo apt-get update

# Install mssql-cli
sudo apt-get install mssql-cli

# Install missing dependencies
sudo apt-get install -f
```

## <a name="mssql-cli-documentation"></a>Documentazione di mssql-cli

La documentazione per mssql-cli si trova nel [repository GitHub mssql-cli](https://github.com/dbcli/mssql-cli).

- [Pagina principale/file readme](https://github.com/dbcli/mssql-cli)
- [Guida all'installazione](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)
- [Guida all'utilizzo](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)

La documentazione aggiuntiva si trova nella [cartella doc](https://github.com/dbcli/mssql-cli/tree/master/doc).
