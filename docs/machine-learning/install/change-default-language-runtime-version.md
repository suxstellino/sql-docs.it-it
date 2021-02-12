---
title: Modificare la versione predefinita del runtime dei linguaggi R o Python
description: Informazioni su come modificare la versione predefinita del runtime R o Python usato da un'istanza di SQL con SQL Server 2017 Machine Learning Services o R Services per SQL Server.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/14/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: eacae756f788a5cf42c19cae27a975f98111fb71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068960"
---
# <a name="change-the-default-r-or-python-language-runtime-version"></a>Modificare la versione predefinita del runtime dei linguaggi R o Python

[!INCLUDE[SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Questo articolo descrive come modificare la versione predefinita di R o Python usata in [R Services per SQL Server 2016](../r/sql-server-r-services.md) o [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md).

Di seguito sono elencate le versioni del runtime R e Python incluse nelle diverse versioni di SQL Server.

| Versione di SQL Server | Servizio | Aggiornamento cumulativo | Versioni di runtime R | Versione runtime di Python |
|-|-|-|-|-|
| SQL Server 2016 | Servizi R | RTM - SP2 CU13 | 3.2.2 | Non disponibile |
| SQL Server 2016 | Servizi R | SP2 CU14 e versioni successive | 3.2.2 e 3.5.2 | Non disponibile |
| SQL Server 2017 | Machine Learning Services | RTM - CU21 | 3.3.3 | 3.5.2 |
| SQL Server 2017 | Machine Learning Services | CU22 e versioni successive | 3.3.3 e 3.5.2 | 3.5.2 e 3.7.2 |

## <a name="prerequisites"></a>Prerequisiti

È necessario installare un aggiornamento cumulativo per modificare la versione predefinita del runtime del linguaggio R o Python:

- **SQL Server 2016:** Service Pack (SP) 2 aggiornamento cumulativo 14 o versione successiva
- **SQL Server 2017:** Aggiornamento cumulativo 22 o versione successiva

Per scaricare l'aggiornamento cumulativo più recente, vedere gli [Aggiornamenti più recenti per Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

> [!NOTE]
> Se l'aggiornamento cumulativo viene integrato con una nuova installazione di SQL Server, verranno installate solo le versioni più recenti del runtime R e Python.

## <a name="change-r-runtime-version"></a>Modificare la versione del runtime R

Se è stato installato uno degli aggiornamenti cumulativi precedenti per SQL Server 2016 o 2017, è possibile che siano presenti più versioni di R in un'istanza di SQL. Ogni versione è contenuta in una sottocartella della cartella dell'istanza con il nome `R_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (la cartella dell'installazione originale potrebbe non avere un numero di versione aggiunto al nome della cartella).

Se si installa un aggiornamento cumulativo contenente R 3.5, la nuova cartella `R_SERVICES` è:

- SQL Server 2016: `C:\Program Files\Microsoft SQL Server\MSSQL13.<INSTANCE_NAME>\R_SERVICES.3.5`
- SQL Server 2017: `C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\R_SERVICES.3.5`

Ogni istanza di SQL usa una di queste versioni come versione predefinita di R. È possibile modificare la versione predefinita usando l'utilità della riga di comando **RegisterRext.exe**. L'utilità si trova nella cartella R in ogni istanza di SQL:

*&lt;Percorso dell'istanza di SQL&gt;* \R_SERVICES.n.n\library\RevoScaleR\rxLibs\x64\RegisterRext.exe

> [!Note]
> La funzionalità descritta in questo articolo è disponibile solo con la copia di **RegisterRext.exe** inclusa negli aggiornamenti cumulativi di SQL. Non usare la copia fornita con l'installazione di SQL originale.

Per modificare la versione del runtime R, passare gli argomenti della riga di comando seguenti a **RegisterRext.exe**:

- `/configure` -obbligatorio, specifica che si sta configurando la versione di R predefinita.

- `/instance:`*&lt;nome istanza&gt;* - facoltativo, ovvero l'istanza che si desidera configurare. In assenza di specificazioni viene configurata l'istanza predefinita.

- `/rhome:`*&lt;percorso della cartella R_SERVICES[n.n] folder&gt;* - facoltativo, percorso alla cartella della versione di runtime che si desidera impostare come versione di R predefinita.

  Se non si specifica /rhome, il percorso configurato è il percorso in cui si trova **RegisterRext.exe**.

### <a name="examples"></a>Esempi

Di seguito sono riportati alcuni esempi su come modificare la versione del runtime R in SQL Server 2016 e 2017.

#### <a name="change-r-runtime-version-in-sql-server-2016"></a>Modificare la versione del runtime R in SQL Server 2016

Ad esempio, per configurare **R 3.5** come versione predefinita di R per l'istanza MSSQLSERVER01 in SQL Server 2016:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

#### <a name="change-r-runtime-version-in-sql-server-2017"></a>Modificare la versione del runtime R in SQL Server 2017

Ad esempio, per configurare **R 3.5** come versione predefinita di R per l'istanza MSSQLSERVER01 in SQL Server 2017:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

In questi esempi non è necessario includere l'argomento `/rhome` perché si sta specificando la stessa cartella in cui si trova **RegisterRext.exe**.

## <a name="change-python-runtime-version"></a>Modificare la versione runtime di Python

Se è stato installato CU22 o versione successiva per SQL Server 2017, è possibile che siano presenti più versioni di Python in un'istanza di SQL. Ogni versione è contenuta in una sottocartella della cartella dell'istanza con il nome `PYTHON_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (la cartella dell'installazione originale potrebbe non avere un numero di versione aggiunto al nome della cartella).

Se ad esempio si installa un aggiornamento cumulativo contenente Python 3.7, viene creata una nuova cartella `PYTHON_SERVICES`:

`C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\PYTHON_SERVICES.3.7`  

Ogni istanza di SQL usa una di queste versioni come versione predefinita di Python. È possibile modificare la versione predefinita usando l'utilità della riga di comando **RegisterRExt.exe**. L'utilità si trova nella cartella Python in ogni istanza di SQL:

*&lt;Percorso dell'istanza SQL&gt;* `\PYTHON_SERVICES.n.n\Lib\site-packages\revoscalepy\rxLibs\RegisterRExt.exe`

> [!Note]
> La funzionalità descritta in questo articolo è disponibile solo con la copia di **RegisterRExt.exe** inclusa negli aggiornamenti cumulativi di SQL. Non usare la copia fornita con l'installazione di SQL originale.

Per modificare la versione del runtime Python, passare gli argomenti della riga di comando seguenti a **RegisterRext.exe**:

- `/configure` -obbligatorio, specifica che si sta configurando la versione di Python predefinita.

- `/python` -Specifica che si sta configurando la versione di Python predefinita. Facoltativo se si specifica `/pythonhome`.

- `/instance:`*&lt;nome istanza&gt;* - facoltativo, ovvero l'istanza che si desidera configurare. In assenza di specificazioni viene configurata l'istanza predefinita.

- `/pythonhome:`*&lt;percorso della cartella PYTHON_SERVICES[n.n]&gt;* - Facoltativo, percorso della cartella della versione di runtime che si desidera impostare come versione di Python predefinita.

  Se non si specifica /pythonhome, il percorso configurato è il percorso in cui si trova **RegisterRExt.exe**.

### <a name="example"></a>Esempio

Ad esempio, per configurare **Python 3.7** come versione predefinita di Python per l'istanza MSSQLSERVER01 in SQL Server 2017:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7\Lib\site-packages\revoscalepy\rxLibs"

.\RegisterRext.exe /configure /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

In questo esempio non è necessario includere l'argomento `/pythonhome` perché si sta specificando la stessa cartella in cui si trova **RegisterRext.exe**.

## <a name="remove-a-runtime-version"></a>Rimuovere una versione del runtime

Per rimuovere una versione di R o Python, usare **RegisterRExt.exe** con l'argomento della riga di comando `/cleanup`, usando gli stessi argomenti `/rhome`, `/pythonhome` e `/instance` descritti in precedenza.

Ad esempio, per rimuovere la cartella **R 3.2** dall'istanza MSSQLSERVER01:

```cmd
.\RegisterRext.exe /cleanup /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES" /instance:MSSQLSERVER01
```

Ad esempio, per rimuovere la cartella **Python 3.7** dall'istanza MSSQLSERVER01:

```cmd
.\RegisterRExt.exe /cleanup /python /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

**RegisterRext.exe** chiederà di confermare la pulizia del runtime R specificato:

> *Eliminare definitivamente il runtime specificato insieme a tutti i pacchetti installati su di esso? \[Yes(Y)/No(N)/Valore predefinito(Yes)\]:*

Per confermare, rispondere `Y` o premere INVIO. In alternativa, è possibile ignorare questa richiesta passando `/y` o `/Yes` lungo l'opzione `/cleanup`.

> [!NOTE]
> È possibile rimuovere una versione solo se non è configurata come predefinita e non è attualmente usata per eseguire **RegisterRext.exe**.

## <a name="next-steps"></a>Passaggi successivi

- [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md)
- [Ottenere informazioni sui pacchetti Python](../package-management/python-package-information.md)
- [Installare pacchetti con gli strumenti R](../package-management/install-r-packages-standard-tools.md)
- [Installare i pacchetti con gli strumenti Python](../package-management/install-python-packages-standard-tools.md)
