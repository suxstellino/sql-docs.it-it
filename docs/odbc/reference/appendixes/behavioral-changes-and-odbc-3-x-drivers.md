---
description: Modifiche del comportamento e driver ODBC 3.x
title: Modifiche comportamentali e driver ODBC 3. x | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_attr_odbc_version [ODBC]
- backward compatibility [ODBC], behavioral changes
- compatibility [ODBC], behavioral changes
ms.assetid: 88a503cc-bff7-42d9-83ff-8e232109ed06
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a76e950ad8c929d701a3c967b3c5f5422fd3db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212493"
---
# <a name="behavioral-changes-and-odbc-3x-drivers"></a>Modifiche del comportamento e driver ODBC 3.x
L'attributo Environment SQL_ATTR_ODBC_VERSION indica al driver se deve presentare il comportamento ODBC *2. x* o il comportamento ODBC *3. x* . La modalità di impostazione dell'attributo SQL_ATTR_ODBC_VERSION Environment dipende dall'applicazione. Le applicazioni ODBC *3. x* devono chiamare **SQLSetEnvAttr** per impostare questo attributo dopo aver chiamato **SQLAllocHandle** per allocare un handle di ambiente e prima di chiamare **SQLAllocHandle** per allocare un handle di connessione. In caso contrario, gestione driver restituisce SQLSTATE HY010 (errore della sequenza di funzioni) nella seconda chiamata a **SQLAllocHandle**.  
  
> [!NOTE]  
>  Per ulteriori informazioni sulle modifiche comportamentali e sul modo in cui un'applicazione agisce, vedere [modifiche del comportamento](../../../odbc/reference/develop-app/behavioral-changes.md).  
  
 Le applicazioni ODBC *2. x* e le applicazioni ODBC *2. x* ricompilate con i file di intestazione ODBC *3. x* non chiamano **SQLSetEnvAttr**. Tuttavia, chiamano **SQLAllocEnv** anziché **SQLAllocHandle** per allocare un handle di ambiente. Pertanto, quando l'applicazione chiama **SQLAllocEnv** in Gestione driver, gestione driver chiama **SQLAllocHandle** e **SQLSetEnvAttr** nel driver. Pertanto, i driver ODBC *3. x* possono sempre contare sull'impostazione di questo attributo.  
  
 Se un'applicazione conforme agli standard compilata con il flag di compilazione ODBC_STD chiama **SQLAllocEnv** (che potrebbe verificarsi poiché **SQLAllocEnv** non è deprecato in ISO), viene eseguito il mapping della chiamata a **SQLAllocHandleStd** in fase di compilazione. In fase di esecuzione, l'applicazione chiama **SQLAllocHandleStd**. Gestione driver imposta l'attributo SQL_ATTR_ODBC_VERSION Environment su SQL_OV_ODBC3. Una chiamata a **SQLAllocHandleStd** equivale a una chiamata a **SQLAllocHandle** con *HandleType* di SQL_HANDLE_ENV e una chiamata a **SQLSetEnvAttr** per impostare SQL_ATTR_ODBC_VERSION SQL_OV_ODBC3.  
  
 In alcune architetture di driver è necessario che il driver appaia come un driver ODBC *2. x* o un driver ODBC *3. x* , a seconda della connessione. Il driver in questo caso potrebbe non essere effettivamente un driver, ma un livello che risiede tra Gestione driver e un altro driver. Ad esempio, potrebbe simulare un driver come ODBC Spy. In un altro esempio, può fungere da gateway, come EDA/SQL. Per visualizzare un driver ODBC *3. x* , è necessario che tale driver sia in grado di **esportare SQLAllocHandle** e che venga visualizzato come driver *ODBC 2. x* , sia in grado di esportare **SQLAllocConnect**, **SQLAllocEnv** e **SQLAllocStmt**. Quando è necessario allocare un ambiente, una connessione o un'istruzione, gestione driver verifica se il driver Esporta **SQLAllocHandle**. Poiché il driver esegue tale operazione, gestione driver chiama **SQLAllocHandle** nel driver. Se il driver utilizza un driver ODBC *2. x* , il driver deve eseguire il mapping della chiamata a **SQLAllocHandle** a **SQLAllocConnect**, **SQLAllocEnv** o **SQLAllocStmt**, a seconda dei casi. Inoltre, non deve eseguire alcuna operazione con la chiamata **SQLSetEnvAttr** quando si comporta come un driver ODBC *2. x* .  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Tipi di dati datetime](../../../odbc/reference/appendixes/datetime-data-types.md)  
  
-   [Compatibilità con le versioni precedenti dei tipi di dati C](../../../odbc/reference/appendixes/backward-compatibility-of-c-data-types.md)  
  
-   [Segnalibri di lunghezza fissa](../../../odbc/reference/appendixes/fixed-length-bookmarks.md)  
  
-   [Supporto per SQLGetInfo](../../../odbc/reference/appendixes/sqlgetinfo-support.md)  
  
-   [Restituzione di SQL_NO_DATA](../../../odbc/reference/appendixes/returning-sql-no-data.md)  
  
-   [Chiamata di SQLSetPos per inserire i dati](../../../odbc/reference/appendixes/calling-sqlsetpos-to-insert-data.md)  
  
-   [Caricamento per ordinale](../../../odbc/reference/appendixes/loading-by-ordinal.md)
