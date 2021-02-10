---
description: Tipi di dati specifici del driver, tipi di descrittori, tipi di informazioni, tipi di diagnostica e attributi
title: Tipi di Driver-Specific-dati, descrittori, informazioni, diagnostica | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver-specific diagnostic values [ODBC]
- diagnostic information [ODBC], driver-specific values
- ODBC drivers [ODBC], driver-specific diagnostic values
ms.assetid: ad4c76d3-5191-4262-b47c-5dd1d19d1154
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03cd6b0ed9a424a161f88f4bd525941895d3d201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062566"
---
# <a name="driver-specific-data-types-descriptor-types-information-types-diagnostic-types-and-attributes"></a>Tipi di dati specifici del driver, tipi di descrittori, tipi di informazioni, tipi di diagnostica e attributi
I driver possono allocare i valori specifici del driver per gli elementi seguenti:  
  
-   **Indicatori del tipo di dati SQL** Questi vengono usati in *ParameterType* in **SQLBindParameter** e in *DataType* in **SQLGetTypeInfo** e restituiti da **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLGetTypeInfo**, **SQLDescribeParam**, **SQLProcedureColumns** e **SQLSpecialColumns**.  
  
-   **Campi del descrittore** Questi vengono usati in *FieldIdentifier* in **SQLColAttribute**, **SQLGetDescField** e **SQLSetDescField**.  
  
-   **Campi di diagnostica** Questi vengono usati in *DiagIdentifier* in **SQLGetDiagField** e **SQLGetDiagRec**.  
  
-   **Tipi di informazioni** Questi vengono usati in *InfoType* in **SQLGetInfo**.  
  
-   **Attributi di connessione e di istruzione** Questi vengono usati nell' *attributo* in **SQLGetConnectAttr**, **SQLGetStmtAttr**, **SQLSetConnectAttr** e **SQLSetStmtAttr**.  
  
 Per ognuno di questi elementi, sono disponibili due set di valori: valori riservati per l'utilizzo da ODBC e valori riservati per l'utilizzo da driver. Prima di implementare valori specifici del driver, un writer di driver deve richiedere un valore per ogni tipo, campo o attributo specifico del driver del gruppo aperto. Per lo sviluppo di nuovi driver, usare l'intervallo descritto nella tabella seguente. In Gestione driver ODBC 3,8 non verrà generato un errore se viene utilizzato un valore sconosciuto non compreso nell'intervallo descritto di seguito. Tuttavia, le versioni successive di gestione driver potrebbero generare un errore se vengono ricevuti valori sconosciuti che non sono compresi nell'intervallo.  
  
 Quando uno di questi valori viene passato a una funzione ODBC, il driver deve controllare se il valore è valido. I driver restituiscono SQLSTATE HYC00 (funzionalità facoltativa non implementata) per i valori specifici del driver applicabili ad altri driver.  
  
 A partire da ODBC 3,8, i writer di driver possono allocare attributi specifici del driver all'interno di un intervallo riservato.  
  
> [!NOTE]  
>  Gestione driver ODBC 3,8 non convalida né impone questi intervalli per la compatibilità con le versioni precedenti. Una versione futura di gestione driver potrebbe tuttavia applicarli.  
  
|Tipo di attributo|Tipo di dati ODBC|Base intervallo specifico del driver|Limite intervallo specifico del driver|Costante ODBC per base intervallo di valori specifico del driver|  
|--------------------|--------------------|---------------------------------|----------------------------------|---------------------------------------------------------|  
|Indicatori del tipo di dati SQL|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_SQL_TYPE_BASE|  
|Campi del descrittore|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DESCRIPTOR_BASE|  
|Campi di diagnostica|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DIAGNOSTIC_BASE|  
|Tipi di informazioni|SQLUSMALLINT|0x4000|0x7FFF|SQL_DRIVER_INFO_TYPE_BASE|  
|Attributi di connessione|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_CONNECT_ATTR_BASE|  
|Attributi di istruzione|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_STATEMENT_ATTR_BASE|  
  
> [!NOTE]  
>  I tipi di dati specifici del driver, i campi di descrizione, i campi di diagnostica, i tipi di informazioni, gli attributi di istruzione e gli attributi di connessione devono essere descritti nella documentazione del driver. Quando uno di questi valori viene passato a una funzione ODBC, il driver deve controllare se il valore è valido. I driver restituiscono SQLSTATE HYC00 (funzionalità facoltativa non implementata) per i valori specifici del driver applicabili ad altri driver.  
  
 I valori di base sono definiti per semplificare lo sviluppo di driver. Ad esempio, gli attributi di diagnostica specifici del driver possono essere definiti nel formato seguente:  
  
```  
SQL_DRIVER_DIAGNOSTIC_BASE+0, SQL_DRIVER_DIAGNOSTIC_BASE +1  
```
