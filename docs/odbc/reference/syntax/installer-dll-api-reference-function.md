---
description: Informazioni di riferimento sulle funzioni dell'API DLL del programma di installazione
title: Funzione di riferimento API DLL del programma di installazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- installer DLL [ODBC]
ms.assetid: 47fcadc3-f102-4989-9ee7-a1c65233142a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 88b1d85e731108984b1eb9f001363bd26401f91d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174681"
---
# <a name="installer-dll-api-reference-function"></a>Informazioni di riferimento sulle funzioni dell'API DLL del programma di installazione
Questa sezione descrive la sintassi delle funzioni nell'API DLL del programma di installazione. L'API DLL del programma di installazione è costituita da 20 funzioni. Tre di queste funzioni, **SQLGetTranslator**, **SQLRemoveDSNFromIni** e **SQLWriteDSNToIni**, vengono chiamate solo dalle DLL di installazione. Le altre funzioni vengono chiamate dai programmi di installazione e amministrazione.  
  
 Ogni funzione è contrassegnata con la versione di ODBC in cui è stata introdotta.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Funzione SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)  
  
-   [Funzione SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)  
  
-   [Funzione SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)  
  
-   [Funzione SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)  
  
-   [Funzione SQLGetInstalledDrivers](../../../odbc/reference/syntax/sqlgetinstalleddrivers-function.md)  
  
-   [Funzione SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)  
  
-   [Funzione SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)  
  
-   [Funzione SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)  
  
-   [Funzione SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)  
  
-   [Funzione SQLInstallerError](../../../odbc/reference/syntax/sqlinstallererror-function.md)  
  
-   [Funzione SQLInstallTranslator](../../../odbc/reference/syntax/sqlinstalltranslator-function.md)  
  
-   [Funzione SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)  
  
-   [SQLManageDataSources (funzione)](../../../odbc/reference/syntax/sqlmanagedatasources.md)  
  
-   [Funzione SQLPostInstallerError](../../../odbc/reference/syntax/sqlpostinstallererror-function.md)  
  
-   [Funzione SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)  
  
-   [Funzione SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)  
  
-   [Funzione SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)  
  
-   [Funzione SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)  
  
-   [Funzione SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)  
  
-   [Funzione SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)  
  
-   [Funzione SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)  
  
-   [Funzione SQLValidDSN](../../../odbc/reference/syntax/sqlvaliddsn-function.md)  
  
-   [Funzione SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)  
  
-   [Funzione SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)  
  
-   [Funzione SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)
