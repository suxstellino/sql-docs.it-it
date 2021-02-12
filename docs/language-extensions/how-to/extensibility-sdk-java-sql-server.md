---
title: Microsoft Extensibility SDK per Java
description: Informazioni su come implementare un programma Java per SQL Server usando Microsoft Extensibility SDK per Java.
ms.prod: sql
ms.technology: language-extensions
ms.date: 11/05/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 153ea1440b3a56eabd98533600de1620c9d6cb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072645"
---
# <a name="microsoft-extensibility-sdk-for-java-for-sql-server"></a>Microsoft Extensibility SDK per Java per SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni su come implementare un programma Java per SQL Server usando Microsoft Extensibility SDK per Java. L'SDK è un'interfaccia per l'estensione del linguaggio Java utilizzabile per scambiare dati con SQL Server ed eseguire codice Java da SQL Server.

L'SDK è installato come parte di SQL Server 2019 Release Candidate 1 sia in Windows che in Linux:

+ Percorso di installazione predefinito in Windows: **[home directory installazione istanza]\MSSQL\Binn\mssql-java-lang-extension.jar**
+ Percorso di installazione predefinito in Linux: **/opt/mssql/lib/mssql-java-lang-extension.jar**

Il codice è open source ed è disponibile nel [repository GitHub delle estensioni del linguaggio di SQL Server](https://github.com/microsoft/sql-server-language-extensions).

## <a name="implementation-requirements"></a>Requisiti di implementazione

L'interfaccia dell'SDK definisce un set di requisiti che devono essere soddisfatti affinché SQL Server sia in grado di comunicare con il runtime Java. Per usare l'SDK, è necessario seguire alcune regole di implementazione nella classe principale. SQL Server può quindi eseguire un metodo specifico nella classe Java e scambiare i dati usando l'estensione del linguaggio Java.

Per un esempio di come usare l'SDK, vedere [Esercitazione: cercare una stringa usando espressioni regolari (regex) in Java](../tutorials/search-for-string-using-regular-expressions-in-java.md).

## <a name="sdk-classes"></a>Classi SDK

L'SDK è costituito da tre classi.

Due classi astratte definiscono l'interfaccia usata dall'estensione Java per scambiare dati con SQL Server:

- **AbstractSqlServerExtensionExecutor**
- **AbstractSqlServerExtensionDataset**

La terza è una classe helper contenente un'implementazione di un oggetto set di dati. Si tratta di una classe facoltativa che è possibile usare per iniziare a lavorare con facilità. È anche possibile usare un'implementazione personalizzata di tale classe.

- **PrimitiveDataset**

Di seguito sono riportate le descrizioni di ogni classe nell'SDK. Il codice sorgente delle classi SDK è disponibile nel [repository GitHub delle estensioni del linguaggio di SQL Server](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/java/sdk).

### <a name="class-abstractsqlserverextensionexecutor"></a>Classe: AbstractSqlServerExtensionExecutor

La classe astratta **AbstractSqlServerExtensionExecutor** contiene l'interfaccia usata per eseguire il codice Java dall'estensione del linguaggio Java per SQL Server.

La classe Java principale deve ereditare da questa classe. Ereditare da questa classe significa che esistono alcuni metodi della classe che è necessario implementare nella propria classe.

Per ereditare da questa classe astratta, si estende con il nome della classe astratta nella dichiarazione della classe:

```java
public class <MyClass> extends AbstractSqlServerExtensionExecutor {}
```

La classe principale deve implementare almeno il metodo execute(...).

#### <a name="method-execute"></a>Metodo execute

Il metodo execute è il metodo chiamato da SQL Server tramite l'estensione del linguaggio Java per richiamare il codice Java da SQL Server. Si tratta di un metodo chiave in cui si includono le operazioni principali che si vuole eseguire da SQL Server.

Per passare gli argomenti del metodo a Java da SQL Server, usare il parametro `@param` in `sp_execute_external_script`. Il metodo **execute** accetta gli argomenti nel modo seguente.

```java
public AbstractSqlServerExtensionDataset execute(AbstractSqlServerExtensionDataset input, LinkedHashMap<String, Object> params)  {}
```

#### <a name="method-init"></a>Metodo init

Il metodo init viene eseguito dopo il costruttore e prima del metodo execute. Qualsiasi operazione che deve essere eseguita prima di execute(...) può essere eseguita in questo metodo.

```java
public void init(String sessionId, int taskId, int numtask) {}
```

### <a name="class-abstractsqlserverextensiondataset"></a>Classe: AbstractSqlServerExtensionDataset

La classe astratta **AbstractSqlServerExtensionDataset** contiene l'interfaccia per la gestione dei dati di input e output usati dall'estensione Java.


### <a name="class-primitivedataset"></a>Classe: PrimitiveDataset

La classe **PrimitiveDataset** è un'implementazione di **AbstractSqlServerExtensionDataset** che archivia i tipi semplici come matrici primitive.

È disponibile nell'SDK semplicemente come una classe helper facoltativa. Se non si usa questa classe, è necessario implementarne una personalizzata che eredita da **AbstractSqlServerExtensionDataset**.  

## <a name="next-steps"></a>Passaggi successivi

+ [Esercitazione: cercare una stringa usando espressioni regolari (regex) in Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)
+ [Come chiamare Java in SQL Server](call-java-from-sql.md)
