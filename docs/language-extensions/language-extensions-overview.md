---
title: Cosa sono le estensioni del linguaggio di SQL Server?
titleSuffix: ''
description: Le estensioni del linguaggio sono una funzionalità di SQL Server usata per l'esecuzione di codice esterno. In SQL Server sono supportati Java, Python e R. I dati relazionali possono essere usati nel codice esterno tramite il framework di estendibilità.
author: dphansen
ms.author: davidph
ms.date: 11/06/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 50ebe13c330dba1067c2ccc61bcbc779f7bf0b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070623"
---
# <a name="what-is-sql-server-language-extensions"></a>Cosa sono le estensioni del linguaggio di SQL Server?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Le estensioni del linguaggio sono una funzionalità di SQL Server usata per l'esecuzione di codice esterno. I dati relazionali possono essere usati nel codice esterno tramite il [framework di estendibilità](concepts/extensibility-framework.md). In SQL Server 2019 sono supportati i runtime Java, Python e R.

> [!NOTE]
> Per l'esecuzione di Python o R in SQL Server, vedere la documentazione di [Machine Learning Services](../machine-learning/sql-server-machine-learning-services.md). Con SQL Server 2019 e versioni successive, è possibile usare un runtime personalizzato di Python e R con le estensioni del linguaggio. Per altre informazioni, vedere come installare il [runtime personalizzato di Python](../machine-learning/install/custom-runtime-python.md) e il [runtime personalizzato di R](../machine-learning/install/custom-runtime-r.md).

## <a name="what-you-can-do-with-language-extensions"></a>Potenzialità delle estensioni del linguaggio

Le estensioni del linguaggio usano il [framework di estendibilità](concepts/extensibility-framework.md) per l'esecuzione di codice esterno. L'esecuzione del codice è isolata dai processi del motore di base, ma completamente integrata con l'esecuzione delle query di SQL Server. È possibile eseguire il codice nell'origine dati, eliminando la necessità di eseguire il pull dei dati attraverso la rete.

I linguaggi esterni sono definiti con [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). La stored procedure di sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) viene usata come interfaccia per l'esecuzione del codice.

Le estensioni del linguaggio offrono diversi vantaggi:

+ Sicurezza dei dati. L'esecuzione di un linguaggio esterno più vicino all'origine dei dati evita spostamenti di dati non sicuri.
+ Velocità. I database sono ottimizzati per le operazioni basate su set. 
+ Facilità di distribuzione e integrazione. [!INCLUDE [ssNoVersion](../includes/ssnoversion-md.md)] è il punto centrale delle operazioni per molte altre attività di gestione dati e applicazioni. Usando i dati nel database, è possibile assicurarsi che i dati usati dall'estensione del linguaggio siano coerenti e aggiornati.

## <a name="next-steps"></a>Passaggi successivi

+ Installare le [estensioni del linguaggio di SQL Server in Windows](install/windows-java.md) o [in Linux](../linux/sql-server-linux-setup-language-extensions-java.md)
+ Installare un [runtime personalizzato di Python per SQL Server](../machine-learning/install/custom-runtime-python.md)
+ Installare un [runtime personalizzato di R per SQL Server](../machine-learning/install/custom-runtime-r.md)
+ Installare [Microsoft Extensibility SDK per Java](how-to/extensibility-sdk-java-sql-server.md)
