---
description: Riepilogo delle funzioni DLL programma di installazione
title: Riepilogo delle funzioni DLL del programma di installazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], installer DLL functions
- installer DLL [ODBC]
ms.assetid: 666c09d3-1e10-4d89-9b42-eda2957a87f0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f5cce294142a716180b66544ebe12638e83f792e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174663"
---
# <a name="installer-dll-function-summary"></a>Riepilogo delle funzioni DLL programma di installazione
Nella tabella seguente vengono descritte le funzioni nella DLL del programma di installazione. Per ulteriori informazioni sulla sintassi e la semantica per ogni funzione, vedere Guida di [riferimento all'API DLL del programma di installazione](../../../odbc/reference/syntax/installer-dll-api-reference-function.md).  
  
|Attività|Nome della funzione|Scopo|  
|----------|-------------------|-------------|  
|Installazione di ODBC|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|Carica la DLL di installazione specifica del driver.|  
||[SQLGetInstalledDrivers](../../../odbc/reference/syntax/sqlgetinstalleddrivers-function.md)|Restituisce un elenco di driver installati.|  
||[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|Aggiunge un driver alle informazioni di sistema.|  
||[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|Restituisce la directory di destinazione per Gestione driver.|  
||[SQLInstallerError](../../../odbc/reference/syntax/sqlinstallererror-function.md)|Restituisce informazioni sull'errore o sullo stato per le funzioni del programma di installazione.|  
||[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|Aggiunge un convertitore alle informazioni di sistema.|  
||[SQLPostInstallerError](../../../odbc/reference/syntax/sqlpostinstallererror-function.md)|Consente a una libreria di installazione di un driver o di un convertitore di segnalare gli errori.|  
||[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|Rimuove un driver dalle informazioni di sistema.|  
||[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|Rimuove i componenti di base di ODBC dalle informazioni di sistema.|  
||[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|Rimuove il convertitore dalle informazioni di sistema.|  
|Configurazione delle origini dati|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|Chiama la DLL di installazione specifica del driver.|  
||[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|Consente di visualizzare una finestra di dialogo per l'aggiunta di un'origine dati.|  
||[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|Recupera la modalità di configurazione che indica il punto in cui la voce Odbc.ini elencando i valori DSN è presente nelle informazioni di sistema.|  
||[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|Scrive un valore nelle informazioni di sistema.|  
||[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|Visualizza una finestra di dialogo per selezionare un convertitore.|  
||[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|Visualizza una finestra di dialogo per configurare le origini dati e i driver.|  
||[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|Legge le informazioni dai DSN del file.|  
||[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|Rimuove l'origine dati predefinita.|  
||[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|Rimuove un'origine dati.|  
||[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|Imposta la modalità di configurazione che indica il punto in cui la voce Odbc.ini elencando i valori DSN è presente nelle informazioni sul sistema.|  
||[SQLValidDSN](../../../odbc/reference/syntax/sqlvaliddsn-function.md)|Verifica la lunghezza e la validità del nome dell'origine dati.|  
||[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|Aggiunge un'origine dati.|  
||[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|Scrive le informazioni nei DSN del file.|  
||[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|Ottiene un valore dalle informazioni di sistema.|
