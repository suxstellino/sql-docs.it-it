---
title: Matrice di supporto di Microsoft JDBC Driver per SQL Server
description: Questa pagina contiene i criteri relativi al ciclo di vita e alla matrice del supporto di Microsoft JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: c5769e67-99f7-4bc1-a4fa-8941dad33d35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 21b977b950babad552246925d78878c08cd36af3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163605"
---
# <a name="microsoft-jdbc-driver-for-sql-server-support-matrix"></a>Matrice di supporto di Microsoft JDBC Driver per SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Questa pagina contiene i criteri relativi al ciclo di vita e alla matrice del supporto di Microsoft JDBC Driver per SQL Server.  
  
## <a name="microsoft-jdbc-driver-support-lifecycle-matrix-and-policy"></a>Criteri relativi alla matrice e al ciclo di vita del supporto di Microsoft JDBC Driver  

I criteri relativi al ciclo di vita del supporto Microsoft (MSL) forniscono informazioni trasparenti e prevedibili riguardanti il ciclo di vita del supporto dei prodotti Microsoft. Il driver JDBC versione 4. x, 6. x, 7. x, 8. x e 9. x hanno un supporto mainstream di cinque anni dalla data di rilascio del driver. Il supporto Mainstream viene definito nel sito Web del ciclo di vita del supporto Microsoft.  
  
Le opzioni di supporto esteso e personalizzato non sono disponibili per Microsoft JDBC Driver.  

I seguenti driver Microsoft JDBC Driver sono supportati fino alla data di fine del supporto indicata.  
  
|Nome del driver|Versione del pacchetto driver|JAR applicabili|Fine del supporto Mainstream|
|-|-|-|-|  
|Microsoft JDBC driver 9,2 per SQL Server|9.2|MSSQL-JDBC-9.2.0. JRE15. jar<br> MSSQL-JDBC-9.2.0. jre11. jar<br> MSSQL-JDBC-9.2.0. jre8. jar|29 gennaio 2026|
|Microsoft JDBC Driver 8.4 per SQL Server|8.4|mssql-jdbc-8.4.1.jre14.jar<br> mssql-jdbc-8.4.1.jre11.jar<br> mssql-jdbc-8.4.1.jre8.jar|31 luglio 2025|
|Microsoft JDBC Driver 8.2 per SQL Server|8.2|mssql-jdbc-8.2.2.jre13.jar<br> mssql-jdbc-8.2.2.jre11.jar<br> mssql-jdbc-8.2.2.jre8.jar|31 gennaio 2025|
|Microsoft JDBC Driver 7.4 per SQL Server|7.4|mssql-jdbc-7.4.1.jre12.jar<br> mssql-jdbc-7.4.1.jre11.jar<br> mssql-jdbc-7.4.1.jre8.jar|31 luglio 2024|
|Microsoft JDBC Driver 7.2 per SQL Server|7.2|mssql-jdbc-7.2.2.jre11.jar<br> mssql-jdbc-7.2.2.jre8.jar|31 gennaio 2024|
|Microsoft JDBC Driver 7.0 per SQL Server|7.0|mssql-jdbc-7.0.0.jre10.jar<br> mssql-jdbc-7.0.0.jre8.jar|31 luglio 2023|
|Microsoft JDBC Driver 6.4 per SQL Server|6.4|mssql-jdbc-6.4.0.jre9.jar<br> mssql-jdbc-6.4.0.jre8.jar<br> mssql-jdbc-6.4.0.jre7.jar|27 febbraio 2023|
|Microsoft JDBC Driver 6.2 per SQL Server|6.2|mssql-jdbc-6.2.2.jre8.jar<br> mssql-jdbc-6.2.2.jre7.jar|30 giugno 2022|
|Microsoft JDBC Driver 6.0 per SQL Server|6.0|sqljdbc42.jar<br>sqljdbc41.jar|14 luglio 2021|
  
 I seguenti driver Microsoft JDBC Driver non sono più supportati.  

|Nome del driver|Versione del pacchetto driver|Fine del supporto Mainstream|  
|-|-|-|
|Microsoft JDBC Driver 4.2 per SQL Server|4.2|24 agosto 2020|
|Microsoft JDBC Driver 4.1 per SQL Server|4.1|12 dicembre 2019|  
|Microsoft JDBC Driver 4.0 per SQL Server|4.0|6 marzo 2017|  
|Driver JDBC 3.0 per Microsoft SQL Server|3.0|23 aprile 2015|  
|Microsoft SQL Server JDBC Driver 2.0|2.0|31 dicembre 2012|  
|Driver JDBC 1.2 per Microsoft SQL Server 2005|1.2|25 giugno 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.1|1.1|25 giugno 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.0|1.0|25 giugno 2011|  
|Microsoft SQL Server 2000 JDBC Driver|2000|9 luglio 2010|  
  
## <a name="sql-version-compatibility"></a>Compatibilità tra versioni SQL  
  
|Versione del database&nbsp;&#8594;<br />&#8595; Versione del driver|database SQL di Azure|Azure Synapse Analytics|Istanza gestita di SQL di Azure|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|PDW 2008R2 AU3<sup>4</sup>|SQL Server 2008 R2|SQL Server 2008|
|---|---|---|---|---|---|---|---|---|---|---|---|
|9.2|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |   |   |
|8.4|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|8.2|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|7.4|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|7.2|Sì|Sì|Sì|   |Sì|Sì|Sì|Sì|Sì|Sì|   |
|7.0|Sì|Sì|Sì|   |Sì|Sì|Sì|Sì|Sì|Sì|   |
|6.4|Sì|Sì|Sì|   |Sì|Sì|Sì|Sì|Sì|Sì|   |
|6.2|Sì|Sì|   |   |Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|6.1|Sì|   |   |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|6,0|Sì|   |   |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|4.2|Sì|   |   |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|4.1|Sì|   |   |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|4,0|Sì|   |   |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|3,0|Sì<sup>2</sup>|   |   |   |   |   |Sì<sup>5</sup>|Sì<sup>1</sup>|   |Sì|Sì|
|2,0|   |   |   |   |   |   |   |   |   |Sì<sup>3</sup>|Sì<sup>3</sup>|
|1.2|   |   |   |   |   |   |   |   |   |   |Sì<sup>3</sup>|

 <sup>1</sup> Microsoft SQL Server JDBC Driver versione 3.0 può connettersi a SQL Server 2012 come client di livello inferiore.  
  
 <sup>2</sup> Il supporto per il database SQL di Azure è stato introdotto nel driver 3.0 come hotfix. Si consiglia ai clienti del database SQL di Azure di usare la versione del driver più recente disponibile.  
  
 <sup>3</sup> Microsoft SQL Server JDBC Driver versione 2.0 e Microsoft SQL Server 2005 JDBC Driver versione 1.2 possono connettersi a SQL Server 2008 come client di livello inferiore. Quando sono consentite le conversioni di livello inferiore, le applicazioni possono eseguire query e aggiornamenti nei nuovi tipi di dati di SQL Server 2008, ad esempio time, date, datetime2, datetimeoffset e FILESTREAM. Per altre informazioni su come usare questi nuovi tipi di dati con il driver JDBC, vedere i post di blog relativi all'  [uso dei tipi di dati per data/ora di SQL Server 2008 con il driver JDBC](/archive/blogs/jdbcteam/) e all'  [uso di SQL Server 2008 FileStream con il driver JDBC](/archive/blogs/jdbcteam/). Per altre informazioni sulla compatibilità di livello inferiore di questi nuovi tipi di dati, vedere gli argomenti  [Utilizzo di dati relativi a data e ora](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105))e  [Supporto FILESTREAM](../../relational-databases/native-client/features/filestream-support.md) nella documentazione online di SQL Server.  
  
 <sup>4</sup> Il supporto per le connessioni tra Microsoft JDBC Driver e Parallel Data Warehouse è stato introdotto per la prima volta in Microsoft JDBC Driver 4.0 per SQL Server e Microsoft SQL Server 2008 R2 Parallel Data Warehouse Appliance Update 3.  
  
 <sup>5</sup> Microsoft SQL Server JDBC Driver versione 3.0 può connettersi a SQL Server 2014 come client di livello inferiore.  
  
## <a name="java-and-jdbc-specification-support"></a>Supporto per le specifiche Java e JDBC
  
|Versione driver JDBC|Versioni JRE|Versioni API JDBC|
|-|-|-|
|[9.2](release-notes-for-the-jdbc-driver.md#92)|1,8, 11, 15|4.2 o 4.3 (parziale)|
|[8.4](release-notes-for-the-jdbc-driver.md#84)|1.8, 11, 14|4.2 o 4.3 (parziale)|
|[8.2](release-notes-for-the-jdbc-driver.md#82)|1.8, 11, 13|4.2 o 4.3 (parziale)|
|[7.4](release-notes-for-the-jdbc-driver.md#74)|1.8, 11, 12|4.2 o 4.3 (parziale)|
|[7.2](release-notes-for-the-jdbc-driver.md#72)|1.8, 11|4.2 o 4.3 (parziale)|
|[7.0](release-notes-for-the-jdbc-driver.md#70)|1.8, 10|4.2 o 4.3 (parziale)|
|[6.4](release-notes-for-the-jdbc-driver.md#64)|1.7, 1.8, 9|4.1, 4.2, 4.3 (parziale)|
|[6.2](release-notes-for-the-jdbc-driver.md#62)|1.7, 1.8|4.1, 4.2|
|[6.1](release-notes-for-the-jdbc-driver.md#61)|1.7, 1.8|4.1, 4.2|
|[6.0](release-notes-for-the-jdbc-driver.md#60)|1.7, 1.8|4.1, 4.2|
|4.2|1.7, 1.8|4.1, 4.2|
|4.1|1.7|4.0|
|4.0|1.5, 1.6, 1.7|3.0, 4.0|
|3.0|1.5, 1.6,|3.0, 4.0|
|2.0|1.5, 1.6|3.0, 4.0|
|1.2|1.4, 1.5, 1.6|3.0|
|1.1|1.4|3.0|
|1.0|1.4|3.0|
|2000|1.4|3.0|

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Microsoft JDBC Driver è stato sviluppato per essere usato in qualsiasi sistema operativo che supporti l'utilizzo di Java Virtual Machine (JVM). Alcune delle piattaforme usate comunemente sono Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2008 R2, Linux, Unix, AIX, macOS e altre.  

Il team del prodotto JDBC testa il driver in Windows, Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux e macOS.

## <a name="application-server-support"></a>Supporto per server applicazioni

Microsoft JDBC Driver per SQL Server viene testato con diversi server applicazioni.  Rivolgersi al fornitore del server applicazioni per altre informazioni sulla versione del driver compatibile con il prodotto fornito.