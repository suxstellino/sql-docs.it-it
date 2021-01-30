---
description: Mapping di funzioni deprecate
title: Mapping di funzioni deprecate | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], about mapping deprecated functions
- backward compatibility [ODBC], mapping deprecated functions
- deprecated functions [ODBC]
- compatibility [ODBC], mapping deprecated functions
- functions [ODBC], mapping deprecated functions
- mapping deprecated functions [ODBC]
ms.assetid: ee462617-1d79-4c88-afeb-b129cff34cc6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f52a5f0a5c46c2f4de81f99440d657ae68b925a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206819"
---
# <a name="mapping-deprecated-functions"></a>Mapping di funzioni deprecate
In questa sezione viene descritto il mapping delle funzioni deprecate da parte di gestione driver ODBC *3. x* per garantire la compatibilità con le versioni precedenti dei driver ODBC *3. x* utilizzati con le applicazioni ODBC *2. x* . Gestione driver esegue questo mapping indipendentemente dalla versione dell'applicazione. Poiché ogni funzione ODBC *2. x* nell'elenco seguente viene mappata alla funzione ODBC *3. x* corrispondente quando viene chiamata in un driver ODBC *3. x* , non è necessario che il driver ODBC *3. x* implementi le funzioni ODBC *2. x* .  
  
 Il mapping nell'elenco viene attivato quando il driver è un driver ODBC *3. x* e il driver non supporta la funzione di cui è in corso il mapping.  
  
 Nella tabella seguente sono elencate tutte le funzionalità duplicate introdotte in ODBC *3. x*.  
  
|Funzione ODBC *2. x*|Funzione ODBC *3. x*|  
|-------------------------|-------------------------|  
|**SQLAllocConnect**|**SQLAllocHandle**|  
|**SQLAllocEnv**|**SQLAllocHandle**|  
|**SQLAllocStmt**|**SQLAllocHandle**|  
|**SQLBindParam**[1]|**SQLBindParameter**|  
|**SQLColAttributes**|**SQLColAttribute**|  
|**SQLError**|**SQLGetDiagRec**|  
|**SQLFreeConnect**|**SQLFreeHandle**|  
|**SQLFreeEnv**|**SQLFreeHandle**|  
|**SQLFreeStmt** con l' *opzione* SQL_DROP|**SQLFreeHandle**|  
|**SQLGetConnectOption**|**SQLGetConnectAttr**|  
|**SQLGetStmtOption**|**SQLGetStmtAttr**|  
|**SQLParamOptions**|**SQLSetStmtAttr**|  
|**SQLSetConnectOption**|**SQLSetConnectAttr**|  
|**SQLSetParam**[2]|**SQLBindParameter**|  
|**SQLSetScrollOption**|**SQLSetStmtAttr**|  
|**SQLSetStmtOption**|**SQLSetStmtAttr**|  
|**SQLTransact**|**SQLEndTran**|  
  
 [1] anche se questa funzione non esisteva in ODBC *2. x*, si trova negli standard Open Group e ISO.  
  
 [2] si tratta di una funzione ODBC 1,0.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Mapping di SQLAllocConnect](../../../odbc/reference/appendixes/sqlallocconnect-mapping.md)  
  
-   [Mapping di SQLAllocEnv](../../../odbc/reference/appendixes/sqlallocenv-mapping.md)  
  
-   [Mapping di SQLAllocStmt](../../../odbc/reference/appendixes/sqlallocstmt-mapping.md)  
  
-   [Mapping di SQLBindParam](../../../odbc/reference/appendixes/sqlbindparam-mapping.md)  
  
-   [Mapping di SQLColAttributes](../../../odbc/reference/appendixes/sqlcolattributes-mapping.md)  
  
-   [Mapping di SQLError](../../../odbc/reference/appendixes/sqlerror-mapping.md)  
  
-   [Mapping di SQLFreeConnect](../../../odbc/reference/appendixes/sqlfreeconnect-mapping.md)  
  
-   [Mapping di SQLFreeEnv](../../../odbc/reference/appendixes/sqlfreeenv-mapping.md)  
  
-   [Mapping di SQLFreeStmt](../../../odbc/reference/appendixes/sqlfreestmt-mapping.md)  
  
-   [Mapping di SQLGetConnectOption](../../../odbc/reference/appendixes/sqlgetconnectoption-mapping.md)  
  
-   [Mapping di SQLGetStmtOption](../../../odbc/reference/appendixes/sqlgetstmtoption-mapping.md)  
  
-   [Mapping di SQLInstallTranslator](../../../odbc/reference/appendixes/sqlinstalltranslator-mapping.md)  
  
-   [Mapping di SQLParamOptions](../../../odbc/reference/appendixes/sqlparamoptions-mapping.md)  
  
-   [Mapping di SQLSetConnectOption](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md)  
  
-   [Mapping di SQLSetParam](../../../odbc/reference/appendixes/sqlsetparam-mapping.md)  
  
-   [Mapping di SQLSetScrollOptions](../../../odbc/reference/appendixes/sqlsetscrolloptions-mapping.md)  
  
-   [Mapping di SQLSetStmtOption](../../../odbc/reference/appendixes/sqlsetstmtoption-mapping.md)  
  
-   [Mapping di SQLTransact](../../../odbc/reference/appendixes/sqltransact-mapping.md)
