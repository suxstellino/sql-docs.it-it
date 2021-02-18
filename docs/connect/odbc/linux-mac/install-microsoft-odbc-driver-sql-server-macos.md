---
title: Installare Microsoft ODBC Driver for SQL Server (macOS)
description: Informazioni su come installare Microsoft ODBC Driver for SQL Server nei client macOS per abilitare la connettività del database.
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2f07e613a6bd5ce8416c4b2c015749a1e631ea3f
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525145"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-macos"></a>Installare Microsoft ODBC Driver for SQL Server (macOS)

Questo articolo illustra come installare Microsoft ODBC Driver for SQL Server in macOS. Sono inoltre incluse le istruzioni per gli strumenti della riga di comando facoltativi per SQL Server (`bcp` e `sqlcmd`) e le intestazioni di sviluppo unixODBC.

Questo articolo fornisce i comandi per l'installazione del driver ODBC dalla shell Bash. Se si vogliono scaricare direttamente i pacchetti, vedere [Scaricare ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

> [!Note]
> Microsoft ODBC driver for SQL Server in macOS è supportato solo nell'architettura x64. Apple M1 non è supportato.

## <a name="microsoft-odbc-17"></a>Microsoft ODBC 17

Per installare Microsoft ODBC Driver 17 for SQL Server in macOS, eseguire i comandi seguenti:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=Y brew install msodbcsql17 mssql-tools
```

> [!IMPORTANT]
> Se è stato installato il pacchetto `msodbcsql` v17 rimasto disponibile per poco tempo, rimuoverlo prima di installare il pacchetto `msodbcsql17`. Si eviteranno così conflitti. È possibile installare il pacchetto `msodbcsql17` side-by-side con il pacchetto `msodbcsql` v13.

## <a name="previous-versions"></a>Versioni precedenti

Nelle sezioni seguenti vengono fornite le istruzioni per l'installazione delle versioni precedenti di Microsoft ODBC Driver in macOS.

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Usare i comandi seguenti per installare Microsoft ODBC Driver 13.1 for SQL Server in OS X 10.11 (El Capitan) e macOS 10.12 (Sierra):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="driver-files"></a>File del driver

Il driver ODBC in macOS include i componenti seguenti:

|Componente|Descrizione|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib o libmsodbcsql.13.dylib|File della libreria di collegamento dinamico (`dylib`) che contiene tutte le funzionalità del driver. Questo file viene installato in `/usr/local/lib/`.|  
|`msodbcsqlr17.rll` o `msodbcsqlr13.rll`|File di risorse associato per la libreria del driver. Questo file viene installato in `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` per Driver 17 e in `[driver .dylib directory]../share/msodbcsql/resources/en_US/` per Driver 13. | 
|msodbcsql.h|File di intestazione che contiene tutte le nuove definizioni necessarie per usare il driver.<br /><br /> **Nota:**  non è possibile fare riferimento a msodbcsql.h e a odbcss.h nello stesso programma.<br /><br /> msodbcsql.h viene installato in `/usr/local/include/msodbcsql17/` per Driver 17 e in `/usr/local/include/msodbcsql/` per Driver 13. |
|LICENSE.txt|File di testo che contiene i termini del contratto di licenza con l'utente finale. Questo file viene inserito in `/usr/local/share/doc/msodbcsql17/` per Driver 17 e in `/usr/local/share/doc/msodbcsql/` per Driver 13. |
|RELEASE_NOTES|File di testo che contiene le note sulla versione. Questo file viene inserito in `/usr/local/share/doc/msodbcsql17/` per Driver 17 e in `/usr/local/share/doc/msodbcsql/` per Driver 13. |

## <a name="resource-file-loading"></a>Caricamento del file di risorse

Perché possa funzionare, il driver deve caricare il file di risorse. Questo file è denominato `msodbcsqlr17.rll` o `msodbcsqlr13.rll` a seconda della versione del driver. La posizione del file `.rll` dipende dalla posizione del driver stesso (`so` o `dylib`), come indicato nella tabella precedente. A partire dalla versione 17.1 il driver tenterà anche di caricare il file `.rll` dalla directory predefinita se il caricamento dal percorso relativo non riesce. Il percorso del file di risorse predefinito in macOS è `/usr/local/share/msodbcsql17/resources/en_US/`

## <a name="troubleshooting"></a>Risoluzione dei problemi

Alcuni utenti riscontrano un problema quando tentano di connettersi dopo aver installato il driver ODBC e ricevono un errore come: `"[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 17 for SQL Server' : file not found (0) (SQLDriverConnect)"`. È possibile che unixODBC non sia configurato correttamente per trovare i driver registrati. In questi casi la creazione di un paio di collegamenti simbolici può risolvere il problema.

```bash
sudo ln -s /usr/local/etc/odbcinst.ini /etc/odbcinst.ini
sudo ln -s /usr/local/etc/odbc.ini /etc/odbc.ini
```

Per altri casi in cui non è possibile effettuare una connessione a SQL Server usando il driver ODBC, vedere l'articolo sui problemi noti dedicato alla [risoluzione dei problemi di connessione](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver installato il driver, è possibile provare l'[applicazione di esempio ODBC C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Per altre informazioni sullo sviluppo di applicazioni ODBC, vedere [Sviluppo di applicazioni](../../../odbc/reference/develop-app/developing-applications.md).

Per altre informazioni, vedere le [note sulla versione](release-notes-odbc-sql-server-linux-mac.md) e i [requisiti di sistema](system-requirements.md) del driver ODBC.
