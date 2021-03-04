---
title: Scaricare Microsoft JDBC Driver per SQL Server
description: Scaricare Microsoft JDBC Driver per SQL Server per sviluppare applicazioni Java che si connettono a SQL Server e al database SQL di Azure.
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 451181b8-11e6-4d01-b547-9ac5aada8238
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a87da2ebba4c28574155be69e60313cc877970f6
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837118"
---
# <a name="download-microsoft-jdbc-driver-for-sql-server"></a>Scaricare Microsoft JDBC Driver per SQL Server

Microsoft JDBC Driver per SQL Server è un driver JDBC di tipo 4 che offre connettività di database tramite le API JDBC standard disponibili nella piattaforma Java. I download dei driver sono disponibili per tutti gli utenti senza costi aggiuntivi. Forniscono l'accesso a SQL Server da qualsiasi applicazione Java, server applicazioni o applet abilitata per Java.

## <a name="download"></a>Download

La versione 9,2 è la versione di disponibilità generale più recente. Supporta Java 8, 11 e 15. Se è necessario eseguire in un runtime Java precedente, vedere la [matrice di supporto per le specifiche Java e JDBC](microsoft-jdbc-driver-for-sql-server-support-matrix.md#java-and-jdbc-specification-support) per verificare se è disponibile una versione del driver supportata che è possibile usare. Il supporto per la connettività Java è in continuo miglioramento. Di conseguenza, è consigliabile usare la versione più recente di Microsoft JDBC Driver.

**[![Download ](../../ssms/media/download-icon.png) scaricare Microsoft JDBC Driver 9,2 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2155948)**  
**[![Download ](../../ssms/media/download-icon.png) scaricare Microsoft JDBC Driver 9,2 per SQL Server (tar. gz)](https://go.microsoft.com/fwlink/?linkid=2155949)**  

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 9.2.1
- Rilasciata: 02 marzo 2021

Quando si Scarica il driver sono presenti più file JAR. Il nome del file JAR indica la versione di Java supportata.

> [!Note]
> Se si accede a questa pagina da una versione non in lingua inglese e si vuole visualizzare il contenuto più aggiornato, visitare la [versione in inglese del sito](). È possibile scaricare una versione del sito diversa da quella in lingua inglese selezionando [available languages](#available-languages) (lingue disponibili).

## <a name="available-languages"></a>Lingue disponibili

Questa versione di Microsoft JDBC Driver per SQL Server è disponibile nelle lingue seguenti:

Microsoft JDBC driver 9.2.1 for SQL Server (zip): [cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804)  |  [cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404)  |  [inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409)  |  [francese](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c)  |  [tedesco](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407)  |  [Italiano](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410)  |  [giapponese](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411)  |  [coreano](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412)  |  [portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416)  |  [russo](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419)  |  [spagnolo](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)

Microsoft JDBC driver 9.2.1 for SQL Server (tar. gz): [cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804)  |  [cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404)  |  [inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409)  |  [francese](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c)  |  [tedesco](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407)  |  [Italiano](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410)  |  [giapponese](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411)  |  [coreano](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412)  |  [portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416)  |  [russo](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419)  |  [spagnolo](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)

### <a name="release-notes"></a>Note sulla versione

Per informazioni dettagliate su questa versione, vedere [le note sulla versione](release-notes-for-the-jdbc-driver.md) e [i requisiti di sistema](system-requirements-for-the-jdbc-driver.md).

### <a name="previous-releases"></a>Versioni precedenti

Per scaricare le versioni precedenti, vedere [Versioni precedenti di Microsoft JDBC Driver per SQL Server](release-notes-for-the-jdbc-driver.md#previous-releases).

## <a name="using-the-jdbc-driver-with-maven-central"></a>Uso del driver JDBC con Maven Central

Il driver JDBC può essere aggiunto a un progetto Maven aggiungendolo come dipendenza nel file POM.xml con il codice seguente:

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```  

## <a name="unsupported-drivers"></a>Driver non supportati

Le versioni non supportate dei driver non sono disponibili per il download in questo contesto. Il supporto per la connettività Java è in continuo miglioramento, Di conseguenza, è consigliabile usare la versione più recente di Microsoft JDBC Driver.  
  
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Microsoft JDBC Driver per SQL Server, vedere [Panoramica del driver JDBC](overview-of-the-jdbc-driver.md) e il [repository GitHub del driver JDBC](https://github.com/microsoft/mssql-jdbc/blob/dev/README.md).