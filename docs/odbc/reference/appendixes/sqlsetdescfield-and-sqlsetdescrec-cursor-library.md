---
description: SQLSetDescField and SQLSetDescRec (libreria di cursori)
title: SQLSetDescField e SQLSetDescRec (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDescField function [ODBC], Cursor Library
- SQLSetDescRec function [ODBC], Cursor Library
ms.assetid: 4ccff067-85cd-4bfa-a6cd-7f28051fb5b9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c97bbb38b5050461d3ca305d8ac895d693567be3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202647"
---
# <a name="sqlsetdescfield-and-sqlsetdescrec-cursor-library"></a>SQLSetDescField and SQLSetDescRec (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo delle funzioni **SQLSetDescField** e **SQLSetDescRec** nella libreria di cursori. Per informazioni generali su queste funzioni, vedere [funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) e [funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 La libreria di cursori esegue **SQLSetDescField** quando viene chiamato per restituire il valore dei campi impostati per le colonne Bookmark:  
  
 SQL_DESC_DATA_PTR  
  
 SQL_DESC_INDICATOR_PTR  
  
 SQL_DESC_OCTET_LENGTH_PTR  
  
 SQL_DESC_LENGTH  
  
 SQL_DESC_OCTET_LENGTH  
  
 SQL_DESC_DATETIME_INTERVAL_CODE  
  
 SQL_DESC_SCALE  
  
 SQL_DESC_PRECISION  
  
 SQL_DESC_TYPE  
  
 SQL_DESC_NAME  
  
 SQL_DESC_UNNAMED  
  
 SQL_DESC_NULLABLE  
  
 La libreria di cursori esegue le chiamate a **SQLSetDescRec** per una colonna di segnalibri.  
  
 Quando si utilizza un driver ODBC *2. x* , la libreria di cursori restituisce SQLSTATE HY090 (lunghezza di stringa o di buffer non valida) quando viene chiamato **SQLSetDescField** o **SQLSetDescRec** per impostare il campo SQL_DESC_OCTET_LENGTH per il record di segnalibro di un ARD su un valore diverso da 4. Quando si utilizza un driver ODBC *3. x* , la libreria di cursori consente che il buffer sia di qualsiasi dimensione.  
  
 La libreria di cursori esegue **SQLSetDescField** quando viene chiamato per restituire il valore del campo SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE o SQL_DESC_ROW_STATUS_PTR. Questi campi possono essere restituiti per qualsiasi riga, non solo per la riga del segnalibro.  
  
 La libreria di cursori non esegue **SQLSetDescField** per modificare alcun campo del descrittore oltre ai campi citati in precedenza. Se un'applicazione chiama **SQLSetDescField** per impostare qualsiasi altro campo durante il caricamento della libreria di cursori, la chiamata viene passata al driver.  
  
 La libreria di cursori supporta la modifica dinamica dei campi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR di qualsiasi riga di un descrittore di righe dell'applicazione (dopo una chiamata a **SQLExtendedFetch**, **SQLFetch** o **SQLFetchScroll**). Il campo SQL_DESC_OCTET_LENGTH_PTR può essere modificato in un puntatore null solo per disassociare il buffer di lunghezza per una colonna.  
  
 La libreria di cursori non supporta la modifica del campo SQL_DESC_BIND_TYPE in un oggetto APD o ARD quando un cursore è aperto. Il campo SQL_DESC_BIND_TYPE può essere modificato solo dopo la chiusura del cursore e prima dell'apertura di un nuovo cursore. Gli unici campi del descrittore che la libreria di cursori supporta cambiando quando un cursore è aperto sono SQL_DESC_ARRAY_STATUS_PTR, SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR e SQL_DESC_ROWS_PROCESSED_PTR.  
  
 La libreria di cursori non supporta la modifica del campo SQL_DESC_COUNT di ARD dopo la chiamata di **SQLExtendedFetch** o **SQLFetchScroll** e prima della chiusura del cursore.
