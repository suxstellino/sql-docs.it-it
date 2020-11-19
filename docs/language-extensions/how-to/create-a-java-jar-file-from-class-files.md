---
title: Creare un file con estensione jar di Java da file di classe
description: Creare un pacchetto per assemblare i file di classe in un file con estensione jar quando si usano le estensioni del linguaggio di SQL Server per eseguire codice Java.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 076b20d277ef8608fd8c9fc30b4bce303d4ef06b
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870173"
---
# <a name="create-a-java-jar-file-from-class-files"></a>Creare un file con estensione jar di Java da file di classe
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni su come creare un pacchetto per assemblare i file di classe in un file con estensione jar quando si usano le [estensioni del linguaggio di SQL Server](../language-extensions-overview.md) per eseguire codice Java. Creare un pacchetto dei file è la procedura consigliata.

## <a name="create-a-jar-file"></a>Creare un file con estensione jar

Per creare un file con estensione jar da file di classe, passare alla cartella contenente il file di classe ed eseguire il comando seguente:

```cmd
jar -cf <MyJar.jar> *.class
```

Verificare che il percorso di `jar.exe` sia incluso nella variabile di sistema Path. In alternativa, specificare il percorso completo del file con estensione jar, disponibile nella directory `/bin` della cartella JDK. Ad esempio:

```cmd
C:\Users\MyUser\Desktop\jdk1.8.0_201\bin\jar -cf <MyJar.jar> *.class
```

## <a name="next-steps"></a>Passaggi successivi

+ [Come chiamare il runtime Java nelle estensioni del linguaggio di SQL Server](../how-to/call-java-from-sql.md)