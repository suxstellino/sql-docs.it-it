---
title: Novità nelle estensioni del linguaggio di SQL Server
titleSuffix: ''
description: Informazioni sulle novità delle estensioni del linguaggio di SQL Server che espande, estende e approfondisce l'integrazione tra i linguaggi esterni e la piattaforma dati.
author: dphansen
ms.author: davidph
ms.date: 11/09/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: e72b202230573352eca54bd8bc9d2afc0a58f29e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070579"
---
# <a name="whats-new-in-sql-server-language-extensions"></a>Novità nelle estensioni del linguaggio di SQL Server
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

In ogni versione di SQL Server vengono aggiunte funzionalità di [estensione del linguaggio](language-extensions-overview.md) man mano che l'integrazione tra linguaggi esterni e la piattaforma dati viene espansa, estesa e approfondita.

## <a name="sql-server-2019"></a>SQL Server 2019

Di seguito sono elencate le nuove funzionalità dell'[estensione del linguaggio](language-extensions-overview.md) in SQL Server 2019. Per altre informazioni su tutte le funzionalità di questa versione, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) e [Note sulla versione per SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

### <a name="new-python-and-r-language-extensions"></a>Nuove estensioni di linguaggio Python e R

- Un [runtime personalizzato Python](../machine-learning/install/custom-runtime-python.md) è disponibile con le estensioni del linguaggio. Per altre informazioni, vedere come [Installare un runtime personalizzato Python in Windows](../machine-learning/install/custom-runtime-python.md?view=sql-server-ver15&preserve-view=true) o come [Installare un runtime personalizzato Python in Linux](../machine-learning/install/custom-runtime-python.md?view=sql-server-linux-ver15&preserve-view=true).

- Un [runtime personalizzato R](../machine-learning/install/custom-runtime-r.md) è disponibile con le estensioni del linguaggio. Per altre informazioni, vedere come [Installare un runtime personalizzato R in Windows](../machine-learning/install/custom-runtime-r.md?view=sql-server-ver15&preserve-view=true) o come [Installare un runtime personalizzato R in Linux](../machine-learning/install/custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

### <a name="new-java-language-extension"></a>Nuova estensione del linguaggio Java

- Il runtime Java predefinito in Windows e Linux è Open Zulu JRE e viene incluso con l'[installazione delle estensioni del linguaggio di SQL Server in Windows](install/windows-java.md) e con l'[installazione delle estensioni del linguaggio di SQL Server in Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
- [Tipi di dati Java](how-to/java-to-sql-data-types.md) supportati.
- [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) per la registrazione di un linguaggio esterno (ad esempio, Java) in SQL Server.
- [Microsoft Extensibility SDK per Java](how-to/extensibility-sdk-java-sql-server.md).
- In Windows e Linux è possibile accedere al codice Java in una libreria esterna usando l'istruzione [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md). Altre informazioni: [Come chiamare Java da SQL Server](how-to/call-java-from-sql.md).
- [Estensione del linguaggio Java](language-extensions-overview.md) in Windows e Linux. È possibile rendere disponibile a SQL Server il codice Java compilato assegnando autorizzazioni e impostando il percorso. Le app client con accesso a SQL Server possono usare ed eseguire il codice chiamando [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), ovvero adottando la stessa procedura usata per l'integrazione di R e Python in SQL Server Machine Learning Services.

## <a name="next-steps"></a>Passaggi successivi

+ Installare le [estensioni del linguaggio di SQL Server in Windows](install/windows-java.md) o [in Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
