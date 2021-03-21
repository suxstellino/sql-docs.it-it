---
description: Origini dati ODBC di SQL Server Native Client
title: Origini dei dati ODBC
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC data sources, about data sources
- ODBC data sources, names
- data sources [SQL Server Native Client]
- names [ODBC]
- ODBC applications, data sources
- SQL Server Native Client ODBC driver, data sources
- ODBC data sources
ms.assetid: a6a50fd0-d439-43fd-b76f-16ec02f478c5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ee2f38182085114f23bb0912cfc1d3334c1f0b85
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756331"
---
# <a name="sql-server-native-client-odbc-data-sources"></a>Origini dati ODBC di SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Un nome di origine dati (DSN, Data Source Name) di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifica un'origine dati ODBC che contiene tutte le informazioni necessarie a un'applicazione ODBC per connettersi a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un server specifico. Per definire un nome di origine dati ODBC, è possibile procedere in due modi diversi:  
  
-   In un computer client, aprire strumenti di amministrazione nel pannello di controllo e fare doppio clic su **origini dati (ODBC)**. Verrà visualizzato Amministratore origine dati ODBC, in cui è possibile creare un nome di origine dati.  
  
-   In un'applicazione ODBC, chiamare [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md).  
  
 Un'origine dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene gli elementi seguenti:  
  
-   Nome dell'origine dati.  
  
-   Tutte le informazioni necessarie per connettersi a un'istanza specifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Database predefinito da utilizzare in un'istanza specifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (facoltativo).  
  
-   Impostazioni quali le opzioni ANSI da utilizzare, la registrazione o meno delle statistiche sulle prestazioni e così via (facoltativo).  
  
 Un'applicazione ODBC non è necessaria per connettersi tramite un'origine dati. L'applicazione, tuttavia, deve fornire le stesse informazioni sulla connettività a una funzione di connessione ODBC altrimenti individuate dal driver in un nome di origine dati.  
  
## <a name="see-also"></a>Vedere anche  
 [Comunicazione con SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
